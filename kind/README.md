# Create the k8s cluster

## Create the k8s cluster with kind

```console
❯ cd kind

❯ kind create cluster --config  config.yaml
Creating cluster "netbox-k8s-poc" ...
 ✓ Ensuring node image (kindest/node:v1.32.0) 🖼
 ✓ Preparing nodes 📦 📦 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
 ✓ Joining worker nodes 🚜
Set kubectl context to "kind-netbox-k8s-poc"
You can now use your cluster with:

kubectl cluster-info --context kind-netbox-k8s-poc

Have a nice day! 👋
```

## Check all nodes are ready

```console
❯ kubectl get nodes
NAME                       STATUS   ROLES           AGE   VERSION
argocd-poc-control-plane   Ready    control-plane   93s   v1.32.0
argocd-poc-worker          Ready    <none>          81s   v1.32.0
argocd-poc-worker2         Ready    <none>          81s   v1.32.0
```

## Delete the k8s cluster with kind

```console
❯ kind delete cluster --name netbox-k8s-poc
Deleting cluster "netbox-k8s-poc" ...
Deleted nodes: ["netbox-k8s-poc-worker2" "netbox-k8s-poc-control-plane" "netbox-k8s-poc-worker"]

❯ kind get clusters
No kind clusters found
```
