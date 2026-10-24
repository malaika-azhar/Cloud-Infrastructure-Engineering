# Commands: Project 01

Every command used in this lab, in the order it was run. Each block says **where** it is run, because the three places are not interchangeable:

| Label | Where | Prompt looks like |
|---|---|---|
| **Cloud Shell** | Azure Portal, Cloud Shell, Bash | `malaika [ ~ ]$` |
| **Laptop PowerShell** | Windows PowerShell on the laptop | `PS C:\Users\RM>` |
| **SSH session (VM)** | Inside the VM after logging in | `azureuser@vm-cie-p01:~$` |

`<public-ip>` stands for the VM's public IP address, which is blurred in the screenshots.
Commands marked 📝 are not visible in a kept screenshot; the result is (see the README exhibit named in each note).

---

## 1. Pre-deployment checks

**Run in: Cloud Shell** (Exhibits 1 and 2)

```bash
az account show --output table
az vm list-skus --location indiasouthcentral --size Standard_B2 --output table
```

## 2. Resource group

**Run in: Cloud Shell** (Exhibit 3)

```bash
az group create --name rg-cie-p01-vm --location indiasouthcentral --tags project=cie-p01 owner=malaika env=lab
az group show --name rg-cie-p01-vm --output json
```

## 3. SSH key pair

**Run in: Laptop PowerShell** (Exhibit 4)

```powershell
New-Item -ItemType Directory -Force $HOME\.ssh
ssh-keygen -t rsa -b 4096 -f $HOME\.ssh\cie_p01_key2 -C "cie-p01" -N '""'
Get-Content $HOME\.ssh\cie_p01_key2.pub
Get-Content $HOME\.ssh\cie_p01_key2.pub | Set-Clipboard
```

Only the `.pub` file is pasted into Azure. The private key (`cie_p01_key2`, no extension) stays on the laptop.

## 4. Create the VM

**Run in: Azure Portal, Virtual machines, Create** (Exhibits 5 to 8). This step is done in the portal, not with a command. The settings are listed in the README.

## 5. Log in and verify

**Run in: Laptop PowerShell** (Exhibit 9)

```powershell
ssh -i $HOME\.ssh\cie_p01_key2 azureuser@<public-ip>
```

**Run in: SSH session (VM)** (Exhibits 9 to 12)

```bash
whoami
hostnamectl | cat
ip -br addr
systemctl status ssh --no-pager
sudo ss -tulpn
sudo journalctl -u ssh --no-pager | grep "Accepted publickey"
```

## 6. Tag every resource

📝 **Run in: Cloud Shell** (result in Exhibit 13)

```bash
for id in $(az resource list -g rg-cie-p01-vm --query "[].id" -o tsv); do
  az resource tag --tags project=cie-p01 owner=malaika env=lab --ids $id --is-incremental
done

az resource list -g rg-cie-p01-vm \
  --query "[].{Name:name, Project:tags.project, Owner:tags.owner, Env:tags.env}" -o table
```

## 7. Data disk

📝 **Run in: Cloud Shell** (result in Exhibit 14)

```bash
az vm disk attach --resource-group rg-cie-p01-vm --vm-name vm-cie-p01 \
  --name disk-cie-p01-data --new --size-gb 4 --sku StandardSSD_LRS --lun 0
```

**Run in: Cloud Shell** (Exhibit 14)

```bash
az vm show -g rg-cie-p01-vm -n vm-cie-p01 \
  --query "storageProfile.dataDisks[].{Name:name, SizeGB:diskSizeGb, LUN:lun, Type:managedDisk.storageAccountType}" -o table
```

**Run in: SSH session (VM)** (Exhibits 15 to 19)

```bash
lsblk
sudo parted /dev/sdb --script mklabel gpt mkpart primary ext4 0% 100%
lsblk /dev/sdb
sudo mkfs.ext4 /dev/sdb1
lsblk -f /dev/sdb
sudo mkdir /datadisk
sudo mount /dev/sdb1 /datadisk
df -h /datadisk
echo 'UUID=f87cbd7d-6c45-43d7-a596-91e590290f04 /datadisk ext4 defaults,nofail 0 2' | sudo tee -a /etc/fstab
sudo umount /datadisk
sudo mount -a
findmnt /datadisk
sudo systemctl daemon-reload
echo "cie-p01 data disk test" | sudo tee /datadisk/test.txt
cat /datadisk/test.txt
ls -l /datadisk
```

Check the device name with `lsblk` before running `parted` or `mkfs.ext4`. Pointing them at the OS disk would destroy the VM.
The UUID above belongs to this lab's disk. A new disk gets a different UUID; read it from `lsblk -f`.

## 8. Reboot test

**Run in: SSH session (VM)** (Exhibits 20 and 21)

```bash
sudo reboot
```

After reconnecting from the laptop:

```bash
uptime -p
findmnt /datadisk
cat /datadisk/test.txt
lsblk -f
findmnt /
```

## 9. Resize

**Run in: Cloud Shell** (Exhibit 22)

```bash
az vm list-vm-resize-options -g rg-cie-p01-vm -n vm-cie-p01 \
  --query "[?contains(name, 'Standard_B')].{Name:name, vCPU:numberOfCores}" -o table
```

**Run in: SSH session (VM)** (Exhibit 23)

```bash
nproc
free -h
```

📝 **Run in: Cloud Shell** (result in Exhibit 24)

```bash
az vm resize -g rg-cie-p01-vm -n vm-cie-p01 --size Standard_B2s_v2
```

**Run in: Cloud Shell** (Exhibit 24)

```bash
az vm list -d -g rg-cie-p01-vm \
  --query "[].{Name:name, Size:hardwareProfile.vmSize, State:powerState, IP:publicIps}" -o table
```

**Run in: SSH session (VM)** after reconnecting (Exhibit 25)

```bash
uptime -p
nproc
free -h
findmnt /datadisk
```

## 10. Stop, deallocate, start

📝 **Run in: Cloud Shell** (results in Exhibits 26 to 28)

```bash
az vm stop -g rg-cie-p01-vm -n vm-cie-p01
az vm deallocate -g rg-cie-p01-vm -n vm-cie-p01
az vm start -g rg-cie-p01-vm -n vm-cie-p01
```

**Run in: Cloud Shell** after each command (Exhibits 26 to 28)

```bash
az vm list -d -g rg-cie-p01-vm \
  --query "[].{Name:name, Size:hardwareProfile.vmSize, State:powerState, IP:publicIps}" -o table
```

**Run in: SSH session (VM)** after the start (Exhibit 29)

```bash
uptime -p
findmnt /datadisk
cat /datadisk/test.txt
```

## 11. Boot diagnostics

**Run in: Azure Portal, `vm-cie-p01`, Help, Boot diagnostics** (Exhibit 30). Portal step, no command.

## 12. Cleanup

See [`cleanup.md`](cleanup.md).
