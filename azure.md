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
