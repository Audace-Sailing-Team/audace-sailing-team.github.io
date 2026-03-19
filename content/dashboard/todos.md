---
title: 'To do'
weight: 2
date: 2026-03-19
---

<!--more-->
# Progetti in corso  

## UC1: alimentazione
- [x] Inventariare attuali strumenti di immagazzinamento energia (18650, LiPo, etc..)
- [x] Studiare attuale composizione delle unità per quanto riguarda i consumi e l'energia fornibile in uscita dal RasPi
- [x] Studiare caratteristiche dei vari modi di alimentare ogni board (USB-C, JST,...) con relative caratteristiche di sicurezza e requirement di tensione/corrente
- [x] Scrivere report riguardante le possibili architetture dell'unità centrale e alternative strade di alimentazione
- [ ] Esegure studio su carico "realistico" (raspi + mothics a vuoto + script di polling dei sensori) sulla durata della configurazione di 4x18650 in 2s2p
- [ ] Progettare scatola per l'unità centrale  
	- [ ] Cercare e aggiungere i modelli di GPS, IMU e 18650 al CAD della scatola
	- [ ] Preparare un possibile "render" per suddetta scatola da allegare nel report S1 (che sia più definitiva possibile)

---

## UC2: RasPi e sensori
È caldamente suggerito di curiosare nella [documentazione](https://audace-sailing-team.github.io/mothics/api/mothics) del codice (in particolare `comm_interface`) e di comprendere il funzionamento del protocollo di comunicazione I2C.
I filoni principali di lavoro sono UART (GPS)
- [ ] Costruire una funzione "ready to deploy" che esegua il parsing dei dati NMEA dal GPS e che possa nel finale simulare una chiamata a `on_message_callback()` con i dati di `topic: valore`.  
- [ ] Scrivere una classe (tempornaneamente definitiva) `UARTGPSInterface` (o simili), ereditaria di `SerialInterface` che implementi, nel suo `_run_loop()` la funzione descritta al ToDo precedente.  
e I2C (IMU e altri sensori) 
- [ ] Prototipare bus I2C
- [ ] Provare script di test in Python per comunicare coi sensore in I2C
- [ ] Capire che informazioni danno i sensori in bus I2C 
- [ ] Scrivere `I2CInterface`

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


