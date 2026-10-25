# Project 03 — Commands

All commands were run in **Cloud Shell (Bash)** unless the heading says another place. Placeholders in angle brackets (`<...>`) replace values that are not kept in the repo: IP addresses, the public key text and a version ID.

📝 = ran, but no screenshot of it is in the repo. The exhibit named next to it proves the result.

---

## Setup

**1. Quota check** — Exhibit 1

```bash
az vm list-usage --location indiasouthcentral --query "[?name.value=='cores'].{Used:currentValue, Limit:limit}" -o table
```

**2. Pre-checks** 📝 (exhibit 18 proves the VM size)

```bash
az storage account check-name --name ciep03malaika01 -o json
az provider show --namespace Microsoft.Storage --query registrationState -o tsv
az vm list-skus --location indiasouthcentral --size Standard_B2ls_v2 --query "[].{Name:name, Restriction:restrictions[0].reasonCode}" -o table
```

**3. Resource group** — Exhibit 2

```bash
az group create --name rg-cie-p03-storage --location indiasouthcentral --tags project=cie-p03 owner=malaika env=lab
```

---

## Module 1 — Blob Storage

**4. Storage account** — Exhibit 3 (the `show` command below; the create output was not screenshotted 📝)

```bash
az storage account create --name ciep03malaika01 --resource-group rg-cie-p03-storage --location indiasouthcentral --sku Standard_LRS --kind StorageV2 --access-tier Hot --min-tls-version TLS1_2 --allow-blob-public-access false --tags project=cie-p03 owner=malaika env=lab

az storage account show -g rg-cie-p03-storage -n ciep03malaika01 --query "{Name:name, Sku:sku.name, State:provisioningState, Tier:accessTier, Tls:minimumTlsVersion, PublicBlob:allowBlobPublicAccess, Tags:tags}" -o json
```

**5. Container** — Exhibit 4

```bash
az storage container create --account-name ciep03malaika01 --name labdata --public-access off --auth-mode key -o json
```

**6. Upload** — Exhibit 5

```bash
echo "hello from cie p03" > test1.txt && az storage blob upload --account-name ciep03malaika01 --container-name labdata --name test1.txt --file test1.txt --auth-mode key --only-show-errors -o json
```

**7. Download** — Exhibit 6

```bash
az storage blob download --account-name ciep03malaika01 --container-name labdata --name test1.txt --file test1-down.txt --auth-mode key --only-show-errors -o none && cat test1-down.txt
```

**8. Access tier** — Exhibit 7

```bash
az storage blob set-tier --account-name ciep03malaika01 --container-name labdata --name test1.txt --tier Cool --auth-mode key --only-show-errors && az storage blob show --account-name ciep03malaika01 --container-name labdata --name test1.txt --auth-mode key --only-show-errors --query "{Name:name, Tier:properties.blobTier}" -o json
```

**9. Lifecycle rule** — Exhibit 8

```bash
az storage account management-policy create --account-name ciep03malaika01 -g rg-cie-p03-storage --policy '{"rules":[{"enabled":true,"name":"cool-then-delete","type":"Lifecycle","definition":{"filters":{"blobTypes":["blockBlob"],"prefixMatch":["labdata/"]},"actions":{"baseBlob":{"tierToCool":{"daysAfterModificationGreaterThan":30},"delete":{"daysAfterModificationGreaterThan":365}}}}}]}' --query "policy.rules[0].{Name:name, Enabled:enabled, CoolAfterDays:definition.actions.baseBlob.tierToCool.daysAfterModificationGreaterThan, DeleteAfterDays:definition.actions.baseBlob.delete.daysAfterModificationGreaterThan}" -o json
```

---

## Module 2 — Data Protection

**10. Soft delete and versioning** — Exhibit 9

```bash
az storage account blob-service-properties update --account-name ciep03malaika01 -g rg-cie-p03-storage --enable-delete-retention true --delete-retention-days 7 --enable-versioning true --query "{SoftDelete:deleteRetentionPolicy, Versioning:isVersioningEnabled}" -o json
```

**11. Overwrite and list versions** — Exhibit 10

```bash
echo "version two" > test1.txt && az storage blob upload --account-name ciep03malaika01 --container-name labdata --name test1.txt --file test1.txt --overwrite true --auth-mode key --only-show-errors -o none && az storage blob list --account-name ciep03malaika01 --container-name labdata --include v --auth-mode key --only-show-errors --query "[].{Name:name, VersionId:versionId, Current:isCurrentVersion}" -o table
```

