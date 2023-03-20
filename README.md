# Smarter Briefkasten
Diese ReadMe File dient als Anleitung zur Konstruktion eines smarten Briefkastens mit einem Arduino Nano BLE (Sense) und einem MacOs Computer. 
Die Anleitung ist in die Punkte, Einleitung, Hardware & Software, Installation, Datenerhebung, Training des Modells und Ausführung aufgeteilt.
Das Ergebnis dieser Anleitung ist ein funktionierendes Tiny Machine Learning Modell auf einem Arduino Nano BLE 33 (Sense), dass durch eigenständige Anbringung an einen Briefkasten, die im Punkt "Einleitung" beschriebende Funktionalität aufweist.

## 1. Einleitung
Im Rahmen des Moduls "Wissensmanagement" der HTW Berlin wurde ein smarter Briefkasten entworfen, der dem/der Nutzer/in über eine LED mitteilt, ob sich Post in dem Briefkasten befindet oder nicht. Dabei wurde mithilfe eines Arduino Nano BLE 33 (Sense) ein Tiny Machine Learning Modell trainiert, welches auf die Ereignisse Posteinwurf & Postentnahme reagiert. Hierbei leuchtet die LED bei dem Ereignis Posteinwurf auf, während bei der Postentnahme die LED aufhört zu leuchten.
Dies hat den Zweck, dass sich der/die Nutzer/in des Briefkastens im Falle von einem leeren Briefkasten Zeit sparen kann, da er/sie den Briefkasten nicht mehr öffnen muss wenn sich keine Post im Briefkasten befindet.

## 2. Hardware & Software
In diesem Abschnitt werden zu Beginn, die benötigten Hardware- & Softwarekomponenten aufgelistet, die in dem Projekt verwendet wurden.

### 2.1. Hardware
- Arduino Nano BLE 33 (Sense)
- Computer (MacOs)
- Micro USB Stromkabel
- Stromquelle
- Breadboard
- (Blaue) LED für das Breadboard
- Ein Kabel für das Breadboard (Erdung)
-  Einen Widerstand (220 Ohm (Rot Rot Braun))

### 2.2. Software & Tools
- Arduino IDE
- Terminal
- Node.js
- Edge Impulse (als Datenerhebungsoberfläche) 

## 3. Installation
In diesem Abschnitt wie die Software und Tools für das Projekt eines smarten Briefkasten installiert wurden. Dies wird für Übersichtlichkeit in Stichpunkten beschrieben.

### 3.1. Arduino IDE (Version 2.0.4 oder höher)
Beschreibung: Mit der Arduino IDE kann man Code für Arduino Mikrocontroller schreiben und hochladen, um elektronische Projekte zu erstellen und zu automatisieren.

- Download der Software
- Öffnen des Programs
- In der Taskleiste: Tools > Board > Board Manager > Nach "Arduino Mbed OS Nano Boards" suchen > "Arduino Mbed OS Nano Boards" installieren
- Arduino Nano BLE 33 mit Computer per USB verbinden
- Wenn per USB verbunden: Tools > Port und Arduino auswählen ( "/dev/cu.usbmodem14112" (Arduino Nano BLE 33) )

