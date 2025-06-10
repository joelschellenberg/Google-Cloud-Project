# M300 - Plattformübergreifende Dienste in ein Netzwerk integrieren

## Einleitung
Diese Dokumentation umfasst das Projekt, welches im Modul 300 bewältigt wurde. Da ich mich das erste Mal mit der Google Cloud auseinandersetzte, hab ich mich dazu entschieden mehrere kleine Teilprojekte zu erstellen. Dies hilft mir verschiedene Services innerhalb der Google Cloud zu testen und kennenzulernen. Im Inhaltsverzeichnis ist vermerkt, welche Themenbereiche angeschnitten wurden.

## Projektbeschrieb
**1. Google Kubernetes Engine Environment aufsetzen**
In diesem Abschnitt wurde die Umgebung für die Google Kubernetes Engine (GKE) eingerichtet. Die Schritte beinhalteten die Aktivierung der Kubernetes Engine API, das Einrichten eines Kubernetes Clusters und das Erstellen sowie Bauen einer React-Anwendung. Anschliessend wurde ein Docker-Image der Anwendung erstellt und in Google Container Registry (GCR) hochgeladen. Die Anwendung wurde schliesslich als Kubernetes Deployment und Service bereitgestellt. Der Status der Bereitstellung wurde überprüft und die Funktionen getestet.

**2. Verwaltung des React-App-Services**
Dieser Teil des Projekts befasste sich mit der Verwaltung des React-App-Services innerhalb des Kubernetes Clusters. Es wurden die erforderlichen Dateien eingerichtet, der Status der laufenden Pods überprüft und gegebenenfalls gestoppt.

**3. MySQL-Datenbank in Kubernetes erstellen**
Ein weiteres Teilprojekt beinhaltete die Erstellung und Verwaltung einer MySQL-Datenbank innerhalb des Kubernetes Clusters. Dies umfasste die Konfiguration und Bereitstellung der Datenbankdienste sowie die Sicherstellung der Datenpersistenz.

**4. MediaServer konfigurieren**
In diesem Abschnitt wurde ein MediaServer aufgesetzt und konfiguriert, um img und Videos in der Google Cloud zu speichern und zu verwalten. Die Konfiguration beinhaltete die Installation und Einrichtung des MediaServers sowie die Verwaltung von Benutzerkonten und Medienbibliotheken. Das Ganze wurde mit dem emby Service erstellt.

**5.1 Minecraft Server auf Google Cloud betreiben**
Ein besonderes Highlight war das Einrichten eines Minecraft Servers in der Google Cloud. Die Schritte umfassten das Erstellen einer virtuellen Instanz, das Formatieren und Mounten der Disk, das Herunterladen und Aufsetzen des Minecraft Servers sowie die Installation von Fabric. Der Server wurde konfiguriert, eine Firewall-Regel erstellt und schliesslich getestet.

**5.2 Containersierung des Minecraft Servers**
Abschliesend wollte ich den Minecraft Server in einen Docker Container migrieren und in die Google Kubernetes Engine (GKE) integrieren. Leider hat das Ganze nicht so geklappt, wie ich wollte, da etwas mit den API-Berechtigungen nicht stimmte. Ich konnte den Fehler nach langer analysierung nicht finden.

## Inhaltsverzeichnis

[TOC]

---

# Google Kubernetes Engine Environment aufsetzen
In diesem Teilprojekt setze ich ein Kubernetes Enginge Environment auf und betreibe dieses.

Als erstes habe ich mich in der Google Cloud Angemeldet. Link: https://console.cloud.google.com

Anschliessend habe ich ein neues Projekt erstellt.
- Name: micorservice-m300
- ID: micorservice-m300
- Nummer: 439958502201

Zunächst habe ich die Shell in der Google Cloud geöffnet und die Version geprüft.
Anschliessend habe ich eine React App erstellt:

```bash
npx create-react-app my-react-app
```

Als nächstes wechsle ich in das Verzeichnis meiner neu erstellten React-Anwendung.

```bash
cd my-react-app
```

Mit dem nächsten Befehl startete ich meine React-Anwendung.

```bash
npm start
```

