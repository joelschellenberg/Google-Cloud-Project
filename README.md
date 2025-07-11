# M300 - Integrate cross-platform services into a network

## Introduction
This documentation covers the project that was completed in module 300. As this was my first time dealing with the Google Cloud, I decided to create several small sub-projects. This helps me to test and get to know different services within the Google Cloud. The table of contents shows which topics were covered.

## Project description
**1. Set up Google Kubernetes Engine Environment**
In this section, the environment for the Google Kubernetes Engine (GKE) was set up. The steps included activating the Kubernetes Engine API, setting up a Kubernetes cluster and creating and building a React application. A Docker image of the application was then created and uploaded to Google Container Registry (GCR). The application was finally deployed as a Kubernetes deployment and service. The status of the deployment was checked and the functions tested.

**2. Management of the React app service**
This part of the project dealt with the administration of the React app service within the Kubernetes cluster. The required files were set up, the status of the running pods was checked and stopped if necessary.

**3. Create MySQL database in Kubernetes**
Another sub-project involved the creation and administration of a MySQL database within the Kubernetes cluster. This included configuring and providing the database services and ensuring data persistence.

**4. Configure MediaServer**
In this section, a MediaServer was set up and configured to store and manage img and videos in the Google Cloud. The configuration included the installation and setup of the MediaServer as well as the management of user accounts and media libraries. The whole thing was created with the emby service.

**5.1 Run Minecraft Server on Google Cloud**
A particular highlight was setting up a Minecraft server in the Google Cloud. The steps included creating a virtual instance, formatting and mounting the disk, downloading and setting up the Minecraft server and installing Fabric. The server was configured, a firewall rule was created and finally tested.

**5.2 Containerization of the Minecraft server**
Finally, I wanted to migrate the Minecraft server into a Docker container and integrate it into the Google Kubernetes Engine (GKE). Unfortunately, the whole thing didn't work out the way I wanted because something was wrong with the API permissions. I couldn't find the error after a long analysis.

## Table of contents

[TOC]

---

# Set up Google Kubernetes Engine Environment
In this subproject I set up and operate a Kubernetes Enginge Environment.

The first thing I did was log in to the Google Cloud Console - Link: https://console.cloud.google.com

I then created a new project with following data:
- Name: micorservice-m300
- ID: micorservice-m300
- Nummer: 439958502201

First, I opened the shell in the Google Cloud and checked the version.
Then I created a React app:

```bash
npx create-react-app my-react-app
```

Next, I change to the directory of my newly created React application.

```bash
cd my-react-app
```

I started my React application with the next command.

```bash
npm start
```

After running npm start, I get a message with the URLs where my React application can be reached. Normally this is http://localhost:3000. I open this URL in my web browser to see and interact with my React application.

