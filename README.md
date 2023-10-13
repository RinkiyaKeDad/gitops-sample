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