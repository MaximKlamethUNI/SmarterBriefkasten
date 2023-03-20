# Smarter Briefkasten
Diese ReadMe File dient als Anleitung zur Konstruktion eines smarten Briefkastens mit einem Arduino BLE (Sense) und einem MacOs Computer. 
Die Anleitung ist in die Punkte, Einleitung, Hardware & Software, Installation, Datenerhebung, Datenaufbereitung und Ausführung aufgeteilt.
Das Ergebnis dieser Anleitung ist ein funktionierendes Tiny Machine Learning Modell auf einem Arduino BLE 33 (Sense), dass durch eigenständige Anbringung an einen Briefkasten, die im Punkt "Einleitung" beschriebende Funktionalität aufweist.

## Einleitung
Im Rahmen des Moduls "Wissensmanagement" der HTW Berlin wurde ein smarter Briefkasten entworfen, der dem/der Nutzer/in über eine LED mitteilt, ob sich Post in dem Briefkasten befindet oder nicht. Dabei wurde mithilfe eines Arduino BLE 33 (Sense) ein Tiny Machine Learning Modell trainiert, welches auf die Ereignisse Posteinwurf & Postentnahme reagiert. Hierbei leuchtet die LED bei dem Ereignis Posteinwurf auf, während bei der Postentnahme die LED aufhört zu leuchten.
Dies hat den Zweck, dass sich der/die Nutzer/in des Briefkastens im Falle von einem leeren Briefkasten Zeit sparen kann, da er/sie den Briefkasten nicht mehr öffnen muss wenn sich keine Post im Briefkasten befindet.

## Hardware & Software
In diesem Abschnitt werden zu Beginn, die benötigten Hardware- & Softwarekomponenten aufgelistet, die in dem Projekt verwendet wurden.

### Hardware
- Arduino Nano BLE (Sense)
- Computer (MacOs)
- Micro USB Stromkabel
- Stromquelle
- Breadboard
- (Blaue) LED für das Breadboard
- Ein Kabel für das Breadboard (Erdung)
-  Einen Widerstand (220 Ohm (Rot Rot Braun))

### Software & Tools
- Arduino IDE
- Terminal
- Node.js
- Edge Impulse (als Datenerhebungsoberfläche) 

## Installation
In diesem Abschnitt wie die Software und Tools für das Projekt eines smarten Briefkasten installiert wurden. Dies wird für Übersichtlichkeit in Stichpunkten beschrieben.

### Arduino IDE (Version 2.0.4 oder höher)
Beschreibung: Mit der Arduino IDE kann man Code für Arduino Mikrocontroller schreiben und hochladen, um elektronische Projekte zu erstellen und zu automatisieren.

- Download der Software
- Öffnen des Programs
- In der Taskleiste: Tools > Board > Board Manager > Nach "Arduino Mbed OS Nano Boards" suchen > "Arduino Mbed OS Nano Boards" installieren
- Arduino BLE 33 mit Computer per USB verbinden
- Wenn per USB verbunden: Tools > Port und Arduino auswählen ( "/dev/cu.usbmodem14112" (Arduino Nano BLE) )

### Brew 
Beschreibung: Mit Brew (auch bekannt als Homebrew) kann man unter macOS zusätzliche Softwarepakete und Tools installieren und verwalten, die nicht standardmäßig im Betriebssystem enthalten sind. Wenn Brew noch nicht installiert ist, kann dies über folgenden Link getan werden. (https://brew.sh/)

- Terminal aufrufen
- brew install arduino-cli im Terminal ausführen (Nun kann man Kommandos an den Arduino senden)

### Node.js (14 oder höher)
Beschreibung: Node.js ist eine JavaScript-Laufzeitumgebung, mit der man serverseitige Anwendungen entwickeln und ausführen kann.
Es wird für NPM Installationen der Edge Impulse Tools, benötigt.

- Terminal aufrufen
- npm install -g edge-impulse-cli --force im Terminal ausführen

### Edge Impulse
Mit Edge Impulse können Entwickler Machine-Learning-Modelle für die Verarbeitung von Sensor- und IoT-Daten erstellen, trainieren und bereitstellen.

- Account erstellen (https://studio.edgeimpulse.com/studio/profile/projects)


