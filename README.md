## Requirement/My environement
A. Controller machine/laptop   
- Install ansible
- Make passwordless ssh to all master node
- Make passwordless ssh to all worker node

B. One Master node
- hostname: m1, IP: 192.168.119.104, User: root
C. One Worker node
- hostname: m1, IP: 192.168.119.104, User: root  

D. Update hosts file as oer your master and worker nodes details


## From Controller machine, setup k8s cluster via ansbile
```
$ ansible all -m ping -i hosts
$ ansible-playbook  k8-setup.yml -i hosts
```
## Login to Master node
```
$ kubectl get nodes
```


### Commands
1. To update the label of node n1 froom none to worker
```
root@m1:~# kubectl get nodes
NAME   STATUS     ROLES           AGE   VERSION
m1     Ready      control-plane   17h   v1.32.1
n1     Ready   <none>             21s   v1.32.1

root@m1:~# kubectl label node n1 node-role.kubernetes.io/worker=worker

root@m1:~# kubectl get nodes
NAME   STATUS   ROLES           AGE   VERSION
m1     Ready    control-plane   18h   v1.32.1
n1     Ready    worker          20m   v1.32.1
```

## Deploy Argo CD via helm
```
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
helm install argocd argo/argo-cd --namespace argocd --create-namespace
kubectl get pods -n argocd

# Run the following command to access the ArgoCD UI locally:
kubectl port-forward svc/argocd-server -n argocd 8080:443

# To get password for argocd login, USER-> admin 
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
http://localhost:8080
```

## To change default password for argocd
```
$ htpasswd -bnBC 10 "" "admin" | tr -d ':\n'

$ kubectl -n argocd patch secret argocd-secret -p '{"stringData": {"admin.password": "<HASH>"}}
```
## Adding ssh keys for private repos
```
 kubectl create secret generic argocd-ssh-key --from-file=ssh-privatekey=/home/pawank/.ssh id_ed25519 -n argocd

 kubectl get secret argocd-ssh-key -n argocd -o yaml

```