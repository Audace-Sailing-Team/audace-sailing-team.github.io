---
title: 'To do'
date: 2025-11-13
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
 - [ ] Attivare pin I2C software su RasPi (GPIO0, GPIO1)
 - [ ] Scrivere script di test in Python per comunicare col sensore in I2C
 - [ ] Capire che informazioni danno i sensori in I2C e fare parsing; cercare librerie adatte per Python
 - [ ] Studiare `Communicator`, interfacce di comunicazione e scrivere `I2CInterface`
 - [ ] Studiare bus I2C

## AU1: Produzione anemometro meccanico elettromagnetico.
- [x] Testare e validare il primo codice
- [x] Risolvere i problemi del primo codice
- [x] Aggiungere i file .3mf della banderuola a `3mf_files`
- [x] Aggiornare il sito web con le info presenti in questo documento
- [x] Implementare la lettura dell’angolo del vento relativo dall’AS5600
- [ ] Primi test di comunicazione con l’unità centrale
- [ ] Calibrazione effettiva con i vecchi dati di calibrazione
- [ ] Aggiornare la pagina `Progetti in corso` con la sezione `Current state` dell'anemometro.

# Progetti futuri
## Unità Centrale
...WIP...

## Server e comunicazione rete cellulare
...WIP...