```bash
You can now view my-react-app in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://10.88.0.4:3000
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/NewProject.png)

To prepare my application for deployment to a live server, I run the following command:

```bash
npm run build
```

This is followed by the following:

```Bash
The build folder is ready to be deployed.
You may serve it with a static server:

  npm install -g serve
  serve -s build
  ```

Result in the browser:

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/ReactAppBrowser.png)


## Deploy a Docker Container

```bash
gcloud services enable container.googleapis.com containerregistry.googleapis.com
```

I then created the cluster called “m300-project-cluster”.
At first I had problems with the creation because the zone “europe-west1-a” was overloaded. I then decided to switch to the “europe-west1-c” zone.

```bash
gcloud container clusters create m300-project-cluster --zone europe-west1-c
```

The result was as follows:

```Bash
Default change: VPC-native is the default mode during cluster creation for versions greater than 1.21.0-gke.1500. To create advanced routes based clusters, please pass the `--no-enable-ip-alias` flag
Note: Your Pod address range (`--cluster-ipv4-cidr`) can accommodate at most 1008 node(s).
Creating cluster m300-project-cluster in europe-west1-c... Cluster is being health-checked (master is healthy)...done.                                   
Created [https://container.googleapis.com/v1/projects/micorservice-m300/zones/europe-west1-c/clusters/m300-project-cluster].
To inspect the contents of your cluster, go to: https://console.cloud.google.com/kubernetes/workload_/gcloud/europe-west1-c/m300-project-cluster?project=micorservice-m300
kubeconfig entry generated for m300-project-cluster.
NAME: m300-project-cluster
LOCATION: europe-west1-c
MASTER_VERSION: 1.28.9-gke.1000000
MASTER_IP: 34.38.139.119
MACHINE_TYPE: e2-medium
NODE_VERSION: 1.28.9-gke.1000000
NUM_NODES: 3
STATUS: RUNNING
```

Subsequently

```Bash
gcloud container clusters get-credentials m300-project-cluster --zone europe-west1-c
```

Result:
![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/cluster1.png)


The file can be edited with the following command.

```bash
cloudshell edit app.js
```

## Edit file within the cloud shell

Edit file:
```bash
nano app.js
```
Read file:
```bash
cat app.js
```

Preparation yaml file:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: myapp
spec:
  containers:
  - name: mycontainer
    image: nginx:latest
    ports:
    - containerPort: 80
```

# Kubernetes Engine 
This guide explains step by step how to create a Kubernetes Engine in the Google Cloud.

**Informations:**

Project-Name: microservice-m300

Project-ID: microservice-m300

---

## Step 1 - Activate Kubernetes Engine API
First select the current project with the command.

```Bash
gcloud config set project microservice-m300
```

Activate Kubernetes Engine API:
```Bash
gcloud services enable container.googleapis.com
```

---

## Step 2 - Set up the Kubernetes cluster
Now create a cluster as follows. A suitable zone must be selected for the zone. This process will take some time, as a complete cluster is created.

```Bash
gcloud container clusters create cluster-m300 --num-nodes 1 --zone europe-west1-c
```

Now you need the credentials for further configuration.

```Bash
gcloud container clusters get-credentials cluster-m300 --zone europe-west1-c
```

---

## Step 3 - Create and build React application
In this step, we create a new React app:
```Bash
npx create-react-app my-react-app
cd my-react-app
```

Create build:
```Bash
npm run build
```

---

## Step 4 - Create Docker image and upload to GCR
Create the Dockerfile in the root directory of the application.
```bash
# Dockerfile
FROM nginx:alpine
COPY build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Build Docker image:
```bash
docker build -t gcr.io/micorservice-m300/my-react-app .
```

Authenticate with Google Cloud:
```bash
gcloud auth configure-docker
```

Push Docker image to GCR:
```bash
docker push gcr.io/micorservice-m300/my-react-app
```

---

## Step 5 - Create Kubernetes deployment and service
Prepare YAML file and save as “deployment.yaml”
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: react-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: react-app
  template:
    metadata:
      labels:
        app: react-app
    spec:
      containers:
      - name: react-app
        image: gcr.io/YOUR_PROJECT_ID/my-react-app
        ports:
        - containerPort: 80
```

Now a service YAML file must be created. “service.yaml”
```bash
apiVersion: v1
kind: Service
metadata:
  name: react-app-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 80
  selector:
    app: react-app
```

Deploy deployment and service files to Kubernetes:
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## Step 6 - Check status
Check pods:
```bash
kubectl get pods
```

Check services:
```bash
kubectl get services
```

Result for me in the Cloud Console:

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/kubectl-apply.png)

## Step 7 - Test functions
This step is about testing the application.
I check which external IP my application is running with.

```bash
kubectl get services
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/serviceipaddress.png)

The external IP in this case is 34.78.211.40. This IP address is used to open the app in the browser.
I can now enter the address “http://35.233.49.4” in the browser and access the app.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/browserapp1.png)

Now I will test whether the application is really no longer accessible if I set the replicas to 0.
The deployment can be scaled with the following command:

```bash
kubectl scale deployment react-app --replicas=0
```

The service should still exist, but should not have any running pods to serve the traffic. In the screenshot you can see that the React application has been stopped.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/browserapp2.png)

Of course, this also works the other way round.

```bash
kubectl scale deployment react-app --replicas=4
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/browserapp3.png)

-------------------

## Management of the React app service

### Required files
Ensure that the following files are present in the React application directory:
- deployment.yaml
- service.yaml
- Dockerfile

### Install
First, the Kubernetes cluster access data is retrieved with the following command.
```bash
gcloud container clusters get-credentials m300-project-cluster --zone europe-west1-c
```

The deployment and the service are then provided.
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### Status prüfen
After the files have been provided, the service is tested as follows.
```bash
kubectl get pods
kubectl get services
```

### Stop running pods

List deployments:
```bash
kubectl get deployments
```

Set replicas to 0 with the found deployment name “react-app”:
```bash
kubectl scale deployment react-app --replicas=0
```

Now you can see that all pods have been shut down:
```bash
kubectl get pods
```
![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/Replicas.png)

---

