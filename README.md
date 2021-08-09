# openservicemesh

# Specify the OSM version that will be leveraged throughout these instructions
OSM_VERSION=v0.8.4

curl -sL "https://github.com/openservicemesh/osm/releases/download/$OSM_VERSION/osm-$OSM_VERSION-linux-amd64.tar.gz" | tar -vxzf -

#copy the osm client binary to the standard user program location in your PATH.
sudo mv ./linux-amd64/osm /usr/local/bin/osm
sudo chmod +x /usr/local/bin/osm

#verify the osm client library has been correctly added to your path and its version number with the following command.
osm version

#Register the AKS-OpenServiceMesh preview feature
az login
az feature register --namespace "Microsoft.ContainerService" --name "AKS-OpenServiceMesh"

#Verify registration
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-OpenServiceMesh')].{Name:name,State:properties.state}"

#When ready , refresh
az provider register --namespace Microsoft.ContainerService


# Please provide your subscription id here
export APP_SUBSCRIPTION_ID=c2483929-bdde-40b3-992e-66dd68f52928
# Please provide your unique prefix to make sure that your resources are unique


# Please provide your subscription id here
export APP_SUBSCRIPTION_ID=c2483929-bdde-40b3-992e-66dd68f52928
# Please provide your unique prefix to make sure that your resources are unique
export APP_PREFIX=preastus2
# Please provide your region
export LOCATION=EastUS2
export REGISTRY_LOCATION=EastUS2

export VNET_PREFIX="192.168."

export AKS_PE_DEMO_RG=$APP_PREFIX"-aksdemo-rg"
export AKS_PRIVATE_CLUSTER=$APP_PREFIX"-aksdemo-aks"
export ADO_PE_DEMO_RG=$APP_PREFIX"-adodemo-rg"
export DEMO_VNET=$APP_PREFIX"-aksdemo-vnet"
export DEMO_VNET_CIDR=$VNET_PREFIX"0.0/16"
export DEMO_VNET_APP_SUBNET=app_subnet
export DEMO_VNET_APP_SUBNET_CIDR=$VNET_PREFIX"1.0/24"
export AKS_PE_SUBNET=aks_pe_subnet
export AKS_PE_SUBNET_CIDR="10.0.1.0/24"

#Upgrade az command
az ugprade 
#Enable the OSM add-on to existing AKS cluster
az extension add --name aks-preview
az aks enable-addons --addons open-service-mesh -g AKS_PE_DEMO_RG -n $APP_PREFIX"-aksdemo-aks

#Install jq on wsl for ubuntu 
sudo apt install jq
az aks list -g $AKS_PE_DEMO_RG-o json | jq -r '.[].addonProfiles.openServiceMesh.enabled'

#Status of OSM controller
kubectl get deployments -n kube-system --selector app=osm-controller
kubectl get pods -n kube-system --selector app=osm-controller
kubectl get services -n kube-system --selector app=osm-controller



