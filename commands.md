$ kubectl create namespace test
$ kubectl create namespace production

$ kubectl apply -f jr_production_rbac.yml
$ kubectl apply -f jr_test_rbac.yml
$ kubectl apply -f sr_cluster_rbac.yml
$ kubectl apply -f sr_test_rbac.yml

$ kubectl label nodes minikube-m03 tier=production
$ kubectl label nodes minikube-m04 tier=production
$ kubectl label nodes minikube-m05 tier=production

$ kubectl taint node minikube-m03 tier=production:NoSchedule
$ kubectl taint node minikube-m04 tier=production:NoSchedule
$ kubectl taint node minikube-m05 tier=production:NoSchedule

$ kubectl apply -f mysql-test-deploy.yml
$ kubectl apply -f wp-test-deploy.yml
$ kubectl apply -f mysql-prod-deploy.yml
$ kubectl apply -f wp-prod-deploy.yml

$ kubectl port-forward deployment/wordpress-deployment -n production 8080:80 
$ kubectl port-forward deployment/wordpress-deployment -n test 8080:80 

$ kubectl apply -f ingress-test-prod.yml