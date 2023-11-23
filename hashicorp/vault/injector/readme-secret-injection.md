# Secret Injection Guides

I've broken this down into basic guides to avoid this document from becoming too large.

## Basic Secrets

Objective:

---

- Let's create a basic secret in vault manually
- Application consumes the secret automatically

[Try it](./example-apps/basic-secret/readme.md)

## Dynamic Secrets: Postgres

Objective:

---

- We have a Postgres Database
- Let's delegate Vault to manage life cycles of our database credentials
- Deploy an app, that automatically gets it's credentials from vault

[Try it](./example-apps/dynamic-postgresql/readme.md)
