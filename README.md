# Okta SSO and MFA Lab

**The problem:** people juggle a separate password for every app, and a stolen password is often all an attacker needs.

**What I did:** set up Okta so users sign in once to reach 3 apps (single sign-on), required a second proof of identity (MFA), and reproduced 5 common sign-in problems a help desk or IAM analyst gets asked to fix.

*Screenshot lab from earlier in my learning. My main project is [Havlund Pharma](https://github.com/FernandoHuizar/havlund-identity-lifecycle).*

## IAM ideas this lab shows

| Idea | What it means | Where it shows up here |
|---|---|---|
| Single sign-on (SSO) | Sign in once, get into every app you're allowed to use | Salesforce, Zendesk and an internal app all trust Okta |
| Identity provider and app | The identity provider (Okta) checks who you are. The app trusts its answer | Set up on both sides for Salesforce |
| MFA | A second proof besides the password, like a phone prompt | Okta Verify required for everyone |
| Group-based access | Give access to a group, not one person at a time | Users put in department groups, apps assigned to groups |
| Sign-on policy | Rules for when and how often people must prove who they are | MFA plus sign in again every 4 hours for the 3 apps |
| Troubleshooting | Finding why someone can't get in, then fixing the cause | 5 problems reproduced and fixed below |

## What I did

**Users and groups.** Created 4 test users and put each in a department group (named after football clubs). Gave app access through groups and, in a few cases, to single users, to see the difference.

<img width="1321" height="564" alt="Okta users" src="https://github.com/user-attachments/assets/1051fd9b-c7f5-47f9-b7d1-93f899886d16" />
<img width="780" height="546" alt="Okta groups" src="https://github.com/user-attachments/assets/64f4c56b-df67-40ce-801f-eece56592ab9" />

**Single sign-on for 3 apps.**
- **Salesforce (SAML):** set up Okta as the identity provider, then set up a real free Salesforce org to trust Okta. Signing in worked end to end.
- **Zendesk (SAML):** a second app the same way, to show it's repeatable.
- **InternalTools (OIDC):** a custom app using OIDC, the newer standard most cloud apps use.

<img width="790" height="560" alt="Salesforce app in Okta" src="https://github.com/user-attachments/assets/3a6e768a-7307-4c28-bf12-7c9adb10d4dc" />
<img width="813" height="772" alt="Salesforce single sign-on settings" src="https://github.com/user-attachments/assets/cdcc9be1-0264-44a0-a077-0bf15c058a77" />
<img width="771" height="525" alt="Zendesk app in Okta" src="https://github.com/user-attachments/assets/e122ec7b-c98f-4f1e-afea-1eed89bcc79b" />
<img width="710" height="378" alt="OIDC app" src="https://github.com/user-attachments/assets/18bbbb5b-ee7a-4504-a562-8d479a4510ea" /> <img width="544" height="438" alt="OIDC app settings" src="https://github.com/user-attachments/assets/c69c19c4-bc0f-4c3b-8add-ee098b2ae52c" /> <img width="552" height="252" alt="OIDC app assignment" src="https://github.com/user-attachments/assets/519c0e5a-66db-47ea-b98b-2827f2d77ddf" />

**MFA for everyone.** Made Okta Verify required in the default policy, so a password alone isn't enough.

<img width="769" height="342" alt="Okta Verify required" src="https://github.com/user-attachments/assets/ba4e5285-2861-41ae-92f8-24fdc929434b" />

**Sign-on policy.** One policy for all 3 apps: MFA required, and users sign in again after 4 hours.

<img width="1063" height="699" alt="Sign-on policy" src="https://github.com/user-attachments/assets/00edd905-1e2c-4a44-b649-81a7519841e5" /> <img width="400" height="425" alt="Sign-on rule" src="https://github.com/user-attachments/assets/707da041-4796-43b8-8c5f-a6529a822742" />
<img width="1383" height="517" alt="Policy applied to apps" src="https://github.com/user-attachments/assets/c1a957fd-fc33-4af2-89e0-a4967152ea03" />

## 5 sign-in problems I reproduced and fixed

1. **New user stuck in "Password Expired."** The account was made without a password. Fix: turned it off and created it again properly.
<img width="760" height="291" alt="Password expired status" src="https://github.com/user-attachments/assets/58fcc253-6b2a-4175-baa2-8ee597826e6b" />

2. **User lost an app after one assignment was removed.** The app was given to that person directly, not through their group, so removing it took it away completely. Fix: assigned it again. Lesson: giving access by group is easier to manage.
<img width="760" height="389" alt="Missing app access" src="https://github.com/user-attachments/assets/c7e5445c-be5f-4035-ab2e-09652930f844" />

3. **User lost their MFA phone.** Fix: used Reset Authenticators so they could set up MFA again.
<img width="585" height="536" alt="Reset authenticators" src="https://github.com/user-attachments/assets/2750c473-ba16-4f06-b19e-e31844f92f63" />

4. **Salesforce single sign-on error.** Only the Okta side was set up. SAML needs both sides to trust each other. Fix: loaded Okta's details into Salesforce.
<img width="1151" height="618" alt="Salesforce SSO error" src="https://github.com/user-attachments/assets/1a2600f7-54e2-437f-9936-94f08620c0a7" />

5. **"redirect_uri mismatch" error.** I broke the OIDC app's return address on purpose. The address must match exactly. Fix: corrected it.
<img width="1221" height="880" alt="Redirect URI error" src="https://github.com/user-attachments/assets/4881ff94-9362-4a7e-a97a-641c44600347" />

## Terms in plain words

| Term | Meaning |
|---|---|
| Identity provider (IdP) | The system that checks who you are. Here, Okta |
| Service provider (SP) | The app that trusts the identity provider. Here, Salesforce or Zendesk |
| SAML | An older, common way for an app to trust Okta's sign-in |
| OIDC | A newer way to do the same thing, used by most modern apps |
| Redirect URI | Where the user is sent back after signing in. Must match exactly |

## Built with

Okta (Integrator Free Plan), SAML 2.0, OIDC, Okta Verify, Salesforce Developer Edition, Zendesk.