**12. Delete the blob** 📝 (Exhibit 11 shows the result)

```bash
az storage blob delete --account-name ciep03malaika01 --container-name labdata --name test1.txt --auth-mode key --only-show-errors
```

I also listed the blobs with `--include dv` after the delete 📝. The two versions were still listed.

**12b. Empty list and recover the old version** — Exhibit 11 (`<OLD_VERSION_ID>` is the older version time from Exhibit 10)

```bash
az storage blob list --account-name ciep03malaika01 --container-name labdata --auth-mode key --only-show-errors -o json && az storage blob download --account-name ciep03malaika01 --container-name labdata --name test1.txt --version-id <OLD_VERSION_ID> --file recovered.txt --auth-mode key --only-show-errors -o none && cat recovered.txt
```

---

## Module 3 — Access Control

**13. No credentials** — Exhibit 12

```bash
echo "sas test file" > sas-test.txt && az storage blob upload --account-name ciep03malaika01 --container-name labdata --name sas-test.txt --file sas-test.txt --auth-mode key --only-show-errors -o none && curl -s -o /dev/null -w "HTTP code: %{http_code}\n" https://ciep03malaika01.blob.core.windows.net/labdata/sas-test.txt
```

**14. SAS token, 3 minutes, read only** — Exhibit 13

```bash
SAS=$(az storage blob generate-sas --account-name ciep03malaika01 --container-name labdata --name sas-test.txt --permissions r --expiry $(date -u -d "+3 minutes" '+%Y-%m-%dT%H:%MZ') --https-only --auth-mode key -o tsv) && curl -s -w "\nHTTP code: %{http_code}\n" "https://ciep03malaika01.blob.core.windows.net/labdata/sas-test.txt?$SAS"
```

The same Cloud Shell session must stay open, because `$SAS` only exists in that session.

**15a. Re-test before expiry** 📝 (returned 200, token still valid)

```bash
curl -s -w "\nHTTP code: %{http_code}\n" "https://ciep03malaika01.blob.core.windows.net/labdata/sas-test.txt?$SAS"
```

**15b. Compare the expiry time with the clock** 📝 (prints only the `se=` part, not the signature)

```bash
echo "Token expiry: $(echo "$SAS" | grep -o 'se=[^&]*')"; echo "Now (UTC):   $(date -u '+%Y-%m-%dT%H:%MZ')"
```

**15c. Re-test after expiry** — Exhibit 14

```bash
date -u '+Now (UTC): %Y-%m-%dT%H:%MZ'; curl -s -w "\nHTTP code: %{http_code}\n" "https://ciep03malaika01.blob.core.windows.net/labdata/sas-test.txt?$SAS"
```

**16. Firewall default Deny** — Exhibit 15

```bash
az storage account update -n ciep03malaika01 -g rg-cie-p03-storage --default-action Deny --query "{DefaultAction:networkRuleSet.defaultAction, IpRules:networkRuleSet.ipRules}" -o json
```

**17a. First blocked test, with a `grep` filter** 📝 (printed nothing, so it proved nothing)

```bash
sleep 30; az storage blob list --account-name ciep03malaika01 --container-name labdata --auth-mode key --only-show-errors -o table 2>&1 | grep -o "Code: [A-Za-z]*\|ErrorCode:[A-Za-z]*\|AuthorizationFailure\|not authorized[^.]*" | head -3
```

**17b. Blocked test without the filter** — Exhibit 16

```bash
az storage blob list --account-name ciep03malaika01 --container-name labdata --auth-mode key --only-show-errors -o table 2>&1 | cut -c1-150 | head -8
```

**18. Firewall back to Allow** — Exhibit 17

```bash
az storage account update -n ciep03malaika01 -g rg-cie-p03-storage --default-action Allow --query "networkRuleSet.defaultAction" -o tsv && sleep 30 && az storage blob list --account-name ciep03malaika01 --container-name labdata --auth-mode key --only-show-errors --query "[].name" -o tsv
```

---

## Module 4 — Azure Files

**19a. Quota re-check** 📝 (still 2 used of 6)

```bash
az vm list-usage --location indiasouthcentral --query "[?name.value=='cores'].{Used:currentValue, Limit:limit}" -o table
```

**19b. Public key to clipboard** — Laptop PowerShell. Nothing prints. The key text was pasted into the chat as text, not read from a screenshot.

