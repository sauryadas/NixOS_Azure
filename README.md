# NixOS_Azure
Deploying NixOS on Azure

## Downloading the latest NixOS image 
Use this [link](https://nixos.blob.core.windows.net/images/nixos-unstable-nixops.vhd) to download the lates NixOS image to your local file system

### Login 
  ```$ azure login```

### Create a resource group
  ```$ azure group create nixResGrp "West US”```

### Create a storage account, set primary key, create a container and upload the blob
```
$ azure storage account create bg09p --resource-group nixResGrp --location "West US" --sku-name “GRS"
$ azure storage account keys list bg09p
$ export KEY={Primary Key from previous step}
$ echo $KEY
$ azure storage container create -a bg09p -k $KEY vm-images
$ azure storage blob upload -t page -a bg09p -k $KEY --container vm-images nixos-unstable-standalone.vhd
```

### Create a VM 
```
$ azure vm create nixrg nixVm001 westus Linux —storage-account-name bg09p -d https://bg09p.blob.core.windows.net/vm-images/root.vhd --image-urn https://bg09p.blob.core.windows.net/vm-images/nixos-nstable-standalone.vhd --nic-name nixNic1 --vnet-name nix-vnet1 --vnet-address-prefix 10.0.0.1/16 --vnet-subnet-name nix-subnet1 --vnet-subnet-address-prefix 10.0.1.1/24 --public-ip-name nix-public-ip --public-ip-domain-name nix-public-ip-domain --admin-username azureuser
```
