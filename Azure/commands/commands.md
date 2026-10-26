<a id="top"></a>
<div align="center">

# ⌨️ Project 04 — Commands
### Microsoft Entra ID and Azure RBAC
**Project 04 of 6 — Cloud Infrastructure Engineering**

![Azure CLI](https://img.shields.io/badge/Azure_CLI-Cloud_Shell-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-Commands-4EAA25?style=for-the-badge&logo=gnubash&logoColor=white)
![Portal](https://img.shields.io/badge/Azure_Portal-Steps-0078D4?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

**Every command used in this project, in order, with where it ran and which exhibit proves it.**

### [📖 README](../README.md) &nbsp;·&nbsp; [📑 Visual Index](../Entra-RBAC-INDEX.md) &nbsp;·&nbsp; [🧹 Cleanup](cleanup.md)

</div>

---

## 🧭 How to Read This File

| Mark | Meaning |
|:---:|---|
| ✅ | Ran, and a screenshot in `../Screenshots/` proves the result. The exhibit number is given. |
| 📝 | Ran, but there is no screenshot of it in the repo. The text says which exhibit proves the result, or that nothing proves it. |
| 🌐 | Done in the Azure portal, not in Cloud Shell. |
| `<...>` | A value that is not kept in the repo. |

| 📍 Where it ran | Shown as |
|---|:---:|
| Azure Cloud Shell (Bash), prompt `malaika [ ~ ]` | ☁️ **Cloud Shell** |
| Azure portal | 🌐 **Portal** |
| Private browser window, signed in as the test user | 🕶️ **Test user** |

### 🔁 Placeholders

| Placeholder | What it stands for |
|---|---|
| `<GROUP_OBJECT_ID>` | Object ID of the group `cie-p04-readers`, copied from the portal. Not stored in the repo. |
| `<SUBSCRIPTION_ID>` | Subscription ID. Commands read it with `az account show`. |
| `<DEMO_SECRET_VALUE>` | The 17-character test value of `demo-secret`. Never printed. |
| `<TENANT_DOMAIN>` | The directory's `onmicrosoft.com` domain. |

---

## 📊 At a Glance

| 🔵 Setup | 🟢 Access | 🟠 Guardrails | 🟣 Identity | 🔴 Cleanup |
|:---:|:---:|:---:|:---:|:---:|
| **Steps 1-4** | **Steps 5-9** | **Steps 10-13** | **Steps 14-16** | **Steps 17-20** |
| check, group, user | Reader, allowed, denied | custom role, lock, policy | VM reads secret | remove and prove |

---

## 🔵 Setup &nbsp;·&nbsp; Steps 1-4

### Step 1 — Combined check &nbsp; 📝 + ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell, then 🌐 Portal | **Exhibit 1** (the portal form) | quota `Used 2` of `6`, name free, create-user form opens |

**1a. First check.** 📝 The users, groups and domain parts failed. `--top` is not a valid option for `az ad user list`, and the Microsoft Graph call timed out.

```bash
az ad user list --top 1 --query "[0].userPrincipalName" -o tsv
```

**1b. Quota, name and providers.** 📝

```bash
az vm list-usage --location indiasouthcentral --query "[?name.value=='cores'].{Used:currentValue, Limit:limit}" -o table && az keyvault check-name --name kv-cie-p04-malaika --query "{available:nameAvailable, reason:reason}" -o json && az provider show --namespace Microsoft.PolicyInsights --query registrationState -o tsv && az provider show --namespace Microsoft.KeyVault --query registrationState -o tsv
```

**1c. Register the provider.** 📝 `Microsoft.KeyVault` was `NotRegistered`.

```bash
az provider register --namespace Microsoft.KeyVault --wait && az provider show --namespace Microsoft.KeyVault --query registrationState -o tsv
```

**1d. Graph token attempts.** 📝 Each returned `Timeout waiting for token from portal`.

```bash
az ad user list --query "[0].userPrincipalName" -o tsv
az ad group list --query "[0].displayName" -o tsv
az ad signed-in-user show --query userPrincipalName -o tsv
az login --use-device-code --only-show-errors
```

**1e. Portal check.** 🌐 `Microsoft Entra ID`, `Users`, `New user`, `Create new user`. The form opened (Exhibit 1).

### Step 2 — Resource group with tags &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 2** | `Succeeded`, three tags |

```bash
az group create --name rg-cie-p04-iam --location indiasouthcentral --tags project=cie-p04 owner=malaika env=lab --only-show-errors --query "{name:name, location:location, state:properties.provisioningState, tags:tags}" -o json
```

### Step 3 — Test user &nbsp; 🌐 ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| 🌐 Portal | **Exhibit 3** | `CIE P04 Test User` in the Users list |

- `Microsoft Entra ID`, `Users`, `New user`, `Create new user`.
- User principal name `cie-p04-testuser`, display name `CIE P04 Test User`, `Auto-generate password` ticked.
- `Review + create`, then `Create`. The password was copied to my own notes.

### Step 4 — Security group with the user &nbsp; 🌐 ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| 🌐 Portal | **Exhibit 4** | `cie-p04-readers`, Security, Assigned, 1 user |

- `Microsoft Entra ID`, `Groups`, `New group`.
- Group type `Security`, name `cie-p04-readers`, membership `Assigned`.
- Members: search `CIE P04 Test User`, tick the checkbox until `Selected (1)`, `Select`, `Create`.

---

## 🟢 Access &nbsp;·&nbsp; Steps 5-9

### Step 5 — Reader role to the group &nbsp; 📝

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | none (Step 6 proves it) | the output printed `role: null`, so it is not kept |

```bash
G="<GROUP_OBJECT_ID>" && SUB=$(az account show --query id -o tsv) && az role assignment create --assignee-object-id "$G" --assignee-principal-type Group --role "Reader" --scope "/subscriptions/$SUB/resourceGroups/rg-cie-p04-iam" --only-show-errors --query "{role:roleDefinitionName, principalType:principalType, scope:scope}" -o json
```

> [!NOTE]
> The first try had the text `PASTE_OBJECT_ID` left in the command and failed with `InvalidPrincipalId`. The second try had the real ID.

### Step 6 — Verify the assignment &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 5** | `Reader`, `Group`, resource group scope |

```bash
G="<GROUP_OBJECT_ID>" && az role assignment list --resource-group rg-cie-p04-iam --fill-principal-name false --fill-role-definition-name true --query "[?principalId=='$G'].{role:roleDefinitionName, type:principalType, scope:scope}" -o json --only-show-errors | sed -E 's#/subscriptions/[^/]+#/subscriptions/<hidden>#'
```

### Step 7 — Key Vault and a test secret &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 6** | vault `Succeeded`, `rbac: true`; `demo-secret` enabled |

```bash
az keyvault create --name kv-cie-p04-malaika --resource-group rg-cie-p04-iam --location indiasouthcentral --enable-rbac-authorization true --tags project=cie-p04 owner=malaika env=lab --only-show-errors --query "{name:name, rbac:properties.enableRbacAuthorization, softDelete:properties.enableSoftDelete, state:properties.provisioningState}" -o json && ME=$(az ad signed-in-user show --query id -o tsv 2>/dev/null || az account show --query user.name -o tsv) && SUB=$(az account show --query id -o tsv) && az role assignment create --assignee "$ME" --role "Key Vault Secrets Officer" --scope "/subscriptions/$SUB/resourceGroups/rg-cie-p04-iam/providers/Microsoft.KeyVault/vaults/kv-cie-p04-malaika" --only-show-errors -o none 2>&1 | cut -c1-150 | head -3; sleep 45 && az keyvault secret set --vault-name kv-cie-p04-malaika --name demo-secret --value "<DEMO_SECRET_VALUE>" --only-show-errors --query "{name:name, enabled:attributes.enabled}" -o json 2>&1 | cut -c1-150 | head -6
```

### Step 8 — Allowed action as the test user &nbsp; 🕶️ ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| 🕶️ Test user | **Exhibit 7** | resource group opens, "Showing 1 of 1" |

- Private browser window, sign in to the portal as `cie-p04-testuser@<TENANT_DOMAIN>`.
- First sign-in: new password, then Microsoft Authenticator setup 📝.
- `Resource groups`, then `rg-cie-p04-iam`, `Overview`.

### Step 9 — Denied action as the test user &nbsp; 🕶️ ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| 🕶️ Test user | **Exhibit 8** | `AuthorizationFailed` |

- Same window, resource group `rg-cie-p04-iam`, `Tags`.
- Add the tag `test` with value `denied`, then `Apply`.

---

## 🟠 Guardrails &nbsp;·&nbsp; Steps 10-13

### Step 10 — Custom role &nbsp; 📝

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | none (Step 11 shows the role name) | `CIE P04 VM Operator`, `CustomRole` |

```bash
SUB=$(az account show --query id -o tsv) && R='{"Name":"CIE P04 VM Operator","Description":"Read and restart VMs only","Actions":["Microsoft.Compute/virtualMachines/read","Microsoft.Compute/virtualMachines/restart/action","Microsoft.Resources/subscriptions/resourceGroups/read"],"NotActions":[],"AssignableScopes":["/subscriptions/'"$SUB"'/resourceGroups/rg-cie-p04-iam"]}' && az role definition create --role-definition "$R" --only-show-errors --query "{name:roleName, type:roleType, actions:permissions[0].actions, scopes:assignableScopes}" -o json | sed -E 's#/subscriptions/[^/]+#/subscriptions/<hidden>#'
```

> [!NOTE]
> The `sed` pattern `[^/]+` also matched the word `resourceGroups` inside the third action, so the output showed `Microsoft.Resources/subscriptions/<hidden>/read`. The role itself was created with the text above. A pattern that matches only GUIDs avoids this.

### Step 11 — Assign the custom role, verify &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 9** | `Reader` and `CIE P04 VM Operator`, both `Group` |

```bash
G="<GROUP_OBJECT_ID>" && SUB=$(az account show --query id -o tsv) && az role assignment create --assignee-object-id "$G" --assignee-principal-type Group --role "CIE P04 VM Operator" --scope "/subscriptions/$SUB/resourceGroups/rg-cie-p04-iam" --only-show-errors -o none 2>&1 | cut -c1-150 | head -3; sleep 20 && az role assignment list --resource-group rg-cie-p04-iam --fill-role-definition-name true --query "[?principalId=='$G'].{role:roleDefinitionName, type:principalType}" -o json --only-show-errors
```

### Step 12 — Lock and delete test &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 10** | lock `CanNotDelete`, then `ScopeLocked` |

```bash
az lock create --name lock-cie-p04 --lock-type CanNotDelete --resource-group rg-cie-p04-iam --only-show-errors --query "{name:name, level:level}" -o json && sleep 30 && az keyvault delete --name kv-cie-p04-malaika --resource-group rg-cie-p04-iam --only-show-errors 2>&1 | cut -c1-200 | head -4
```

### Step 13 — Azure Policy, tag required &nbsp; ✅ + 📝

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 11** | `RequestDisallowedByPolicy` for `pip-notag-test2` |

**13a. Assign the policy and test.** 📝 The policy definition was found by its display name. The long `sleep 400` in this command was lost when Cloud Shell opened a new session, so the test was run again below.

```bash
SUB=$(az account show --query id -o tsv) && PD=$(az policy definition list --query "[?displayName=='Require a tag on resources'].name | [0]" -o tsv) && echo "policy def found: ${PD:+yes}" && az policy assignment create --name pol-cie-p04-tag --display-name "CIE P04 require project tag" --policy "$PD" --params '{"tagName":{"value":"project"}}' --scope "/subscriptions/$SUB/resourceGroups/rg-cie-p04-iam" --only-show-errors --query "{name:name, scope:scope}" -o json | sed -E 's#/subscriptions/[^/]+#/subscriptions/<hidden>#' && sleep 400 && az network public-ip create --name pip-notag-test --resource-group rg-cie-p04-iam --only-show-errors 2>&1 | cut -c1-220 | head -4
```

**13b. First test after the restart.** 📝 The public IP was **created**, so the policy was not enforced yet. It had no tags.

```bash
az policy assignment list --resource-group rg-cie-p04-iam --query "[].{name:name, display:displayName}" -o json --only-show-errors && az network public-ip create --name pip-notag-test --resource-group rg-cie-p04-iam --only-show-errors 2>&1 | cut -c1-220 | head -4
```

**13c. Effect and tags.** 📝 Effect `deny`, `tags: null`, and the delete failed with `ScopeLocked`.

```bash
PD=$(az policy assignment show --name pol-cie-p04-tag --resource-group rg-cie-p04-iam --query policyDefinitionId -o tsv) && az policy definition show --name "${PD##*/}" --query "policyRule.then.effect" -o tsv && az network public-ip show --name pip-notag-test --resource-group rg-cie-p04-iam --query "{name:name, tags:tags}" -o json && az network public-ip delete --name pip-notag-test --resource-group rg-cie-p04-iam --only-show-errors 2>&1 | cut -c1-200 | head -3
```

**13d. Second test, a new name.** ✅ Exhibit 11.

```bash
az network public-ip create --name pip-notag-test2 --resource-group rg-cie-p04-iam --only-show-errors 2>&1 | cut -c1-230 | head -4
```

---

## 🟣 Identity &nbsp;·&nbsp; Steps 14-16

### Step 14 — VM with a system-assigned identity &nbsp; 📝

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 13** (name and identity), **Exhibit 14** (no public IP, no NSG) | `VM running`, `SystemAssigned` |

```bash
az vm create --name vm-cie-p04 --resource-group rg-cie-p04-iam --location indiasouthcentral --image Ubuntu2204 --size Standard_B2ls_v2 --public-ip-address "" --nsg "" --assign-identity "[system]" --admin-username azureuser --generate-ssh-keys --tags project=cie-p04 owner=malaika env=lab --only-show-errors --query "{name:name, state:powerState, identity:identity.type, publicIp:publicIpAddress}" -o json 2>&1 | cut -c1-200 | head -12
```

> [!NOTE]
> The output printed `name: null` and `identity: null`, because `az vm create` does not return those fields in that shape. It is not kept. `az vm show` in Step 16 proves the name and identity.

### Step 15 — Role for the VM identity &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 12** | `Key Vault Secrets User`, `ServicePrincipal` |

```bash
SUB=$(az account show --query id -o tsv) && PID=$(az vm show --name vm-cie-p04 --resource-group rg-cie-p04-iam --query identity.principalId -o tsv) && az role assignment create --assignee-object-id "$PID" --assignee-principal-type ServicePrincipal --role "Key Vault Secrets User" --scope "/subscriptions/$SUB/resourceGroups/rg-cie-p04-iam/providers/Microsoft.KeyVault/vaults/kv-cie-p04-malaika" --only-show-errors -o none && az role assignment list --scope "/subscriptions/$SUB/resourceGroups/rg-cie-p04-iam/providers/Microsoft.KeyVault/vaults/kv-cie-p04-malaika" --fill-role-definition-name true --query "[?principalId=='$PID'].{role:roleDefinitionName, type:principalType}" -o json
```

### Step 16 — Read the secret from inside the VM &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell (runs in the VM through `run-command`) | **Exhibit 13** | `host: vm-cie-p04`, HTTP `200`, value length `17` |

```bash
az vm show --name vm-cie-p04 --resource-group rg-cie-p04-iam --query "{vm:name, identity:identity.type}" -o json --only-show-errors && sleep 60 && az vm run-command invoke --name vm-cie-p04 --resource-group rg-cie-p04-iam --command-id RunShellScript --only-show-errors --scripts 'echo "host: $(hostname)"; T=$(curl -s -H "Metadata: true" "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net" | python3 -c "import sys,json;print(json.load(sys.stdin)[\"access_token\"])"); echo "token obtained: ${T:+yes}"; curl -s -o /tmp/s.json -w "secret read HTTP status: %{http_code}\n" -H "Authorization: Bearer $T" "https://kv-cie-p04-malaika.vault.azure.net/secrets/demo-secret?api-version=7.4"; python3 -c "import json;d=json.load(open(\"/tmp/s.json\"));print(\"secret id found:\", \"id\" in d, \"value length:\", len(d.get(\"value\",\"\")))"; rm -f /tmp/s.json' --query "value[0].message" -o tsv
```

> [!IMPORTANT]
> The token stays in a variable and the secret value is never printed. Only the status code and the length of the value are shown.

---

## 🔴 Cleanup &nbsp;·&nbsp; Steps 17-20

### Step 17 — Remove the lock and the leftover public IP &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 14** | `LOCKS LEFT 0`, resources listed without a public IP |

```bash
az lock delete --name lock-cie-p04 --resource-group rg-cie-p04-iam --only-show-errors && echo "--- LOCKS LEFT ---" && az lock list --resource-group rg-cie-p04-iam --query "length(@)" -o tsv && sleep 20 && az network public-ip delete --name pip-notag-test --resource-group rg-cie-p04-iam --only-show-errors 2>&1 | cut -c1-150 | head -3; echo "--- RESOURCES ---" && az resource list --resource-group rg-cie-p04-iam --query "[].{name:name, type:type}" -o table
```

### Step 18 — Policy assignment, custom role, role assignments &nbsp; 📝

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibits 15 and 18** show the end state | this command ran after the resource group was already deleted |

```bash
SUB=$(az account show --query id -o tsv) && G="<GROUP_OBJECT_ID>" && az policy assignment delete --name pol-cie-p04-tag --resource-group rg-cie-p04-iam --only-show-errors && az role assignment delete --assignee "$G" --role "CIE P04 VM Operator" --resource-group rg-cie-p04-iam --only-show-errors && az role assignment delete --assignee "$G" --role "Reader" --resource-group rg-cie-p04-iam --only-show-errors && az role definition delete --name "CIE P04 VM Operator" --scope "/subscriptions/$SUB/resourceGroups/rg-cie-p04-iam" --only-show-errors
```

The resource group was already gone, so the first call returned `ResourceGroupNotFound` and the rest of the chain did not run. Those objects were scoped inside the resource group.

### Step 19 — Delete the resource group, the test user and the group &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 15** | `rg-cie-p04-iam` gone, four older groups remain |
| 🌐 Portal | **Exhibits 16 and 17** | 1 user, 0 groups |

```bash
az group delete --name rg-cie-p04-iam --yes --only-show-errors && az group list --query "[].name" -o tsv
```

- 🌐 `Microsoft Entra ID`, `Users`, tick `CIE P04 Test User`, `Delete`, confirm.
- 🌐 `Groups`, `All groups`, tick `cie-p04-readers`, `Delete`, confirm.

### Step 20 — Purge the Key Vault, final proof &nbsp; ✅

| 📍 Where | 🖼️ Proof | Result |
|:---:|:---:|---|
| ☁️ Cloud Shell | **Exhibit 18** | vault listed, then count `0`, name free `true`, custom role `0` |

```bash
az keyvault list-deleted --query "[].name" -o tsv && az keyvault purge --name kv-cie-p04-malaika --only-show-errors && sleep 20 && echo "--- AFTER PURGE ---" && az keyvault list-deleted --query "length(@)" -o tsv && az keyvault check-name --name kv-cie-p04-malaika --query nameAvailable -o tsv && echo "--- CUSTOM ROLE ---" && az role definition list --custom-role-only true --query "[?roleName=='CIE P04 VM Operator'] | length(@)" -o tsv
```

Full cleanup notes: [`cleanup.md`](cleanup.md).

---

## 🚫 Not Run in This Project

| Item | Note |
|---|---|
| Commands as the test user with the custom role | The test user never tried a VM restart or a delete, so the custom role is not exercised |
| Users and groups from the CLI | Cloud Shell could not get a Microsoft Graph token, so the portal was used |
| Conditional Access, Privileged Identity Management, access reviews | Not part of Entra ID Free |

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 README](../README.md) &nbsp;·&nbsp; [📑 Visual Index](../Entra-RBAC-INDEX.md) &nbsp;·&nbsp; [🧹 Cleanup](cleanup.md)

☁️ **[Microsoft Azure](https://azure.microsoft.com)** · 🔐 **[Azure RBAC documentation](https://learn.microsoft.com/azure/role-based-access-control/)**

</div>
