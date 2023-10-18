# Introduction to Flux CD

## Create a kubernetes cluster

In this guide we we''ll need a Kubernetes cluster for testing. Let's create one using [kind](https://kind.sigs.k8s.io/) </br>

```
kind create cluster --name fluxcd --image kindest/node:v1.26.3
```

## Run a container to work in

### run Alpine Linux: 
```
docker run -it --rm -v ${HOME}:/root/ -v ${PWD}:/work -w /work --net host alpine sh
```

### install some tools

```
# install curl 
apk add --no-cache curl

# install kubectl 
curl -sLO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl

```

### test cluster access:
```
/work # kubectl get nodes
NAME                    STATUS   ROLES    AGE   VERSION
fluxcd-control-plane   Ready    control-plane   54s   v1.26.3
```

## Get the Flux CLI

Let's download the `flux` command-line utility. </br>
We can get this utility from the GitHub [Releases page](https://github.com/fluxcd/flux2/releases) </br>

It's also worth noting that you want to ensure you get a compatible version of flux which supports your version of Kubernetes. Checkout the [prerequisites](https://fluxcd.io/flux/installation/#prerequisites) page. </br>

```
curl -o /tmp/flux.tar.gz -sLO https://github.com/fluxcd/flux2/releases/download/v2.1.1/flux_2.1.1_linux_amd64.tar.gz
tar -C /tmp/ -zxvf /tmp/flux.tar.gz
mv /tmp/flux /usr/local/bin/flux
chmod +x /usr/local/bin/flux
```

Now we can run `flux --help` to see its installed

## Documentation

As with every guide, we start with the documentation </br>
The [Core Concepts](https://fluxcd.io/flux/concepts/) is a good place to start. </br>

We begin by following the steps under the [bootstrap](https://fluxcd.io/flux/installation/#bootstrap) section for GitHub </br>

We'll need to generate a [personal access token (PAT)](https://github.com/settings/tokens/new) that can create repositories by checking all permissions under `repo`.  </br>

Once we have a token, we can set it: 

```
export GITHUB_TOKEN=<your-token>
```

Then we can bootstrap it using the GitHub bootstrap method

```
flux bootstrap github \
  --token-auth \
  --owner=Kolawole-Ikeoluwa-Joshua \
  --repository=k8s-admin-dev \
  --path=fluxcd/repositories/infra-repo/clusters/dev-cluster \
  --personal \
  --branch main

flux check

# flux manages itself using GitOps objects:
kubectl -n flux-system get GitRepository
kubectl -n flux-system get Kustomization
```

Check the source code that `flux bootstrap` created 

```
git pull origin <branch-name>
```
# Understanding GitOps Repository structures

Generally, in GitOps you have a dedicated repo for infrastructure templates. </br>
Your infrastructure will "sync" from this repo </br>

```
                                                    
 developer    +-----------+     +----------+           
              |           |     | CI       |           
  ----------> | REPO(code)|---> | PIPELINE |           
              +-----------+     +----------+           
                                         |  commit     
                                         v             
           +----------+ sync    +----------+           
           |  INFRA   |-------> |INFRA     |           
           |  (k8s)   |         |REPO(yaml)|           
           +----------+         +----------+           
                                                            
```

Flux repository structure [documentation](https://fluxcd.io/flux/guides/repository-structure/)

* Mono Repo (all k8s YAML in same "infra repo")
* Repo per team
* Repo per app 

Take note in this guide the folders under `fluxcd/repositories` represent different GIT repos

```
- repositories
  - infra-repo
  - example-app-1
  - example-app-2
```
## build our example apps

Let's say we have a microservice called `example-app-1` and it has its own GitHub repo somewhere. </br>
For demo, it's code is under `fluxcd/repositories/example-app-1/`

```
# go to our "git repo"
cd fluxcd/repositories/example-app-1
# check the files
ls

cd src
docker build . -t example-app-1:0.0.0

#load the image to our test cluster so we dont need to push to a registry
# pretend to be a CI server, since there is none setup in this demo
kind load docker-image example-app-1:0.0.0 --name fluxcd 
```
## setup our gitops pipeline

Now we will also have a "infra-repo" GitHub repo where infrastructure configuration files for GitOps live.

```
cd fluxcd

# tell flux where our Git repo is and where the YAML is
# this is one off
# flux will monitor the example-app-1 Git repo for when any infrastructure changes, it will sync
kubectl -n default apply -f repositories/infra-repo/apps/example-app-1/gitrepository.yaml
kubectl -n default apply -f repositories/infra-repo/apps/example-app-1/kustomization.yaml

# check our flux resources 
kubectl -n default describe gitrepository example-app-1
kubectl -n default describe kustomization example-app-1

# check deployed resources
kubectl get all

kubectl port-forward svc/example-app-1 80:80

```

Now we have setup CD, let's take a look at CI </br>