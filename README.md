# openshift-monitoring


### Login OCP


``` sh
oc login --username <your_user> --password <your_password> <your_api_uri_cluster_ocp>
```

### Create a project 

``` bash
oc new-project local-dev 
```

### Verify exists configmap cluster-monitoring-config in openshift-monitoring

``` bash
oc -n openshift-monitoring get configmap cluster-monitoring-config
Error from server (NotFound): configmaps "cluster-monitoring-config" not found
```
### Enbable UserWorkload

``` bash
oc apply -f cluster-monitoring-config.yaml
configmap/cluster-monitoring-config created
```

### Verify pods openshift-user-workload-monitoring

``` bash
oc get pods -n openshift-user-workload-monitoring
```

![Pods openshift-user-workload-monitoring](/images/openshift-user-workload-monitoring.png "Pods openshift-user-workload-monitoring")

### Create database mysql from template in namespace local-dev

``` bash
oc -n local-dev new-app \
  --name=local-dev-mysql \
  --file=./mysql/mysql-persistent-template.json \
  --param=MYSQL_USER=ocpdbuser1 \
  --param=MYSQL_PASSWORD=ocpdbpass1 \
  --param=MYSQL_DATABASE=ocpdev \
  --param=MYSQL_ROOT_PASSWORD=ocpdbadminpass1 \
  --param=VOLUME_CAPACITY=1Gi
``` 

![create-database](/images/create-database.png "create-database")

### RHOCP PVC 1GiB

![local-dev-pvc-mysql](/images/local-dev-pvc-mysql.png "local-dev-pvc-mysql")

### Metrics

![metrics-pvc-total-and-consumer-capacity](/images/metrics-pvc-total-and-consumer-capacity.png "metrics-pvc-total-and-consumer-capacity")

### Free capacity

``` bash
sum(kubelet_volume_stats_capacity_bytes * on (persistentvolumeclaim, namespace) group_left kube_persistentvolumeclaim_info{namespace="local-dev"}) by (persistentvolumeclaim, namespace) / 1024^3 - 
sum(kubelet_volume_stats_used_bytes * on (persistentvolumeclaim, namespace) group_left kube_persistentvolumeclaim_info{namespace="local-dev"}) by (persistentvolumeclaim, namespace) / 1024^3
```

#### Total capacity

``` bash
sum(kubelet_volume_stats_capacity_bytes * on (persistentvolumeclaim, namespace) group_left kube_persistentvolumeclaim_info{namespace="local-dev"}) by (persistentvolumeclaim, namespace) / 1024^3
``` 

#### Used capacity

``` bash
sum(kubelet_volume_stats_used_bytes * on (persistentvolumeclaim, namespace) group_left kube_persistentvolumeclaim_info{namespace="local-dev"}) by (persistentvolumeclaim, namespace) / 1024^3
```
