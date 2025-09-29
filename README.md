# Kubernetes-Cluster-with-Minikube
Step 1: Prerequisites

Install Docker
---------------
Since Minikube runs containers, you’ll need Docker.
On Ubuntu:

sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
docker --version


On Windows (with WSL2):

Install Docker Desktop → enable WSL2 integration.

Verify:

docker --version


Install kubectl (Kubernetes CLI)

curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client


Install Minikube
On Linux:

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube


On Windows:

Download from Minikube Releases
.

Add to PATH.

Verify:

minikube version

🔹 Step 2: Start Your Cluster
minikube start --driver=docker


Check the status:

minikube status

🔹 Step 3: Create Deployment

Create a file deployment.yaml:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
      - name: hello-app
        image: nginx:latest
        ports:
        - containerPort: 80


Apply it:

kubectl apply -f deployment.yaml


Verify:

kubectl get deployments
kubectl get pods

🔹 Step 4: Expose App with a Service

Create service.yaml:

apiVersion: v1
kind: Service
metadata:
  name: hello-service
spec:
  selector:
    app: hello
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort


Apply:

kubectl apply -f service.yaml
kubectl get svc


Access the app:

minikube service hello-service

🔹 Step 5: Scale Deployment
kubectl scale deployment hello-deployment --replicas=4
kubectl get pods

🔹 Step 6: Inspect & Logs
kubectl describe deployment hello-deployment
kubectl logs <pod-name>


✅ Deliverables:

deployment.yaml

service.yaml

Screenshots of:

kubectl get pods

kubectl get svc

App opened in browser
