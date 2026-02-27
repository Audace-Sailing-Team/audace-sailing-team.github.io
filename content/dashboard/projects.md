---
title: 'Progetti in corso'
date: 2025-11-19
weight: 4
---

Ogni progetto in corso richiede un luogo nel quale annotare idee, progetti, proposte e riflessioni. Chiunque prenda parte al processo di sviluppo di un progetto del reparto con qualcosa da dire è il benvenuto!
<!--more-->

# UC1: alimentazione
...WIP...

# UC2: RasPi e I2C
L'obiettivo del gruppo di lavoro è studiare la possibile rimozione dei microcontrollori dall'Unità Centrale, connettendo direttamente i sensori al Raspberry Pi. 
Un possibile approccio richiede la costruzione di un bus I2C (o una qualunque alternativa equivalente) che connetta i sensori ai GPIO del RasPi. A esso, va affiancata una interfaccia di comunicazione (*e.g.* `I2CInterface`) per `Communicator`, in Mothics; il suo scopo è l'acquisizione e decodifica dei dati ricevuti.

## Generalità su I2C
I2C è un protocollo seriale, multi-master, multi-slave, che richiede due linee dati:
 - SCL (serial clock): linea di clock, gestita dal master
 - SDA (serial data): linea dati, bidirezionale (master <-> slave)
Ogni dispositivo/sensore slave è dotato di un proprio indirizzo a 7 o 10 bit, non noto a priori al master.

In linea di massima, è possibile costruire una dorsale di comunicazione (bus) che connette tutti i sensori con il master. Esso dovrà procedere a 
 - ottenere gli indirizzi di tutti gli slave manualmente (inseriti dall'utente), o automaticamente con strumenti come `i2cdetect`
 - emettere la condizione di start e inviare a tutti i dispositivi la richiesta di lettura/scrittura corredata dell'indirizzo dello slave di interesse
 - ottenere l'`ACK` dal dispositivo con l'indirizzo corretto e leggere/scrivere i registri dello stesso
 - emettere la condizione di stop.

## Design del bus
...WIP...

### Sensori
#### GPS
Fra i sensori GPS a disposizione è presente l'Arduino MKR GPS, basato su u-blox SAM-M8Q GNSS. Fortunatamente, è disponibile la [schematica](https://docs.arduino.cc/resources/schematics/ASX00017-schematics.pdf). Il sensore verrà usato quasi esclusivamente per la prototipazione e il debugging; è ideale se utilizzato con un Arduino MKR, mentre non è la soluzione ottimale in qualunque altro caso.

Da quanto emerge dalla [documentazione](https://docs.arduino.cc/resources/pinouts/ABX00023-full-pinout.pdf) del microcontrollore associato (famiglia Arduino MKR), la porta di comunicazione alternativa alla modalita HAT è un connettore JST-PH a 5 pin che fornisce alimentazione a 5V e GND, i due pin I2C canonici e un terzo pin corrispondente al pin D7 dell'header.

L'altro sensore attualmente disponibile è il GY-NEO6MV2, basato su u-blox NEO-6M. 

## Interfaccia per Communicator
L'approccio più congruo alle scelte di design del codice richiede la creazione di un'interfaccia unica, `I2CInterface`. Essa agirà da *master*, con i vari sensori I2C del bus come *slaves*. I suoi compiti sono
 1. interfacciarsi *con il metallo*, *i.e.* con i pin I2C-enabled dei GPIO del RasPi
 2. acquisire gli indirizzi dei sensori sul bus
 3. interrogare periodicamente i vari sensori connessi al bus per acquisirne i dati
 4. tradurre i dati per renderli utili e fruibili ai livelli superiori di Mothics (`Aggregator`, ..., utente finale)
 
### 1. Interazione con i GPIO
Il RasPi offre i pin
 - GPIO 2 (SDA), GPIO 3 (SCL) come principali pin I2C
 - GPIO 0 (SDA), GPIO 1 (SCL) come pin I2C abilitabili via software.

### 4. Parsing dei dati
Il parsing può essere effettuato sfruttando due librerie di Python
 - [`pynmea2`](https://github.com/Knio/pynmea2) per il parsing dei dati del GPS (protocollo NMEA 0183)
 - una libreria stile-`smbus` per il parsing dei dati degli altri sensori.

# AU1: Anemometro

### Obiettivo
Realizzare un anemometro a coppette e banderuola affidabile che possa comunicare con l’unità centrale tramite [inserire protocollo].  
Il design utilizza tecnologia a sensori ad effetto Hall per rilevare il movimento dei componenti mantenendo comunque la tenuta stagna.

### Stato attuale  
Effettuati i primi test con la comunicazione MQTT.  
MQTT non sembra promettente per questa applicazione, il tempo tra i messaggi è lungo e alcuni si perdono.  
Necessario valutare e approfondire le alternative a MQTT  

# MQTT
## Idee per migliorare la trasmissione
- Inviare i dati grezzi dal sensore e non pacchetti o Json riduce il carico in ricezione;
- Impostare la comunicazione in QoS = 0 (Quality Of Service);
- Aumentare la frequenza di comunicazione a meno di 2 secondi (1s, 500 ms): l'aumento della frequenza di trasmissione aumenta il carico sulla rete e il rischio di congestione, si può arrivare a 200 ms come margine di sicurezza;
- Diminuire la frequenza di Keep-alive (tempo in cui la comunicazione può rimanere inattiva prima che il broker chiuda il collegamento) può ridurre il traffico sulla rete e lasciare maggiori risorse alla trasmissione dati;
- Evitare di aprire/chiudere la connessione ad ogni messaggio, ma utilizzare una sessione persistente;
- Uso di MQTT v5, che supporta i topic alias, permettendo di non inviare ogni volta il nome completo del topic: meno byte, meno latenza.


## Quality of Service
La libreria PubSubClient, implementa esclusivamente QoS 0 per la comunicazione MQTT, e non supporta QoS 1 o QoS 2.  
