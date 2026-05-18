---
title: 'To do'
weight: 2
date: 2026-03-19
---

<!--more-->
# Progetti in corso  

## UC_HW

- [ ] Creazione scatola 2.0
	- [ ] Guscio
		- [ ] Stampa guscio
		- [ ] Fitta guscio su barca
	- [ ] Scheletro interno
		- [ ] Modellazione 
			- [ ] Pinne di supporto in compressione sulla testa della scatola
			- [ ] Valutare inserti a caldo per componenti
			- [ ] DHT22
			- [ ] RTC
			- [ ] ADC
		- [ ] Creazione
			- [ ] Stampa prototipo
			- [ ] Fitta
- [ ] Creazione scatola 2.0 backup
	- [ ] Scheletro Interno
		- [ ] Modellazione
			- [ ] Adafruit ultimate gps
			- [ ] IMU nuovo (non mi ricordo il codice)
- [ ] Test di alimentazione e autonomia in scenario realistico della configurazione adottata (3,7V LiPo 2p)

---

## UC_SW
È caldamente suggerito di curiosare nella [documentazione](https://audace-sailing-team.github.io/mothics/api/mothics) del codice (in particolare `comm_interface`) e di comprendere il funzionamento del protocollo di comunicazione I2C.
I filoni principali di lavoro sono:
### UART (GPS)
- [ ] Adafruit Ultimate GPS
	- [ ] Creare modulo software
	- [ ] Testare
		- [ ] Funzionamento
		- [ ] Tempo cold start (avvio con presa satelliti)
		- [ ] Velocità di campionamento
- [ ] IMU nuovo (non mi ricordo il codice)
	- [ ] Creare modulo software
- [ ] Test di precisione, affidabilità ed accuratezza del GPS appena implementato.
- [ ] Creazione di un programmino di traduzione dal nostro tipo di salvataggio .json a .gpx
### I2C (IMU e altri sensori) 

### WebApp
- [ ] Aggiustare la webapp
	- [ ] Rimuovere la sezione di metadati relativa ad ogni traccia nella sezione "tracks" della webapp live per la seguente motivazione:
	      All'apertura della pagina, nel caso di tracce particolarmente lunghe, i metadati di descrizione delle tracce vengono ricavati *caricando* (?)...
	- [ ] Implementare modulo RTC
	- [ ] Implementare ADC lettura carica batteria
	- [ ] Implementare DHT22
- [ ] Aggiornare e fare ordine nei thesauri

### Mothics general
- [ ] Modificare il comportamento di salvataggio, dal contesto attuale al seguente:
      Il salvataggio avviene direttamente su un unico file con un append periodico
	- [ ] Definire attributo di track "track_file" di track e definire l'apertura della risorsa di sistema all'inizio del periodo di salvataggio
	- [ ] Sostituire alla logica del vecchio "checkpoint" la logica di "append" alla coda del file
	- [ ] Gestire poi correttamente la chiusura della risorsa
- [ ] Sistemare per bene il toml
- [ ] Aggiungere utilities alla CLI
	- [ ] salvataggio
	- [ ] altra roba
	- [ ] testare tutto
- [ ] Sistemare bene l'architettura dei nomi dei sensori


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


