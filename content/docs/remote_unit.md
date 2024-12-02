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

## Design
Concettualmente, le UR sono standardizzate nella loro struttura. Esse
presentano quasi invariabilmente un Arduino (o microcontrollore
equivalente) capace di comunicare con l'UC via seriale, MQTT o TCP/IP,
uno o più sensori e un'adeguata fonte di alimentazione.

Un'eccezione è l'UR-GPS/IMU, che acquisisce i dati inerziali
dell'imbarcazione; necessariamente, essa è collocata in posizione
prossima al centro di massa del moth, dunque nello stesso luogo (e
contenitore) dell'UC. Perciò, non si tratta di un'unità *remota* in
senso stretto, visto che condivide la stessa batteria dell'UC ed è
collegata ad essa via seriale.

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


