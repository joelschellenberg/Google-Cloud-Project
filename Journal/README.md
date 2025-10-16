# Journal
Dieses Journal wird wöchentlich geführt. Es wird über die Fortschritte vom Projekt berichtet und Probleme werden bekannt gegeben.

## Inhaltsverzeichnis
[TOC]

---

### Lektion 1 (17.05.2024)

#### Tagesziele:
- Brainstorming
- AWS Academy Learner verstehen

#### Brainstorming
Nach der kurzen Einführung habe ich mich im GitLab nach den verschiedenen Themen erkundigt. Anschliessend überlegte ich mir, was ich aufsetzen will. Da ich mit AWS noch wenig Erfahrung habe, wusste ich am Anfang nicht, was ich dort einrichten könnte. Ich habe mich dann an unserem Tisch ein wenig erkundigt und kam auf eine Idee. Mein Ziel ist es ein DomainContoller aufzusetzen und ein File-Server etc. einzurichten. Für den genauen Projektbeschrieb hatte ich leider noch keine Zeit. Ich habe dann mein LAB gestartet und die Konsole erkundigt.

#### AWS
Wie bereits beschrieben, habe ich noch fast keine Erfahrung mit AWS und musste mich zuerst einlesen. Ich habe mich in der Konsole im Verlauf der Lektionen erkundigt und konnte einige Features entdecken. 

#### Probleme
Diese Woche traten bei mir noch keine Probleme auf.


### Lektion 2 (24.05.2024)
Abwesenheit

### Lektion 3 (31.05.2024)

#### Tagesziele
- Projekt neu definieren.
- Erste Schritte mit Google Cloud

#### Projekt neu definieren
Ich habe mich für ein neues Projekt entschieden. Ich werde das Projekt in der Google Cloud einsetzen und konfigurieren. Im Projekt beschäftige ich mich intensiv mit verschiedenen Aspekten des Cloud-Computings, insbesondere mit der Anwendungsentwicklung und bereitstellung auf der Google Cloud Platform (GCP). Hier sind einige der Schwerpunkte und Tutorials, die ich bearbeite:

- Ich erstelle eine Full-Stack-React-Anwendung auf GCP, indem ich den Schritt-für-Schritt-Anleitungen folge, um meine erste React-Anwendung mit Amplify und GraphQL aufzubauen.

- Des Weiteren lerne ich, wie ich Docker-Container auf Google Kubernetes Engine (GKE), dem Kubernetes-Dienst von Google Cloud, orchestriere und verwende, um meine Anwendungen effizient zu verwalten.

- Ein weiterer Schritt in meinem Projekt umfasst die Bereitstellung einer LAMP-Stack-Anwendung auf Google Cloud, ähnlich wie bei Amazon Lightsail, jedoch mit den Ressourcen und Diensten von GCP.

- Ausserdem richte ich eine Continuous Delivery-Pipeline auf Google Cloud ein, um automatisiertes Deployment und Testing meiner Anwendungen zu ermöglichen.

- Des Weiteren erkunde ich fortgeschrittene Themen wie die Entwicklung einer einfachen Webanwendung mit Docker-Containern, beispielsweise einer Blogplattform mit React, Node.js und einer relationalen Datenbank, sowie die Entwicklung einer Kubernetes-basierten Microservices-Anwendung, wie eines Online-Shops mit Microservices für Benutzerkonten, Produktkatalog und Bestellabwicklung, bereitgestellt auf GCP mit Kubernetes.

- Zusätzlich beschäftige ich mich mit fortgeschrittenen Themen wie der Einrichtung einer eigenen Container Registry, Sicherheitsanalysen, Monitoring und Logging für verschiedene Anwendungsfälle sowie Multi-Cloud-Deployment-Szenarien für eine Blogplattform, die Microservices auf GCP und möglicherweise anderen Cloud-Plattformen nutzt.

Insgesamt bin ich mit meinen Lernzielen zufrieden und denke, dass ich das Projekt gut abschliessen kann. Es ist mir aber klar, dass ich auf einige Probleme stossen werde, welche ich mit Recherchieren lösen kann. In den nächsten Lektionen werde ich mit den Aufgaben fortsetzen.

#### Erste Schritte mit Google Cloud
Ich habe begonnen, die verschiedenen Funktionen der GCP zu erkunden, darunter die Google Cloud Console, die als zentrale Plattform für die Verwaltung von Cloud-Ressourcen dient. Ich habe auch einen Blick auf die verschiedenen Dienste geworfen, die die GCP bietet, wie virtuelle Maschinen, Speicherlösungen und Datenbanken.

