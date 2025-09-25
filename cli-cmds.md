# Azure CLI commands used in this exercise
### Login to azure portal
`az login`

### Create a resource group
`az group create --name <your_RG_name> --location <azure_region_name>`

### Create VM with your public key
```
az vm create `
  --resource-group <your_RG_name> `
  --name <your_VM_name> `
  --image Ubuntu2204 `
  --size <your_VM_size such as Standard_B1s> `
  --admin-username <your_azureuser> `
  --generate-ssh-keys 
```

### Check all sizes in your region that can be requested:
`az vm list-sizes --location <your_azure_region_name> --output table`
Some regions have multiple availability zones. You can pick one to avoid capacity issues, e.g. `-- zone 2`

### Verify VM got created
`az vm list --resource-group <your_RG_name> --output table`


### Open port for web traffic to your e-Commerce App
```
az vm open-port `
  --resource-group <your_RG_name> `
  --name <your_VM_name> `
  --port 80 `
  --nsg-name <VM_NSG_name>
```

### Identify your VM's IP
`az vm list-ip-addresses --resource-group <your_RG_name> --name <your_VM_name> --output table`

******************************************************
## Connect to the VM via SSH
From your local machine's shell:
```ssh -i ~/.ssh/<vm_key.pem> <VM_user_name>@<VM_PUBLIC_IP>``` OR

**If ssh connection doesn't work, check:**

```
ping <VM_PUBLIC_IP>
telnet <VM_PUBLIC_IP> 22
ssh -v -i ~/.ssh/<vm_key.pem> <VM_user_name>@<VM_PUBLIC_IP>
```

### Inside the VM, install Docker
```
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker
sudo usermod -aG docker $USER && newgrp docker
```
**OR**
### Clean Install Docker using Docker's official script
```
sudo apt-get remove -y docker docker-engine docker.io containerd runc || true \
curl -fsSL https://get.docker.com -o get-docker.sh \
sudo sh get-docker.sh \
sudo usermod -aG docker $USER \
sudo systemctl enable docker \
sudo systemctl start docker \
docker --version
```