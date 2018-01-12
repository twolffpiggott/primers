# Infrastructure Design

## Net
* virtual network
* subnets
  * mgmt
  * production
* dns zone
* security groups
  * http
  * https
  * ssh

## Compute
* virtual_machine
* disks
* network adapter
* dns record
* cloud init script (os_profile.custom_data)
  * hostname
  * salt bootstrap
* ssh keys
* public IP

# Working with Azure CLI 2.0

## Install Azure CLI 2.0 on Ubuntu:
1. Modify sources list for a 64-bit system
```shell
echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ wheezy main" | \
     sudo tee /etc/apt/sources.list.d/azure-cli.list
```
2. Run the following sudo commands:
```shell
sudo apt-key adv --keyserver packages.microsoft.com --recv-keys 52E16F86FEE04B979B07E28DB02C46DF417A0893
sudo apt-get install apt-transport-https
sudo apt-get update && sudo apt-get install azure-cli
```
3. Run the CLI from terminal with the `az` command.

## Create a resource group:
Create a resource group named "MyResourceGroup" in the westus2 region of Azure.
```shell
az group create -n MyResourceGroup -l westus2
```
The command outputs several properties of the newly created resource
```json
{
  "id": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/MyResourceGroup",
  "location": "westus2",
  "managedBy": null,
  "name": "MyResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## Create a Linux virtual machine within the resource group
You can create a Linux VM using the popular UbuntuLTS image, with two attached storage disks of 10 GB and 20 GB, with the following command:
```shell
az vm create -n MyLinuxVM -g MyResourceGroup --image UbuntuLTS --data-disk-sizes-gb 10 20
```
When you run the preceding command, the Azure CLI 2.0 looks for an SSH key pair stored under your ~/.ssh directory. If you don't already have an SSH key pair stored there, you can ask the Azure CLI to automatically create one for you by passing the --generate-ssh-keys parameter:
```shell
az vm create -n MyLinuxVM -g MyResourceGroup --image UbuntuLTS --data-disk-sizes-gb 10 20 --generate-ssh-keys
```
The `az vm create` command returns output once the VM has been fully created and is ready to be accessed and used. The output includes several properties of the newly created VM including its public IP address:
```json
{
  "fqdns": "",
  "id": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyLinuxVM",
  "location": "westus2",
  "macAddress": "xx-xx-xx-xx-xx-xx",
  "powerState": "VM running",
  "privateIpAddress": "xx.x.x.x",
  "publicIpAddress": "xx.xxx.xxx.xx",
  "resourceGroup": "MyResourceGroup"
}
```

## Log in to the Linux virtual machine
Now that the VM has been created, you can log on to your new Linux VM using **SSH** with the public IP address of the VM you created:
```shell
ssh xx.xxx.xxx.xxx
```

# Create a complete Linux virtual machine 

## Create resource group
An Azure resource group is a logical container into which Azure resources are deployed and managed. A resource group must be created before a virtual machine and supporting virtual network resources. Create the resource group with az group create. The following example creates a resource group named myResourceGroup in the eastus location:
```shell
az group create --name myResourceGroup --location eastus
```
By default, the output of Azure CLI commands is in JSON (JavaScript Object Notation). To change the default output to a list or table, for example, use az configure --output. You can also add `--output` to any command for a one time change in output format. The following example shows the JSON output from the `az group create` command:
```json
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "eastus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```
