## Commands


```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml

kubectl describe service rhythmapp-service -n myapps
kubectl describe service nginx-service -n myapps

kubectl apply -f ingress/ingress.yaml
kubectl get ingress -n myapps
kubectl describe ingress my-ingress -n myapp
kubectl get ingressclasses

kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 8080:80
```
