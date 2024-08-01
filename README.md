# k8s-manifests

# Argo CD server setup
* Step 1: Create Namespace argocd
  ```
  kubectl create namespace argocd
  ```
* Step 2: Install Argocd services
  ```
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
  ```
* Step 3: Deploy Argo CD service
  ```
  git clone https://github.com/ramupunuruorg/k8s-manifests.git &&  cd argocd && kubectl apply -f service.yml
  ```
* Step 4: Access Argo CD UI
  ```
  kubectl get svc -n argocd
  ```
  ```
  MASTER_NODE_IP:31698
  ```
  ![image](https://github.com/cloudstonesorg/csp-deployments/assets/112494492/f412d5ee-262f-45e1-b140-26d99d0f2f2b)

* Step 5: To fetch default password for admin user
  ```
  kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"| base64 -d
  ```
* Step 6: Authenticate k8s manifests repo
  ```
  Argo CD UI => Settings => Repositories => Connect Repo => Select HTTPs => fill repo details
  ```


# Nginx ingress server setup

Step 1: Clone kubernetes ingress repo
git clone https://github.com/nginxinc/kubernetes-ingress.git --branch v3.3.0 && cd kubernetes-ingress/deployments

Step 2: SA
kubectl apply -f common/ns-and-sa.yaml

Step 3: RBAC
kubectl apply -f rbac/rbac.yaml

Step 4: Config
kubectl apply -f common/nginx-config.

Step 5: Class
kubectl apply -f common/ingress-class.

Step 6: Servers
kubectl apply -f common/crds/k8s.nginx.org_virtualservers.yaml
kubectl apply -f common/crds/k8s.nginx.org_virtualserverroutes.yaml
kubectl apply -f common/crds/k8s.nginx.org_transportservers.yaml
kubectl apply -f common/crds/k8s.nginx.org_policies.yaml
kubectl apply -f common/crds/k8s.nginx.org_globalconfigurations.yaml

Step 7: Daemonset
kubectl apply -f daemon-set/nginx-ingress.yaml

Step 8: Loadbalancer
kubectl apply -f service/loadbalancer.yaml

Step 9: Fetch all services under namespace nginx-ingree
kubectl get svc -n nginx-ingress

Step 10: Access Nginx LB
MASTER_NODE_IP:80


