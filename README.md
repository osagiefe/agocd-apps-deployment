### Deploy-applications through Argocd

## project overview
A Dockerised React application build into a container and pushed into Docker Hub, created the index.js file
as landing page an server test page,to check if the server is litening at port:3000 as specified,
### Featured technologies
•	Node.js: An open-source JavaScript run-time environment for executing server-side JavaScript code.
•	Cloud: Accessing computer and information technology resources through the Internet.
•	Container Orchestration: Automating the deployment, scaling and management of containerized applications.

  #####  Prerequisites:
  $ export docker_username="MY_DOCKER_USERNAME"

  install GIt
  install Node.js
  install npm     $npm install
  install Express.js $install npm express --save
   Install Node.js
   install aws CLi
   Create IAM user with administrative access
   aws configure
   aws s3 ls
   aws sts get-caller-identity
   install kubernetstes cluster  AWs EKS
   install kubeconfig
   install kubectl CLI (command line inerface)
   install helm
  Run the following commands in a terminal:

  Create Docker account

  Install Docker CLI or Docker deckstop

  Retrieve and save your Dockerhub user id

  Build the image

 ## In a terminal, run: to start 
 node -v
 npm init -y
 npm install express -save

$ docker build -t $docker_username/deploy-react-kubernetes .

 #####  your image should be listed by running:

  $ docker images

## Ran the command below

 $ node index.js 

# ##### Now inside the dockerfile created

 CMD ["npm","start"]

## test the application locally

docker run -p 8080:8080 osagiefe/pyapplication:v1
## Tools stack:

project workflow
What the project Does:

--Installement of argocd
--Application deployment using argocd
-- Roleback to different working version using argocd

1. Create an EKS Cluster using this command:

# Create EKS cluster
  eksctl create cluster --name eks-cluster-222 --node-type t3.medium --nodes 2 --nodes-min 2 --nodes-max 3 --region us-east-1


# Another command to creating the eks cluster
  eksctl create cluster \
  --name eks-cluster-222 \
  --version 1.29 \
  --region us-east-1 \
  --nodegroup-name ng-1 \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3


# Get EKS Cluster service
eksctl get cluster --name eks-cluster-222 --region us-east-1

<img width="2222" height="258" alt="Image" src="https://github.com/user-attachments/assets/1dfc2873-aabd-4c92-b537-58f2200726ce" />
 
 # Update eks kubeconfig once k8s cluster is installed successfully
aws eks update-kubeconfig --name eks-cluster-222 --region us-east-1

<img width="2184" height="122" alt="Image" src="https://github.com/user-attachments/assets/203aa32a-f387-4b7a-970d-24192907571a" />

# create argocd namespace
kubectl create namespace argocd

# install argocd from argocd repository 

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

<img width="1846" height="492" alt="Image" src="https://github.com/user-attachments/assets/851f3845-6ace-4bb1-9bb4-7e073fb73a25" />


# kubectl get pods -n argocd
kubectl get pods -n argocd

<img width="1894" height="418" alt="Image" src="https://github.com/user-attachments/assets/b7643c38-225e-4e75-939d-7518f90df65e" />

# then get argocd service
kubectl get svc -n argocd

<img width="2156" height="772" alt="Image" src="https://github.com/user-attachments/assets/cd52302a-129f-4973-9c98-30ed162960ce" />

# Get argocd from the webbrowser by edit the service of LoadBalancer

kubectl edit svc argocd-server -n argocd

# If the above command fails, then below is the work around of the above command. Change the clusterIP to LoadBalancer:
kubectl -n argocd patch svc argocd-server -p '{"spec": {"type": "LoadBalancer"}}'

# Get the application running service

kubectl get svc -n argocd

<img width="2170" height="788" alt="Image" src="https://github.com/user-attachments/assets/12007782-cbeb-42c9-ba9d-d0237da67fff" />

# copy the loadbalancer endpoints and take to the browser
a3b8eedf3068e47138671446a49e2078-2029049580.eu-west-2.elb.amazonaws.com

# on the browser

<img width="2276" height="1556" alt="Image" src="https://github.com/user-attachments/assets/5bc79605-f1b6-4fc0-ab8d-bee945c25024" />

# argocd GUI

<img width="2144" height="1540" alt="Image" src="https://github.com/user-attachments/assets/f96c4278-f341-4368-8854-93886a8d3682" />

# username is 
admin
# get argocd password

- Get ArgoCD default password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# login to the argocd
<img width="2580" height="1436" alt="Image" src="https://github.com/user-attachments/assets/e96165e8-8391-4256-9802-cd9bda8094c1" />

<img width="2606" height="1508" alt="Image" src="https://github.com/user-attachments/assets/70a8d1cf-3258-414e-aba4-06a950cfc6bc" />

<img width="2544" height="1532" alt="Image" src="https://github.com/user-attachments/assets/15d93d44-f509-4c5b-b0c7-6331f47e82f2" />

## Now lets check the terminal at the default namespace

kubectl get pods

kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
pyapp-7ccc494bbd-9fzv5   1/1     Running   0          16m
pyapp-7ccc494bbd-llfj6   1/1     Running   0          16m
pyapp-7ccc494bbd-rc7xg   1/1     Running   0          16m


# lets also checked the service object
kubectl get svc

kubectl get svc
NAME            TYPE           CLUSTER-IP      EXTERNAL-IP                                                               PORT(S)        AGE
kubernetes      ClusterIP      10.100.0.1      <none>                                                                    443/TCP        82m
pyapp-service   LoadBalancer   10.100.168.29   a6fbf8f936c0d4999b52db71cd07bec0-1865929378.eu-west-2.elb.amazonaws.com   80:31051/TCP   18m

now access the pyappservice and put into the browser from the load balancer


# view on the browser


<img width="2446" height="1386" alt="Image" src="https://github.com/user-attachments/assets/f7f4a439-60f6-4d4f-92b8-d308ef04af33" />




# now after we refresh argocd it will detect the changes

# now run 
kubectl get pods


# to delete cluster

eksctl delete cluster --name eks-cluster-222


## Lessons/Challenges learned
In this project I learnt how to install ArgoCD on Kubernetes, deploy an application, and rollback to a previous version using the Web UI. The command "kubectl edit svc argocd-server -n argocd" to edit the application svc failed to launch the editor (vi) to enable my changing the cluster IP to Loadblancer. I sort help from ChatGPT to resolve the issue. 






