
##### Check Terraform Version

```execute
terraform version
```

##### Change the ci

```execute
cd ~/terraform-tmc
```

##### Read the file

```execute
cat provider-tmc.tf
```

edit and change the API token in provider.tf file

##### Initialize the providers

```execute
terraform init
```

##### 

```execute-2
cat ~/clustergroup.txt
```

```execute-1
vi ~/terraform-tmc/provider-tmc.tf
```

export TF_VAR_SESSION_NAMESPACE=$SESSION_NAMESPACE



Create a cluster group

```execute
terraform plan
```

```execute
terraform apply -auto-approve
```

Create a workload cluster

```execute
terraform plan
```

```execute
terraform apply -auto-approve
```

```execute
terraform state list
```

Delete cluster group

```execute
terraform destroy -target tanzu-mission-control_cluster_group.cluster_group_create_min_info
```

Delete cluster

```execute
terraform destroy -target tanzu-mission-control_cluster_group.cluster_group_create_min_info
```