Nach dem Ausführen von npm start erhalte ich eine Meldung mit den URLs, unter denen meine React-Anwendung erreichbar ist. Normalerweise ist dies http://localhost:3000. Ich öffne diese URL in meinem Webbrowser, um meine React-Anwendung zu sehen und mit ihr zu interagieren. 

```bash
You can now view my-react-app in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://10.88.0.4:3000
```

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/NewProject.png)

Um meine Anwendung für die Bereitstellung auf einem Live-Server vorzubereiten, führe ich folgenden Befehl aus:

```bash
npm run build
```

Anschliessend kommt folgendes:

```Bash
The build folder is ready to be deployed.
You may serve it with a static server:

  npm install -g serve
  serve -s build
  ```

Ergebnis im Browser:

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/ReactAppBrowser.png)


## Docker Container Deploy

```bash
gcloud services enable container.googleapis.com containerregistry.googleapis.com
```

Anschliessend erstellte ich den Cluster namens "m300-project-cluster".
Zuerst hatte ich Probleme mit der Erstellung, da die Zone "europe-west1-a" überlastet war. Ich habe mich dann entschieden auf die Zone "europe-west1-c" zu wechseln.

```bash
gcloud container clusters create m300-project-cluster --zone europe-west1-c
```

Ergebnis war folgendes:

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

Anschliessend

```Bash
gcloud container clusters get-credentials m300-project-cluster --zone europe-west1-c
```

Ergebnis:
![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/cluster1.png)


Die Datei kann mit folgendem Befehl editiert werden.

```bash
cloudshell edit app.js
```

## File editieren in der Shell

File editieren mit:
```bash
nano app.js
```
File auslesen mit:
```bash
cat app.js
```

Vorbereitung yaml file:

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
In dieser Anleitung wird Schritt für Schritt erklärt, wie man eine Kubernetes Engine in der Google Cloud erstellt.

Informationen
Projektname: microservice-m300
ID: microservice-m300

---

## Schritt 1 - Kubernetes Engine API aktivieren
Zuerst wird mit dem Befehl das aktuelle Projekt ausgewählt.

```Bash
gcloud config set project microservice-m300
```

Kubernetes Engine API aktivieren:
```Bash
gcloud services enable container.googleapis.com
```

---

## Schritt 2 - Kubernetes Cluster einrichten
Nun erstellt man einen Kluster wie folgt. Bei der Zone muss man eine geeignete Zone nehmen. Dieser Vorgang wird einige Zeit dauern, da ein kompletter Kluster aufgebaut wird.

```Bash
gcloud container clusters create cluster-m300 --num-nodes 1 --zone europe-west1-c
```

Nun braucht man die Credentials für die weitere Konfiguration.

```Bash
gcloud container clusters get-credentials cluster-m300 --zone europe-west1-c
```

---

## Schritt 3 - React Anwendung erstellen und builden
In diesem Schritt erstellen wir eine neue React-App:
```Bash
npx create-react-app my-react-app
cd my-react-app
```

Build erstellen:
```Bash
npm run build
```

---

## Schritt 4 - Docker-Image erstellen und in GCR hochladen
Das Dockerfile im Stammverzeichnis der Anwendung erstellen.
```bash
# Dockerfile
FROM nginx:alpine
COPY build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Docker-Image bauen:
```bash
docker build -t gcr.io/micorservice-m300/my-react-app .
```

Bei Google Cloud authentifizieren:
```bash
gcloud auth configure-docker
```

Docker-Image zu GCR pushen:
```bash
docker push gcr.io/micorservice-m300/my-react-app
```

---

## Schritt 5 - Kubernetes Deployment und Service erstellen
YAML-File vorbereiten und speichern als "deployment.yaml"
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

Nun muss ein Service YAML-File erstellt werden. "service.yaml"
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

Deployment und Service zu Kubernetes deployen:
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## Schritt 6 - Status überprüfen
Pods überprüfen:
```bash
kubectl get pods
```

Service überprüfen:
```bash
kubectl get services
```

Ergebnis bei mir in der Cloud Console:

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/kubectl-apply.png)

## Schritt 7 - Funktionen testen
In diesem Schritt geht es darum, dass ich die Anwendung teste.
Ich schaue nach mit welcher External-IP meine Anwendung läuft.
```bash
kubectl get services
```

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/serviceipaddress.png)

Die External-IP in diesem Fall ist 34.78.211.40. Diese IP-Adresse wird verwendet, um die App im Browser zu öffnen.
Ich kann die Adresse "http://35.233.49.4" nun im Browser eingeben und kommen auf die App.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/browserapp1.png)

Nun werde ich testen, ob die Anwendung auch wirklich nicht mehr erreichbar ist, wenn ich die replicas auf 0 setze.
Das Deployment kann mit folgendem Befehl skaliert werden:
```bash
kubectl scale deployment react-app --replicas=0
```

Der Service sollte zwar weiterhin existieren, aber keine laufenden Pods haben, um den Datenverkehr zu bedienen. Im Screenshot sieht man, dass die React-Anwendung gestoppt wurde.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/browserapp2.png)

Umgekehrt funktioniert dies natürlich auch.
```bash