# Create MySQL database in Kubernetes.
I have decided to create a database and run it in the React app. I realize that in a production environment, the database and the application would be hosted separately. However, in a development environment, I think it may be acceptable to combine the two to simplify the setup.

First, I create a new directory for the MySQL configuration files:
```Bash
mkdir mysql
```

I then created a new file called “mysql-pv.yaml” and inserted the required data:
```Bash
nano mysql-pv.yaml
```
```Bash
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  hostPath:
    path: /data/mysql
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi

```

First, I created the deployment file:
```Bash
nano mysql-deployment.yaml
```
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - image: mysql:5.7
          name: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: "rootpassword"
          ports:
            - containerPort: 3306
              name: mysql
          volumeMounts:
            - name: mysql-pv-claim
              mountPath: /var/lib/mysql
```

I then created the service file:

```bash
nano mysql-service.yaml
```
```bash
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  ports:
    - port: 3306
  selector:
    app: mysql
```

I provide the YAML files with the following commands.

```bash
kubectl apply -f mysql-pv.yaml
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml
```

I will then check the services and pods:

```bash
kubectl get pods
kubectl get services
```
![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/database-1.png)

I then start a temporary MySQL ClientPod and connect to the database.

```bash
kubectl run -it --rm --image=mysql:5.7 --restart=Never mysql-client -- mysql -h mysql -p
```

This command can then be used to connect to the database:
```bash
kubectl exec -it <mysql-client-pod-name> -- mysql -u root -p
```

# Configure Mediaserver
I have set up my personal media server, which runs on the Google Cloud. This is useful for storing img and videos in the cloud. The special thing about it is that the img is stored privately and is not accessible to everyone. I have created a network map for my orientation. The media server is operated in a virtual machine that was set up in the Google Cloud. Access works via an ssh connection. **However, it is important to note that the media server was not implemented in a Docker container. This was initially planned, but it turned out that it would not make much sense.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/Netzwerkplan-emby.png)

**Project name: mediasrvm300**
Root user password: xxx

**Step 1:** First, I create a new VM instance in the Google Cloud.
- **Name:** emby
- **Region:** europe-west1
- **Zone:** europe-west1-b
![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web1.png)

Machine Type: e2-standard-2 (2 vCPU, 1 core, 8GB memory)

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web2.png)

**Step 3:** I had to configure the boot disk with Ubuntu, as the service works with Linux. Then I had to accept HTTP and HTTPS.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web3.png)

Now I see that the virtual machine has been created.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web4.png)

**Step 4:** First, I create a connection with the SSH cloud shell.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web5.png)

I am updating Ubuntu.
```bash
sudo apt-get update
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web6.png)

```bash
sudo apt-get upgrade
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web7.png)

After updating Ubuntu, I download the Ubuntu application.
```bash
wget https://github.com/MediaBrowser/Emby.Releases/releases/download/4.8.8.0/emby-server-deb_4.8.8.0_amd64.deb
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web8.png)

After the installation, I had to set a password for the root user.
```bash
sudo passwd root
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web9.png)

Now I was able to log in.
```bash
su root
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web10.png)

I create a new user.
```bash
dpkg -i emby-server-deb_4.8.8.0_amd64.deb
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web11.png)

**Step 4:** I create a new firewall rule and name it embyrule with the following configuration:

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web12.png)

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web13.png)

After I have created the firewall rule, you can see it in the overview.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web14.png)

**Step 5:** I go back to the VM instance and click on “Edit”.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web15.png)

I add the new firewall rule to network tags.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web16.png)

**Step 6:** I can then open the media server, which is operated in the cloud, in my browser.
**URL:** http://35.195.66.149:8096

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web17.png)

Next, I created a new user. This user is necessary for the administration of the media server.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web18.png)

"Next"

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web19.png)

"Next"

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web20.png)

"Next"

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web21.png)

Now I can log in with the previously created user.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web22.png)

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web23.png)

**Step 7:** I create a new directory. The files will be uploaded to this directory later.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web24.png)

I then create a new library.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web25.png)

Then click on “Add” to add a new one.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web26.png)

In the menu, you have to enter the path to the created folder under Folder.
In my case, this would be “/home/marentonizda/hmovies”

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web27.png)

You can now see the newly created library.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web28.png)

**Step 8:** Next, I generate an SSH key and save the key in the folder “C:\Users\Joels\.ssh\embly.pub”

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web29.png)

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web30.png)

I then copy the key to the Google Cloud VM Instance.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web31.png)

**Step 9:** Next, I open the “FileZilla” application.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web32.png)

I have to add the created Google Cloud VM instance in the Server Manager.
**Important data:**
ProtokolL: SFTP (SSH File Transfer Protocol)
Host: 35.195.66.149 (External IP address of the VM)
Key file: C:\Users\joels\.ssh\embly

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web33.png)

After completing, I see that the connection has worked.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web34.png)

To transfer the media, I downloaded 3 img and uploaded them from my local Downloads folder to the Media Server folder.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web36.png)

**Step 10:** After the successful transfer, I was able to check whether the data had actually been loaded into the MediaServer.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/web37.png)

The media can be successfully uploaded to the media server. The service makes it possible to manage media content efficiently and access it from anywhere, making it an ideal solution for modern media management and provision.

**SSH Public Key**

```bash
SSH Public Key: ssh-rsa AAAAB3Nxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

