<a id="top"></a>
<div align="center">

# ⚖️ Project 06 — Index
### Azure Standard Load Balancer
**Project 06 of 6 — Cloud Infrastructure Engineering**

![Azure](https://img.shields.io/badge/Azure-Load_Balancer-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Standard](https://img.shields.io/badge/SKU-Standard-0A6EBD?style=for-the-badge)
![nginx](https://img.shields.io/badge/nginx-2_Servers-009639?style=for-the-badge&logo=nginx&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete_with_limits-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this project.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Modules | 🪜 Steps | 🖼️ Screenshots |
|:---:|:---:|:---:|
| **5** | **18** | **15** |

</div>

<p align="center">🧩 <b>Lab:</b> one VNet and subnet · one NSG · two Ubuntu web servers with no public IP · one Standard Load Balancer with a public IP · failover test · resource group deleted at the end</p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Check the vCPU quota and VM size | 🔵 Module 1 | Page showed no data, size chosen, run without screenshot 📝 | — |
| 2 | Create the resource group | 🔵 Module 1 | India South Central, three tags | [Exhibit 1](#ex1) |
| 3 | Create the VNet and subnet | 🟠 Module 2 | `10.0.0.0/16`, one subnet | [Exhibit 2](#ex2) |
| 4 | Create the NSG with an HTTP rule | 🟠 Module 2 | `allow-http-80`, priority 100 | [Exhibit 3](#ex3) |
| 5 | Create `vm-web-01` | 🟢 Module 3 | Running, no public IP | [Exhibit 4](#ex4) |
| 6 | Create `vm-web-02` | 🟢 Module 3 | Running, no public IP | [Exhibit 5](#ex5) |
| 7 | Install nginx with a page on each server | 🟢 Module 3 | Run command, no screenshot 📝 | — |
| 8 | Create the public IP | 🟣 Module 4 | Standard, run without screenshot 📝 | — |
| 9 | Create the load balancer | 🟣 Module 4 | Standard, pool, rule and probe listed | [Exhibit 6](#ex6) |
| 10 | Check the backend pool | 🟣 Module 4 | `10.0.0.4` and `10.0.0.5` | [Exhibit 7](#ex7) |
| 11 | Check the health probe | 🟣 Module 4 | HTTP 80, path `/`, 5 seconds | [Exhibit 8](#ex8) |
| 12 | Browser test through the load balancer | 🟡 Module 5 | `vm-web-01` answers | [Exhibit 9](#ex9) |
| 13 | Stop `vm-web-01` | 🟡 Module 5 | Stopped (deallocated) | [Exhibit 10](#ex10) |
| 14 | Browser test while stopped | 🟡 Module 5 | `vm-web-02` answers | [Exhibit 11](#ex11) |
| 15 | Start `vm-web-01` | 🟡 Module 5 | Running again | [Exhibit 12](#ex12) |
| 16 | Browser test after the start | 🟡 Module 5 | `vm-web-01` answers again | [Exhibit 13](#ex13) |
| 17 | Check the rule health status | 🟡 Module 5 | 100% healthy, both Up | [Exhibit 14](#ex14) |
| 18 | Delete the resource group | 🧹 Cleanup | Group gone, 4 older groups remain | [Exhibit 15](#ex15) |

---

## 🔵 Module 1 — Setup

Exhibit 1.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex1"></a>
<a href="Screenshots/ss-01-rg-created.png"><img src="Screenshots/ss-01-rg-created.png" width="380" alt="Exhibit 1"></a>
<br><b>Exhibit 1 — Resource group</b>
<br><sub>India South Central, three tags</sub>
</td>
<td></td>
</tr>
</table>

---

## 🟠 Module 2 — VNet and NSG

Exhibits 2 to 3.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex2"></a>
<a href="Screenshots/ss-02-vnet-created.png"><img src="Screenshots/ss-02-vnet-created.png" width="380" alt="Exhibit 2"></a>
<br><b>Exhibit 2 — VNet</b>
<br><sub><code>10.0.0.0/16</code>, one subnet</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex3"></a>
<a href="Screenshots/ss-03-nsg-http-rule.png"><img src="Screenshots/ss-03-nsg-http-rule.png" width="380" alt="Exhibit 3"></a>
<br><b>Exhibit 3 — NSG rule</b>
<br><sub><code>allow-http-80</code>, TCP 80, Allow</sub>
</td>
</tr>
</table>

---

## 🟢 Module 3 — Web Servers

Exhibits 4 to 5.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex4"></a>
<a href="Screenshots/ss-04-vm-web-01.png"><img src="Screenshots/ss-04-vm-web-01.png" width="380" alt="Exhibit 4"></a>
<br><b>Exhibit 4 — vm-web-01</b>
<br><sub>Running, no public IP</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex5"></a>
<a href="Screenshots/ss-05-vm-web-02.png"><img src="Screenshots/ss-05-vm-web-02.png" width="380" alt="Exhibit 5"></a>
<br><b>Exhibit 5 — vm-web-02</b>
<br><sub>Running, no public IP</sub>
</td>
</tr>
</table>

---

## 🟣 Module 4 — Load Balancer

Exhibits 6 to 8.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex6"></a>
<a href="Screenshots/ss-06-lb-overview.png"><img src="Screenshots/ss-06-lb-overview.png" width="380" alt="Exhibit 6"></a>
<br><b>Exhibit 6 — Load balancer overview</b>
<br><sub>Standard, Regional, pool, rule, probe</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex7"></a>
<a href="Screenshots/ss-07-backend-pool.png"><img src="Screenshots/ss-07-backend-pool.png" width="380" alt="Exhibit 7"></a>
<br><b>Exhibit 7 — Backend pool</b>
<br><sub>Both VMs in <code>bepool-web</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex8"></a>
<a href="Screenshots/ss-08-health-probe.png"><img src="Screenshots/ss-08-health-probe.png" width="380" alt="Exhibit 8"></a>
<br><b>Exhibit 8 — Health probe</b>
<br><sub>HTTP, port 80, path <code>/</code>, 5 seconds</sub>
</td>
<td></td>
</tr>
</table>

---

## 🟡 Module 5 — Tests and Failover

Exhibits 9 to 14.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex9"></a>
<a href="Screenshots/ss-09-browser-vm-web-01.png"><img src="Screenshots/ss-09-browser-vm-web-01.png" width="380" alt="Exhibit 9"></a>
<br><b>Exhibit 9 — Browser test</b>
<br><sub>Address returns <code>vm-web-01</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="Screenshots/ss-10-vm-web-01-stopped.png"><img src="Screenshots/ss-10-vm-web-01-stopped.png" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — VM stopped</b>
<br><sub><code>vm-web-01</code> Stopped (deallocated)</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="Screenshots/ss-11-browser-vm-web-02-failover.png"><img src="Screenshots/ss-11-browser-vm-web-02-failover.png" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — Failover</b>
<br><sub>Same address returns <code>vm-web-02</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="Screenshots/ss-12-vm-web-01-started.png"><img src="Screenshots/ss-12-vm-web-01-started.png" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — VM started</b>
<br><sub><code>vm-web-01</code> Running again</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="Screenshots/ss-13-browser-vm-web-01-back.png"><img src="Screenshots/ss-13-browser-vm-web-01-back.png" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — Recovery</b>
<br><sub><code>vm-web-01</code> answers again</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="Screenshots/ss-14-rule-health-status.png"><img src="Screenshots/ss-14-rule-health-status.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — Rule health status</b>
<br><sub>100% healthy, both Up</sub>
</td>
</tr>
</table>

---

## 🧹 Cleanup

Exhibit 15.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="Screenshots/ss-15-cleanup-rg-list.png"><img src="Screenshots/ss-15-cleanup-rg-list.png" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — Cleanup proof</b>
<br><sub>Project group gone, four older groups remain</sub>
</td>
<td></td>
</tr>
</table>

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|---|:---:|
| VMs have no public IP | VM overview | Module 3 | ✅ Confirmed |
| Both VMs in the pool | Backend pools page | Module 4 | ✅ Confirmed |
| Probe settings | Health probes page | Module 4 | ✅ HTTP 80, 5 seconds |
| Traffic reaches a server | Browser | Module 5 | ✅ `vm-web-01` |
| Failover to the other server | Browser, one VM stopped | Module 5 | ✅ `vm-web-02` |
| Recovery after a start | Browser, rule health status | Module 5 | ✅ `vm-web-01` back, both Up |
| Both servers alternating | Repeated refresh | Module 5 | ➖ Not shown |
| Unhealthy state while stopped | Probe status during the stop | Module 5 | ➖ Not captured |
| Metrics charts | Health Probe Status, Data Path Availability | Module 5 | ➖ Stayed empty 📝 |
| HTTPS | Not part of this lab | Module 4 | ➖ Not attempted |
| Project group deleted | Resource groups list | Cleanup | ✅ Confirmed |

> [!NOTE]
> The load balancer's public IP, the subscription ID, my email and my tenant name are blurred. Items marked 📝 in the README were run but have no screenshot in this repo.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

☁️ **[Microsoft Azure](https://azure.microsoft.com)** · ⚖️ **[Azure Load Balancer documentation](https://learn.microsoft.com/azure/load-balancer/)**

</div>
