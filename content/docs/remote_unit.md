---
title: 'Unità Remote'
date: 2024-11-28
weight: 2
---

Le **unità remote (UR)** sono tutti i moduli collegati mediante
protocollo MQTT, TCP/IP o seriale che si occupano dell'acquisizione
dei dati dai sensori e dell'invio delle misure all'unità
centrale. Generalmente, esse sono costituite da un Arduino e da uno o
più sensori connessi.


<!--more-->


## Note tecniche
Ecco alcune note tecniche sulle varie componenti delle unità remote.

### Design low-power Arduino
Viste le scarse risorse energetiche che _vogliamo_ allocare ai moduli
remoti, può essere saggio ridurre il consumo dell'Arduino al minimo
sindacale. Arduino fornisce alcune linee guida sul design low-power.

Alcuni riferimenti:
 - [The Arduino Guide to Low Power Design](https://docs.arduino.cc/learn/electronics/low-power/)

#### Batterie Arduino
Alcuni riferimenti:
 - [Notes on using batteries with the Arduino MKR series](https://docs.arduino.cc/tutorials/mkr-wifi-1010/mkr-battery-app-note/)


### Anemometro
Alcuni riferimenti:
 - [Advancement of the Ultrasonic Anemometer - DL1GLH](https://www.dl1glh.de/ultrasonic-anemometer.html#advancement)


