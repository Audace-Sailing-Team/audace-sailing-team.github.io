---
title: 'To do'
date: 2024-11-28
---

<!--more-->
# Generale
## Post arrivo componenti
 - [ ] [1] Setup RasPi (access point, MQTT, Mothics `lite`)
 - [ ] [1] Conversione *Vacca Boia* in unità remota (scrittura
       software, riconfigurazione hardware)
 - [ ] [2] Assemblaggio pacco batterie
 - [ ] [3] Assemblaggio unità remota anemometro (almeno software)
 - [ ] [4] Test, test, test

## Meta (sito)
 - [ ] [1] (*Francesco, Iacopo*) Creare repo per dati
 - [ ] [1] Caricare dati allenamento 01/2025
 - [ ] [2] (*Francesco*) Riempire *Bill of Materials*
 - [ ] Fare schemi del progetto (unità, protocolli di comunicazione)
 - [ ] (*Iacopo*) Aggiornare UML
 
# Unità Centrale
## Elettronica
 - [ ] Fare schemi elettrici dell'UC (alimentazione)
 - [ ] Studiare spegimento *sicuro* con bottone
 
## Comms
 - [ ] (*Iacopo*) Testare approfonditamente `Communicator`
 - [ ] (*Iacopo*) Implementare metodi di comunicazione TCP/IP
 - [ ] (*Iacopo*) Scrivere unit test `central-unit`
 - [x] (*Iacopo*) Scrivere metodo di acquisizione dati seriale
 - [x] (*Iacopo*) Testare metodo di acquisizione dati seriale con
       Arduino
 - [x] (*Iacopo*) Scrivere `Communicator`

## Mothics
### Data handling (`Aggregator` e `Database`)
 - [ ] [1] (*Iacopo, Francesco*) Mettere nero su bianco in documentazione
       requisiti minimi di analisi dati (e dati da mostrare, refresh
       rate, ...)
 - [ ] Definire dati utili e metodi di conversione/manipolazione con
       setup minimale (GPS, IMU, anemometro)
 - [ ] Fornire metodo di conversione a file GPX (*e.g.* con `gpxpy`) 

### Server
 - [x] (*Iacopo*) Studiare disaccoppiamento live/post-mortem
 - [x] (*Iacopo*) Strutturare scheletro di pagina Flask
 - [x] (*Iacopo*) Test di visualizzazione della pagina barebones

### Analisi live (Mothics `lite`)
 - [ ] (*Iacopo*) Aggiungere settings (*i.e.* threshold offline/noncomm, ...)

### Analisi post-mortem (Mothics `full`)
 - [ ] (*Iacopo*) Setup `TinyDB`
 - [ ] (*Iacopo*) Adattare `WebApp` live a post mortem
 - [ ] (*Iacopo*) Implementare visualizzazione track
 - [ ] (*Iacopo*) Mostrare dati su track (*e.g.* colormap su traccia per
       velocità, ...)

## Raspberry Pi
 - [ ] (*Francesco*) Studiare connettività mobile (4G) RasPi
 - [x] (*Francesco*) Acquisizione Raspberry Pi

### Hands-on
 - [ ] Setup IP statici
 - [ ] Setup broker MQTT (`mosquitto`)
 - [ ] Setup access point WiFi

# Unità Remote
## Anemometro
 - [ ] Fare schemi elettrici 
 - [ ] (*Enrico, Emil*) Prendere dati da prototipo
 - [ ] (*Iacopo, Francesco*) Studio della fisica del modello di
       anemometro
	   
### Studio fattibilità
 - [ ] (*Enrico, Emil*) Studiare perdita di precisione per
       miniaturizzazione del prototipo
 - [ ] (*Enrico, Emil*) Studiare geometrie/soluzioni alternative
 
# Testing
## *Vacca Boia*
 - [ ] (*Francesco*) Migliorare sampling rate GPS
 - [ ] (*Francesco*) Salvare CSV con data-ora
 - [ ] (*Francesco*) Correggere salvataggio dati IMU
 - [ ] (*Samuele*) Test sul campo
 - [ ] (*Samuele*) Caricare dati su repo
 - [ ] (*Samuele*) Stilare report su campagna testing VB
