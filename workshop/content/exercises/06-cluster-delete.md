## In this section, lets see the procedure to detach the clusters from TMC console and delete management, workload clusters. 

#### DETACH Workload cluster from TMC Console

###### Navigate to TMC console > Clusters > select the cluster {{ session_namespace }} > Actions (can be located in top right corner) > Detach 

![Application](images/TMC-26.png)

###### Provide name: {{ session_namespace }}

DETACH

![Application](images/TMC-27.png)

##### Verify the pod status, should take few mins to complete

```execute
kubectl get pods -n vmware-system-tmc --kubeconfig ~/.kube/config-tkg
```

#### Deregister Management cluster from TMC console

###### Navigate to TMC console > Administration > Management clusters > click on {{ session_namespace }}-mgmt > Actions (can be located in top right corner) > Deregister 

![Application](images/TMC-28.png)

###### Provide name: {{ session_namespace }}-mgmt

DEREGISTER

![Application](images/TMC-29.png)

##### Verify the pod termination status in management cluster context, should take few mins to complete

```execute
kubectl get pods -n vmware-system-tmc
```

```execute
tmc managementcluster list
```

```execute
tmc clustergroup list
```

#### Delete workload cluster using Tanzu CLI, should take few mins to complete

```execute
tanzu cluster delete {{ session_namespace }} -y
```

```execute
tanzu cluster list
```

##### Proceed further once the workload cluster is deleted completely

#### Delete Management cluster using Tanzu CLI, should take few mins to complete

```execute
tanzu mc delete -y ###### Can be deleted from JB itself due to docker pre req (Kind)
```

#### Delete the localhost file

###### In your local machine, delete the entry that has hostname mapped with name {{ session_namespace }}.captainvirtualization.in from /etc/hosts