```powershell
Get-Content "$HOME\.ssh\cie_p02_key.pub" | Set-Clipboard
```

**19c. Create the VM** — Exhibit 18

```bash
az vm create -g rg-cie-p03-storage -n vm-files --image Ubuntu2204 --size Standard_B2ls_v2 --admin-username azureuser --authentication-type ssh --ssh-key-values "<PUBLIC_KEY_TEXT>" --nsg-rule NONE --public-ip-sku Standard --tags project=cie-p03 owner=malaika env=lab --query "{PowerState:powerState, ResourceGroup:resourceGroup, PrivateIp:privateIpAddress}" -o json
```

**20a. NSG before the rule** 📝 (one NSG, `vm-filesNSG`, 0 rules)

```bash
az network nsg list -g rg-cie-p03-storage --query "[].{Name:name, Rules:length(securityRules)}" -o table
```

**20b. My current public IP** — Laptop PowerShell. Copied to the clipboard and given as text. Not saved anywhere.

```powershell
(Invoke-RestMethod https://api.ipify.org) | Set-Clipboard
```

**20c. Allow SSH from that one IP** — Exhibit 19

```bash
az network nsg rule create -g rg-cie-p03-storage --nsg-name vm-filesNSG -n allow-ssh-myip --priority 100 --direction Inbound --access Allow --protocol Tcp --destination-port-ranges 22 --source-address-prefixes <MY_PUBLIC_IP> --query "{Name:name, Access:access, Port:destinationPortRange, Source:sourceAddressPrefix}" -o json
```

**21a. VM public IP** — printed in Cloud Shell and copied by hand. Not kept in the repo.

```bash
az vm show -d -g rg-cie-p03-storage -n vm-files --query publicIps -o tsv
```

**21b. SSH to the VM** — Laptop PowerShell, Exhibit 20

```powershell
ssh -i "$HOME\.ssh\cie_p02_key" azureuser@<VM_PUBLIC_IP>
```

**21c. Install SMB tools** — SSH session on `vm-files` 📝 (showed `ii cifs-utils`)

```bash
sudo apt-get update -qq && sudo apt-get install -y -qq cifs-utils && dpkg -l cifs-utils | tail -1
```

**22. File share** — Exhibit 21

```bash
az storage share-rm create --storage-account ciep03malaika01 -g rg-cie-p03-storage --name labshare --quota 5 --enabled-protocols SMB --query "{Name:name, QuotaGiB:shareQuota, Protocols:enabledProtocols}" -o json
```

**23. Port 445 test** — SSH session on `vm-files`, Exhibit 22

```bash
timeout 5 bash -c '</dev/tcp/ciep03malaika01.file.core.windows.net/445' && echo "Port 445: OPEN" || echo "Port 445: CLOSED or timed out"
```

**24. Renew the key, mount the share and write a file** — Cloud Shell, Exhibit 23. The key stays in `$K` and is not printed.

```bash
K=$(az storage account keys renew -g rg-cie-p03-storage -n ciep03malaika01 --key key1 --query "[?keyName=='key1'].value | [0]" -o tsv) && az vm run-command invoke -g rg-cie-p03-storage -n vm-files --command-id RunShellScript --scripts "hostname; mkdir -p /mnt/labshare; mount -t cifs //ciep03malaika01.file.core.windows.net/labshare /mnt/labshare -o username=ciep03malaika01,password='$K',serverino,vers=3.1.1; echo 'hello from vm-files' > /mnt/labshare/from-vm.txt; ls -l /mnt/labshare; df -hT /mnt/labshare" --query "value[0].message" -o tsv
```

**25a. First read-back attempt** 📝 (failed: `--dest` was a file name, the tool treated it as a folder)

```bash
az storage file download --account-name ciep03malaika01 --share-name labshare --path from-vm.txt --dest from-vm-down.txt --auth-mode key --only-show-errors -o none
```

**25b. Read-back that worked** — Exhibit 24

```bash
az storage file list --account-name ciep03malaika01 --share-name labshare --auth-mode key --only-show-errors --query "[].{Name:name, Size:properties.contentLength}" -o table && az storage file download --account-name ciep03malaika01 --share-name labshare --path from-vm.txt --dest . --auth-mode key --only-show-errors -o none && cat from-vm.txt
```

---

## Not run in this project

- Key rotation was done once by hand and once inside step 24. The first time is not in the repo, because the account key was visible in the output 📝.
- No `/etc/fstab` entry was added for the mount.
