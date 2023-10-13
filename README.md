# GitOps Sample

This is a sample project to demonstrate GitOps with ArgoCD.

## Prerequisites

1. Create a K8s cluster

1. Install ingress-nginx

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx --create-namespace --namespace ingress-nginx
```

1. Get the nginx loadbalancer service's external IP

```bash
$ kubectl get svc -n ingress-nginx
NAME                                 TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)                      AGE
ingress-nginx-controller-admission   ClusterIP      10.43.132.65    <none>          443/TCP                      3m12s
ingress-nginx-controller             LoadBalancer   10.43.172.201   **74.220.19.189**   80:31297/TCP,443:32408/TCP   3m12s
```

Add a DNS entry of A type for your domain to point to this IP.

1. Install ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

To access the Argo CD UI:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Login to localhost:8080 with username `admin` and password you get via running:
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
Or if you have `argocd` CLI installed, you can run:
```bash
argocd admin initial-password -n argocd
```

1. Click New App from the Argo CD UI and click edit as YAML and copy paste the config in `/apps/go-server-prod.yaml` and click create. And then click sync.