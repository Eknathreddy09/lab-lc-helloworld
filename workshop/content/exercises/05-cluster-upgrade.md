

```execute-1
az vm start -n {{ session_namespace }} -g {{ session_namespace }}-JB
```

### Connect using DNS hostname

```execute-2
ssh -i id_rsa azureuser@{{ session_namespace }}.centralindia.cloudapp.azure.com -o StrictHostKeyChecking=accept-new
```

```execute-2
/bin/sh ~/script-session-upgrade.sh
```

```execute-1
az group delete -n {{ session_namespace }}-JB --yes
```


