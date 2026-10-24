# Cleanup: Project 01

All resources for this lab are in one resource group, `rg-cie-p01-vm`. Deleting the group removes all of them.

## Resources removed

- Virtual machine `vm-cie-p01`
- OS disk and data disk `disk-cie-p01-data`
- Network interface `vm-cie-p01223`, public IP `vm-cie-p01-ip` and NSG `vm-cie-p01-nsg`
- Virtual network `vnet-indiasouthcentral-3`
- VMAccess extension `enablevmAccess`

The list comes from the tag table in the README (Exhibit 13).

## Command

**Run in: Cloud Shell (Bash)**

```bash
az group delete --name rg-cie-p01-vm --yes --no-wait
```

## Verification

After a few minutes, **run in: Cloud Shell (Bash)**

```bash
az group exists --name rg-cie-p01-vm
```

The output `false` confirms the resource group no longer exists.

## Note

Only `rg-cie-p01-vm` is deleted. Resource groups from earlier labs are not touched.
