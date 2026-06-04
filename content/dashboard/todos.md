---
title: 'To do'
weight: 2
date: 2026-03-19
---

<!--more-->
# Progetti in corso  

## UC_HW

- [ ] Creazione scatola
	- [ ] Scheletro Interno
		- [ ] Modellazione
			- [ ] Adafruit ultimate gps (per la versione backup)
			- [ ] IMU nuovo
			- [ ]  Implementare modulo RTC
	- [ ] Stampa
	- [ ] Guscio
		- [ ] Coating
	- [ ] Tappo
		- [ ] Ingrossare
		- [ ] Stampare
	- [ ] Test impermeabilità del tutto
- [ ] Creare schematica scheda secondo piano scatola, con partitore, adc, imu e tutti i rail necessari
- [ ] Creare HAT raspi per quest'ultima scheda
- [x] Test di alimentazione e autonomia in scenario realistico della configurazione adottata (3,7V LiPo 2p)
---

## UC_SW
È caldamente suggerito di curiosare nella [documentazione](https://audace-sailing-team.github.io/mothics/api/mothics) del codice (in particolare `comm_interface`) e di comprendere il funzionamento del protocollo di comunicazione I2C.
I filoni principali di lavoro sono:

### General
- [ ] Setuppare scheda "vergine" con Dietpi, installazione minimale ma sopratutto, pronta a regatare 
	- [ ] Accensione automatica
	- [ ] Vedi /docs/setup.rst
### Sensori
- [ ] Adafruit Ultimate GPS
	- [x] Creare modulo software
	      In teoria dovrebbe andare uguale con NMEAGPSInterface (è un gps nmea)
	- [ ] Testare
		- [ ] Funzionamento
		- [ ] Tempo cold start (avvio con presa satelliti)
		- [ ] Velocità di campionamento
- [x] IMU nuovo (non mi ricordo il codice)
	- [x] Creare modulo software
	- [ ] Sistemare saldatura errata sull'hat che si incroca con il DHT2
	      Ricorda!!: 
	      **TXD5 (Transmit):** **Physical Pin 32** (GPIO12 / BCM 12)
	      **RXD5 (Receive):** **Physical Pin 33** (GPIO13 / BCM 13)
- [ ] RTC
	- [ ] https://pimylifeup.com/raspberry-pi-rtc/
	- [ ] https://thepihut.com/blogs/raspberry-pi-tutorials/17209332-adding-a-real-time-clock-to-your-raspberry-pi
- [ ] DHT22
	- [ ] Modulo software
	- [ ] Sistemare saldatura errata sull'hat che si incrocia con il BNO
- [ ] ADC
	- [ ] Modulo software
- [ ] Test di precisione, affidabilità ed accuratezza del GPS appena implementato.
- [ ] Creazione di un programmino di traduzione dal nostro tipo di salvataggio .json a .gpx

### WebApp
- [ ] TEST-Mothics
	- [ ] Rimuovere la sezione di metadati relativa ad ogni traccia nella sezione "tracks" della webapp live per la seguente motivazione:
	      All'apertura della pagina, nel caso di tracce particolarmente lunghe, i metadati di descrizione delle tracce vengono ricavati *caricando* (?)...
	- [ ] Implementare modulo RTC
	- [ ] Implementare ADC lettura carica batteria
	- [ ] Implementare DHT22
- [ ] Aggiornare e fare ordine nei thesauri
- [ ] Sistemare nomenclatura e ordinamento topic root, idee:
	- rm# indica il numero dell'unità fisica piantata sulla barca (1 per la centrale, 2 per anemometro o simili, ecc...)
	- /---/ (la seconda parte) è il tipo di sensore
	- l'ultima parte è l'unità di misura
	  (ha senso?)
- [ ] Finire cherrypick Webapp Lite
- [ ] Sistemare config (toml e default)
	- [ ] dare nomi giusti unità remote
	- [ ] cdn controllare correttezza e rimuovere extra
	- [ ] controllare gestione tiles (e magari toglierla)
	- [ ] confronto generale con config dell'anno scorso
- [ ] Rimuovere mappa GPS (come al solito)

### Mothics general
- [x] Modificare il comportamento di salvataggio, dal contesto attuale al seguente:
      Il salvataggio avviene direttamente su un unico file con un append periodico
	- [x] Definire attributo di track "track_file" di track e definire l'apertura della risorsa di sistema all'inizio del periodo di salvataggio
	- [x] Sostituire alla logica del vecchio "checkpoint" la logica di "append" alla coda del file
	- [x] Gestire poi correttamente la chiusura della risorsa
- [ ] Test salvataggio
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


