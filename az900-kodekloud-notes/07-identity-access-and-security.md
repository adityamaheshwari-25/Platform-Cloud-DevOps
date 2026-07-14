# Module 7 — Identity, Access, and Security

## Microsoft Entra ID

Microsoft Entra ID, formerly Azure Active Directory, is Azure’s cloud identity and access-management service.

It supports:

- Users, groups, applications, and devices
- Single sign-on (SSO)
- Multifactor authentication (MFA)
- Conditional Access
- Business partner and external identities
- Integration with Azure RBAC

Entra ID is not the same as traditional Windows Server Active Directory Domain Services, although the two can integrate.

## Microsoft Entra Domain Services

A managed domain service for workloads that need traditional domain features without deploying domain controllers.

- Domain join
- Group Policy
- LDAP
- Kerberos and NTLM authentication

Use it for legacy applications that need domain protocols. Microsoft operates patching, backups, and domain controllers.

## Authentication and authorization

| Concept | Question answered | Example |
|---|---|---|
| Authentication | Who are you? | Sign in with a password and phone prompt |
| Authorization | What are you allowed to do? | Reader access to a resource group |

Authentication happens before authorization.

## Multifactor authentication

MFA requires factors from at least two different categories:

- **Something you know:** password or PIN
- **Something you have:** phone, token, or security key
- **Something you are:** fingerprint or face

Two passwords are not MFA because both are the same factor type.

## Conditional Access

Conditional Access uses **if-then** policies to make access decisions.

Signals may include:

- User or group
- Application
- Device state
- Location
- Sign-in or user risk

Controls may include:

- Allow or block access
- Require MFA
- Require a compliant device
- Require an approved application

## External identities

| Scenario | Course term | Purpose |
|---|---|---|
| Partner or supplier collaborates with your organization | B2B | Guest uses an external identity to access organizational resources |
| Customer signs into a consumer-facing app | B2C | Customer identity and social/local sign-in experiences |

Microsoft’s external-identity product names evolve, but the exam distinction remains: workforce collaboration versus customer identity.

## Azure role-based access control (RBAC)

Azure RBAC controls who can perform management actions at a scope.

A role assignment combines:

`Security principal + Role definition + Scope`

### Common built-in roles

| Role | Permission summary |
|---|---|
| Owner | Full resource management and can assign access |
| Contributor | Full resource management but cannot assign RBAC roles |
| Reader | View resources but cannot change them |
| User Access Administrator | Manage user access to Azure resources |

### Scopes

`Management group → Subscription → Resource group → Resource`

Assignments inherit downward. Follow **least privilege**: grant only the access needed, at the narrowest practical scope.

## Zero Trust

Three core principles:

1. **Verify explicitly** using all available signals.
2. **Use least-privilege access** and limit duration and scope.
3. **Assume breach** and reduce blast radius through segmentation and monitoring.

Zero Trust does not automatically trust a request because it comes from an internal network.

## Defense in depth

Multiple security layers reduce the chance that one failed control compromises the whole system:

1. Physical security
2. Identity and access
3. Perimeter
4. Network
5. Compute
6. Application
7. Data

Data is the final asset being protected, while identity increasingly acts as the modern security perimeter.

## Microsoft Defender for Cloud

- Assesses security posture across Azure and connected environments.
- Produces a secure score and prioritized recommendations.
- Helps monitor compliance.
- Provides workload threat-protection capabilities.
- Can cover Azure, on-premises, and supported multicloud resources.

## Exam traps

- Authentication proves identity; authorization grants permissions.
- RBAC controls actions, while Conditional Access controls sign-in conditions.
- Contributor cannot grant RBAC access; Owner can.
- Entra Domain Services supplies managed legacy domain protocols; Entra ID is cloud IAM.
- Zero Trust means “verify explicitly,” not “block every request.”

## Quick check

1. Which service manages Azure cloud identities? **Microsoft Entra ID.**
2. Which control can require MFA only for a risky sign-in? **Conditional Access.**
3. Which role can manage resources but not assign access? **Contributor.**
4. What are the three Zero Trust principles? **Verify explicitly, least privilege, and assume breach.**

## References

- [KodeKloud — Identity, Access, and Security](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Identity-Access-and-Security/Introduction)
- [KodeKloud — RBAC](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Identity-Access-and-Security/RBAC)
- [KodeKloud — Zero Trust](https://notes.kodekloud.com/docs/Microsoft-Azure-Fundamentals-AZ-900/Identity-Access-and-Security/Zero-Trust)