Durch diese Erkundungen konnte ich ein besseres Verständnis für die Funktionalität und die Möglichkeiten der GCP entwickeln.


### Freizeit

03.06.2024:
Die Änderungen, welche ich vorgenommen habe sind in der Anelitung zu sehe

---

### Lektion 4 (07.06.2024)

#### Tagesziele
- Erstellung des Verzeichnisses und der YAML-Dateien
- Erstellung des Deployment-Files für MySQL
- Erstellung des Service-Files für MySQL
- Bereitstellung der YAML-Dateien in Kubernetes
- Verbindung zum MySQL-Client-Pod herstellen und Konfiguration der MySQL-Datenbank

#### Erstellung des Verzeichnisses und der YAML-Dateien
Zuerst habe ich ein neues Verzeichnis für die MySQL-Konfigurationsdateien erstellt, um die Struktur zu organisieren. Anschliessend habe ich die PersistentVolume (PV) und PersistentVolumeClaim (PVC) YAML-Dateien erstellt, um persistenten Speicher für die MySQL-Datenbank bereitzustellen.

#### Erstellung des Deployment-Files für MySQL
 Danach habe ich das Deployment-File für MySQL erstellt, um den MySQL-Pod zu konfigurieren. Dabei habe ich sicherstellen müssen, dass das Passwort für den Root-Benutzer korrekt als Umgebungsvariable festgelegt wurde.

#### Erstellung des Service-Files für MySQL
Nachdem das Deployment-File fertig war, ging ich zur Erstellung des Service-Files über, um den MySQL-Service zu definieren und die Kommunikation mit dem MySQL-Pod zu ermöglichen.

#### Bereitstellung der YAML-Dateien in Kubernetes
Mit den erstellten YAML-Dateien habe ich die Ressourcen in Kubernetes bereitgestellt, indem ich die Befehle `kubectl apply -f` verwendet habe, um die PV, PVC, das Deployment und den Service in Kubernetes zu implementieren.

#### Verbindung zum MySQL-Client-Pod herstellen und Konfiguration der MySQL-Datenbank
Als nächstes habe ich versucht, eine Verbindung zum MySQL-Client-Pod herzustellen, um die MySQL-Datenbank zu konfigurieren. Dabei habe ich den Befehl `kubectl run -it --rm --image=mysql:5.7 --restart=Never mysql-client -- mysql -h mysql -p` verwendet, um den temporären MySQL-Client-Pod zu starten und mich mit der Datenbank zu verbinden. 

#### Probleme
Trotz mehrerer Versuche konnte ich keine erfolgreiche Verbindung zur MySQL-Datenbank herstellen. Es gab Probleme bei der Authentifizierung, möglicherweise aufgrund eines Fehlers bei der Passwortkonfiguration oder beim Festlegen des Hostnamens. Ich werde mir am Wochenend dafür Zeit nehmen und den Fehler bestmöglich beheben.

#### Reflexion
Die Arbeit an der MySQL-Datenbank in Kubernetes stellte sich als herausfordernder heraus als erwartet. Trotz der vorherigen erfolgreichen Schritte bei der Einrichtung der Kubernetes-Ressourcen konnte ich keine Verbindung zur Datenbank herstellen. Es war frustrierend, dass ich das Problem nicht sofort lösen konnte. In Zukunft werde ich mehr Zeit für die Fehlerbehebung einplanen und möglicherweise zusätzliche Ressourcen zur Hilfe heranziehen, um ähnliche Probleme schneller zu lösen. Es war dennoch eine lehrreiche Erfahrung, die mir half, meine Fähigkeiten in der Kubernetes- und Datenbankverwaltung weiterzuentwickeln.

---

### Lektion 5 (14.06.2024)

#### Tagesziele
- Konfiguration eines MediaServers in der Google Cloud
- Installation und Einrichtung des Emby Media Servers
- Konfiguration von Firewall-Regeln und Netzwerk-Tags
- Verwaltung von Benutzerkonten und Medienbibliotheken
- Einbindung von SSH und SFTP für Dateiübertragungen

#### Tagesjournal

#### Mediaserver konfigurieren
Heute habe ich meinen Mediaserver in der Google Cloud konfiguriert. Der Server soll mir ermöglichen, Bilder und Videos privat in der Cloud zu speichern und zu verwalten.

#### Erstellung und Konfiguration der VM-Instanz
Ich habe eine neue VM-Instanz in der Google Cloud erstellt und konfiguriert. Die Auswahl der richtigen Region und Zone war hierbei entscheidend, um eine gute Performance zu gewährleisten.

