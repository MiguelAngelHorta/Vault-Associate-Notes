# Vault-Associate-Notes
vault exam

![image](https://github.com/MiguelAngelHorta/Vault-Associate-Notes/assets/106134627/ab1c39f7-91b9-4506-9a10-2e2deafb6816)

# HashiCorp Vault Associate Exam Study Guide

## Exam Objectives

### 1. Authentication Methods
- **Describe Authentication Methods:**
  - `vault auth list`: List enabled authentication methods.
  - Common methods include token-based, LDAP, AppRole, etc.
  - Consider factors like security, ease of management, and integration with existing systems when choosing.
- **Choose an Authentication Method Based on Use Case:**
  - `vault auth enable <method>`: Enable a specific authentication method.
  - Evaluate methods based on factors like security requirements, ease of integration, etc.
  - For example, LDAP might be suitable for integrating with corporate directories, while AppRole provides machine-to-machine authentication.
- **Differentiate Human vs. System Auth Methods:**
  - `vault auth list -format=json`: List authentication methods in JSON format.
  - Human methods are typically interactive, requiring user input, while system methods are automated and intended for machine-to-machine communication.

### 2. Vault Policies
- **Illustrate the Value of Vault Policy:**
  - Policies control access to Vault resources, ensuring proper authorization.
  - Well-defined policies enhance security and enforce least privilege principles.
  - Regularly review and update policies to align with changing access requirements and security standards.
- **Describe Vault Policy Syntax:**
  - Policies are written in HashiCorp Configuration Language (HCL).
  - They define paths and capabilities, restricting or allowing actions.
  - Utilize comments and clear naming conventions to enhance policy readability and maintainability.
- **Craft a Vault Policy Based on Requirements:**
  - `vault policy write <name> <policy.hcl>`: Write a policy to Vault.
  - Consider the principle of least privilege when crafting policies.
  - Test policies thoroughly to ensure they provide the necessary access without exposing sensitive information.

### 3. Vault Tokens
- **Describe Vault Token:**
  - Tokens are issued upon successful authentication to Vault.
  - They act as credentials for accessing Vault resources.
  - Implement token management practices such as rotation, expiration, and revocation to maintain security.
- **Differentiate Between Service and Batch Tokens:**
  - `vault token create -type=<type>`: Create a token of a specific type.
  - Service tokens are long-lived and used by applications, while batch tokens are short-lived and for automated processes.
  - Monitor token usage and revoke unnecessary tokens to reduce the attack surface.
- **Describe Root Token Uses and Lifecycle:**
  - Root tokens have superuser privileges and should be tightly controlled.
  - They are used for initial setup and emergency access but should be phased out.
  - Safeguard root tokens by storing them securely and minimizing their use in day-to-day operations.
- **Define Token Accessors:**
  - Accessors are unique identifiers for tokens, used for token-related operations.
  - Accessors provide a way to interact with tokens without exposing the token itself, enhancing security.

### 4. Manage Vault Leases
- **Explain the Purpose of a Lease ID:**
  - Lease IDs are associated with every secret in Vault.
  - They determine the lease duration and govern the lifecycle of secrets.
  - Understand the implications of lease durations on secret management and application behavior.
- **Renew Leases:**
  - `vault lease renew <lease_id>`: Renew a lease to extend its validity.
  - Renewal is possible if the token has sufficient privileges.
  - Implement automated lease renewal mechanisms to prevent disruption to applications relying on Vault secrets.
- **Revoke Leases:**
  - `vault lease revoke <lease_id>`: Revoke a lease to immediately invalidate a secret.
  - Revocation is irreversible and should be used judiciously.
  - Regularly audit and revoke unnecessary leases to maintain control over secrets and reduce security risks.

### 5. Compare and Configure Vault Secrets Engines
- **Choose a Secret Method Based on Use Case:**
  - `vault secrets list`: List enabled secrets engines.
  - Consider factors like data sensitivity, rotation requirements, and integration capabilities when selecting a secrets engine.
  - Evaluate the trade-offs between dynamic and static secrets to choose the most suitable approach for the application architecture.


# DEV Server
Start DEV server
```ps
vault server -dev
```

# Before login
Set vault address before log in
```ps
$env:VAULT_ADDR="http://127.0.0.1:8200"
```

# Login
## Login using token
```ps
vault login
```

## Login using userpass
```ps
vault login -method=userpass username=$userName
```

## Secret / KV
### Set a secret in KV Secret Engine
```ps
vault kv put $kvName/$secretName $key=$value
```

```ps
vault kv put secret/secretName key=value
// or old syntax
vault write secret/secretName key=value
```

### Get a secret from KV Secret Engine
```ps
vault kv get secret/secretName
// or old syntax
vault read secret/secretName
```

### Move a secret between KV Secret Engines
```ps
vault secrets move kvSecret1/secretName kvSecret2/secretName 
```

### List all secrets in a Secret Engine
```ps
vault kv list secretEngineName
```

### List all Secrets Engines
```ps
vault secrets list
```

### Upgrade Secret Engine to v2 (v1 is default)
```ps
vault kv enable-versioning secret
```
### Create new KV Secret Engine
```ps
vault secrets enable -path=devkv kv
```

# Token
## Get information about current token
```ps
vault token lookup
```

# Userpass
## Login
```ps
vault login -method=userpass username=$userName
```

## Add a user
```ps
vault write auth/userpass/users/arthur password=dent
```

## List users
```ps
vault list auth/userpass/users
```

# Policies
## List all policies
```ps
vault policy list
```
