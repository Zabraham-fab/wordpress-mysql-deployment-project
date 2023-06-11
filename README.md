# Installing 5 Node Cluster in Kubernetes and Deploying Wordpress Application with MySQL database connected

<img src="171921">

<img src="172038">

Hello... This repository is about a portfolio project that demonstrates the installation of a 5-node Kubernetes cluster and the deployment of a WordPress application in Kubernetes. I have personally performed this project by doing a hands-on exercise in a local environment, and I'm sharing it with you.
Step 1: Installation of a 5-Node Kubernetes Cluster
- I set up a Kubernetes cluster consisting of one master node and four worker nodes. In this step, I used Minikube to create the cluster.
Step 2: Creation of namespaces
- I created two namespaces named "test" and "production". These namespaces provide separate areas for different environments.
Step 3: Creation of Groups and Assignment of Roles
- I created a role that provides full access to all resources in the "test" namespace for the "junior" group. In the "production" namespace, I created a role that grants only "read and list" permissions.
- For the "senior" group, I created a role that provides full access to all resources in both the "production" and "test" namespaces. Additionally, they only had "read and list" permissions on other resources in the cluster.
Step 4: Installation of Ingress Controller
- I installed the selected "nginx" ingress controller to ensure proper routing of incoming requests.
Step 5: Worker Node Restrictions
- I configured three selected worker nodes to serve only the pods deployed in the "production" environment. Other pods were not created on these worker nodes.
Step 6: Deployment of the WordPress Application
- I deployed the WordPress application in both the "test" and "production" namespaces. The images used were "wordpress:latest" and "mysql:5.6".
- MySQL was made accessible within the cluster by using a "ClusterIP" type service.
- Both applications stored their long-term data on "persistent volumes".
- Sensitive information (such as passwords, database root passwords, database names, etc.) was not stored in the application or YAML files. This information was provided through secret objects in base64 format.
- Both applications were scheduled on the same worker node, and resource constraints (CPU and memory) were defined.
- To demonstrate the functionality of the distributed WordPress application, I performed port forwarding. Using the command 'kubectl port-forward deployment/wordpress-deployment –n test 8080:80', I redirected traffic from the local machine's browser to access and view the application running in both the test and production environments by visiting 'localhost:8080'.
This project helped me enhance my skills in working with Kubernetes and container technologies. Covering topics such as cluster management, namespace usage, role and permission assignments, ingress control, and application deployment, this project presents an applicable approach in real-world scenarios.

<img src="171818" >

Used commands:

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

<img src="171827">

<img src="184748">