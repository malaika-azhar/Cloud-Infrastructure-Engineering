<a id="top"></a>
<div align="center">

# ☁️ Project 01 — Index
### Azure Virtual Machine Deployment & Administration
**Project 01 — Cloud Infrastructure Engineering**

![Azure](https://img.shields.io/badge/Azure_VM-Ubuntu_24.04-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![SSH](https://img.shields.io/badge/SSH_Key_Auth-4A3FA6?style=for-the-badge)
![Disk](https://img.shields.io/badge/Data_Disk-UUID_fstab-76448A?style=for-the-badge)
![Resize](https://img.shields.io/badge/Resize-B2ls__v2_to_B2s__v2-B9770E?style=for-the-badge)
![Status](https://img.shields.io/badge/Lab-Complete-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this project.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Modules | 🖼️ Screenshots |
|:---:|:---:|
| **4** | **31** |

</div>

<p align="center">🧩 <b>Lab:</b> One Ubuntu 24.04 VM in India South Central · SSH key login · 4 GB data disk mounted by UUID · resize · stop, deallocate and start · boot diagnostics</p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Subscription check | 🔵 Module 1 | Azure for Students, Enabled | [Exhibit 1](#ex1) |
| 2 | VM size availability | 🔵 Module 1 | B2 sizes, no restrictions | [Exhibit 2](#ex2) |
| 3 | Resource group | 🔵 Module 1 | rg-cie-p01-vm with three tags | [Exhibit 3](#ex3) |
| 4 | SSH key pair | 🔵 Module 1 | Second key, no passphrase | [Exhibit 4](#ex4) |
| 5 | Basics: instance | 🔵 Module 1 | Ubuntu 24.04, India South Central | [Exhibit 5](#ex5) |
| 6 | Basics: size and key | 🔵 Module 1 | Standard_B2ls_v2, SSH public key | [Exhibit 6](#ex6) |
| 7 | Networking tab | 🔵 Module 1 | SSH (22) allowed | [Exhibit 7](#ex7) |
| 8 | VM overview | 🔵 Module 1 | Running, no tags yet | [Exhibit 8](#ex8) |
| 9 | SSH login | 🟢 Module 2 | Key login, whoami azureuser | [Exhibit 9](#ex9) |
| 10 | System info | 🟢 Module 2 | hostnamectl and ip -br addr | [Exhibit 10](#ex10) |
| 11 | SSH service | 🟢 Module 2 | active, sshd on port 22 | [Exhibit 11](#ex11) |
| 12 | Login in the log | 🟢 Module 2 | Accepted publickey at 18:50:51 | [Exhibit 12](#ex12) |
| 13 | Resource tags | 🟢 Module 2 | All eight resources tagged | [Exhibit 13](#ex13) |
| 14 | Data disk attached | 🟠 Module 3 | LUN 0, StandardSSD_LRS | [Exhibit 14](#ex14) |
| 15 | New disk in lsblk | 🟠 Module 3 | sdb, 4G, no partition | [Exhibit 15](#ex15) |
| 16 | Partition | 🟠 Module 3 | sdb1 created | [Exhibit 16](#ex16) |
| 17 | ext4 filesystem | 🟠 Module 3 | UUID shown, ends with done | [Exhibit 17](#ex17) |
| 18 | Mount and fstab | 🟠 Module 3 | mount -a test passed | [Exhibit 18](#ex18) |
| 19 | Write test | 🟠 Module 3 | test.txt written and read back | [Exhibit 19](#ex19) |
| 20 | Reboot test | 🟠 Module 3 | Mount and data survived | [Exhibit 20](#ex20) |
| 21 | Device names swapped | 🟠 Module 3 | Data disk sda, OS disk sdb | [Exhibit 21](#ex21) |
| 22 | Resize options | 🟣 Module 4 | Standard_B sizes allowed | [Exhibit 22](#ex22) |
| 23 | Before resize | 🟣 Module 4 | 2 vCPUs, 3.8Gi | [Exhibit 23](#ex23) |
| 24 | VM resized | 🟣 Module 4 | Standard_B2s_v2, running | [Exhibit 24](#ex24) |
| 25 | After resize | 🟣 Module 4 | 2 vCPUs, 7.7Gi | [Exhibit 25](#ex25) |
| 26 | VM stopped | 🟣 Module 4 | State: VM stopped | [Exhibit 26](#ex26) |
| 27 | VM deallocated | 🟣 Module 4 | Public IP unchanged | [Exhibit 27](#ex27) |
| 28 | VM started | 🟣 Module 4 | Running again | [Exhibit 28](#ex28) |
| 29 | Disk after restart | 🟣 Module 4 | Mounted, file intact | [Exhibit 29](#ex29) |
| 30 | Boot diagnostics | 🟣 Module 4 | Login prompt on the console | [Exhibit 30](#ex30) |
| 31 | First key rejected | 🟢 Module 2 | Passphrase, Permission denied | [Exhibit 31](#ex31) |

---

## 🔵 Module 1 — Setup and Deployment

Exhibits 1 to 8.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex1"></a>
<a href="Screenshots/ss-01-subscription-check.png"><img src="Screenshots/ss-01-subscription-check.png" width="380" alt="Exhibit 1"></a>
<br><b>Exhibit 1 — Subscription check</b>
<br><sub>Azure for Students, Enabled</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex2"></a>
<a href="Screenshots/ss-02-vm-size-availability.png"><img src="Screenshots/ss-02-vm-size-availability.png" width="380" alt="Exhibit 2"></a>
<br><b>Exhibit 2 — VM size availability</b>
<br><sub>B2 sizes, no restrictions</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex3"></a>
<a href="Screenshots/ss-03-resource-group-created.png"><img src="Screenshots/ss-03-resource-group-created.png" width="380" alt="Exhibit 3"></a>
<br><b>Exhibit 3 — Resource group</b>
<br><sub>rg-cie-p01-vm with three tags</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex4"></a>
<a href="Screenshots/ss-04-ssh-key-generated.png"><img src="Screenshots/ss-04-ssh-key-generated.png" width="380" alt="Exhibit 4"></a>
<br><b>Exhibit 4 — SSH key pair</b>
<br><sub>Second key, no passphrase</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex5"></a>
<a href="Screenshots/ss-05-vm-basics-project-instance.png"><img src="Screenshots/ss-05-vm-basics-project-instance.png" width="380" alt="Exhibit 5"></a>
<br><b>Exhibit 5 — Basics: instance</b>
<br><sub>Ubuntu 24.04, India South Central</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex6"></a>
<a href="Screenshots/ss-06-vm-basics-size-auth-key.png"><img src="Screenshots/ss-06-vm-basics-size-auth-key.png" width="380" alt="Exhibit 6"></a>
<br><b>Exhibit 6 — Basics: size and key</b>
<br><sub>Standard_B2ls_v2, SSH public key</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex7"></a>
<a href="Screenshots/ss-07-networking-ssh-port.png"><img src="Screenshots/ss-07-networking-ssh-port.png" width="380" alt="Exhibit 7"></a>
<br><b>Exhibit 7 — Networking tab</b>
<br><sub>SSH (22) allowed</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex8"></a>
<a href="Screenshots/ss-08-vm-overview-running.png"><img src="Screenshots/ss-08-vm-overview-running.png" width="380" alt="Exhibit 8"></a>
<br><b>Exhibit 8 — VM overview</b>
<br><sub>Running, no tags yet</sub>
</td>
</tr>
</table>

---

## 🟢 Module 2 — Access and Verification

Exhibits 9 to 13 and 31.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex9"></a>
<a href="Screenshots/ss-09-ssh-login-whoami.png"><img src="Screenshots/ss-09-ssh-login-whoami.png" width="380" alt="Exhibit 9"></a>
<br><b>Exhibit 9 — SSH login</b>
<br><sub>Key login, whoami azureuser</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="Screenshots/ss-10-system-info.png"><img src="Screenshots/ss-10-system-info.png" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — System info</b>
<br><sub>hostnamectl and ip -br addr</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="Screenshots/ss-11-ssh-service-ports.png"><img src="Screenshots/ss-11-ssh-service-ports.png" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — SSH service</b>
<br><sub>active, sshd on port 22</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="Screenshots/ss-12-ssh-accepted-publickey.png"><img src="Screenshots/ss-12-ssh-accepted-publickey.png" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — Login in the log</b>
<br><sub>Accepted publickey at 18:50:51</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="Screenshots/ss-13-resource-tags.png"><img src="Screenshots/ss-13-resource-tags.png" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — Resource tags</b>
<br><sub>All eight resources tagged</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex31"></a>
<a href="Screenshots/ss-31-first-key-passphrase-denied.png"><img src="Screenshots/ss-31-first-key-passphrase-denied.png" width="380" alt="Exhibit 31"></a>
<br><b>Exhibit 31 — First key rejected</b>
<br><sub>Passphrase, Permission denied</sub>
</td>
</tr>
</table>

---

## 🟠 Module 3 — Data Disk

Exhibits 14 to 21.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="Screenshots/ss-14-data-disk-attached.png"><img src="Screenshots/ss-14-data-disk-attached.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — Data disk attached</b>
<br><sub>LUN 0, StandardSSD_LRS</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="Screenshots/ss-15-lsblk-new-disk.png"><img src="Screenshots/ss-15-lsblk-new-disk.png" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — New disk in lsblk</b>
<br><sub>sdb, 4G, no partition</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex16"></a>
<a href="Screenshots/ss-16-disk-partitioned.png"><img src="Screenshots/ss-16-disk-partitioned.png" width="380" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — Partition</b>
<br><sub>sdb1 created</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex17"></a>
<a href="Screenshots/ss-17-disk-formatted.png"><img src="Screenshots/ss-17-disk-formatted.png" width="380" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — ext4 filesystem</b>
<br><sub>UUID shown, ends with done</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex18"></a>
<a href="Screenshots/ss-18-mount-and-fstab.png"><img src="Screenshots/ss-18-mount-and-fstab.png" width="380" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — Mount and fstab</b>
<br><sub>mount -a test passed</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex19"></a>
<a href="Screenshots/ss-19-disk-write-test.png"><img src="Screenshots/ss-19-disk-write-test.png" width="380" alt="Exhibit 19"></a>
<br><b>Exhibit 19 — Write test</b>
<br><sub>test.txt written and read back</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex20"></a>
<a href="Screenshots/ss-20-reboot-persistence.png"><img src="Screenshots/ss-20-reboot-persistence.png" width="380" alt="Exhibit 20"></a>
<br><b>Exhibit 20 — Reboot test</b>
<br><sub>Mount and data survived</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex21"></a>
<a href="Screenshots/ss-21-device-name-swap.png"><img src="Screenshots/ss-21-device-name-swap.png" width="380" alt="Exhibit 21"></a>
<br><b>Exhibit 21 — Device names swapped</b>
<br><sub>Data disk sda, OS disk sdb</sub>
</td>
</tr>
</table>

---

## 🟣 Module 4 — Resize, Power States and Boot

Exhibits 22 to 30.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex22"></a>
<a href="Screenshots/ss-22-resize-options.png"><img src="Screenshots/ss-22-resize-options.png" width="380" alt="Exhibit 22"></a>
<br><b>Exhibit 22 — Resize options</b>
<br><sub>Standard_B sizes allowed</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex23"></a>
<a href="Screenshots/ss-23-before-resize.png"><img src="Screenshots/ss-23-before-resize.png" width="380" alt="Exhibit 23"></a>
<br><b>Exhibit 23 — Before resize</b>
<br><sub>2 vCPUs, 3.8Gi</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex24"></a>
<a href="Screenshots/ss-24-vm-resized.png"><img src="Screenshots/ss-24-vm-resized.png" width="380" alt="Exhibit 24"></a>
<br><b>Exhibit 24 — VM resized</b>
<br><sub>Standard_B2s_v2, running</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex25"></a>
<a href="Screenshots/ss-25-after-resize.png"><img src="Screenshots/ss-25-after-resize.png" width="380" alt="Exhibit 25"></a>
<br><b>Exhibit 25 — After resize</b>
<br><sub>2 vCPUs, 7.7Gi</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex26"></a>
<a href="Screenshots/ss-26-vm-stopped.png"><img src="Screenshots/ss-26-vm-stopped.png" width="380" alt="Exhibit 26"></a>
<br><b>Exhibit 26 — VM stopped</b>
<br><sub>State: VM stopped</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex27"></a>
<a href="Screenshots/ss-27-vm-deallocated.png"><img src="Screenshots/ss-27-vm-deallocated.png" width="380" alt="Exhibit 27"></a>
<br><b>Exhibit 27 — VM deallocated</b>
<br><sub>Public IP unchanged</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex28"></a>
<a href="Screenshots/ss-28-vm-started.png"><img src="Screenshots/ss-28-vm-started.png" width="380" alt="Exhibit 28"></a>
<br><b>Exhibit 28 — VM started</b>
<br><sub>Running again</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex29"></a>
<a href="Screenshots/ss-29-disk-after-restart.png"><img src="Screenshots/ss-29-disk-after-restart.png" width="380" alt="Exhibit 29"></a>
<br><b>Exhibit 29 — Disk after restart</b>
<br><sub>Mounted, file intact</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex30"></a>
<a href="Screenshots/ss-30-boot-diagnostics.png"><img src="Screenshots/ss-30-boot-diagnostics.png" width="380" alt="Exhibit 30"></a>
<br><b>Exhibit 30 — Boot diagnostics</b>
<br><sub>Login prompt on the console</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|:---:|:---:|
| Subscription and VM size usable | `az account show`, `az vm list-skus` | Module 1 | ✅ Confirmed |
| VM deployed with SSH key login | Portal wizard, SSH login, `whoami` | Module 1 and 2 | ✅ Confirmed |
| Login recorded on the server | `journalctl` shows `Accepted publickey` | Module 2 | ✅ Confirmed |
| Data disk mounted and persistent | `mount -a`, reboot, `findmnt` | Module 3 | ✅ Confirmed |
| Mount safe against device renaming | UUID entry, disks swapped names | Module 3 | ✅ Observed |
| Resize proven inside the guest | `free -h`: 3.8Gi to 7.7Gi | Module 4 | ✅ Confirmed |
| Stopped, deallocated and started | `az vm list -d` states | Module 4 | ✅ Confirmed |
| Boot diagnostics reviewed | Console shows login prompt | Module 4 | ✅ Confirmed |
| Cost measured | Not part of this lab | Module 4 | ➖ Not measured |
| SSH restricted to known addresses | Port 22 stayed open to all | Module 1 | ➖ Not done |
| Resource group deleted | `az group delete` | Cleanup | ⏳ Pending |

> [!NOTE]
> The public IP address, subscription ID, tenant ID, account e-mail and machine identifiers are blurred in the screenshots. Times are UTC. Commands marked 📝 in the README ran in Cloud Shell but are not visible in a kept screenshot.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

☁️ **[Microsoft Azure](https://azure.microsoft.com)** · 🐧 **[Ubuntu](https://ubuntu.com)** · 📚 **[Azure CLI documentation](https://learn.microsoft.com/cli/azure/)**

</div>
