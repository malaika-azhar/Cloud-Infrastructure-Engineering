# Project 03 — Cleanup

Cleanup was the last step. Only this project's resource group was deleted. The older groups were not touched.

## Command (Cloud Shell, Bash)

```bash
az group delete --name rg-cie-p03-storage --yes && az group list --query "[].name" -o tsv
```

## What this removed

Everything inside `rg-cie-p03-storage`:

- Storage account `ciep03malaika01` (the blob container `labdata`, the file share `labshare`, the lifecycle rule, and all blobs and versions)
- VM `vm-files` with its disk, network card, public IP and network security group `vm-filesNSG`

The VM and the file share were still in use when the group was deleted. The shared folder was not unmounted first, because the VM was deleted too.

## What proves it

Exhibit 25 (`Screenshots/ss-25-cleanup-proof.PNG`):

- `rg-cie-p03-storage` is **not** in the list
- The four older groups are still in the list: `ForensicsCapstone-RG`, `osticket-rg`, `NetworkWatcherRG`, `sysadmin-labs`

## Not checked

- Whether the storage account name `ciep03malaika01` can be reused right away. I did not try to create it again.
- The vCPU quota after the delete was not checked again, so I make no claim about it.
