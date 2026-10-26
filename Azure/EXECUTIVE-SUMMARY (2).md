<a id="top"></a>
<div align="center">

# 🧭 Executive Summary
### Cloud Infrastructure Engineering — Azure

![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu_LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Networking](https://img.shields.io/badge/VNet_·_NSG_·_Peering-1A5276?style=for-the-badge)
![Identity](https://img.shields.io/badge/Entra_ID_·_RBAC-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Monitoring](https://img.shields.io/badge/Log_Analytics_·_KQL-2C3E70?style=for-the-badge)
![Load Balancer](https://img.shields.io/badge/Standard_Load_Balancer-0A6EBD?style=for-the-badge)

**A one-page read of what this portfolio proves — deploy, network, store, secure, monitor, balance.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Projects | 🖼️ Screenshots | 🖥 VMs Built | 🌍 Regions | 🔔 Alerts Fired | 🧱 Resource Groups |
|:---:|:---:|:---:|:---:|:---:|:---:|
| **6** | **145** | **8** | **2** | **2** | **6** |

</div>

---

## 🧩 The Six Projects, One Thread

```mermaid
flowchart LR
    A["🖥️ Deploy<br/>Virtual Machine"] --> B["🌐 Network<br/>VNet, NSG"]
    B --> C["🗄️ Store<br/>Blob, Files"]
    C --> D["🔐 Secure<br/>Entra ID, RBAC"]
    D --> E["📡 Monitor<br/>Logs, alerts"]
    E --> F["⚖️ Balance<br/>Load Balancer"]
    style A fill:#2C3E70,color:#fff,stroke:#131B3A
    style B fill:#1A5276,color:#fff,stroke:#0B2E43
    style C fill:#117864,color:#fff,stroke:#083D33
    style D fill:#76448A,color:#fff,stroke:#432752
    style E fill:#943126,color:#fff,stroke:#561C16
    style F fill:#1E8449,color:#fff,stroke:#0E4A28
```

---

## 📂 Project Summary

<table>
<tr>
<td width="50%" valign="top">

**🖥️ 01 — VM Deployment & Administration**
`Azure CLI` `Ubuntu 24.04` `SSH key` `ext4` `fstab`

Deployed an Ubuntu VM with key-based SSH, mounted a 4 GB data disk by UUID, resized the VM and compared the stopped and deallocated states.

**Status:** ✅ Complete
[README](./Azure/01-vm-deployment-administration/README.md) · [Index](./Azure/01-vm-deployment-administration/VM-INDEX.md)

</td>
<td width="50%" valign="top">

**🌐 02 — Azure Virtual Networking**
`VNet` `NSG` `Peering` `Network Watcher`

Built a VNet with two subnets and two NSGs, limited SSH to one address, blocked and then allowed the same traffic, and created a VNet peering.

**Status:** ✅ Complete
[README](./Azure/02-azure-virtual-networking/README.md) · [Index](./Azure/02-azure-virtual-networking/Virtual-Networking-INDEX.md)

</td>
</tr>
<tr>
<td width="50%" valign="top">

**🗄️ 03 — Azure Storage Services**
`Blob` `Azure Files` `SAS` `Soft delete`

Stored and tiered blobs, recovered a deleted file, tested anonymous, SAS and firewall access, and mounted an Azure Files share on a Linux VM.

**Status:** ✅ Complete
[README](./Azure/03-azure-storage-services/README.md) · [Index](./Azure/03-azure-storage-services/Storage-INDEX.md)

</td>
<td width="50%" valign="top">

**🔐 04 — Entra ID & RBAC**
`Entra ID` `RBAC` `Policy` `Key Vault`

Gave a group the Reader role, tested allowed and denied actions as a real user, added a lock and a policy, and read a secret with a managed identity.

**Status:** ✅ Complete
[README](./Azure/04-entra-id-rbac/README.md) · [Index](./Azure/04-entra-id-rbac/Entra-RBAC-INDEX.md)

</td>
</tr>
<tr>
<td width="50%" valign="top">

**📡 05 — Azure Monitor & Log Analytics**
`Azure Monitor Agent` `KQL` `Alerts` `Workbook`

Sent VM logs and CPU data to Log Analytics, found failed SSH logins with KQL, made a metric alert and a log alert fire, and saved a workbook.

**Status:** ✅ Complete
[README](./Azure/05-azure-monitor-log-analytics/README.md) · [Index](./Azure/05-azure-monitor-log-analytics/Monitor-INDEX.md)

</td>
<td width="50%" valign="top">

**⚖️ 06 — Standard Load Balancer**
`Load Balancer` `nginx` `Health probe` `Failover`

Put two private web servers behind one public address, stopped one on purpose, and watched the other answer, then brought the first back.

**Status:** ✅ Complete
[README](./Azure/06-load-balancer/README.md) · [Index](./Azure/06-load-balancer/LoadBalancer-INDEX.md)

</td>
</tr>
</table>

---

## 🔎 How a Claim Gets Verified

```mermaid
flowchart LR
    C["📝 Claim made<br/>in a project"] --> E{"Evidence<br/>exists?"}
    E -->|"Screenshot"| S["🖼️ Backed by<br/>a screenshot"]
    E -->|"Command output"| L["🔍 Backed by a live<br/>command check"]
    E -->|"Neither"| N["📝 Marked as a note,<br/>not a proven result"]
    S --> V["✅ Verified"]
    L --> V

    style C fill:#1A5276,color:#fff,stroke:#0B2E43
    style E fill:#2874A6,color:#fff,stroke:#154360
    style S fill:#117864,color:#fff,stroke:#083D33
    style L fill:#117864,color:#fff,stroke:#083D33
    style N fill:#943126,color:#fff,stroke:#571C16
    style V fill:#1E8449,color:#fff,stroke:#0E4A28
```

<p align="center"><em>This is the rule every project follows, applied to the checks below.</em></p>

---

## 🎯 Verification Snapshot

| Check | Status |
|---|:---:|
| Data disk mounted by UUID and still mounted after a reboot | ✅ |
| Resize proven from inside the VM (memory 3.8Gi → 7.7Gi) | ✅ |
| NSG deny gave `curl` exit 28, allow gave exit 0, IP flow verify named the rule | ✅ |
| SAS token returned 200 before expiry and 403 after | ✅ |
| Deleted blob recovered from an older version | ✅ |
| Reader role allowed a view and denied a write as a real user | ✅ |
| Lock and policy each blocked an action | ✅ |
| VM read a Key Vault secret with a managed identity (HTTP 200) | ✅ |
| Metric alert and log alert both fired, Activated e-mail received | ✅ |
| Failover shown: `vm-web-02` answered while `vm-web-01` was stopped | ✅ |
| Cleanup proven with a final-state screenshot (Projects 02 to 06) | ✅ |

> [!NOTE]
> This is a summary only — full methodology and screenshots for each project live in that project's own README.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

</div>