kubectl scale deployment react-app --replicas=4
```
![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/browserapp3.png)

-------------------

## Verwaltung vom React-App-Service

### Erforderliche Dateien
Sicherstellten, dass folgende Dateien im Verzeichnis der React Applikation vorhanden sind:
- deployment.yaml
- service.yaml
- Dockerfile

### Einrichtung
Als erstes werden mit folgendem Befehl die Kubernetes-Cluster-Zugangsdaten abgerufen.
```bash
gcloud container clusters get-credentials m300-project-cluster --zone europe-west1-c
```

Anschliessend wird das Deployment und der Service bereitgestellt.
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### Status prüfen
Nachdem die Files bereitgestellt wurden, wird der Service wie folgt getestet.
```bash
kubectl get pods
kubectl get services
```

### Laufende Pods stoppen

Deployments auflisten: 
```bash
kubectl get deployments
```

Mit dem herausgefundenen Deploymentnamen "react-app" replicas auf 0 setzen:
```bash
kubectl scale deployment react-app --replicas=0
```

Nun Sieht man, dass alle Pods heruntergefahren wurden:
```bash
kubectl get pods
```
![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/Replicas.png)

---

# MySQL-Datenbank in Kubernetes erstellen.
Ich habe mich entschieden eine Datenbank zu erstellen und diese in der React-App zu betreiben. Es ist mir klar, dass in einer Produktionsumgebung die Datenbank und die Anwendung getrennt voneinander hosten würde. In einer Entwicklungsumgebung kann es meiner meinung nach jedoch akzeptabel sein, beides zusammenzufassen,um die Einrichtung zu vereinfachen.

Zuerst erstelle ich ein neues Verzeichnis für die MySQL-Konfigurationsdateien:
```Bash
mkdir mysql
```

Anschliessend habe ich eine neue Datei Namens "mysql-pv.yaml" erstellt und die erforderlichen Daten eingefügt:
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

Zunächst habe ich das Deployment-File erstellt:
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

Anschliessend habe ich as Service-File erstellt:

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

Mit den folgenden Befehlen stelle ich die YAML-Dateien bereit.

```bash
kubectl apply -f mysql-pv.yaml
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml
```

Die Services und Pods prüfe ich dann:

```bash
kubectl get pods
kubectl get services
```
![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/database-1.png)

Danach starte ich einen temporären MySQL-ClientPod und verbinde mich mit der Datenbank.

```bash
kubectl run -it --rm --image=mysql:5.7 --restart=Never mysql-client -- mysql -h mysql -p
```

Mit diesem Befehl kann man sich dann auf die Datenbank connecten:
```bash
kubectl exec -it <mysql-client-pod-name> -- mysql -u root -p
```

# MediaServer konfigurieren
Ich habe meinen persönlichen Mediaserver aufgesetzt, welcher auf der Google Cloud läuft. Dieser ist nützlich, um img und Videos in der Cloud zu speichern. Das spezielle daran ist, dass die img privat gespeichert werden und nicht für jeden zugänglich sind. Dazu habe ich für meine Orientierung einen Netzplan erstellt. Der Mediaserver wird in einer virtuellen Maschine betrieben, welche in der Google Cloud aufgesetzt wurde. Der Zugriff funktioniert über eine ssh-Verbindung. **Es ist jedoch wichtig zu beachten, dass der Mediaserver nicht in einem Docker Container umgesetzt wurde. Dies war zuerst so geplant, doch es stellte sich heraus, dass es nicht viel Sinn machen würde.**

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/Netzwerkplan-emby.png)

**Projektname: mediasrvm300**
Root-User Passwort: xxx

**Schritt 1:** Als erstes erstelle ich eine neue VM-Instance in der Google Cloud.
- **Name:** emby
- **Region:** europe-west1
- **Zone:** europe-west1-b
![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web1.png)

Machine Type: e2-standard-2 (2 vCPU, 1 core, 8GB memory)

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web2.png)

**Schritt 3:** Die Boot-Disk musste ich mit Ubuntu konfigurieren, da der Service mit Linux arbeitet. Anschliessend musste ich noch HTTP und HTTPS akzeptieren.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web3.png)

Nun sehe ich, dass die Virtuelle Maschine erstellt wurde.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web4.png)

**Schritt 4:** Zunächst erstelle ich eine Verbindung mit der SSH-Cloud-Shell.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web5.png)

Ich aktualisiere Ubuntu.
```bash
sudo apt-get update
```

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web6.png)

```bash
sudo apt-get upgrade
```

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web7.png)

Nach der aktualisierung von Ubuntu lade ich die Ubuntu-Anwendung herunter.
```bash
wget https://github.com/MediaBrowser/Emby.Releases/releases/download/4.8.8.0/emby-server-deb_4.8.8.0_amd64.deb
```

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web8.png)

Nach der Installation musste ich ein Passwort für den Root-User setzen.
```bash
sudo passwd root
```

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web9.png)

Nun konnte ich mich einloggen.
```bash
su root
```

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web10.png)

Ich erstelle einen neuen Benutzer.
```bash
dpkg -i emby-server-deb_4.8.8.0_amd64.deb
```

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web11.png)

**Schritt 4:** Ich erstelle eine neue Firewall-Rule und nenne sie embyrule mit folgender Konfiguration:

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web12.png)

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web13.png)

Nachdem ich die Firewall-Rule erstellt habe sieht man diese in der Übersicht.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web14.png)

**Schritt 5:** Ich gehe zurück zur VM-Instance und klicke auf "Edit".

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web15.png)

Bei Netzwork-Tags füge ich die neue Firewall-Rule hinzu.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web16.png)

**Schritt 6:** Anschliessend kann ich den Media-Server, welcher in der Cloud betrieben wird in meinem Browser öffnen.
**URL:** http://35.195.66.149:8096

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web17.png)

Als nächstes habe ich einen neuen Benutzer erstellt. Dieser Benutzer ist für die administration des Mediaservers notwendig.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web18.png)

"Next"

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web19.png)

"Next"

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web20.png)

"Next"

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web21.png)

Nun kann ich mich mit dem zuvor erstellten Benutzer anmelden.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web22.png)

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web23.png)

**Schritt 7:** Ich erstelle ein neues Verzeichnis. In diesem Verzeichnis werden die Dateien später hochgeladen.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web24.png)

Danach erstelle ich eine neue Bibliothek.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web25.png)

Anschliessend auf "Add" klicken, um eine neue hinzuzufügen.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web26.png)

In dem Menü muss man unter Folder den Pfad zum erstellten Ordner eintragen.
In meinem Fall wäre das "/home/marentonizda/hmovies"

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web27.png)

Nun sieht man die neue erstellte Bibliothek.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web28.png)

**Schritt 8:** Als nächstes generiere ich einen SSH-Key und speichere den Schlüssel in dem Ordner "C:\Users\Joels\.ssh\embly.pub"

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web29.png)

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web30.png)

Anschliessend kopiere ich den Schlüssel in die Google Cloud VM Instance.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web31.png)

**Schritt 9:** Als nächstes öffne ich die Anwendung "FileZilla".

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web32.png)

Im Server Manager muss ich die erstellte Google Cloud VM Instance hinzufügen.
**Wichtige Daten:**
ProtokolL: SFTP (SSH File Transfer Protocol)
Host: 35.195.66.149 (External IP-Adresse von der VM)
Schlüsseldatei: C:\Users\joels\.ssh\embly

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web33.png)

Nach dem abschliessen sehe ich, dass die Verbindung funktioniert hat.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web34.png)

Für den Transfer der Medien habe ich 3 img heruntergeladen und diese von meinem lokalen Downloads Ordner in den Ordner vom Media Server geladen.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web36.png)

**Schritt 10:** Nach der erfolgreichen Übertragung konnte ich prüfen, ob die Daten auch wirklich in den MediaServer geladen wurden.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/web37.png)

Die Medien können erfolgreich auf den MedienServer geladen werden. Der Service ermöglicht es, Medieninhalte effizient zu verwalten und von überall darauf zuzugreifen, was ihn zu einer idealen Lösung für moderne Medienverwaltung und -bereitstellung macht.

**SSH Public Key**

```bash
SSH Public Key: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCtsQ4IzsIfa87cJtE+oemYh2TatFWw2VhoPCW2qREx8eRbw5L3J1JGHPrTwAVspzG+Z55y4sZAkc6gB0KP2xAsk8X6HA0AEgRHhpVNafUX5VqeP97d67lZsyVPsTVcJbgc4Rt/yUOq3vjaOv2KTVpa+hkyPgJX6Spquy7iqopHgctY8oLE5VBY/kpPFrq3zSFrCbWNiVWNndmfHC2DJS4vD/Mvp7URztpK9O238GsYsCZT4nXNbbPaKGDt+zPTMA79P4ePC1wKAmeWlJup2HwJiAAGdCiOgV9UqHkbWD4DLr/1hBvrSWcAxtHVMQjoKuUhcr1AAei943pkwTDlQW0yh27wQh9VWdpm57HHVkICmP4+cSw9jYAhVFf8cWtLrNQAN42YvOH3MppSbg7vcUPiSEghoiy1xYzq2TsaoQL1wPuwuyF6EhavHvirlh3PpKFbvQJGvrV7f9xEwyjejKKz9tTZyRKOgQ0f0Oj1yoejV/eX1yn8J/0OLbbKCR7aIQc= marentonizda
```

------------------------

# Minecraft Server auf Google Cloud betreiben.
![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/Netzwerkplan-Minecraft1.png)


In diesem Projekt wird ein Minecraft-Server in der Google Cloud gehostet und verwaltet. Der Server wird innerhalb einer virtuellen Maschine bereitgestellt und kann über eine SSH-Verbindung administriert werden. Die gesamte Konfiguration und Bereitstellung des Servers erfolgt codebasiert, was eine einfache Migration ermöglicht.

**Virtuelle Instanz vorbereiten**
Name: minecraft-server

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/CreateVM.png)

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/CreateVM1.png)


**Disk Formatting and Mounting**
Neues Verzeichnis erstellen: 
```bash
sudo mkdir -p /home/Minecraft
```

Disk formatieren: 
```bash
sudo mkfs.ext4 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/disk/by-id/google-minecraft-disk
```

Disk mounten: 
```bash
sudo mount -o discard,defaults /dev/disk/by-id/google-minecraft-disk /home/Minecraft
```
 
**Server herunterladen und aufsetzen:**
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

## Konfiguration der einzelnen Einstellungen
Im "server.properties" File kann man bestimmte Regeln für den Minecraft-Server festlegen, wie z.B den Schwierigkeitsgrad, die Uhrzeit oder die maximale Spieleranzahl.
```bash
nano server.properties
```

Das File sieht wie folgt aus:

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/properties2.png)

## Firewall Regel erstellen
Nachdem der Minecraft Server erfolgreich erstellt wurde muss ich eine Firewall-Regel erstellen. Ohne diese Regel können keine Spieler auf den Minecraft Server connecten. Der Port muss geöffnet werden, damit der Server public ist. Die Regel wird in folgenden Schritten erstellt:

Ich habe den zuvor erstellten Tag "minecraft-server" eingetragen und die source IP Range auf 0.0.0.0/0. Anschliessend habe ich den Port 25565 gesetzt. Dieser ist später wichtig für die Verbindung zum Minecraft Server.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/Firewall.png)

Nach der Erstellung der Firewall konnte ich mit dem Testing fortfahren, da alle nötigen Schritte nun durchgeführt wurden.

## Testing
Als erstes habe ich auf der Website https://mcsrvstat.us/ nachgeschaut, ob der Server im Minecraft-Universum erreiechbar ist.

**Server-Adresse:** 34.175.175.3:25565
**Version:** 1.20.2

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/TestingMC1.png)

Nachdem dies erfolgreich funktionierte habe ich Minecraft gestartet. Dort habe ich mich auf den Server verbunden. Dies war erfolgreich und ich konnte mich frei im Server bewegen. Für die ausführliche nachvollziehung habe ich ein Video aufgenommen.

**Video Vorschau**
![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/TestingMC3.mp4)

## Minecraft Server Monitoring
Um den Traffic des Minecraft Servers zu überwachen, musste ich einige Schritte vorgehen. Zuerst aktualisierte ich die Paketliste des Paketmanagers, um sicherzustellen, dass die neuesten Versionen der Pakete und deren Abhängigkeiten installieren kann.

```bash
sudo apt-get update
```

Anschliessend habe ich das Installationsskript für das Hinzufügen des Google Cloud Operations Suite Agent Repository heruntergeladen.

```bash
curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
```

Als nächstes Führte ich das heruntergeladene Installationsskript aus, um das Google Cloud Operations Suite Repository hinzuzufügen und gleichzeitig den Ops Agent zu installieren.

```bash
sudo bash add-google-cloud-ops-agent-repo.sh --also-install
```

Ich aktualisierte die Paketlisten erneut.

```bash
sudo apt-get update
```

Anschliessend installiere ich den Google Cloud Ops Agent, der für die Überwachung und das Logging meiner VM zuständig ist.

```bash
sudo apt-get install google-cloud-ops-agent
```

Danach konnte ich den Google Cloud Ops Agent starten, damit er mit der Überwachung meiner VM beginnen kann.

```bash
sudo service google-cloud-ops-agent start
```

Zum Schluss überprüfte ich den Status des Google Cloud Ops Agent, um sicherzustellen, dass er ordnungsgemäss läuft und keine Probleme vorliegen.

```bash
sudo service google-cloud-ops-agent status
```

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/Monitoring1.png)

Im Monitoring sieht man die wichtigsten Daten. Im GUI der Google Cloud kann man folgende Informationen vom Diagramm auslesen:

**1. bytes_used**
Überwacht den Speicherverbrauch meines Servers. Ein hoher Speicherverbrauch könnte auf viele geladene Welten, viele aktive Spieler oder viele installierte Plugins hinweisen. Bei meinem Fall wird nicht viel Speicher verbraucht, da auch keine Spieler auf dem Server aktiv sind.

**2. load_1m**
Diese Statistik gibt Informationen darüber, wie ausgelastet der Server ist. Eine solche Last wie 0.188 bedeutet z.B, dass der Server bei weitem nicht überlastet ist.

**3. tcp_connections**
Überwacht die Anzahl der Verbindungen, die zu meinem Server bestehen. Diese könnte die Anzahl der Spieler zeigen, die momentan auf dem Server spielen oder andere Verbindungen, wie z.B von Verwaltungstools.

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/Monitoring2.png.png)

# Containerisierung des Minecraft Server
Die Containerisierung des bestehenden Minecraft Servers bringt mehrere Vorteile mit sich, wie z.B:
- Portabilität
- Isolierung
- Ressourceneffizienz
- Skalierbarkeit
- Schnelle Bereitstellung

Die Umgebung sieht nach der Containerisierung wie folgt aus:

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/Netzwerkplan-Minecraft-Docker.png)

**Erste Schritte zur Vorbereitung von Docker:**
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker

Dockerfile erstellen im Verzeichnis /home/Minecraft mit folgendem Inhalt:
```bash
# Basisimage verwenden
FROM openjdk:16-slim

