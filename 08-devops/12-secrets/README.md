# Secrets

Practical, structured notes for securely storing, managing, rotating, accessing, and protecting **application and infrastructure secrets**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Secrets Management|Secret management concepts, credentials, API keys, tokens, certificates, and secure handling practices|
|02|Environment Variables|Environment variables, configuration injection, runtime secrets, and environment-specific configuration|
|03|Secret Storage|Secure secret storage, encrypted storage, access control, secret stores, and avoiding secrets in source code|
|04|Secret Rotation|Secret rotation strategies, credential expiration, automated rotation, revocation, and rotation workflows|
|05|Vault|HashiCorp Vault fundamentals, secret engines, authentication, policies, and dynamic secrets|
|06|Secret Security|Secret exposure risks, access control, encryption, auditing, monitoring, and secret security practices|

## Structure

The notes progress from **secrets management fundamentals → environment variables → secure secret storage → secret rotation → Vault → secret security**.

The early sections establish what secrets are and how applications and infrastructure receive sensitive configuration at runtime. Secret storage and rotation then cover how credentials should be protected, updated, revoked, and managed throughout their lifecycle.

The Vault section focuses on centralized secret management and dynamic credentials, while the final section brings these practices together for protecting secrets against accidental exposure, unauthorized access, and operational mistakes.

Each topic is separated into focused notes for easier learning, reference, and expansion.

## Focus

- Secrets management
- Passwords and credentials
- API keys and access tokens
- Certificates and private keys
- Environment variables
- Runtime configuration
- Environment-specific secrets
- Secure secret storage
- Encryption and access control
- Secret stores
- Secret rotation
- Credential expiration and revocation
- Automated secret rotation
- HashiCorp Vault
- Vault secret engines
- Vault authentication
- Vault policies
- Dynamic secrets
- Secret exposure prevention
- Secret auditing and monitoring
- Secret security practices

## Goal

> Build a practical secrets management knowledge base that makes it easy to store, access, rotate, audit, and protect sensitive credentials across development, CI/CD, infrastructure, and production environments.
