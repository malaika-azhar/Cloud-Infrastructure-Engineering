<a id="top"></a>
<div align="center">

# 🔐 Project 04 — Index
### Microsoft Entra ID and Azure RBAC
**Project 04 of 6 — Cloud Infrastructure Engineering**

![Azure](https://img.shields.io/badge/Azure-RBAC-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Entra ID](https://img.shields.io/badge/Entra_ID-Free-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Policy](https://img.shields.io/badge/Azure_Policy-Deny-76448A?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this project.**

### [📖 Open the full README](README.md) &nbsp;·&nbsp; [⌨️ Commands](commands/commands.md) &nbsp;·&nbsp; [🧹 Cleanup](commands/cleanup.md)

</div>

---

<div align="center">

| 🧩 Steps | 🖼️ Screenshots | 🔵 Modules |
|:---:|:---:|:---:|
| **20** | **18** | **5** |

</div>

<p align="center">🧩 <b>Lab:</b> one test user · one security group · Reader role · lock · policy · VM managed identity · Key Vault</p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Combined check | 🔵 Module 1 | Create-user form opens; Graph token fails in Cloud Shell | [Exhibit 1](#ex1) |
| 2 | Resource group with tags | 🔵 Module 1 | `rg-cie-p04-iam`, `Succeeded` | [Exhibit 2](#ex2) |
| 3 | Test user (portal) | 🔵 Module 1 | 2 users in the directory | [Exhibit 3](#ex3) |
| 4 | Security group (portal) | 🔵 Module 1 | `cie-p04-readers`, 1 member | [Exhibit 4](#ex4) |
| 5 | Reader role to the group | 🟢 Module 2 | Created (the `create` output showed `role: null`) | proved in Step 6 |
| 6 | Verify the role assignment | 🟢 Module 2 | `Reader`, `Group`, resource group scope | [Exhibit 5](#ex5) |
| 7 | Key Vault and secret | 🟢 Module 2 | RBAC vault, `demo-secret` enabled | [Exhibit 6](#ex6) |
| 8 | Test user: allowed action | 🟢 Module 2 | Resource group opens | [Exhibit 7](#ex7) |
| 9 | Test user: denied action | 🟢 Module 2 | `AuthorizationFailed` on a tag write | [Exhibit 8](#ex8) |
| 10 | Custom role | 🟠 Module 3 | Created (output not kept) | proved in Step 11 |
| 11 | Assign the custom role, verify | 🟠 Module 3 | Group holds Reader and the custom role | [Exhibit 9](#ex9) |
| 12 | Resource lock, delete test | 🟠 Module 3 | `ScopeLocked` | [Exhibit 10](#ex10) |
| 13 | Azure Policy, tag required | 🟠 Module 3 | `RequestDisallowedByPolicy` | [Exhibit 11](#ex11) |
| 14 | VM with managed identity | 🟣 Module 4 | `vm-cie-p04`, `SystemAssigned`, no public IP | [Exhibit 13](#ex13), [Exhibit 14](#ex14) |
| 15 | Role for the VM identity | 🟣 Module 4 | `Key Vault Secrets User`, `ServicePrincipal` | [Exhibit 12](#ex12) |
| 16 | VM reads the secret | 🟣 Module 4 | HTTP 200, value length 17 | [Exhibit 13](#ex13) |
| 17 | Remove the lock | 🔴 Module 5 | `LOCKS LEFT 0`, leftover public IP gone | [Exhibit 14](#ex14) |
| 18 | Policy, custom role, assignments | 🔴 Module 5 | Went with the resource group (command ran after the delete) | proved by Exhibits 15 and 18 |
| 19 | Delete group, user, Entra group | 🔴 Module 5 | Resource group, user and group gone | [Exhibit 15](#ex15), [Exhibit 16](#ex16), [Exhibit 17](#ex17) |
| 20 | Purge the Key Vault | 🔴 Module 5 | Soft-deleted count 0, name free | [Exhibit 18](#ex18) |

---

## 🔵 Module 1 — Check and Identity Setup

Exhibits 1 to 4.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex1"></a>
<a href="Screenshots/ss-01-create-user-permission-check.png"><img src="Screenshots/ss-01-create-user-permission-check.png" width="380" alt="Exhibit 1"></a>
<br><b>Exhibit 1 — Create new user form</b>
<br><sub>This account can create users</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex2"></a>
<a href="Screenshots/ss-02-resource-group-created.png"><img src="Screenshots/ss-02-resource-group-created.png" width="380" alt="Exhibit 2"></a>
<br><b>Exhibit 2 — Resource group</b>
<br><sub><code>Succeeded</code>, three tags</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex3"></a>
<a href="Screenshots/ss-03-test-user-created.png"><img src="Screenshots/ss-03-test-user-created.png" width="380" alt="Exhibit 3"></a>
<br><b>Exhibit 3 — Test user created</b>
<br><sub>"2 users found"</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex4"></a>
<a href="Screenshots/ss-04-group-created-one-member.png"><img src="Screenshots/ss-04-group-created-one-member.png" width="380" alt="Exhibit 4"></a>
<br><b>Exhibit 4 — Group with one member</b>
<br><sub>Security, Assigned, 1 user</sub>
</td>
</tr>
</table>

---

## 🟢 Module 2 — Role Assignment and Least Privilege

Exhibits 5 to 8.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex5"></a>
<a href="Screenshots/ss-05-reader-role-on-group.png"><img src="Screenshots/ss-05-reader-role-on-group.png" width="380" alt="Exhibit 5"></a>
<br><b>Exhibit 5 — Reader on the group</b>
<br><sub>Role, type <code>Group</code>, resource group scope</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex6"></a>
<a href="Screenshots/ss-06-key-vault-and-secret.png"><img src="Screenshots/ss-06-key-vault-and-secret.png" width="380" alt="Exhibit 6"></a>
<br><b>Exhibit 6 — Key Vault and secret</b>
<br><sub>RBAC on, soft delete on</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex7"></a>
<a href="Screenshots/ss-07-test-user-allowed-view.png"><img src="Screenshots/ss-07-test-user-allowed-view.png" width="380" alt="Exhibit 7"></a>
<br><b>Exhibit 7 — Allowed as the test user</b>
<br><sub>Resource group opens, 1 resource</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex8"></a>
<a href="Screenshots/ss-08-test-user-denied-tag-write.png"><img src="Screenshots/ss-08-test-user-denied-tag-write.png" width="380" alt="Exhibit 8"></a>
<br><b>Exhibit 8 — Denied as the test user</b>
<br><sub><code>AuthorizationFailed</code> on the tag write</sub>
</td>
</tr>
</table>

---

## 🟠 Module 3 — Custom Role, Lock and Policy

Exhibits 9 to 11.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex9"></a>
<a href="Screenshots/ss-09-custom-role-assigned.png"><img src="Screenshots/ss-09-custom-role-assigned.png" width="380" alt="Exhibit 9"></a>
<br><b>Exhibit 9 — Custom role assigned</b>
<br><sub>Reader and <code>CIE P04 VM Operator</code>. Not exercised</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="Screenshots/ss-10-lock-blocks-delete.png"><img src="Screenshots/ss-10-lock-blocks-delete.png" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — Lock blocks a delete</b>
<br><sub><code>ScopeLocked</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="Screenshots/ss-11-policy-denies-untagged.png"><img src="Screenshots/ss-11-policy-denies-untagged.png" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — Policy denies an untagged resource</b>
<br><sub><code>RequestDisallowedByPolicy</code></sub>
</td>
<td></td>
</tr>
</table>

---

## 🟣 Module 4 — Managed Identity and Key Vault

Exhibits 12 and 13.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="Screenshots/ss-12-vm-identity-keyvault-role.png"><img src="Screenshots/ss-12-vm-identity-keyvault-role.png" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — VM identity role</b>
<br><sub><code>Key Vault Secrets User</code>, <code>ServicePrincipal</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="Screenshots/ss-13-vm-reads-secret-identity.png"><img src="Screenshots/ss-13-vm-reads-secret-identity.png" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — VM reads the secret</b>
<br><sub>HTTP 200, hostname <code>vm-cie-p04</code></sub>
</td>
</tr>
</table>

---

## 🔴 Module 5 — Cleanup

Exhibits 14 to 18.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="Screenshots/ss-14-lock-removed-resources.png"><img src="Screenshots/ss-14-lock-removed-resources.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — Lock removed</b>
<br><sub><code>LOCKS LEFT 0</code>, no public IP, no NSG</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="Screenshots/ss-15-rg-deleted-older-groups.png"><img src="Screenshots/ss-15-rg-deleted-older-groups.png" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — Resource group deleted</b>
<br><sub>Four older groups remain</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex16"></a>
<a href="Screenshots/ss-16-users-after-cleanup.png"><img src="Screenshots/ss-16-users-after-cleanup.png" width="380" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — Test user deleted</b>
<br><sub>"1 user found"</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex17"></a>
<a href="Screenshots/ss-17-groups-after-cleanup.png"><img src="Screenshots/ss-17-groups-after-cleanup.png" width="380" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — Group deleted</b>
<br><sub>"0 groups found"</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex18"></a>
<a href="Screenshots/ss-18-key-vault-purged-final.png"><img src="Screenshots/ss-18-key-vault-purged-final.png" width="380" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — Key Vault purged</b>
<br><sub>Soft-deleted count 0, name free, custom role 0</sub>
</td>
<td></td>
</tr>
</table>

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|---|:---:|
| Account can create users | Create new user form | Module 1 | ✅ Confirmed |
| Reader role assigned to the group | `az role assignment list` | Module 2 | ✅ Confirmed |
| Allowed action as the test user | Resource group opens | Module 2 | ✅ Confirmed |
| Denied action as the test user | `AuthorizationFailed` | Module 2 | ✅ Confirmed |
| Lock blocks a delete | `ScopeLocked` | Module 3 | ✅ Confirmed |
| Policy blocks an untagged resource | `RequestDisallowedByPolicy` | Module 3 | ✅ Confirmed |
| VM identity reads a secret | HTTP 200 | Module 4 | ✅ Confirmed |
| Everything removed | Exhibits 14 to 18 | Module 5 | ✅ Confirmed |
| Custom role permissions | Not used by the test user | Module 3 | ➖ Not tested |
| Users and groups from Cloud Shell | Graph token timed out | Module 1 | ➖ Portal used |

> [!NOTE]
> Email addresses, user principal names, the group object ID and the subscription ID are blurred. The test user's password and the secret value never appear in the repo.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

☁️ **[Microsoft Azure](https://azure.microsoft.com)** · 🔐 **[Azure RBAC](https://learn.microsoft.com/azure/role-based-access-control/)**

</div>
