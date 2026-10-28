---
title: 'To do'
weight: 2
date: 2026-03-19
---

<!--more-->
# Progetti in corso  

## Hardware

- [ ] Creazione scatola
	- [ ] Modellazione
		- [ ] Dimensionamento batteria per esp32
 		- [ ] IMU nuovo ?
   		- [ ] Test servo 
  	- [ ] Creazione guscio
  		- [ ] Impermeabilizzazione
  - [ ] PCB
  	- [ ] Test PCB Hat rasPi quando arriva
  	 - [ ] Corso con il racing
  	  	- [ ] Cominciare a capire come si usa Altium e come progettare PCB
  	    - [ ] Progettare la nostra prima PCB con circuito integrato
---

## Software
È caldamente suggerito di curiosare nella [documentazione](https://audace-sailing-team.github.io/mothics/api/mothics) del codice (in particolare `comm_interface`) e di comprendere il funzionamento del protocollo di comunicazione I2C.
I filoni principali di lavoro sono:

### Esp32
- [ ] Scrittura del codice in linguaggio da decidere per esp32
	- [ ] Decidere il linguaggio
	- [ ] Scegliere protocolli di comunicazione sensori
	- [ ] Test del funizionamento per ogni sensore
		- [ ] IMU funziona e legge dati
		- [ ] GPS funziona e legge dati
		- [ ] DHT funziona e legge dati
		- [ ] ADC funziona e legge dati

### Sensori
- [ ] Adafruit Ultimate GPS
	- [] Creare modulo software
	      In teoria dovrebbe andare uguale con NMEAGPSInterface (è un gps nmea)
	- [ ] Testare
		- [ ] Funzionamento
		- [ ] Tempo cold start (avvio con presa satelliti)
		- [ ] Velocità di campionamento
- [ ] IMU BNO055
	- [ ] Creare modulo software
	- [ ] Capire la questione sistema di riferimento
- [ ] RTC
	- [ ] https://pimylifeup.com/raspberry-pi-rtc/
	- [ ] https://thepihut.com/blogs/raspberry-pi-tutorials/17209332-adding-a-real-time-clock-to-your-raspberry-pi
- [ ] DHT22
	- [ ] Modulo software
- [ ] ADC
	- [ ] Modulo software
    - [ ] Test di precisione, affidabilità ed accuratezza del GPS appena implementato.
    - [ ] Creazione di un programmino di traduzione dal nostro tipo di salvataggio .json a .gpx

### Controllo di volo
- [] Delineare una struttura generale



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
- [ ] Rendere la scatola vecchia (quella rettangolare ) solida, funzionante e ready to use
	- [ ] Fare in maniera che all'avvio del Raspberry si avvi la webapp


---

## AU1: Produzione anemometro meccanico elettromagnetico.

- [ ] Aggiornare e perfezionare design dell'enclosure dell'anemometro
- [ ] Adattare anemometro alla forma della tuga
- [ ] Preparare render da allegare per l'anemometro nel Report S1
- [ ] Primi test di comunicazione con l’unità centrale  
- [ ] Calibrazione effettiva con i vecchi dati di calibrazione  

---