#### Installation und Einrichtung des Emby Media Servers
Die Installation des Emby Media Servers verlief ohne grössere Probleme. Nach dem Herunterladen der Anwendung und der Einrichtung des Root-Users konnte ich mich erfolgreich auf dem Server einloggen und mit der weiteren Konfiguration fortfahren.

#### Konfiguration von Firewall-Regeln und Netzwerk-Tags
Ein wichtiger Schritt war die Erstellung und Konfiguration von Firewall-Regeln, um sicherzustellen, dass nur autorisierte Verbindungen auf den MediaServer zugreifen können. Diese Aufgabe war besonders wichtig, um die Sicherheit meines Servers zu gewährleisten.

#### Verwaltung von Benutzerkonten und Medienbibliotheken
Nachdem der Server eingerichtet war, habe ich Benutzerkonten für die Verwaltung des MediaServers erstellt. Zudem habe ich Bibliotheken angelegt, in denen die Mediendateien gespeichert und organisiert werden können.

#### Einbindung von SSH und SFTP für Dateiübertragungen
Um Dateien auf den MediaServer hochzuladen, habe ich SSH und SFTP eingebunden. Dies ermöglichte es mir, Dateien sicher und effizient von meinem lokalen Rechner auf den Server zu übertragen.

#### Probleme
Ein grösseres Problem trat bei der Konfiguration der Netzwerk-Tags auf. Zunächst wurden die neuen Firewall-Regeln nicht korrekt angewendet, was zu Verbindungsproblemen führte. Nach mehrmaligem Überprüfen und Anpassen der Einstellungen konnte ich dieses Problem jedoch lösen.

Ein weiteres Hindernis war die Installation der erforderlichen Pakete und deren Abhängigkeiten auf der VM. Einige Pakete liessen sich nicht direkt installieren, was zusätzliche Recherchen und manuelle Installationen erforderlich machte.

#### Reflexion
Die heutige Arbeit an der Konfiguration des MediaServers war sowohl herausfordernd als auch lehrreich. Ich konnte die Zeit sinnvoll nutzen und war voll im flow. Dies führte aber dann schlussenldich dazu, dass ich die Dokumentation am Wochenende nachholen musste und keine Zeit mehr für das Tagesjournal hatte.

---

### Lektion 6 (21.06.2024)

#### Tagesziele
- Erstellen eines Minecraft Servers in der Google Cloud
- Vorbereitung der virtuellen Instanz
- Disk-Formatierung und -Mounting
- Herunterladen und Aufsetzen des Minecraft Servers
- Nutzung von Screen für die Serverves

#### Erstellen eines Minecraft Servers in der Google Cloud
Heute habe ich einen Minecraft Server in der Google Cloud erstellt. Dieser Server soll es mir ermöglichen, ein privates Multiplayer-Spielerlebnis zu hosten und zu verwalten.

#### Vorbereitung der virtuellen Instanz
Ich habe eine virtuelle Instanz namens `minecraft-server` erstellt und die notwendigen Einstellungen vorgenommen, um sicherzustellen, dass der Server genügend Ressourcen für eine optimale Performance hat.

#### Disk-Formatierung und -Mounting
Für die Speicherung der Spieldaten und zukünftiger Mods habe ich ein neues Verzeichnis erstellt, die Disk formatiert und gemountet, um sicherzustellen, dass ausreichend Speicherplatz verfügbar ist.

#### Herunterladen und Aufsetzen des Minecraft Servers
Nach der Vorbereitung der Instanz habe ich den Minecraft Server heruntergeladen und erfolgreich aufgesetzt. Dies umfasste die Installation der notwendigen Software und das Starten des Servers.

#### Nutzung von Screen für die Serververwaltung
Um den Server auch im Hintergrund laufen zu lassen und jederzeit wieder darauf zugreifen zu können, habe ich Screen installiert und verwendet. Dies erleichtert die Verwaltung und Überwachung des Servers.

#### Konfiguration der Server-Einstellungen
In der Konfigurationsdatei des Servers habe ich wichtige Einstellungen wie den Schwierigkeitsgrad, die maximale Spieleranzahl und andere Spielregeln angepasst, um das Spielerlebnis individuell zu gestalten.

#### Erstellung von Firewall-Regeln zur Absicherung des Servers
Um den Server vor unautorisierten Zugriffen zu schützen, habe ich spezifische Firewall-Regeln erstellt. Diese Regeln ermöglichen es nur bestimmten IP-Adressen, auf den Server zuzugreifen, und sichern somit die Verbindung der Spieler.

#### Probleme
Ein grösseres Problem trat bei der Formatierung und dem Mounten der Disk auf. Anfangs erkannte das System die Disk nicht korrekt, was zu Verzögerungen führte. Nach einigen Anpassungen und erneuten Versuchen konnte ich das Problem jedoch lösen.