------------------------

# Run Minecraft Server on Google Cloud.
![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/Netzwerkplan-Minecraft1.png)


In this project, a Minecraft server is hosted and managed in the Google Cloud. The server is provided within a virtual machine and can be administered via an SSH connection. The entire configuration and deployment of the server is code-based, which allows for easy migration.

**Prepare virtual instance**
Name: minecraft-server

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/CreateVM.png)

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/CreateVM1.png)


**Disk Formatting and Mounting**
Create new directory: 
```bash
sudo mkdir -p /home/Minecraft
```

Format the disk: 
```bash
sudo mkfs.ext4 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/disk/by-id/google-minecraft-disk
```

Mount the disk: 
```bash
sudo mount -o discard,defaults /dev/disk/by-id/google-minecraft-disk /home/Minecraft
```
 
**Download and set up the server:**
```bash
sudo apt-get update
```
```bash
sudo apt-get install -y openjdk-17-jre-headless
```
```bash
cd /home/Minecraft
```
```bash
sudo su
```
```bash
wget https://piston-data.mojang.com/v1/objects/5b868151bd02b41319f54c8d4061b8cae84e665c/server.jar
```
```bash
java -Xms1G -Xmx3G -jar server.jar nogui
```

**Fabric Installation:**
```bash
curl -OJ https://meta.fabricmc.net/v2/versions/loader/1.20.2/0.14.23/0.11.2/server/jar
```
```bash
java -Xmx3G -jar fabric-server-mc.1.20.2-loader.0.14.23-launcher.0.11.2.jar nogui
```
 
## Screen
```bash
apt-get install -y screen
```
```bash
screen -S mcs java -Xmx3G -jar fabric-server-mc.1.20.2-loader.0.14.23-launcher.0.11.2.jar nogui
```
```bash
screen -r mcs
```

## Configuration of the individual settings
In the “server.properties” file, you can define certain rules for the Minecraft server, such as the difficulty level, the time or the maximum number of players.
```bash
nano server.properties
```

The file looks like this:

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/properties2.png)

## Create firewall rule
After the Minecraft server has been successfully created, I need to create a firewall rule. Without this rule, no players can connect to the Minecraft server. The port must be opened so that the server is public. The rule is created in the following steps:

I entered the previously created tag “minecraft-server” and set the source IP range to 0.0.0.0/0. Then I set the port 25565. This is important later for the connection to the Minecraft server.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/Firewall.png)

After creating the firewall, I was able to continue testing, as all the necessary steps had now been carried out.

## Testing
The first thing I did was check the website https://mcsrvstat.us/ to see if the server is accessible in the Minecraft universe.

**Server-Address:** 34.175.175.3:25565
**Version:** 1.20.2

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/TestingMC1.png)

After this worked successfully, I started Minecraft. There I connected to the server. This was successful and I was able to move freely around the server. I recorded a video for a more detailed explanation.

**Video Preview**
![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/TestingMC3.mp4)

## Minecraft Server Monitoring
To monitor the Minecraft server traffic, I had to take a few steps. First, I updated the package list of the package manager to ensure that the latest versions of the packages and their dependencies can be installed.

```bash
sudo apt-get update
```

I then downloaded the installation script for adding the Google Cloud Operations Suite Agent Repository.

```bash
curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
```

Next, I ran the downloaded install script to add the Google Cloud Operations Suite repository and install the Ops Agent at the same time.

```bash
sudo bash add-google-cloud-ops-agent-repo.sh --also-install
```

I updated the package lists again.

```bash
sudo apt-get update
```

