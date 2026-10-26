<a id="top"></a>
<div align="center">

# 🌐 Project 02 — Index
### Azure Virtual Networking
**Project 02 of 06 — Cloud Infrastructure Engineering**

![Azure](https://img.shields.io/badge/Azure_VNet-indiasouthcentral-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04_LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![NSG](https://img.shields.io/badge/NSG-Block_and_Allow-B03A2E?style=for-the-badge)
![Peering](https://img.shields.io/badge/Peering_Traffic-Not_Tested-D68910?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete_with_limits-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this project.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Modules | 🖼️ Screenshots |
|:---:|:---:|
| **5** | **36** |

</div>

<p align="center">🧩 <b>Lab:</b> 1 VNet with 2 subnets · 2 NSGs · 2 Ubuntu 22.04 VMs · SSH limited to one address · NSG deny then allow · IP flow verify · VNet peering (traffic not tested)</p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Subscription, provider and VM size checks | 🔵 Module 1 | Subscription Enabled, provider Registered, VM size listed | [Exhibit 1](#ex1) |
| 2 | Network Watcher check | 🔵 Module 1 | Network Watcher present, Succeeded | [Exhibit 2](#ex2) |
| 3 | Resource group state and tags | 🔵 Module 1 | RG Succeeded with 3 tags | [Exhibit 3](#ex3) |
| 4 | Main VNet and first subnet | 🟠 Module 2 | VNet and snet-web created | [Exhibit 4](#ex4) |
| 5 | Second subnet | 🟠 Module 2 | snet-app created | [Exhibit 5](#ex5) |
| 6 | NSG for snet-web | 🟠 Module 2 | NSG and tags confirmed | [Exhibit 6](#ex6) |
| 7 | NSG for snet-app | 🟠 Module 2 | NSG and tags confirmed | [Exhibit 7](#ex7) |
| 8 | Attach NSG to snet-web | 🟠 Module 2 | NSG attached to snet-web | [Exhibit 8](#ex8) |
| 9 | Attach NSG to snet-app | 🟠 Module 2 | NSG attached to snet-app | [Exhibit 9](#ex9) |
| 10 | Default NSG rules | 🟠 Module 2 | 6 default rules listed | [Exhibit 10](#ex10) |
| 11 | SSH allowed only from my IP | 🟠 Module 2 | SSH rule limited to one /32 | [Exhibit 11](#ex11) |
| 12 | SSH key pair on the laptop | 🟢 Module 3 | Key created, no passphrase prompt | [Exhibit 12](#ex12) |
| 13 | Public IP for vm-web | 🟢 Module 3 | Static Standard public IP | [Exhibit 13](#ex13) |
| 14 | vm-web created | 🟢 Module 3 | vm-web running, 10.10.1.4 | [Exhibit 14](#ex14) |
| 15 | vm-app created without public IP | 🟢 Module 3 | vm-app running, no public IP | [Exhibit 15](#ex15) |
| 16 | SSH from the laptop to vm-web | 🟢 Module 3 | SSH to vm-web works | [Exhibit 16](#ex16) |
| 17 | Ping from vm-web to vm-app | 🟢 Module 3 | web to app ping, 0% loss | [Exhibit 17](#ex17) |
| 18 | SSH to vm-app through vm-web | 🟢 Module 3 | Jump host login to vm-app | [Exhibit 18](#ex18) |
| 19 | Ping from vm-app to vm-web | 🟢 Module 3 | app to web ping, 0% loss | [Exhibit 19](#ex19) |
| 20 | Test web server on vm-app | 🟢 Module 3 | Local web server on 8080 | [Exhibit 20](#ex20) |
| 21 | curl before any deny rule | 🟣 Module 4 | curl works (before) | [Exhibit 21](#ex21) |
| 22 | Deny rule on nsg-snet-app | 🟣 Module 4 | Deny rule created | [Exhibit 22](#ex22) |
| 23 | curl after the deny rule | 🟣 Module 4 | curl blocked, exit 28 | [Exhibit 23](#ex23) |
| 24 | Network Watcher IP flow verify (Deny) | 🟣 Module 4 | IP flow verify says Deny | [Exhibit 24](#ex24) |
| 25 | Rule changed from Deny to Allow | 🟣 Module 4 | Rule set to Allow | [Exhibit 25](#ex25) |
| 26 | curl after the rule change | 🟣 Module 4 | curl works again, exit 0 | [Exhibit 26](#ex26) |
| 27 | Network Watcher IP flow verify (Allow) | 🟣 Module 4 | IP flow verify says Allow | [Exhibit 27](#ex27) |
| 28 | Second VNet for peering | 🟡 Module 5 | Peer VNet created | [Exhibit 28](#ex28) |
| 29 | Peering, first direction | 🟡 Module 5 | First link Initiated | [Exhibit 29](#ex29) |
| 30 | Peering, second direction | 🟡 Module 5 | Second link Connected | [Exhibit 30](#ex30) |
| 31 | vm-peer fails on quota | 🟡 Module 5 | vm-peer blocked by core quota | [Exhibit 31](#ex31) |
| 32 | vm-app deallocated | 🟡 Module 5 | vm-app deallocated | [Exhibit 32](#ex32) |
| 33 | Quota error after deallocating | 🟡 Module 5 | Same quota error | [Exhibit 33](#ex33) |
| 34 | SSH from a different address | 🟡 Module 5 | SSH from other source times out | [Exhibit 34](#ex34) |
| 35 | Tag check on all resources | 🟡 Module 5 | All 11 resources tagged | [Exhibit 35](#ex35) |
| 36 | Cleanup check | 🟡 Module 5 | RG deleted, older RGs intact | [Exhibit 36](#ex36) |

---

## 🔵 Module 1 — Checks and Resource Group

Exhibits 1 to 3.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex1"></a>
<a href="Screenshots/ss-01-subscription-provider-vmsize-checks.png"><img src="Screenshots/ss-01-subscription-provider-vmsize-checks.png" width="380" alt="Exhibit 1"></a>
<br><b>Exhibit 1 — Subscription, provider and VM size checks</b>
<br><sub>Subscription Enabled, provider Registered, VM size listed</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex2"></a>
<a href="Screenshots/ss-02-network-watcher-check.png"><img src="Screenshots/ss-02-network-watcher-check.png" width="380" alt="Exhibit 2"></a>
<br><b>Exhibit 2 — Network Watcher check</b>
<br><sub>Network Watcher present, Succeeded</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex3"></a>
<a href="Screenshots/ss-03-resource-group-state-tags.png"><img src="Screenshots/ss-03-resource-group-state-tags.png" width="380" alt="Exhibit 3"></a>
<br><b>Exhibit 3 — Resource group state and tags</b>
<br><sub>RG Succeeded with 3 tags</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🟠 Module 2 — VNet, Subnets and NSGs

Exhibits 4 to 11.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex4"></a>
<a href="Screenshots/ss-04-vnet-main-snet-web.png"><img src="Screenshots/ss-04-vnet-main-snet-web.png" width="380" alt="Exhibit 4"></a>
<br><b>Exhibit 4 — Main VNet and first subnet</b>
<br><sub>VNet and snet-web created</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex5"></a>
<a href="Screenshots/ss-05-subnet-snet-app.png"><img src="Screenshots/ss-05-subnet-snet-app.png" width="380" alt="Exhibit 5"></a>
<br><b>Exhibit 5 — Second subnet</b>
<br><sub>snet-app created</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex6"></a>
<a href="Screenshots/ss-06-nsg-snet-web.png"><img src="Screenshots/ss-06-nsg-snet-web.png" width="380" alt="Exhibit 6"></a>
<br><b>Exhibit 6 — NSG for snet-web</b>
<br><sub>NSG and tags confirmed</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex7"></a>
<a href="Screenshots/ss-07-nsg-snet-app.png"><img src="Screenshots/ss-07-nsg-snet-app.png" width="380" alt="Exhibit 7"></a>
<br><b>Exhibit 7 — NSG for snet-app</b>
<br><sub>NSG and tags confirmed</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex8"></a>
<a href="Screenshots/ss-08-attach-nsg-snet-web.png"><img src="Screenshots/ss-08-attach-nsg-snet-web.png" width="380" alt="Exhibit 8"></a>
<br><b>Exhibit 8 — Attach NSG to snet-web</b>
<br><sub>NSG attached to snet-web</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex9"></a>
<a href="Screenshots/ss-09-attach-nsg-snet-app.png"><img src="Screenshots/ss-09-attach-nsg-snet-app.png" width="380" alt="Exhibit 9"></a>
<br><b>Exhibit 9 — Attach NSG to snet-app</b>
<br><sub>NSG attached to snet-app</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="Screenshots/ss-10-nsg-default-rules.png"><img src="Screenshots/ss-10-nsg-default-rules.png" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — Default NSG rules</b>
<br><sub>6 default rules listed</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="Screenshots/ss-11-nsg-ssh-rule-my-ip.png"><img src="Screenshots/ss-11-nsg-ssh-rule-my-ip.png" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — SSH allowed only from my IP</b>
<br><sub>SSH rule limited to one /32</sub>
</td>
</tr>
</table>

---

## 🟢 Module 3 — VMs and Private Connectivity

Exhibits 12 to 20.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="Screenshots/ss-12-ssh-keygen-no-passphrase.png"><img src="Screenshots/ss-12-ssh-keygen-no-passphrase.png" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — SSH key pair on the laptop</b>
<br><sub>Key created, no passphrase prompt</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="Screenshots/ss-13-public-ip-vm-web.png"><img src="Screenshots/ss-13-public-ip-vm-web.png" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — Public IP for vm-web</b>
<br><sub>Static Standard public IP</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="Screenshots/ss-14-vm-web-created.png"><img src="Screenshots/ss-14-vm-web-created.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — vm-web created</b>
<br><sub>vm-web running, 10.10.1.4</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="Screenshots/ss-15-vm-app-no-public-ip.png"><img src="Screenshots/ss-15-vm-app-no-public-ip.png" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — vm-app created without public IP</b>
<br><sub>vm-app running, no public IP</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex16"></a>
<a href="Screenshots/ss-16-ssh-to-vm-web.png"><img src="Screenshots/ss-16-ssh-to-vm-web.png" width="380" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — SSH from the laptop to vm-web</b>
<br><sub>SSH to vm-web works</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex17"></a>
<a href="Screenshots/ss-17-ping-web-to-app.png"><img src="Screenshots/ss-17-ping-web-to-app.png" width="380" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — Ping from vm-web to vm-app</b>
<br><sub>web to app ping, 0% loss</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex18"></a>
<a href="Screenshots/ss-18-ssh-jump-host-vm-app.png"><img src="Screenshots/ss-18-ssh-jump-host-vm-app.png" width="380" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — SSH to vm-app through vm-web</b>
<br><sub>Jump host login to vm-app</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex19"></a>
<a href="Screenshots/ss-19-ping-app-to-web.png"><img src="Screenshots/ss-19-ping-app-to-web.png" width="380" alt="Exhibit 19"></a>
<br><b>Exhibit 19 — Ping from vm-app to vm-web</b>
<br><sub>app to web ping, 0% loss</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex20"></a>
<a href="Screenshots/ss-20-vm-app-web-server.png"><img src="Screenshots/ss-20-vm-app-web-server.png" width="380" alt="Exhibit 20"></a>
<br><b>Exhibit 20 — Test web server on vm-app</b>
<br><sub>Local web server on 8080</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🟣 Module 4 — NSG Block Test and IP Flow Verify

Exhibits 21 to 27.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex21"></a>
<a href="Screenshots/ss-21-curl-allowed-before.png"><img src="Screenshots/ss-21-curl-allowed-before.png" width="380" alt="Exhibit 21"></a>
<br><b>Exhibit 21 — curl before any deny rule</b>
<br><sub>curl works (before)</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex22"></a>
<a href="Screenshots/ss-22-nsg-deny-rule-8080.png"><img src="Screenshots/ss-22-nsg-deny-rule-8080.png" width="380" alt="Exhibit 22"></a>
<br><b>Exhibit 22 — Deny rule on nsg-snet-app</b>
<br><sub>Deny rule created</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex23"></a>
<a href="Screenshots/ss-23-curl-blocked.png"><img src="Screenshots/ss-23-curl-blocked.png" width="380" alt="Exhibit 23"></a>
<br><b>Exhibit 23 — curl after the deny rule</b>
<br><sub>curl blocked, exit 28</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex24"></a>
<a href="Screenshots/ss-24-ipflow-verify-deny.png"><img src="Screenshots/ss-24-ipflow-verify-deny.png" width="380" alt="Exhibit 24"></a>
<br><b>Exhibit 24 — Network Watcher IP flow verify (Deny)</b>
<br><sub>IP flow verify says Deny</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex25"></a>
<a href="Screenshots/ss-25-nsg-rule-changed-allow.png"><img src="Screenshots/ss-25-nsg-rule-changed-allow.png" width="380" alt="Exhibit 25"></a>
<br><b>Exhibit 25 — Rule changed from Deny to Allow</b>
<br><sub>Rule set to Allow</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex26"></a>
<a href="Screenshots/ss-26-curl-allowed-after.png"><img src="Screenshots/ss-26-curl-allowed-after.png" width="380" alt="Exhibit 26"></a>
<br><b>Exhibit 26 — curl after the rule change</b>
<br><sub>curl works again, exit 0</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex27"></a>
<a href="Screenshots/ss-27-ipflow-verify-allow.png"><img src="Screenshots/ss-27-ipflow-verify-allow.png" width="380" alt="Exhibit 27"></a>
<br><b>Exhibit 27 — Network Watcher IP flow verify (Allow)</b>
<br><sub>IP flow verify says Allow</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🟡 Module 5 — Peering, Limits and Cleanup

Exhibits 28 to 36.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex28"></a>
<a href="Screenshots/ss-28-vnet-peer-created.png"><img src="Screenshots/ss-28-vnet-peer-created.png" width="380" alt="Exhibit 28"></a>
<br><b>Exhibit 28 — Second VNet for peering</b>
<br><sub>Peer VNet created</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex29"></a>
<a href="Screenshots/ss-29-peering-main-to-peer.png"><img src="Screenshots/ss-29-peering-main-to-peer.png" width="380" alt="Exhibit 29"></a>
<br><b>Exhibit 29 — Peering, first direction</b>
<br><sub>First link Initiated</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex30"></a>
<a href="Screenshots/ss-30-peering-peer-to-main-connected.png"><img src="Screenshots/ss-30-peering-peer-to-main-connected.png" width="380" alt="Exhibit 30"></a>
<br><b>Exhibit 30 — Peering, second direction</b>
<br><sub>Second link Connected</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex31"></a>
<a href="Screenshots/ss-31-vm-peer-quota-error.png"><img src="Screenshots/ss-31-vm-peer-quota-error.png" width="380" alt="Exhibit 31"></a>
<br><b>Exhibit 31 — vm-peer fails on quota</b>
<br><sub>vm-peer blocked by core quota</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex32"></a>
<a href="Screenshots/ss-32-vm-app-deallocated.png"><img src="Screenshots/ss-32-vm-app-deallocated.png" width="380" alt="Exhibit 32"></a>
<br><b>Exhibit 32 — vm-app deallocated</b>
<br><sub>vm-app deallocated</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex33"></a>
<a href="Screenshots/ss-33-quota-error-code-check.png"><img src="Screenshots/ss-33-quota-error-code-check.png" width="380" alt="Exhibit 33"></a>
<br><b>Exhibit 33 — Quota error after deallocating</b>
<br><sub>Same quota error</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex34"></a>
<a href="Screenshots/ss-34-ssh-blocked-other-ip.png"><img src="Screenshots/ss-34-ssh-blocked-other-ip.png" width="380" alt="Exhibit 34"></a>
<br><b>Exhibit 34 — SSH from a different address</b>
<br><sub>SSH from other source times out</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex35"></a>
<a href="Screenshots/ss-35-tags-check.png"><img src="Screenshots/ss-35-tags-check.png" width="380" alt="Exhibit 35"></a>
<br><b>Exhibit 35 — Tag check on all resources</b>
<br><sub>All 11 resources tagged</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex36"></a>
<a href="Screenshots/ss-36-cleanup-check.png"><img src="Screenshots/ss-36-cleanup-check.png" width="380" alt="Exhibit 36"></a>
<br><b>Exhibit 36 — Cleanup check</b>
<br><sub>RG deleted, older RGs intact</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|---|:---:|
| VNet, two subnets, NSG attached to each | `az network vnet`, `nsg`, subnet update | Module 2 | ✅ Confirmed |
| SSH limited to one source address | NSG rule, login from laptop, timeout from Cloud Shell | Modules 2, 3, 5 | ✅ Confirmed (one other source) |
| Public vs private IP | `vm-web` public, `vm-app` empty `PublicIP` | Module 3 | ✅ Confirmed |
| Private connectivity both ways | `ping`, `curl` | Modules 3, 4 | ✅ Confirmed |
| NSG blocks, then allows, the same traffic | `curl` exit 28 then 0 | Module 4 | ✅ Confirmed |
| Network Watcher IP flow verify | `test-ip-flow` Deny and Allow | Module 4 | ✅ Confirmed |
| VNet peering links created | `peering create` | Module 5 | ✅ Created |
| Traffic across the peering | Needs `vm-peer` | Module 5 | ➖ Not tested (core quota) |
| Tags on all resources | `az resource list` | Module 5 | ✅ 11 of 11 |
| Resource group deleted | `az group exists` | Module 5 | ✅ `false` |

> [!NOTE]
> Public IPs, key fingerprints and the Windows username are blurred or cropped out of the screenshots. The tests cover one NSG flow (inbound TCP 8080). Exhibit 26 shows the full curl sequence in one frame, so Exhibits 21 and 23 overlap with it by design.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

☁️ **[Microsoft Azure](https://azure.microsoft.com)** · 📚 **[Network security groups](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview)** · 🔗 **[VNet peering](https://learn.microsoft.com/azure/virtual-network/virtual-network-peering-overview)**

</div>
