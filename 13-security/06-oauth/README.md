# OAuth

Practical, structured notes for understanding **OAuth fundamentals, roles, authorization flows, authorization code, PKCE, client credentials, refresh tokens, scopes, consent, OpenID Connect, and secure OAuth implementation**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|OAuth Fundamentals|OAuth concepts, delegated authorization, access tokens, resources, clients, and authorization servers|
|02|OAuth Roles|Resource owner, client, authorization server, resource server, and responsibilities of each role|
|03|OAuth Flows|Authorization flows, grant types, flow selection, redirect-based authorization, and token issuance|
|04|Authorization Code|Authorization-code flow, redirects, authorization codes, token exchange, and client authentication|
|05|PKCE|Code verifiers, code challenges, authorization-code protection, and secure public-client flows|
|06|Client Credentials|Machine-to-machine authorization, confidential clients, client authentication, and service access|
|07|Refresh Tokens|Refresh-token usage, token rotation, expiration, revocation, storage, and lifecycle management|
|08|Scopes & Consent|Scope design, permissions, user consent, delegated access, and least-privilege authorization|
|09|OpenID Connect|Identity layer concepts, ID tokens, user authentication, claims, discovery, and OAuth integration|
|10|OAuth Security Best Practices|Secure redirects, PKCE, token protection, scope control, client security, and implementation hardening|

## Structure

The notes progress from **OAuth fundamentals → OAuth roles → OAuth flows → authorization code → PKCE → client credentials → refresh tokens → scopes and consent → OpenID Connect → OAuth security best practices**.

The first sections establish OAuth's delegated-authorization model and explain the roles involved in an authorization flow. OAuth flows then introduce the different ways clients can obtain authorization and access tokens.

The authorization-code and PKCE sections focus on browser-based authorization and protection against authorization-code interception. Client credentials then covers machine-to-machine authorization, while refresh tokens explain long-lived authorization and token lifecycle management.

Scopes and consent introduce fine-grained delegated permissions. OpenID Connect then extends OAuth with an identity layer for user authentication and identity claims.

The final section brings these concepts together through practical OAuth security practices, including redirect protection, token handling, least privilege, client security, and implementation hardening.

Each topic is separated into focused notes for easier learning, implementation, revision, security review, and practical application.

## Focus

- OAuth fundamentals and roles
- Authorization flows
- Authorization code and PKCE
- Client credentials and refresh tokens
- Scopes and consent
- OpenID Connect
- OAuth security practices

## Goal

> Build a practical OAuth knowledge base that makes it easy to understand delegated authorization, choose appropriate OAuth flows, protect authorization and tokens, implement OpenID Connect, and build secure authorization systems.