# Arbeitsverzeichnis im Container setzen
WORKDIR /Minecraft

COPY server.jar .

EXPOSE 25565

# Minecraft-Server starten beim Start des Containers
CMD ["java", "-Xmx2G", "-Xms1G", "-jar", "minecraft_server.jar", "nogui"]
```

Dockerfile editieren.
```bash
nano dockerfile
```

Nun starte ich Docker.
```bash
docker build -t minecraft-server .
```

Dockervolume erstellen.

```bash
sudo docker volume create minecraft-data
```

Funktioniert nicht!
```bash
docker run -d -p 25565:25565 --name minecraft-server -v minecraft-data:/minecraft minecraft-server
```

**Projekt konnte nicht fertiggestellt werden**

# Allgemein

## Änderungen im GitLab in die GKE Pushen
1. Änderungen im GitLab IDE Commiten.
2. Command "git pull origin master" eingeben
3. npm start
4. http://localhost:3000

![Alt text of the image](https://github.com/JoelIzDa/Google-Cloud-Project/blob/main/img/React-Anwendung-Json.png)

## Übersicht nützliche Befehle mit Kubernetes
Um die Konfiguration des Klusters anzuzeigen:
```Bash
cat .kube/config
```
 oder
```Bash
kubectl config view
```

Cluster Informationen anzeigen lassen:
```Bash
kubectl cluster-info
```

CPU, Memory Auslastungen:
```Bash
kubectl top nodes
```

Einen Pod erstellen:
```Bash
kubectl run nginx-1 --image nginx:latest
```

Laufende Pods anzeigen lassen:
```Bash
kubectl get pods
```

Informationen vom Pod anzeigen lassen:
```Bash
kubectl describe pod nginx-1
```

Cluster löschen:
```Bash
gcloud container clusters delete "cluster_name"
```

---

# Schlusswort
In dieser Arbeit habe ich wertvolle Erfahrungen in der Google Cloud gesammelt. Die einzelnen Projekte ermöglichten mir einen tiefen Einblick in verschiedene Aspekte der Google Cloud Plattform, sowie in moderne Entwicklungsverfahren

Durch das Aufsetzen und Verwalten der Google Kubernetes Engine habe ich ein fundiertes Verständnis für die Einrichtung und Verwaltung von Kubernetes-Clustern entwickelt. Ich habe gelernt, wie man Anwendungen containerisiert, in einem Cloud-Umfeld bereitstellt und die grundlegenden Werkzeuge zur Überwachung und Fehlerbehebung einsetzt.

Die Verwaltung des React-App-Services innerhalb des Kubernetes Clusters lehrte mich wichtige Praktiken für die Überwachung und Wartung von Cloud-Anwendungen. Ich erlernte, wie man den Status von Pods überprüft und Probleme im Betrieb einer Anwendung effizient identifiziert und behebt.

Die Erstellung und Verwaltung einer MySQL-Datenbank innerhalb des Kubernetes Clusters vermittelte mir Kenntnisse in der Konfiguration und Sicherstellung von Datenpersistenz sowie in der Verwaltung relationaler Datenbanken in einer Cloud-Umgebung.

Die Konfiguration eines MediaServers zur Verwaltung von Medieninhalten zeigte mir, wie man Cloud-Dienste für spezielle Anwendungsfälle konfiguriert und verwaltet, und vermittelte mir Fähigkeiten in der Installation und Einrichtung von Server-Software sowie in der Verwaltung von Benutzerkonten und Medienbibliotheken.

Schliesslich bot das Einrichten und Containersieren eines Minecraft Servers auf Google Cloud eine praktische Herausforderung, die mir ermöglichte, meine Fähigkeiten in der Serverkonfiguration und Containerisierung weiterzuentwickeln. Obwohl die Migration des Servers in die Google Kubernetes Engine letztlich nicht erfolgreich war, half mir dieser Prozess, ein tieferes Verständnis für die Komplexität von API-Berechtigungen und Fehlersuche in Cloud-Umgebungen zu erlangen.

Insgesamt habe ich durch diese Arbeit nicht nur technische Fähigkeiten und Kenntnisse in der Cloud-Entwicklung erlangt, sondern auch ein besseres Verständnis für die Herausforderungen und Lösungen in der Verwaltung moderner Cloud-Infrastrukturen gewonnen. 

---

