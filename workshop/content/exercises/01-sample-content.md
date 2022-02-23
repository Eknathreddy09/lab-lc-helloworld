################ Still in progress, but you can still try #######################
Author: Eknath Reddy

In this workshop, TKG version used is v1.4.1
For detailed of Installation procedure refer to official vmware doc. We have already installed the required CLI tools for this workshop. 

```dashboard:open-url
url: https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.4/vmware-tanzu-kubernetes-grid-14/GUID-index.html
```

#### Click text to Hello world (with target)

```execute-1
echo "Hello, Welcome to Partner workshop session"
```

#### Click here to check Tanzu version

```execute
tanzu version
```

#### Click here to check TMC CLI version

```execute
tmc version
```

#### Click here to check terraform CLI version

```execute
terraform --version
```

#### Click here to check AZ CLI version

```execute
az --version
```

#### Export session name as env variable

```execute-all
export SESSION_NAME={{ session_namespace }}
```

#### Set up the environment

```execute-1
source ~/script-session.sh
```

# Preparing your setup
##### please wait for few mins. Continue further once you see the public ip on screen (Terminal-1)

#### Connect to deploy a Management cluster

```execute-2
ssh -i id_rsa azureuser@<ipfromterminal1> -o StrictHostKeyChecking=accept-new
```
Example:

![connect to JB](images/TKG-41.png)

###### Replace "ipfromterminal1" with IP shown in Terminal 1

#### Deploy management cluster

```execute-2
tanzu management-cluster create --ui --bind 0.0.0.0:8080
```

#### Replace ipcollectedfromterminal1 with IP shown in Terminal 1 and select Azure in Installer page

```dashboard:open-url
url: http://ipcollectedfromterminal1:8080
```

Example: 

![Installer](images/TKG-42.png)
    
##### Azure details for management cluster creation can be found by executing this command: 

```execute
cat /home/eduk8s/creds-tkg
```
    
#### Copy and paste the values in Installer page opened in browser

### Fill Iaas provider details as shown in creds-tkg file: 

Resource Group: Create a new resource group and provide below name: 
##### {{ session_namespace }}-RG
    
Azure VNET Settings: 
    
Create a new VNET on Azure > from drop down select the newly created RG:
    
##### {{ session_namespace }}-RG
    
Provide VNET name as:
#### {{ session_namespace }}-vnet
    
Control Plane subnet name : 
#### {{ session_namespace }}-cp
    
Worker node subnet name: 
#### {{ session_namespace }}-worker 
    
Management Cluster Settings: 
  Select Development 
#### Instance Type: Standard_D2s_v3
    
Management Cluster Name: 
####    {{ session_namespace }}-mgmt
    
#### Worker Node Instance Type: Standard_D2s_v3
    
###### Metadata: Leave to default and click NEXT
    
###### Kubernetes Network: Leave to default and click NEXT
    
###### Identity Management: disable "Enable Identity Management Settings" and click NEXT
    
###### OS Image: from dropdown select Ubuntu-20.04
    
###### CEIP Agreement: Leave to default and click NEXT
    
###### Review Configuration and click on Deploy management cluster
    
## Cluster creation takes about 15 mins

#### Please wait till the management cluster is created ####
#### Proceed further only once you see this on screen (terminal-2)

![Management Cluster](images/TKG-1.png)

##### This script copies the config file into Terminal

```execute
/bin/sh /home/eduk8s/script-session-tmc.sh
```

##### Click to check all contexts in management cluster
    
```execute-2
kubectl config get-contexts
```

##### Get Management cluster info, you can see both terminals has same output
    
```execute-all
tanzu mc get
```

##### Click to check the Nodes

```execute-all
kubectl get nodes
```

##### Click to check the pods in all namespaces of management cluster

```execute-all
kubectl get pods -A 
```

##### Read the config file to understand the variables defined for Tanzu Kubernetes cluster which will be deployed shortly

```execute-1
cat /home/eduk8s/wc-config.yaml
```

#### Click here to deploy workload cluster

```execute-1
tanzu cluster create {{ session_namespace }} -f /home/eduk8s/wc-config.yaml
```

Meanwhile you can check the cluster creation logs in Terminal 2
##### Click here to check the progress of workload creation from logs

```execute-2
podname=$(kubectl get pods -n capz-system -o=jsonpath={.items[0].metadata.name})
kubectl logs $podname -n capz-system -c manager -f
```
###########################################################
#### Wait for the cluster to get created ##################
###########################################################

##### Proceed further only once you see this on screen (terminal-1)
![Workload Cluster](images/TKG-2.png)

#### Click here to check the deployed workload clusters 
```execute-1
tanzu cluster list
```
    
#### Execute to terminate the Jumpbox
```execute-1
az group delete -n {{ session_namespace }}-JB --yes
```
###############
#### Wait for delete operation to complete
###############

##### Get credentials and export the config file
```execute
tanzu cluster kubeconfig get {{ session_namespace }} --admin --export-file ~/.kube/config-tkg
```
##### Read the kubeconfig file
```execute
cat  ~/.kube/config-tkg
```
##### Click here to change the context from management cluster to workload
```execute
kubectl config use-context {{ session_namespace }}-admin@{{ session_namespace }} --kubeconfig /home/eduk8s/.kube/config-tkg
```
##### Verify the context
```execute
kubectl config get-contexts --kubeconfig /home/eduk8s/.kube/config-tkg
```
##### Check the nodes in workload cluster
```execute
kubectl get nodes -A --kubeconfig /home/eduk8s/.kube/config-tkg
```
##### Check the pods in workload cluster
```execute
kubectl get pods -A --kubeconfig /home/eduk8s/.kube/config-tkg
```

#### Deploy a test application in workload cluster

##### Create namespace test-application in workload cluster which will be used to deploy an application
```execute
kubectl create ns test-application --kubeconfig /home/eduk8s/.kube/config-tkg
```
    
##### Check all the namespaces in workload cluster
```execute
kubectl get ns --kubeconfig /home/eduk8s/.kube/config-tkg
```
    
##### Create a deployment with 2 replicas
```execute
kubectl create deployment spring-deploy --port=8080 --image=eknath009/tbs-spring-image:3 --replicas=2 -n test-application --kubeconfig /home/eduk8s/.kube/config-tkg
```
    
##### Expose the deployment 
```execute
kubectl expose deployment spring-deploy --port=8080 --type=LoadBalancer -n test-application --kubeconfig /home/eduk8s/.kube/config-tkg
```
    
##### Collect the External IP and access the same in browser with port 8080, wait for a min and execute again

```execute
kubectl get svc -n test-application --kubeconfig /home/eduk8s/.kube/config-tkg
```
    
##### Access the application on port 8080
    
```dashboard:open-url
url: http://externalipcollectedabove:8080
```

############################################################
#### Inform VMware SE if application is not accessible #####
############################################################

![Application](images/test-application.png)

##### Delete the test application

```execute
kubectl delete ns test-application --kubeconfig /home/eduk8s/.kube/config-tkg
```

##### Variable interpolation

workshop_name: {{ workshop_name }}

session_namespace: {{ session_namespace }}

workshop_namespace: {{ workshop_namespace }}

training_portal: {{ training_portal }}

ingress_domain: {{ ingress_domain }}

ingress_protocol: {{ ingress_protocol }}

#### Web site links

[Create TKG Clusters](http://captainvirtualization.com/category/tkg/)