### 3.2. Brew 
Beschreibung: Mit Brew (auch bekannt als Homebrew) kann man unter macOS zusätzliche Softwarepakete und Tools installieren und verwalten, die nicht standardmäßig im Betriebssystem enthalten sind. Wenn Brew noch nicht installiert ist, kann dies über folgenden Link getan werden. (https://brew.sh/)

- Terminal aufrufen
- brew install arduino-cli im Terminal ausführen (Nun kann man Kommandos an den Arduino senden)

### 3.3. Node.js (14 oder höher)
Beschreibung: Node.js ist eine JavaScript-Laufzeitumgebung, mit der man serverseitige Anwendungen entwickeln und ausführen kann.
Es wird für NPM Installationen der Edge Impulse Tools, benötigt.

- Terminal aufrufen
- npm install -g edge-impulse-cli --force im Terminal ausführen

### 3.4. Edge Impulse
Mit Edge Impulse können Entwickler Machine-Learning-Modelle für die Verarbeitung von Sensor- und IoT-Daten erstellen, trainieren und bereitstellen.

- Account erstellen (https://studio.edgeimpulse.com/studio/profile/projects)
- Projekt erstellen

## 4. Datenerhebung
In diesem Abschnitt wird die Datenerhebung über Edge Impulse mithilfe vom Arduino Nano BLE 33 (Sense) beschrieben.
Um ein ML Modell zu trainieren, werden Daten benötigt. Um die Daten zu sammeln, muss der Arduino zu Beginn mit Edge Impulse verbunden werden.
Dies geschieht, indem der Arduino das Projekt aus 3.4. Edge Impulse angesteuert. 
Das Modell muss insgesamt 3 Variationen klassifizieren können. Den Posteinwurf, die Postentnahme und das Ereignis, dass nichts geschieht.

### 4.1. Um den Arduino mit Edge Ipulse zu verbinden, muss dabei zu aller erst, 
1. der Arduino mit dem Laptop per Kabel verbunden werden, sodass der Arduino leuchtet.
2. der Terminal auf dem Laptop geöffnet werden.
3. der Knopf auf dem Arduino gedrückt werden, sodass der Arduino orange aufleuchtet.
4. die Engabe "edge-impulse-daemon" ausgeführt werden. (im Terminal)
5. die E-Mail und das Passwort des Accounts von Edge Impulse eingegeben werden. (im Terminal)
6. das Projekt ausgewählt werden, unter welchem die Daten gesammelt werden sollen. (im Terminal)

Nun kann der Arduino über den Reiter "Data Acquisition" in Edge Impulse angesteuert werden und das Modell mit Daten gefüttert werden.
Der Name, der den Datensätzen gegeben wird, ist gleichzeitig das Label für das Training. Das heißt: Wird das Ereignis "Posteinwurf" genannt, so wird unter dem Label "Posteinwurf" der Datensatz gespeichert und erkannt. 

### 4.2. Aufnahme der Daten in Edge Impulse > Data Acquisition > Record new Data.
1. Sensor in Edge Impulse auf "Internal" Sensor stellen. (dieser misst die Bewegung, das Magnetfeld und die Geschwindigkeit des Arduinos) 
2. Zeitraum der Aufnahme der Daten auf 5 Sekunden stellen.
3. Label festlegen (Posteinwurf, Postentnahme, Idle).
4. Arduino am besten wie im Bild anbringen: ![IMG_5491](https://user-images.githubusercontent.com/128368064/226336569-3c9ff4e2-a47b-458c-9c96-34a20976522a.jpg) (Dies kann jedoch je nach Briefkasten anders aussehen)
5. Auf "Sample" klicken.
6. Bewegung des ausgewählten Labels imitieren. 
7. Wiederholung bis 5 Minuten an Daten pro Label vorhanden sind. 
8. Wiederholung für die anderen beiden Label.

## 5. Training des Modells
In diesem Abschnitt wird das Training des Modells mithilfe der aufgenommenen Daten in Edge Impulse genauer erklärt. 

### 5.1. Create Impulse
Über Edge Impulse > Impulse Design > Create Impulse kann das Modell nun trainiert werden. 

Zu aller erst wird,
1. Time Series gewählt
2. die Window size auf die Zeit der Datenblöcke gesetzt (in diesem Fall 5 Sekunden). 

Danach werden,
3. in der Spektralanalyse die Features ausgewählt, die von Bedeutung sind. (Hierfür ist es zu empfehlen sich die aufgenommenen Daten noch ein Mal genauer unter die Lupe zu nehmen. In diesem Fall hatte das Modell im Livetest am Briefkasten die beste Konfiguration, wenn nur die Gyroskopdaten genutzt wurden. Die Magnetfelddaten und Beschleunigungsdaten waren für die Vorhersage nicht hilfreich.)

Zum Schluss wird,
4. für die Modellerstellung, die Klassifikation gewählt, da eine Zuordnung der drei Klassen erreicht werden soll.
5. durch "Save Impulse" wird der Impulse gespeichert. 

<img width="1153" alt="Bildschirmfoto 2023-03-20 um 13 38 37" src="https://user-images.githubusercontent.com/128368064/226341146-5ff8f0e6-567e-4d84-b318-303f467b0d64.png">

### 5.2. Spectral features
Über Edge Impulse > Impulse Design > Spectral features können die Features (Posteinwurf, Postentnahme, Idle) des Modells nun noch angepasst werden. 
In diesem Fall wurde mit den Default Einstellungen gearbeitet. Demenstprechend wurde an den Einstellungen nichts geändert.
Durch den Klick auf "Save Parameters" werden die Parameter erstellt. Dies kann ein paar Minuten dauern.

### 5.3. Classifier
Über Edge Impulse > Impulse Design > Classifier kann nun das Training beginnen. 
In diesem Fall wurde mit den Default Einstellungen gearbeitet. Demenstprechend wurde an den Einstellungen nichts geändert.
Durch den Klick auf "Start Training" wird das Modell trainiert. Dies kann ein paar Minuten dauern.

## 6. Ausführung
Im letzten Schritt wird der Code des trainierten Modells aus Edge Impulse als ZIP File exportiert und in die Arduino IDE eingepflegt. Nach ein paar kleineren Anpassungen im Code kann das Modell auf den Arduino geladen und letztendlich ausgeführt werden.

Dies erfolgt wie folgt:
1. Über Edge Impulse > Deplyoment > Arduino Library auswählen.
2. Runterscrollen und auf "Build" klicken. (ZIP Datei wird erstellt und heruntergeladen)
<img width="635" alt="Bildschirmfoto 2023-03-20 um 14 18 40" src="https://user-images.githubusercontent.com/128368064/226354730-60ecf580-6258-4107-bc44-1a04a30db8e9.png">
3. In die Arduino IDE wechseln. 
4. In der Taskleiste unter Sketch > Include library > Add .ZIP Library... > "deine erstellte ZIP File aus 2" auswählen.
5. In der Taskleiste unter Files > Examples > "deine erstellte Library File" auswählen > nano_ble33_sense > nano_ble33_sense_accelerometer.
6. Der Code wurde erstellt.

Nun ist der Code für das Modell in der Arduion IDE. Das Problem dabei ist jedoch, dass der Code momentan nur die Accelerometer Daten ausliest und noch noch keine LED bei einem gewissen Ereignis ansteuert. 
Deshalb wird in den folgenden Zeilen zusätzlich der folgende Code hinzgefügt oder abgeändert.




