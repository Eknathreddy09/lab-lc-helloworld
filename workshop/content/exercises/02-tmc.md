In this section, we will get some knowledge on TMC 

#### Follow the below steps to get API Token from TMC console

```dashboard:open-url
url: https://console.cloud.vmware.com/csp/gateway/discovery
```
Navigate to TMC console > My Account > API Token > Generate Token

![TMC Account page](images/tmc-1.png)

![TMC API Token](images/TMC-2.png)

Provide your name followed by partnerworkshop
select All Roles

![TMC Generate API Token](images/TMC-3.png)

##### Copy the token and save it for later use ####

![TMC Token](images/TMC-4.png)

##### Authenticate to TMC

```execute
tmc login -n {{ session_namespace }} --no-configure
```
###### Provide API Token
###### Login context name - Press Enter

```execute
tmc system context configure -l "log" -m {{ session_namespace }}-mgmt
```

#### List the available management cluster

```execute
tmc managementcluster list
```

##### Create Cluster group

```execute
tmc clustergroup create --name {{ session_namespace }}-cg --description "Workshop of {{ session_namespace }}"
```

##### List the cluster groups and verify if the newly one is shown in the list

```execute
tmc clustergroup list
```

##### Register the management cluster into TMC

```execute
tmc managementcluster register {{ session_namespace }}-mgmt  -k ~/.kube/config -p TKG -c {{ session_namespace }}-cg
```

##### List the management clusters and verify if registered management cluster is shown in the list

```execute
tmc managementcluster list
```

##### Check resources in vmware-system-tmc namespace
Dont worry if some pods are in creating state, it generally takes few minutes to complete. 

```execute
kubectl get all -n vmware-system-tmc
```

##### It takes few mins to turn healthy

```execute
tmc managementcluster get {{ session_namespace }}-mgmt
```

##### Check in TMC Console

Navigate to TMC console > Administration > Management clusters > click on {{ session_namespace }}-mgmt

```dashboard:open-url
url: https://console.cloud.vmware.com/csp/gateway/discovery
```

##### Attach the workload cluster using TMC CLI

```execute
tmc cluster attach -g default -n {{ session_namespace }} -m {{ session_namespace }}-mgmt -p TKG --kubeconfig ~/.kube/config-tkg
```

Navigate to  TMC console > Clusters > click on clustername {{ session_namespace }}

![TMC Cluster console](images/TMC-5.png)

```execute
tmc cluster validate --kubeconfig ~/.kube/config-tkg
```
