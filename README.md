# openshift-monitoring


### Login OCP


```sh
oc login --username <your_user> --password <your_password> <your_api_uri_cluster_ocp>
```

### Create a project 

```bash
oc new-project local-dev 
```

### Verify exists configmap cluster-monitoring-config in openshift-monitoring

```bash
oc -n openshift-monitoring get configmap cluster-monitoring-config
```

```bash
Error from server (NotFound): configmaps "cluster-monitoring-config" not found
```

### Enbable UserWorkload

```bash
oc apply -f cluster-monitoring-config.yaml
```
