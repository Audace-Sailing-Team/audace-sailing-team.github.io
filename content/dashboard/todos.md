---
title: 'To do'
weight: 2
date: 2025-11-19
---

<!--more-->
# Progetti in corso
## UC1: alimentazione
 - [ ] Inventariare attuali strumenti di immagazzinamento energia (18650, LiPo, etc..)
 - [ ] Studiare attuale composizione delle unità per quanto riguarda i consumi e l'energia fornibile in uscita dal RasPi
 - [ ] Studiare caratteristiche dei vari modi di alimentare ogni board (USB-C, JST,...) con relative caratteristiche di sicurezza e requirement di tensione/corrente
 - [ ] Scrivere report riguardante le possibili architetture dell'unità centrale e alternative strade di alimentazione
 
## UC2: RasPi e I2C
È caldamente suggerito di curiosare nella [documentazione](https://audace-sailing-team.github.io/mothics/api/mothics) del codice (in particolare `comm_interface`) e di comprendere il funzionamento del protocollo di comunicazione I2C.
I filoni principali di lavoro sono hardware
 - [ ] Prototipare bus I2C
 - [ ] Provare script di test in Python per comunicare coi sensore in I2C
e software
 - [ ] Studiare `Communicator`, interfacce di comunicazione e scrivere `I2CInterface`
 - [ ] Capire che informazioni danno i sensori in bus I2C
 - [x] Cercare librerie adatte per parsing dati I2C in Python
 
# Progetti futuri
## Unità Centrale
...WIP...

## Server e comunicazione rete cellulare
...WIP...


