## In this section, lets see the procedure to delete packages(catalog) from TMC Console. 

#### Delete Harbor Package:

<style>
H5{color:Blue !important;}
H6{color:DarkOrange !important;}
</style>

<p style="color:black"><strong>Navigate to TMC console > Clusters > select the cluster {{ session_namespace }} > Add-ons > click on {{ session_namespace }}-harbor</strong></p>

Click on Delete which can be found in top right corner > Actions > Delete

<p style="color:black"><strong>Example:</strong></p>

![Application](images/TMC-30.png)

-----------------

provide the name:

###### {{ session_namespace }}-harbor

-----------------

DELETE

Check the status of deletion by navigating to TMC console > Clusters > select the cluster  {{ session_namespace }} > Add-ons > Installed

<p style="color:black"><strong>Example:</strong></p>
![Application](images/TMC-22.png)

##### Verify the pods termination status

```execute
kubectl get pods -n tanzu-system-registry --kubeconfig ~/.kube/config-tkg
```

#### Delete Contour Package:

<p style="color:black"><strong>Navigate to TMC console > Clusters > select the cluster  {{ session_namespace }} > Add-ons > click on {{ session_namespace }}-contour</strong></p>

Click on Delete which can be found in top right corner > Actions > Delete

-----------------

provide the name:

###### {{ session_namespace }}-contour

-----------------

DELETE

<p style="color:black"><strong>Example:</strong></p>

![Application](images/TMC-24.png)

##### Verify the services status

```execute
kubectl get svc -n tanzu-system-ingress --kubeconfig ~/.kube/config-tkg
```

#### Delete Cert-manager Package:

<p style="color:black"><strong>Navigate to TMC console > Clusters > select the cluster  {{ session_namespace }} > Add-ons > click on {{ session_namespace }}-certm</strong></p>

Click on Delete which can be found in top right corner > Actions > Delete

<p style="color:black"><strong>Example:</strong></p>

![Application](images/TMC-23.png)

-----------------

provide the name:

###### {{ session_namespace }}-certm

-----------------

DELETE

##### Verify the pods termination status

```execute
kubectl get pods -n cert-manager --kubeconfig ~/.kube/config-tkg
```

<p style="color:black"><Navigate to TMC console > Clusters > select the cluster  {{ session_namespace }} > Add-ons </strong></p>

Verify if the packages are deleted. 

<p style="color:black"><strong>Example:</strong></p>
![Application](images/TMC-25.png)