I then install the Google Cloud Ops Agent, which is responsible for monitoring and logging my VM.

```bash
sudo apt-get install google-cloud-ops-agent
```

I was then able to start the Google Cloud Ops Agent so that it could begin monitoring my VM.

```bash
sudo service google-cloud-ops-agent start
```

Finally, I checked the status of the Google Cloud Ops Agent to make sure it was running properly and that there were no problems.

```bash
sudo service google-cloud-ops-agent status
```

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/Monitoring1.png)

The most important data can be seen in the monitoring. The following information can be read from the diagram in the Google Cloud GUI:

**1. bytes_used**
Monitors the memory consumption of my server. High memory consumption could indicate many loaded worlds, many active players or many installed plugins. In my case, not much memory is used as there are no active players on the server.

**2. load_1m**
This statistic provides information about how busy the server is. A load such as 0.188 means, for example, that the server is far from being overloaded.

**3. tcp_connections**
Monitors the number of connections that exist to my server. This could show the number of players currently playing on the server or other connections, e.g. from administration tools.

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/Monitoring2.png.png)

# Containerization of the Minecraft server
The containerization of the existing Minecraft server brings several advantages, such as
- Portability
- isolation
- Resource efficiency
- Scalability
- Fast deployment

The environment looks as follows after containerization:

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/Netzwerkplan-Minecraft-Docker.png)

**First steps to prepare Docker:**
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker

Create a Dockerfile in the /home/Minecraft directory with the following content:
```bash
# Use base image
FROM openjdk:16-slim

# Arbeitsverzeichnis im Container setzen
WORKDIR /Minecraft

COPY server.jar .

EXPOSE 25565

# Minecraft-Server starten beim Start des Containers
CMD ["java", "-Xmx2G", "-Xms1G", "-jar", "minecraft_server.jar", "nogui"]
```

Edit Dockerfile.
```bash
nano dockerfile
```

Now I start Docker.
```bash
docker build -t minecraft-server .
```

Create docker volume.

```bash
sudo docker volume create minecraft-data
```

Does not work!
```bash
docker run -d -p 25565:25565 --name minecraft-server -v minecraft-data:/minecraft minecraft-server
```

**Project could not be completed**

# Allgemein

## Pushing changes from GitLab to the GKE
1. Commit changes from gitlab
2. Execute "git pull origin master"
3. npm start
4. http://localhost:3000

![Alt text of the image](https://github.com/joelschellenberg/Google-Cloud-Project/blob/main/img/React-Anwendung-Json.png)

## Overview of useful commands with Kubernetes
To display the configuration of the cluster:
```Bash
cat .kube/config
```
or
```Bash
kubectl config view
```

Display cluster information:
```Bash
kubectl cluster-info
```

CPU, memory utilization:
```Bash
kubectl top nodes
```

Create a pod:
```Bash
kubectl run nginx-1 --image nginx:latest
```

Display running pods:
```Bash
kubectl get pods
```

Display pod informations:
```Bash
kubectl describe pod nginx-1
```

Delete clusters:
```Bash
gcloud container clusters delete "cluster_name"
```

---

# Conclusion
In this thesis, I have gained valuable experience in the Google Cloud. The individual projects gave me a deep insight into various aspects of the Google Cloud platform, as well as into modern development processes.

By setting up and managing the Google Kubernetes Engine, I have developed a solid understanding of how to set up and manage Kubernetes clusters. I learned how to containerize applications, deploy them in a cloud environment and use the basic tools for monitoring and troubleshooting.

Managing the React app service within the Kubernetes cluster taught me important practices for monitoring and maintaining cloud applications. I learned how to check the status of pods and efficiently identify and fix problems in the operation of an application.

Creating and managing a MySQL database within the Kubernetes cluster gave me knowledge of configuring and ensuring data persistence as well as managing relational databases in a cloud environment.

Configuring a MediaServer to manage media content showed me how to configure and manage cloud services for specific use cases and gave me skills in installing and setting up server software and managing user accounts and media libraries.

Finally, setting up and containerizing a Minecraft server on Google Cloud provided a hands-on challenge that allowed me to further develop my skills in server configuration and containerization. Although the migration of the server to the Google Kubernetes engine was ultimately unsuccessful, this process helped me gain a deeper understanding of the complexities of API permissions and debugging in cloud environments.

Overall, this work has not only given me technical skills and knowledge in cloud development, but also a better understanding of the challenges and solutions in managing modern cloud infrastructures.

---

