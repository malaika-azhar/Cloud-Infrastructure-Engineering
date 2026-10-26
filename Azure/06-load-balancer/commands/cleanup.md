<a id="top"></a>
<div align="center">

# 🧹 Project 06 — Cleanup
### Azure Standard Load Balancer
**Project 06 of 6 — Cloud Infrastructure Engineering**

![Cleanup](https://img.shields.io/badge/Cleanup-Complete-brightgreen?style=for-the-badge)
![Scope](https://img.shields.io/badge/Scope-One_resource_group-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Proof](https://img.shields.io/badge/Proof-Exhibit_15-B9770E?style=for-the-badge)

**Cleanup was the last step. Only this project's resource group was deleted. The older groups were not touched.**

### [📖 README](../README.md) &nbsp;·&nbsp; [📑 Visual Index](../LoadBalancer-INDEX.md) &nbsp;·&nbsp; [⌨️ Commands](commands.md)

</div>

---

## 🖱️ Portal Steps

This project used the Azure Portal only, so cleanup has no command. The steps are:

| # | 📍 Where | Action |
|:---:|:---:|---|
| 1 | ☁️ Azure Portal | Search **Resource groups** and open the list |
| 2 | ☁️ Azure Portal | Open `rg-cie-p06-load-balancer` and check that no other project's resource is in it |
| 3 | ☁️ Azure Portal | Click **Delete resource group** |
| 4 | ☁️ Azure Portal | Type the exact group name `rg-cie-p06-load-balancer` to confirm |
| 5 | ☁️ Azure Portal | Click **Delete**, then **Delete** again on the confirmation box |
| 6 | ☁️ Azure Portal | Reopen **Resource groups** and check the list |

| 🖼️ Proof | Result |
|:---:|---|
| **Exhibit 15** — `Screenshots/ss-15-cleanup-rg-list.png` | project group gone, four older groups remain |

---

## 🗑️ What This Removed

Everything inside `rg-cie-p06-load-balancer`. The delete pane listed 12 resources 📝 (that screenshot is not kept in this repo):

| Resource | Includes |
|---|---|
| ⚖️ Load balancer `lb-cie-p06` | frontend `fe-lb-cie-p06`, pool `bepool-web`, probe `probe-http-80`, rule `rule-http-80` |
| 🌐 Public IP `pip-lb-cie-p06` | the only public IP in the project |
| 🛡️ NSG `nsg-snet-web` | rule `allow-http-80` |
| 🌐 VNet `vnet-cie-p06-main` | subnet `snet-web` |
| 🖥️ VMs `vm-web-01`, `vm-web-02` | OS disks, network cards, SSH key resources |

Both VMs were running when the group was deleted, and the load balancer was still using them.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontSize': '14px'}, 'flowchart': {'nodeSpacing': 22, 'rankSpacing': 30, 'padding': 8}}}%%
flowchart LR
    G["🧹 Delete resource group<br/>rg-cie-p06-load-balancer"]:::del --> L["⚖️ Load balancer + public IP"]:::gone
    G --> N["🛡️ NSG + 🌐 VNet"]:::gone
    G --> V["🖥️ 2 VMs + disks, NICs, keys"]:::gone
    O["📂 Older groups<br/>4 groups"]:::keep
    classDef del fill:#943126,stroke:#571C16,stroke-width:2px,color:#FFFFFF
    classDef gone fill:#5D6D7E,stroke:#2E4053,stroke-width:2px,color:#FFFFFF
    classDef keep fill:#1E8449,stroke:#0E4A28,stroke-width:2px,color:#FFFFFF
    linkStyle default stroke:#2C3E50,stroke-width:2px
```
<p align="center"><em>Only the project group is deleted. The older groups were never selected.</em></p>

---

## ✅ What Proves It

Exhibit 15 shows the list after the delete:

| Resource group | In the list after cleanup? |
|---|:---:|
| `rg-cie-p06-load-balancer` | ✅ Yes |
| `ForensicsCapstone-RG` | ✅ Yes |
| `NetworkWatcherRG` | ✅ Yes |
| `osticket-rg` | ✅ Yes |
| `sysadmin-labs` | ✅ Yes |

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 README](../README.md) &nbsp;·&nbsp; [📑 Visual Index](../LoadBalancer-INDEX.md) &nbsp;·&nbsp; [⌨️ Commands](commands.md)

</div>
