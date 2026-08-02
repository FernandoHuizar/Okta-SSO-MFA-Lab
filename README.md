# Okta Identity Management Lab

## Overview

Configured an Okta Workforce Identity tenant to simulate enterprise Identity Provider (IdP) operations, including multi-protocol SSO integration (SAML 2.0 and OIDC), organization-wide MFA enforcement, custom Sign-On Policies, and real-world authentication troubleshooting. This lab mirrors the exact Okta workflows used in production enterprise environments, including a live SAML trust relationship configured end-to-end against a real Salesforce Developer org.

## Environment

- **Platform:** Okta Integrator Free Plan
- **Tenant:** integrator-5316119-admin.okta.com
- **Role:** Okta Administrator
- **Users configured:** 5 (4 test users + admin)
- **Secondary environment:** Salesforce Developer Edition org (used to validate real SAML trust configuration)

## What I Built

### User & Group Management

- Created 4 test users, each assigned to a department group (RealMadrid, ManCity, Bayern, PSG)
- Demonstrated least-privilege access through individual and group-based application assignments

### Multi-Protocol SSO Integration

**Salesforce (SAML 2.0)**
- Located Salesforce integration in Okta's App Integration Catalog
- Configured Okta as Identity Provider (IdP) with Salesforce as Service Provider (SP)
- Completed a full, live SAML trust relationship: signed up for a real Salesforce Developer org, imported Okta's IdP metadata into Salesforce's Single Sign-On Settings, and validated the end-to-end SSO handshake

**Zendesk (SAML 2.0)**
- Second SAML integration to demonstrate repeatable, multi-app SSO configuration

**InternalTools (OIDC / OAuth 2.0)**
- Built a custom OIDC Web Application integration using the Authorization Code grant type
- Configured redirect URIs, client credentials, and group-based access assignment
- Demonstrates protocol range beyond SAML, covering the modern OAuth 2.0/OIDC standard used by most cloud-native applications

### MFA Policy Configuration

- Navigated to Security → Authenticators
- Set Okta Verify to Required under the org-wide Default Policy
- Enforces zero trust principles: no user is trusted based on password alone

### Sign-On Policies

- Built a custom Sign-On Policy applied across all 3 integrated applications
- Configured MFA enforcement (any 2 factor types) and a 4-hour re-authentication requirement, tightening session trust beyond default SSO behavior
- Demonstrates policy design distinct from Microsoft's Conditional Access, using Okta's native authentication policy framework

### Troubleshooting Scenarios

Documented 5 real, reproduced authentication failures with root-cause analysis and resolution steps:

1. **Provisioning failure (Password Expired status)** — A user created without a password landed in "Password Expired" status. Resolved by deactivating and recreating the account.
2. **Missing application access after removal** — Removing an individually-assigned application fully revokes access, even if the user remains in other groups tied to that app. Root cause: app assignments are per-user unless explicitly pushed via group assignment. Resolved by reassigning the app directly.
3. **MFA lockout** — Simulated a user losing access to their enrolled authenticator. Resolved using Okta's Reset Authenticators admin action, allowing the user to re-enroll.
4. **SAML trust misconfiguration** — Initial SAML sign-in attempts failed with a Salesforce "Single Sign-On Error" because only the Okta (IdP) side was configured. Root cause: SAML 2.0 requires trust configured on both the IdP and SP. Resolved by importing Okta's SAML metadata into Salesforce's Single Sign-On Settings, establishing a working, verified trust relationship.
5. **OIDC redirect URI mismatch** — Intentionally misconfigured the OIDC app's registered redirect URI, reproducing a real `invalid_request` / `redirect_uri mismatch` error (HTTP 400) during the OAuth authorization flow. Resolved by correcting the registered URI to match the client application's expected callback.

## SAML 2.0 Architecture

| Component | Role |
|-----------|------|
| Okta | Identity Provider (IdP) — authenticates users |
| Salesforce / Zendesk | Service Provider (SP) — trusts Okta for authentication |
| SAML Assertion | Signed XML token Okta sends to the SP after authentication |
| Metadata URL | Contains IdP configuration imported by the SP to establish trust |

## OIDC Architecture

| Component | Role |
|-----------|------|
| Okta | Authorization Server — issues tokens |
| InternalTools | Client Application — requests authentication via Authorization Code grant |
| Redirect URI | Registered callback endpoint; must match exactly or the request is rejected |
| Access/ID Token | Issued after successful authentication and consent |

## Key Concepts Demonstrated

- SAML 2.0 federation protocol (Identity Provider vs Service Provider architecture)
- OAuth 2.0 / OIDC Authorization Code flow
- Multi-protocol SSO integration across 3 applications
- Multi-Factor Authentication enforcement
- Custom Sign-On Policies and session re-authentication controls
- Zero Trust security model
- Least privilege and group-based access control
- Real-world authentication troubleshooting and root-cause documentation
- Cross-platform SSO validation (Okta + a real Salesforce org)

## Technologies Used

`Okta` `SAML 2.0` `OAuth 2.0` `OIDC` `Single Sign-On (SSO)` `Multi-Factor Authentication` `Sign-On Policies` `Identity and Access Management` `Zero Trust` `Salesforce`
