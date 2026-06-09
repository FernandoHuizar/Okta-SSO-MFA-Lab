# Okta SSO & MFA Lab

## Overview
Configured an Okta Workforce Identity tenant to simulate enterprise Identity Provider (IdP) operations including user management, SAML 2.0 SSO application integration, and organization-wide MFA enforcement. This lab mirrors the exact Okta workflows used in production enterprise environments.

## Environment
- **Platform:** Okta Workforce Identity (30-day trial)
- **Tenant:** trial-2361751-admin.okta.com
- **Role:** Okta Administrator
- **Users configured:** 4 (3 test users + admin)
<img width="1882" height="864" alt="image" src="https://github.com/user-attachments/assets/d3c91d61-6a85-4899-91e5-ff3514bc8a75" />


## What I Built

### User Management
- Created and managed user accounts in Okta
- Configured user profiles with department and contact attributes
- Resolved real-world provisioning issue — user created without password
  landed in Password Expired status, resolved by deactivating and recreating
- Demonstrated least privilege access — only assigned users can access
  applications through Okta

### Salesforce SAML 2.0 SSO Integration
- Located Salesforce integration in Okta's App Integration Catalog (8000+ connectors)
- Selected SAML 2.0 as sign-on method over SWA — industry standard federation
  protocol used by enterprises
- Configured Okta as Identity Provider (IdP) and Salesforce as Service Provider (SP)
- Generated SAML Metadata URL containing IdP configuration details
<img width="734" height="775" alt="image" src="https://github.com/user-attachments/assets/a19610da-495a-4e38-88ce-9692233baa4f" />

- Assigned users individually to the Salesforce application demonstrating
  least privilege access control
  <img width="856" height="605" alt="image" src="https://github.com/user-attachments/assets/257879bd-8b6e-46ce-9538-6c0c296e836d" />

- Configuration mirrors real enterprise deployment — identical to what would
  be done in a production environment

### MFA Policy Configuration
- Navigated to Security → Authenticators
- Edited Default Policy applied to Everyone group
- Set Okta Verify to Required — users cannot skip MFA enrollment
- Set Email to Optional
- Password remains Required
- Enforces zero trust principles — no user trusted based on password alone
  regardless of location or network
  <img width="834" height="695" alt="image" src="https://github.com/user-attachments/assets/a873460f-96f8-4114-a5f7-798e0e51bbeb" />


## SAML 2.0 Architecture
| Component | Role |
|-----------|------|
| Okta | Identity Provider (IdP) — authenticates users |
| Salesforce | Service Provider (SP) — trusts Okta for authentication |
| SAML Assertion | Signed XML token Okta sends to Salesforce after authentication |
| Metadata URL | Contains IdP configuration imported by the SP |

## Key Concepts Demonstrated
- SAML 2.0 federation protocol
- Identity Provider vs Service Provider architecture
- Single Sign-On (SSO) configuration
- Multi-Factor Authentication enforcement
- Zero Trust security model
- Least privilege access control
- Real-world user provisioning and troubleshooting
- Enterprise app integration catalog

## Technologies Used
`Okta` `SAML 2.0` `Single Sign-On (SSO)` `Multi-Factor Authentication` `Identity and Access Management` `Zero Trust` `Salesforce`
