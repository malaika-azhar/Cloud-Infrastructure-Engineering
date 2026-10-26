<a id="top"></a>
<div align="center">

# 📊 Project 05 — Index
### Azure Monitor and Log Analytics
**Project 05 of 06 — Cloud Infrastructure Engineering**

![Azure](https://img.shields.io/badge/Azure_Monitor-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![KQL](https://img.shields.io/badge/KQL-Log_Analytics-2C3E70?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this project.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Modules | 🖼️ Screenshots |
|:---:|:---:|
| **4** | **20** |

</div>

<p align="center">🧩 <b>Lab:</b> Ubuntu VM in India South Central · Log Analytics workspace, DCR and log alert in UAE North · one CPU alert and one failed-login alert fired</p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Check providers, quota and VM size | 🔵 Module 1 | Providers `Registered`, 2 of 6 vCPUs used | [Exhibit 1](#ex1) |
| 2 | Create the workspace | 🔵 Module 1 | 30 day retention, 0.023 GB daily cap | [Exhibit 2](#ex2) |
| 3 | Create the VM | 🔵 Module 1 | `vm-cie-p05` running, no public IP | [Exhibit 3](#ex3) |
| 4 | Install Azure Monitor Agent | 🔵 Module 1 | Extension `Succeeded`, version 1.45 | [Exhibit 4](#ex4) |
| 5 | Decide the region, create the DCR | 🟠 Module 2 | DCR in `uaenorth`, linked to the VM | [Exhibit 5](#ex5), [Exhibit 6](#ex6) |
| 6 | Generate failed SSH logins | 🟠 Module 2 | 5 `Invalid user` lines in `auth.log` | [Exhibit 7](#ex7) |
| 7 | Wait for the data | 🟠 Module 2 | Heartbeat 10, Perf 24, Syslog 14 rows | [Exhibit 8](#ex8) |
| 8 | Query the failed logins | 🟠 Module 2 | 5 `baduser` rows found | [Exhibit 9](#ex9) |
| 9 | Create action group and CPU alert | 🟢 Module 3 | Average CPU > 50, window 5 min | [Exhibit 10](#ex10) |
| 10 | Load the CPU, wait for the alert | 🟢 Module 3 | Alert `Fired` at 22:34:44 UTC | [Exhibit 11](#ex11) |
| 11 | Check the alert e-mail | 🟢 Module 3 | Activated e-mail, value 59.698 | [Exhibit 12](#ex12), [Exhibit 13](#ex13) |
| 12 | Create the log alert | 🟢 Module 3 | Every 5 min, window 10 min | [Exhibit 14](#ex14) |
| 13 | Trigger the log alert | 🟢 Module 3 | Alert `Fired` at 23:39:14 UTC | [Exhibit 15](#ex15) |
| 14 | Build and save the workbook | 🟣 Module 4 | `wb-cie-p05` with ten rows | [Exhibit 16](#ex16), [Exhibit 17](#ex17) |
| 15 | Delete both workspaces | 🟣 Module 4 | No workspace left in the group | [Exhibit 18](#ex18) |
| 16 | Delete the resource group | 🟣 Module 4 | `group exists: false` | [Exhibit 19](#ex19) |
| 17 | Check for leftovers | 🟣 Module 4 | Nothing left in the subscription | [Exhibit 20](#ex20) |

---

## 🔵 Module 1 — Workspace, VM and Agent

Exhibits 1 to 4.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex1"></a>
<a href="Screenshots/ss-01-combined-check.png"><img src="Screenshots/ss-01-combined-check.png" width="380" alt="Exhibit 1"></a>
<br><b>Exhibit 1 — Step 1 check</b>
<br><sub>Providers, quota, VM size</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex2"></a>
<a href="Screenshots/ss-02-workspace-created.png"><img src="Screenshots/ss-02-workspace-created.png" width="380" alt="Exhibit 2"></a>
<br><b>Exhibit 2 — First workspace</b>
<br><sub>30 days, 0.023 GB cap</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex3"></a>
<a href="Screenshots/ss-03-vm-created.png"><img src="Screenshots/ss-03-vm-created.png" width="380" alt="Exhibit 3"></a>
<br><b>Exhibit 3 — Linux VM</b>
<br><sub>Running, no public IP</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex4"></a>
<a href="Screenshots/ss-04-azure-monitor-agent.png"><img src="Screenshots/ss-04-azure-monitor-agent.png" width="380" alt="Exhibit 4"></a>
<br><b>Exhibit 4 — Azure Monitor Agent</b>
<br><sub>Identity and extension</sub>
</td>
</tr>
</table>

---

## 🟠 Module 2 — Region Decision and Data Collection

Exhibits 5 to 9.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex5"></a>
<a href="Screenshots/ss-05-allowed-regions-decision.png"><img src="Screenshots/ss-05-allowed-regions-decision.png" width="380" alt="Exhibit 5"></a>
<br><b>Exhibit 5 — Region decision</b>
<br><sub>Policy list and DCR regions</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex6"></a>
<a href="Screenshots/ss-06-dcr-and-association.png"><img src="Screenshots/ss-06-dcr-and-association.png" width="380" alt="Exhibit 6"></a>
<br><b>Exhibit 6 — DCR and association</b>
<br><sub>Rule in uaenorth, linked to VM</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex7"></a>
<a href="Screenshots/ss-07-failed-ssh-logins.png"><img src="Screenshots/ss-07-failed-ssh-logins.png" width="380" alt="Exhibit 7"></a>
<br><b>Exhibit 7 — Failed SSH logins</b>
<br><sub>Services active, 5 log lines</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex8"></a>
<a href="Screenshots/ss-08-heartbeat-syslog-perf.png"><img src="Screenshots/ss-08-heartbeat-syslog-perf.png" width="380" alt="Exhibit 8"></a>
<br><b>Exhibit 8 — Tables with data</b>
<br><sub>Heartbeat, Syslog, Perf</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex9"></a>
<a href="Screenshots/ss-09-failed-login-query.png"><img src="Screenshots/ss-09-failed-login-query.png" width="380" alt="Exhibit 9"></a>
<br><b>Exhibit 9 — Failed-login query</b>
<br><sub>Five baduser rows</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🟢 Module 3 — Alerts and Notifications

Exhibits 10 to 15.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="Screenshots/ss-10-action-group-metric-alert.png"><img src="Screenshots/ss-10-action-group-metric-alert.png" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — Action group and CPU alert</b>
<br><sub>Receiver and rule values</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="Screenshots/ss-11-metric-alert-fired.png"><img src="Screenshots/ss-11-metric-alert-fired.png" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — CPU alert fired</b>
<br><sub>Fired, CPU near 99.6 %</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="Screenshots/ss-12-alert-email-triggered.png"><img src="Screenshots/ss-12-alert-email-triggered.png" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — Alert e-mail</b>
<br><sub>Activated, 22:57 UTC</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="Screenshots/ss-13-alert-email-details.png"><img src="Screenshots/ss-13-alert-email-details.png" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — Alert e-mail details</b>
<br><sub>Value 59.698 over 5 min</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="Screenshots/ss-14-log-alert-rule.png"><img src="Screenshots/ss-14-log-alert-rule.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — Log alert rule</b>
<br><sub>Every 5 min, window 10 min</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="Screenshots/ss-15-log-alert-fired.png"><img src="Screenshots/ss-15-log-alert-fired.png" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — Log alert fired</b>
<br><sub>23:39:14 UTC</sub>
</td>
</tr>
</table>

---

## 🟣 Module 4 — Workbook and Cleanup

Exhibits 16 to 20.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex16"></a>
<a href="Screenshots/ss-16-workbook-query.png"><img src="Screenshots/ss-16-workbook-query.png" width="380" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — Workbook query</b>
<br><sub>Unsaved, rows from the VM</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex17"></a>
<a href="Screenshots/ss-17-workbook-saved.png"><img src="Screenshots/ss-17-workbook-saved.png" width="380" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — Workbook saved</b>
<br><sub><code>wb-cie-p05</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex18"></a>
<a href="Screenshots/ss-18-workspaces-deleted.png"><img src="Screenshots/ss-18-workspaces-deleted.png" width="380" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — Workspaces deleted</b>
<br><sub>Force delete, none left</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex19"></a>
<a href="Screenshots/ss-19-group-deleted.png"><img src="Screenshots/ss-19-group-deleted.png" width="380" alt="Exhibit 19"></a>
<br><b>Exhibit 19 — Group deleted</b>
<br><sub>Four older groups remain</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex20"></a>
<a href="Screenshots/ss-20-final-proof.png"><img src="Screenshots/ss-20-final-proof.png" width="380" alt="Exhibit 20"></a>
<br><b>Exhibit 20 — Final proof</b>
<br><sub>No leftovers in subscription</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|---|:---:|
| Providers, quota and VM size | Check command | Module 1 | ✅ Confirmed |
| Workspace with retention and cap | `workspace show` | Module 1 | ✅ 30 days, 0.023 GB |
| VM without public IP | `az vm list -d` | Module 1 | ✅ Confirmed |
| Azure Monitor Agent installed | `az vm extension list` | Module 1 | ✅ `Succeeded` |
| DCR linked to the VM | DCR show, association list | Module 2 | ✅ Confirmed |
| Failed logins generated | `auth.log` count in the VM | Module 2 | ✅ 5 lines |
| Data in Heartbeat, Syslog, Perf | KQL poll | Module 2 | ✅ 10, 14 and 24 rows |
| Failed-login query | KQL on `Syslog` | Module 2 | ✅ 5 rows |
| Action group and CPU alert | `show` commands | Module 3 | ✅ Confirmed |
| CPU alert fired | Alerts Management API | Module 3 | ✅ `Fired` |
| Alert e-mail received | Gmail | Module 3 | ✅ Activated e-mail (second fire) |
| Log alert rule | `scheduled-query show` | Module 3 | ✅ Confirmed |
| Log alert fired | Alerts Management API | Module 3 | ✅ `Fired` |
| Workbook saved | Azure portal | Module 4 | ✅ `wb-cie-p05` |
| Nothing left after cleanup | Subscription-wide lists | Module 4 | ✅ Confirmed |
| Real attacker IPs observed | Not part of this lab | Module 2 | ➖ Not shown |

> [!NOTE]
> The failed logins and the CPU load were started inside my own VM. The subscription ID and my e-mail address are blurred in the screenshots. Portal and Gmail times are UTC+5; server and API times are UTC.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

☁️ **[Azure Monitor](https://learn.microsoft.com/azure/azure-monitor/)** · 🗄️ **[Log Analytics](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-overview)**

</div>
