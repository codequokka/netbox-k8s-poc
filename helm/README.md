# Install the netbox with helm

## commoln
### Add the repository

```console
❯ helm repo add netbox https://charts.netbox.oss.netboxlabs.com/
"netbox" has been added to your repositories

❯ helm repo ls
NAME    URL
netbox  https://charts.netbox.oss.netboxlabs.com/
```

## netbox/netbox
### Show netbox default values
```console
❯ helm show values netbox/netbox
```

### Deploy netbox with helm

```console
❯ kubectl create namespace netbox
namespace/netbox created

❯ kubectl get namespaces netbox
NAME     STATUS   AGE
netbox   Active   21s

❯ helm install netbox -n netbox netbox/netbox

NAME: netbox
LAST DEPLOYED: Tue May 27 07:22:03 2025
NAMESPACE: netbox
STATUS: deployed
REVISION: 1
NOTES:
CHART NAME: netbox
CHART VERSION: 6.0.18
APP VERSION: v4.3.1

** Please be patient while the chart is being deployed **

Netbox can be accessed through the following DNS name from within the cluster:

    netbox.netbox.svc.cluster.local (port 80)

To access Netbox site from outside the cluster follow the steps below.

Get the application URL by running these commands:

  export POD_NAME=$(kubectl get pods --namespace "netbox" -l "app.kubernetes.io/name=netbox,app.kubernetes.io/instance=netbox" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl port-forward $POD_NAME 8080:8080

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - resources
  - worker.resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

❯ helm list -n netbox
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
netbox  netbox          1               2025-05-27 07:22:03.790067746 +0900 JST deployed        netbox-6.0.18   v4.3.1
```

### Check K8s resouces

```console
❯ kubectl get all -n netbox
NAME                                 READY   STATUS    RESTARTS   AGE
pod/netbox-5497d5bd4b-gpcxp          1/1     Running   0          10m
pod/netbox-postgresql-0              1/1     Running   0          10m
pod/netbox-valkey-primary-0          1/1     Running   0          10m
pod/netbox-valkey-replicas-0         1/1     Running   0          10m
pod/netbox-valkey-replicas-1         1/1     Running   0          9m30s
pod/netbox-valkey-replicas-2         1/1     Running   0          8m53s
pod/netbox-worker-74c9b4fdc8-b285d   1/1     Running   0          10m

NAME                             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/netbox                   ClusterIP   10.96.104.166   <none>        80/TCP     10m
service/netbox-postgresql        ClusterIP   10.96.168.196   <none>        5432/TCP   10m
service/netbox-postgresql-hl     ClusterIP   None            <none>        5432/TCP   10m
service/netbox-valkey-headless   ClusterIP   None            <none>        6379/TCP   10m
service/netbox-valkey-primary    ClusterIP   10.96.226.119   <none>        6379/TCP   10m
service/netbox-valkey-replicas   ClusterIP   10.96.172.46    <none>        6379/TCP   10m

NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/netbox          1/1     1            1           10m
deployment.apps/netbox-worker   1/1     1            1           10m

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/netbox-5497d5bd4b          1         1         1       10m
replicaset.apps/netbox-worker-74c9b4fdc8   1         1         1       10m

NAME                                      READY   AGE
statefulset.apps/netbox-postgresql        1/1     10m
statefulset.apps/netbox-valkey-primary    1/1     10m
statefulset.apps/netbox-valkey-replicas   3/3     10m

NAME                                SCHEDULE    TIMEZONE   SUSPEND   ACTIVE   LAST SCHEDULE   AGE
cronjob.batch/netbox-housekeeping   0 0 * * *   <none>     False     0        <none>          10m

❯ kubectl get pvc -n netbox
NAME                                   STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
data-netbox-postgresql-0               Bound    pvc-0bc634d6-4501-478d-bfe2-e7b49ed05f27   8Gi        RWO            standard       <unset>                 12m
netbox-media                           Bound    pvc-6e9c82e1-671a-4030-8645-a753612cc4e1   1Gi        RWO            standard       <unset>                 12m
valkey-data-netbox-valkey-primary-0    Bound    pvc-4b0438a6-a33e-4799-bc26-a059b8ced0fa   8Gi        RWO            standard       <unset>                 12m
valkey-data-netbox-valkey-replicas-0   Bound    pvc-fba660a9-b2e8-4e63-a189-5a6d80a0577e   8Gi        RWO            standard       <unset>                 12m
valkey-data-netbox-valkey-replicas-1   Bound    pvc-686233e9-c9ed-400c-a21b-7cb135398df3   8Gi        RWO            standard       <unset>                 11m
valkey-data-netbox-valkey-replicas-2   Bound    pvc-51cfbc7d-4d72-4a40-9c4e-0d5ca02e7595   8Gi        RWO            standard       <unset>                 10m

❯ kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                         STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
pvc-0bc634d6-4501-478d-bfe2-e7b49ed05f27   8Gi        RWO            Delete           Bound    netbox/data-netbox-postgresql-0               standard       <unset>                          11m
pvc-4b0438a6-a33e-4799-bc26-a059b8ced0fa   8Gi        RWO            Delete           Bound    netbox/valkey-data-netbox-valkey-primary-0    standard       <unset>                          11m
pvc-51cfbc7d-4d72-4a40-9c4e-0d5ca02e7595   8Gi        RWO            Delete           Bound    netbox/valkey-data-netbox-valkey-replicas-2   standard       <unset>                          10m
pvc-686233e9-c9ed-400c-a21b-7cb135398df3   8Gi        RWO            Delete           Bound    netbox/valkey-data-netbox-valkey-replicas-1   standard       <unset>                          11m
pvc-6e9c82e1-671a-4030-8645-a753612cc4e1   1Gi        RWO            Delete           Bound    netbox/netbox-media                           standard       <unset>                          11m
pvc-fba660a9-b2e8-4e63-a189-5a6d80a0577e   8Gi        RWO            Delete           Bound    netbox/valkey-data-netbox-valkey-replicas-0   standard       <unset>                          11m
```

### Delete netbox with helm

```console
❯ helm uninstall -n netbox netbox
release "netbox" uninstalled

❯ helm list -n netbox
NAME    NAMESPACE       REVISION        UPDATED STATUS  CHART   APP VERSION

❯ kubectl delete namespaces netbox
namespace "netbox" deleted

❯ kubectl delete -n netbox pvc --all
persistentvolumeclaim "data-netbox-postgresql-0" deleted
persistentvolumeclaim "valkey-data-netbox-valkey-primary-0" deleted
persistentvolumeclaim "valkey-data-netbox-valkey-replicas-0" deleted
persistentvolumeclaim "valkey-data-netbox-valkey-replicas-1" deleted
persistentvolumeclaim "valkey-data-netbox-valkey-replicas-2" deleted

❯ kubectl -n netbox get pvc,pv
No resources found
```
