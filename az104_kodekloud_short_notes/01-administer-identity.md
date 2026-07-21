# Module 01 - Administer Identity

## Microsoft Entra ID

Microsoft Entra ID is Microsoft's cloud identity and access management service. It provides authentication, authorization, single sign-on, user/group management, device identity, application identity, and external collaboration.

Key terms:

- **Tenant / directory:** An isolated Entra ID instance for an organization.
- **Identity:** A user, group, device, application, service principal, or managed identity.
- **Authentication:** Proves who or what the identity is.
- **Authorization:** Determines what the identity can access.
- **Member:** Normally belongs to the tenant.
- **Guest:** External identity invited for B2B collaboration.

## Entra ID vs Active Directory Domain Services

| Microsoft Entra ID | Active Directory Domain Services |
|---|---|
| Cloud identity and application access | Traditional Windows domain directory |
| Uses modern protocols such as OAuth, OIDC, and SAML | Uses Kerberos, NTLM, LDAP, and domain join |
| Flat tenant structure | Domains, trees, forests, and organizational units |
| Supports SaaS SSO and Conditional Access | Supports Group Policy and legacy domain workloads |
| Devices can be registered or Entra joined | Devices are domain joined |

They can coexist through hybrid identity. Microsoft Entra Domain Services is a managed domain service for workloads that still require domain protocols.

## Editions and licensing

Entra capabilities depend on licensing. Free features cover core directory functions; premium tiers add advanced identity, access, governance, and risk capabilities. Do not memorize every license feature from an old table - check the current licensing page when implementing a solution.

## Device identities

- **Entra registered:** Common for personal/BYOD devices; the user signs in with a local account but registers a work identity.
- **Entra joined:** Organization-owned, cloud-first device joined directly to Entra ID.
- **Hybrid Entra joined:** Joined to on-premises AD DS and registered with Entra ID.

Device identity is frequently combined with Intune compliance and Conditional Access.

## User accounts

Common sources:

- Cloud-only users created directly in Entra ID.
- Synchronized users originating in on-premises AD DS.
- Guest/external users invited through B2B.

Important properties include user principal name, display name, usage location, account state, group membership, assigned roles, and licenses.

Bulk operations can use CSV files, Microsoft Graph, Azure CLI, or PowerShell. Always validate the CSV headers and use a small test batch before a large import or delete.

## Groups

| Group choice | Use |
|---|---|
| Security group | Assign access to Azure resources and applications |
| Microsoft 365 group | Collaboration resources such as mailbox, SharePoint, and Teams |
| Assigned membership | Administrator explicitly manages members |
| Dynamic membership | Rules automatically add/remove users or devices; licensing required |

Assign permissions to groups rather than individual users whenever practical.

## Self-service password reset (SSPR)

SSPR lets users reset or unlock accounts after proving identity with approved methods.

Implementation checklist:

1. Select the target scope: none, selected groups, or all users.
2. Configure acceptable authentication methods and registration.
3. Define notification and security settings.
4. For hybrid users, configure password writeback when required.
5. Test with a non-administrator account.

SSPR is not the same as multifactor authentication, although both can use similar verification methods.

## Multitenant and external access

- Each tenant is an identity and policy boundary.
- B2B collaboration creates or represents an external identity in the resource tenant.
- Access is still controlled by app permissions, group membership, Conditional Access, and Azure RBAC.
- Cross-tenant access settings govern inbound and outbound trust between organizations.
- Use least privilege and review guest access regularly.

## Exam cues

- **Entra role** manages directory objects; **Azure RBAC role** manages Azure resources.
- A guest can authenticate with an identity from another organization but receives authorization in your tenant.
- Use groups for scalable assignment.
- For user sign-in problems, check account state, license, authentication method, Conditional Access, tenant, and sign-in logs.
- Know member vs guest, cloud vs synchronized identity, and registered vs joined device.
