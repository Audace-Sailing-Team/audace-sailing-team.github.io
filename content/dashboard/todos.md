---
title: 'To do'
weight: 2
date: 2026-03-19
---

<!--more-->
# Progetti in corso  

## UC1: alimentazione
- [ ] Esegure studio su carico "realistico" (raspi + mothics a vuoto + script di polling dei sensori) sulla durata della configurazione di 4x18650 in 2s2p
- [ ] Progettare scatola per l'unità centrale  
	- [ ] Facendo riferimento alle misure dei componenti, aggiungere al CAD i buchi e standoff per avvitare Raspberry e Torpedo2
	- [ ] Aggiungere buchi per inserti a caldo viti di chiusura (Almeno due sopra, due sotto e una per ognuna delle alette laterali)
		- [ ] Trovare specifiche degli inserti a caldo M3 che sono in lab, e nello specifico il diametro del foro e tipo di svaso da dare al foro
		- [ ] Aggiungere i buchi delle dimensioni corrette, aggiungendo materiale dove necessario in modo da avere almeno un **tot** (2/3mm?) di spazio per una guarnizione. La guarnizione deve passare INTERNAMENTE alle viti, in quanto queste ultime non devono essere necessariamente impermeabili.
- [ ] Creazione primo prototipo di scatola
	- [ ] Stampa
		- [ ] GuscioEsterno
		- [ ] ScheletroInterno
		- [ ] Tappo
	- [ ] Preparazione e saldatura delle schede dei moduli sensore (IMU e GPS) con i connettori appropriati.
	- [ ] Salduatura dei sensori sui moduli (Prestando attenzione all'orientamento per IMU!!!).
	- [ ] Progettare dei supporti (con viti o ad incastro) per il fitting dei vari sensori
	- [ ] Fitting


---

## UC2: RasPi e sensori
È caldamente suggerito di curiosare nella [documentazione](https://audace-sailing-team.github.io/mothics/api/mothics) del codice (in particolare `comm_interface`) e di comprendere il funzionamento del protocollo di comunicazione I2C.
I filoni principali di lavoro sono:
### UART (GPS)
- [x] Costruire una funzione "ready to deploy" che esegua il parsing dei dati NMEA dal GPS e che possa nel finale simulare una chiamata a `on_message_callback()` con i dati di `topic: valore`.  
- [x] Scrivere una classe (tempornaneamente definitiva) `UARTGPSInterface` (o simili), ereditaria di `SerialInterface` che implementi, nel suo `_run_loop()` la funzione descritta al ToDo precedente.  
- [ ] Effettuare le ultime considerazioni sull'approccio usato
- [ ] Push del codice
- [ ] Creazione di un programmino di traduzione dal nostro tipo di salvataggio .json a .gpx
- [ ] Test di precisione, affidabilità ed accuratezza del GPS appena implementato.
### I2C (IMU e altri sensori) 
- [x] Prototipare bus I2C
- [x] Finalizzare le scelte di costruzione dell'interfaccia I2C
- [ ]  Scrivere `I2CInterface` 
- [x] Provare script di test in Python per comunicare coi sensore in I2C
- [x] Capire che informazioni danno i sensori in bus I2C 

---

## AU1: Produzione anemometro meccanico elettromagnetico.

- [ ] Aggiornare e perfezionare design dell'enclosure dell'anemometro
- [ ] Adattare anemometro alla forma della tuga
- [ ] Preparare render da allegare per l'anemometro nel Report S1
- [ ] Primi test di comunicazione con l’unità centrale  
- [ ] Calibrazione effettiva con i vecchi dati di calibrazione  

---

## MQTT: Studio su implementazione e latenza di MQTT.
 - [ ] Creare "simulatore" di unità remota (server mqtt + unità remota ardino).
 - [ ] Studio sulla latenza e frequenza massima di trasmissione dei dati.
 - [ ] Ricerca sulla modifica del Q.O.S (Quality Of Service) e l'impatto sulla latenza.

---

# Progetti futuri
## Unità Centrale
 - [ ] Sviluppare un modulo simil-`preprocessors` per creare una API flessibile per il parsing dei dati di qualunque tipo di sensore  

## Server e comunicazione rete cellulare
...WIP...


