## In this section, lets see the procedure to delete packages(catalog) from TMC Console. 

#### Delete Harbor Package:


###### Navigate to TMC console > Clusters > select the cluster {{ session_namespace }} > Add-ons > click on {{ session_namespace }}-harbor

Click on Delete which can be found in top right corner > Actions > Delete

###### Example:

![Application](images/TMC-30.png)

###### provide the name: {{ session_namespace }}-harbor

DELETE

Check the status of deletion by navigating to TMC console > Clusters > select the cluster  {{ session_namespace }} > Add-ons > Installed

###### Example:
![Application](images/TMC-22.png)

##### Verify the pods termination status

```execute
kubectl get pods -n tanzu-system-registry --kubeconfig ~/.kube/config-tkg
```

#### Delete Contour Package:

###### Navigate to TMC console > Clusters > select the cluster  {{ session_namespace }} > Add-ons > click on {{ session_namespace }}-contour

Click on Delete which can be found in top right corner > Actions > Delete

###### provide the name {{ session_namespace }}-contour

DELETE

###### Example:

![Application](images/TMC-24.png)

##### Verify the services status

```execute
kubectl get svc -n tanzu-system-ingress --kubeconfig ~/.kube/config-tkg
```

#### Delete Cert-manager Package:

###### Navigate to TMC console > Clusters > select the cluster  {{ session_namespace }} > Add-ons > click on {{ session_namespace }}-certm

Click on Delete which can be found in top right corner > Actions > Delete

###### Example:

![Application](images/TMC-23.png)

###### provide the name {{ session_namespace }}-certm

DELETE

##### Verify the pods termination status

```execute
kubectl get pods -n cert-manager --kubeconfig ~/.kube/config-tkg
```

###### Navigate to TMC console > Clusters > select the cluster  {{ session_namespace }} > Add-ons 
Verify if the packages are deleted. 

###### Example:
![Application](images/TMC-25.png)

