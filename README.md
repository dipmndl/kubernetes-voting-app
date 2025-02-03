# kubernetes-voting-app
Contain Kubernetes configuration with a sample voting app application deployment using single node cluster.

# Minikube Installation on Ubuntu 22.04 (VirtualBox)

This guide provides step-by-step instructions to install Minikube on an Ubuntu 22.04 virtual machine running inside VirtualBox.

## Configure the Environment

### **1. Install Required Dependencies**
Ensure your system is up to date and install required dependencies:

```sh
$ sudo apt update && sudo apt install -y curl wget git

```
### **2. Install Docker**
 ## Step 1: Install Docker
 
```sh
$ sudo apt install -y docker.io
    
```
 ## Step 2: Add User to Docker Group
To run Docker without sudo, add your user to the docker group:
 
```sh
$ sudo usermod -aG docker $USER 
$ newgrp docker
    
```
Verify Docker installation:
```sh
$ docker --version
$ docker ps
    
```
### **3. Install Minikube**
Download and install Minikube using wget:
```sh
$ wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 -O minikube
$ chmod +x minikube
$ sudo mv minikube /usr/local/bin/
```
Verify installation:
```sh
$ minikube version
```

### **4. Install kubectl**
Download and install kubectl:
```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
Verify installation:
```sh
kubectl version --client
```

### **5. Configure VirtualBox Networking**
Minikube may have issues accessing the internet inside VirtualBox. Change the network adapter:

1. Open VirtualBox

2. Go to Settings > Network > Adapter 1

3. Change Attached to: Bridged Adapter

4. Restart the Ubuntu VM

### **6. Start Minikube**
Start Minikube using Docker as the driver:
```sh
$ minikube start --driver=docker
```
Verify the status:
```sh
$ minikube status
```
Expected output:
minikube
    type: Control Plane
    host: Running
    kubelet: Running
    apiserver: Running
    kubeconfig: Configured


### **7. Verify Minikube Cluster**
Check Kubernetes nodes:
```sh
$ kubectl get nodes
```
Expected output:
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   10m   v1.30.0

Check running pods:
```sh
$ kubectl get pods -A
```
### **8. Deploy a Sample Application**
 1. Deploy Nginx
```sh
$ kubectl create deployment my-nginx --image=nginx
$ kubectl expose deployment my-nginx --type=NodePort --port=80
```
  2. Get Service URL
```sh
$ minikube service my-nginx --url
```
  3. Access Nginx Application
```sh
$ minikube service my-nginx
```
  4. Manage Deployment
To check logs:
```sh
$ kubectl logs -l app=my-nginx
```
To scale up the deployment:
```sh
$ kubectl scale deployment my-nginx --replicas=3
```
To delete the deployment:
```sh
$ kubectl delete deployment my-nginx
$ kubectl delete service my-nginx
```

### **9. Stop and Delete Minikube**
To stop Minikube:
```sh
$ minikube stop
```
To delete Minikube and its data:
```sh
$ minikube delete
```

### **10. Troubleshooting*
# Minikube fails to start with Docker permission error
Error:
permission denied while trying to connect to the Docker daemon socket
Fix:
```sh
$ sudo usermod -aG docker $USER
$ newgrp docker
```
# Minikube exited unexpectedly
If minikube start fails, try deleting and restarting:
```sh
minikube delete
minikube start --driver=docker --kubernetes-version=v1.30.0
```
# Networking Issues in VirtualBox
Ensure Bridged Adapter is enabled in VirtualBox settings.

### **11. Conclusion*
You have successfully installed and configured Minikube on Ubuntu 22.04 inside VirtualBox! 🚀

