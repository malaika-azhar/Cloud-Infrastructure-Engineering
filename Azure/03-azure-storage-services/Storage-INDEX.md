<a id="top"></a>
<div align="center">

# 🗄️ Project 03 — Index
### Azure Storage Services
**Project 03 of 6 — Cloud Infrastructure Engineering**

![Azure](https://img.shields.io/badge/Azure-Storage-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Blob](https://img.shields.io/badge/Blob_Storage-1679A7?style=for-the-badge)
![Files](https://img.shields.io/badge/Azure_Files-SMB-4A3FA6?style=for-the-badge)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this project.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Modules | 🪜 Steps | 🖼️ Screenshots |
|:---:|:---:|:---:|
| **4** | **26** | **25** |

</div>

<p align="center">🧩 <b>Lab:</b> one storage account (LRS) · private blob container · Azure Files share · one Ubuntu VM · resource group deleted at the end</p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Check the vCPU quota | 🔵 Setup | 2 used of 6 | [Exhibit 1](#ex1) |
| 2 | Check name, provider, VM size | 🔵 Setup | Run, no screenshot 📝 | — |
| 3 | Create the resource group | 🔵 Setup | `Succeeded`, three tags | [Exhibit 2](#ex2) |
| 4 | Create the storage account | 🟢 Module 1 | `Standard_LRS`, TLS 1.2, no public blob access | [Exhibit 3](#ex3) |
| 5 | Create a private container | 🟢 Module 1 | `labdata` created | [Exhibit 4](#ex4) |
| 6 | Upload a file | 🟢 Module 1 | Upload finished, encrypted on the server | [Exhibit 5](#ex5) |
| 7 | Download and read it | 🟢 Module 1 | Same text came back | [Exhibit 6](#ex6) |
| 8 | Set the Cool tier | 🟢 Module 1 | `Tier: Cool` | [Exhibit 7](#ex7) |
| 9 | Add a lifecycle rule | 🟢 Module 1 | Cool at 30 days, delete at 365 | [Exhibit 8](#ex8) |
| 10 | Soft delete and versioning | 🟠 Module 2 | 7 days, versioning on | [Exhibit 9](#ex9) |
| 11 | Overwrite and list versions | 🟠 Module 2 | Two versions of `test1.txt` | [Exhibit 10](#ex10) |
| 12 | Delete, then recover | 🟠 Module 2 | Old version returned | [Exhibit 11](#ex11) |
| 13 | Request with no credentials | 🟣 Module 3 | HTTP 409, not 200 | [Exhibit 12](#ex12) |
| 14 | Request with a SAS token | 🟣 Module 3 | HTTP 200 | [Exhibit 13](#ex13) |
| 15 | Use the token after expiry | 🟣 Module 3 | HTTP 403 | [Exhibit 14](#ex14) |
| 16 | Firewall default Deny | 🟣 Module 3 | `DefaultAction: Deny` | [Exhibit 15](#ex15) |
| 17 | List blobs while Deny | 🟣 Module 3 | CLI blocked by network rules | [Exhibit 16](#ex16) |
| 18 | Firewall back to Allow | 🟣 Module 3 | List works again | [Exhibit 17](#ex17) |
| 19 | Create the VM | 🟡 Module 4 | `vm-files` running, `10.0.0.4` | [Exhibit 18](#ex18) |
| 20 | SSH rule for my IP only | 🟡 Module 4 | Port 22, one source (blurred) | [Exhibit 19](#ex19) |
| 21 | Connect over SSH | 🟡 Module 4 | Ubuntu 22.04.4 LTS | [Exhibit 20](#ex20) |
| 22 | Create the file share | 🟡 Module 4 | `labshare`, SMB, 5 GiB | [Exhibit 21](#ex21) |
| 23 | Test port 445 from the VM | 🟡 Module 4 | `Port 445: OPEN` | [Exhibit 22](#ex22) |
| 24 | Mount the share, write a file | 🟡 Module 4 | `cifs` mount, `from-vm.txt` | [Exhibit 23](#ex23) |
| 25 | Read the file from Cloud Shell | 🟡 Module 4 | `hello from vm-files` | [Exhibit 24](#ex24) |
| 26 | Delete the resource group | 🧹 Cleanup | Group gone, 4 older groups remain | [Exhibit 25](#ex25) |

---

## 🔵 Setup

Exhibits 1 to 2.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex1"></a>
<a href="Screenshots/ss-01-quota-check.PNG"><img src="Screenshots/ss-01-quota-check.PNG" width="380" alt="Exhibit 1"></a>
<br><b>Exhibit 1 — vCPU quota</b>
<br><sub>Used 2, Limit 6</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex2"></a>
<a href="Screenshots/ss-02-resource-group-created.PNG"><img src="Screenshots/ss-02-resource-group-created.PNG" width="380" alt="Exhibit 2"></a>
<br><b>Exhibit 2 — Resource group</b>
<br><sub>Succeeded, three tags</sub>
</td>
</tr>
</table>

---

## 🟢 Module 1 — Blob Storage

Exhibits 3 to 8.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex3"></a>
<a href="Screenshots/ss-03-storage-account-lrs.PNG"><img src="Screenshots/ss-03-storage-account-lrs.PNG" width="380" alt="Exhibit 3"></a>
<br><b>Exhibit 3 — Storage account</b>
<br><sub>LRS, Hot, TLS 1.2, public blob off</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex4"></a>
<a href="Screenshots/ss-04-blob-container-created.PNG"><img src="Screenshots/ss-04-blob-container-created.PNG" width="380" alt="Exhibit 4"></a>
<br><b>Exhibit 4 — Container created</b>
<br><sub><code>labdata</code>, <code>created: true</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex5"></a>
<a href="Screenshots/ss-05-blob-upload.PNG"><img src="Screenshots/ss-05-blob-upload.PNG" width="380" alt="Exhibit 5"></a>
<br><b>Exhibit 5 — Blob upload</b>
<br><sub>Finished at 100%, etag returned</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex6"></a>
<a href="Screenshots/ss-06-blob-download.PNG"><img src="Screenshots/ss-06-blob-download.PNG" width="380" alt="Exhibit 6"></a>
<br><b>Exhibit 6 — Blob download</b>
<br><sub>Content matches the upload</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex7"></a>
<a href="Screenshots/ss-07-access-tier-cool.PNG"><img src="Screenshots/ss-07-access-tier-cool.PNG" width="380" alt="Exhibit 7"></a>
<br><b>Exhibit 7 — Cool tier</b>
<br><sub><code>Tier: Cool</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex8"></a>
<a href="Screenshots/ss-08-lifecycle-rule.PNG"><img src="Screenshots/ss-08-lifecycle-rule.PNG" width="380" alt="Exhibit 8"></a>
<br><b>Exhibit 8 — Lifecycle rule</b>
<br><sub>30 days to Cool, 365 days to delete</sub>
</td>
</tr>
</table>

---

## 🟠 Module 2 — Data Protection

Exhibits 9 to 11.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex9"></a>
<a href="Screenshots/ss-09-soft-delete-versioning.PNG"><img src="Screenshots/ss-09-soft-delete-versioning.PNG" width="380" alt="Exhibit 9"></a>
<br><b>Exhibit 9 — Soft delete and versioning</b>
<br><sub>7 days, versioning true</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="Screenshots/ss-10-two-versions.PNG"><img src="Screenshots/ss-10-two-versions.PNG" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — Two versions</b>
<br><sub>Old version and the current one</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="Screenshots/ss-11-delete-and-recover.PNG"><img src="Screenshots/ss-11-delete-and-recover.PNG" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — Delete and recover</b>
<br><sub>Empty list, old content returned</sub>
</td>
<td></td>
</tr>
</table>

---

## 🟣 Module 3 — Access Control

Exhibits 12 to 17.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="Screenshots/ss-12-anonymous-request-409.PNG"><img src="Screenshots/ss-12-anonymous-request-409.PNG" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — No credentials</b>
<br><sub>HTTP 409</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="Screenshots/ss-13-sas-token-200.PNG"><img src="Screenshots/ss-13-sas-token-200.PNG" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — SAS token works</b>
<br><sub>HTTP 200</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="Screenshots/ss-14-sas-expired-403.PNG"><img src="Screenshots/ss-14-sas-expired-403.PNG" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — SAS token expired</b>
<br><sub>HTTP 403, AuthenticationFailed</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="Screenshots/ss-15-firewall-deny.PNG"><img src="Screenshots/ss-15-firewall-deny.PNG" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — Firewall Deny</b>
<br><sub>No IP rules</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex16"></a>
<a href="Screenshots/ss-16-firewall-blocked.PNG"><img src="Screenshots/ss-16-firewall-blocked.PNG" width="380" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — Request blocked</b>
<br><sub>CLI reports network rules</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex17"></a>
<a href="Screenshots/ss-17-firewall-allow.PNG"><img src="Screenshots/ss-17-firewall-allow.PNG" width="380" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — Firewall Allow</b>
<br><sub>Blob list works again</sub>
</td>
</tr>
</table>

---

## 🟡 Module 4 — Azure Files

Exhibits 18 to 24.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex18"></a>
<a href="Screenshots/ss-18-vm-created.PNG"><img src="Screenshots/ss-18-vm-created.PNG" width="380" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — VM created</b>
<br><sub><code>vm-files</code>, private IP 10.0.0.4</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex19"></a>
<a href="Screenshots/ss-19-nsg-ssh-rule.PNG"><img src="Screenshots/ss-19-nsg-ssh-rule.PNG" width="380" alt="Exhibit 19"></a>
<br><b>Exhibit 19 — SSH rule</b>
<br><sub>Port 22, one source (blurred)</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex20"></a>
<a href="Screenshots/ss-20-ssh-connected.PNG"><img src="Screenshots/ss-20-ssh-connected.PNG" width="380" alt="Exhibit 20"></a>
<br><b>Exhibit 20 — SSH connected</b>
<br><sub>Ubuntu 22.04.4 LTS</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex21"></a>
<a href="Screenshots/ss-21-azure-files-share.PNG"><img src="Screenshots/ss-21-azure-files-share.PNG" width="380" alt="Exhibit 21"></a>
<br><b>Exhibit 21 — File share</b>
<br><sub><code>labshare</code>, SMB, 5 GiB</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex22"></a>
<a href="Screenshots/ss-22-port-445-open.PNG"><img src="Screenshots/ss-22-port-445-open.PNG" width="380" alt="Exhibit 22"></a>
<br><b>Exhibit 22 — Port 445</b>
<br><sub>OPEN from the VM</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex23"></a>
<a href="Screenshots/ss-23-share-mounted-via-run-command.PNG"><img src="Screenshots/ss-23-share-mounted-via-run-command.PNG" width="380" alt="Exhibit 23"></a>
<br><b>Exhibit 23 — Share mounted</b>
<br><sub><code>cifs</code>, 5.0G, <code>from-vm.txt</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex24"></a>
<a href="Screenshots/ss-24-file-read-from-cloud-shell.PNG"><img src="Screenshots/ss-24-file-read-from-cloud-shell.PNG" width="380" alt="Exhibit 24"></a>
<br><b>Exhibit 24 — Read from Cloud Shell</b>
<br><sub><code>hello from vm-files</code></sub>
</td>
<td></td>
</tr>
</table>

---

## 🧹 Cleanup

Exhibit 25.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex25"></a>
<a href="Screenshots/ss-25-cleanup-proof.PNG"><img src="Screenshots/ss-25-cleanup-proof.PNG" width="380" alt="Exhibit 25"></a>
<br><b>Exhibit 25 — Cleanup proof</b>
<br><sub>Project group gone, four older groups remain</sub>
</td>
<td></td>
</tr>
</table>

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|---|:---:|
| Blob upload and download | Same text returned | Module 1 | ✅ Confirmed |
| Access tier set | `blob show` | Module 1 | ✅ Cool |
| Lifecycle rule exists | `management-policy create` output | Module 1 | ✅ Created |
| Lifecycle rule executes | Needs 30 days | Module 1 | ➖ Not seen |
| Deleted blob recovered | Version ID download | Module 2 | ✅ Confirmed |
| No-credential request fails | `curl` | Module 3 | ✅ 409 |
| SAS works, then expires | `curl` 200 then 403 | Module 3 | ✅ Confirmed |
| Firewall default Deny blocks | CLI error | Module 3 | ✅ Confirmed |
| Firewall allow-IP rule | Not part of this lab | Module 3 | ➖ Not attempted |
| SMB mount and read back | `df`, file list, download | Module 4 | ✅ Confirmed |
| Mount survives reboot | Not part of this lab | Module 4 | ➖ Not attempted |
| Project group deleted | `az group list` | Cleanup | ✅ Confirmed |

> [!NOTE]
> The public IP addresses and the subscription ID are blurred. Items marked 📝 in the README were run but have no screenshot in this repo.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

☁️ **[Microsoft Azure](https://azure.microsoft.com)** · 🗄️ **[Azure Storage documentation](https://learn.microsoft.com/azure/storage/)**

</div>