Ein weiteres Hindernis war die Einrichtung von Fabric. Einige Abhängigkeiten verursachten Installationsprobleme, die durch zusätzliche Recherchen und manuelle Anpassungen behoben werden mussten.

#### Reflexion
Die Arbeit an der Einrichtung des Minecraft Servers war sowohl herausfordernd als auch lehrreich. Ich konnte viele neue Fähigkeiten im Umgang mit der Google Cloud und der Serververwaltung erlernen. Besonders erfreulich war es, den Server schliesslich erfolgreich zum Laufen zu bringen und die Konfiguration nach meinen Vorstellungen anzupassen. Die auftretenden Probleme haben mir gezeigt, wie wichtig Geduld und sorgfältiges Arbeiten bei der Serveradministration sind. Insgesamt war der Tag sehr produktiv, und ich bin mit dem Ergebnis zufrieden.

### Lektion 7 (28.06.2024)
#### Tagesziele
- Connection Problem lösen
- Dokumentation erfassen
- Brainstorming Minecraft Server Erweiterung

#### Dokumentation erfassen
Ich habe Heute noch einige Anpassungen in meinem Git-Repo vorgenommen.

### Brainstorming Minecraft Server Erweiterung
Ich habe mir Heute überlegt, dass ich den Minecraft Server in einen Container migrieren könnte. Ich denke, dass es eine spannende Aufgabe für die nächste Lektion wäre,

#### Probleme
Heute hatte ich probleme mit dem connecten auf den Server. Folgende Fehlermeldung kam auf:

<img src="https://gitlab.com/joelgit1/m300-rohr/-/raw/main/Bilder/TestingMC2.png?ref_type=heads" alt="Test">

**Fehler:** Nach einem Software-Update konnte der Minecraft-Server nicht gestartet werden und gab eine Fehlermeldung aus.

**Lösung:** Beim Überprüfen der Serverprotokolle stellte ich fest, dass die Serverdatei (server.jar) beschädigt oder nicht korrekt aktualisiert wurde. Ich habe die letzte funktionierende Version der Datei aus der Sicherungskopie wiederhergestellt und den Server neu gestartet, woraufhin er erfolgreich hochgefahren ist.

**Schritte zur Lösung:**
Als erstes habe ich die Serverprotokolle und Fehlermeldungen beim Startversuch des Minecraft-Server überprüft.

```bash
tail -n 100 /home/Minecraft/server.log
```

**Diagnose:** 
Ich konnte dann feststellen, dass die Datei server.jar nicht korrekt aktuelisiert wurde.

**Lösung:**
Als Lösung habe ich die letzte bekannte funktionierende Version des server.jar files aus der Sicherungskopie wiederhergestellt.

```bash
cp /home/minecraft/backup/server.jar /home/minecraft/server.jar
```

Dieser Befehl kopiert die Sicherungskopie der server.jar (server.jar.bak) zurück in das Hauptverzeichnis des Minecraft-Servers, um die beschädigte oder fehlende Datei zu ersetzen.

**Implementierung:**
Neustart des Minecraft-Servers nach der Wiederherstellung der Datei.

```bash
systemctl restart minecraft-server
```

Durch diesen Befehl wird der Minecraft-Server-Dienst neu gestartet, um die Änderungen in der server.jar zu übernehmen und den Server erfolgreich hochzufahren.

**Ergebnis:**
Nach der Implementierung der Lösung konnte der Minecraft-Server erfolgreich gestartet werden und ich konnte mich mit meinem Account auf den Server connecten.

#### Reflexion

### Lektion 8 (05.07.2024)
#### Tagesziele
- Minecraft Server migrieren
- Dokumentation verbessern
- Brainstorming

#### Minecraft Server migrieren
Heute wollte ich meinen Minecraft Server in einen Docker Container Migrieren. Alles funktionierte, bis ich mich authentifizieren musste. Es gab Probleme mit der API Rolle. Obwohl ich diese hinzugefügt habe, hatte ich keine Berechtigungen. Leider musste ich nach mehreren versuchen aufgeben. Ich habe mir vorgenommen, dass ich mich nochmals am Wochenende damit befassen werde. Diese Problemlösung hat mich sehr lange beschäftigt.

#### Dokumentation verbessern
Ich habe noch einen Projektbeschrieb erarbeitet, da ich diesen bis jetzt vergessen habe. Weitere Commandlines habe ich noch beschrieben und erkennbar gemacht. Das Projekt ist nun abgeschlossen!
