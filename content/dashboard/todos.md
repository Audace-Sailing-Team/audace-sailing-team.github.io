---
title: 'To do'
weight: 2
date: 2026-03-19
---

<!--more-->
# Progetti in corso  

## UC1: alimentazione

- [ ] Creazione primo prototipo di scatola
	- [ ] Modellazione
		- [ ] Modificare ScheletroInterno secondo le seguenti:
			- [ ] Traslare verso il centro il Raspberry Pi il più possibile in modo tale da permettere ai pin interessati dalle connessioni I2C, UART e alimentazione di non interferire con il foro per la vite presente in loro corrispondenza (circa 5mm?) Per fare ciò è necessario spostare il foro della vite centrale del guscio e la paratia centrale dello scheletro il più possibile verso il lato di dritta. Inoltre è necessario traslare anche il Torpedo2 più a dritta possibile.
			- [ ] Ruotare di 180 gradi il torpedo in modo che dal lato del tappo venga mostrato il lato con il jack DC. Questo per permettere una maggiore traslazione del torpedo (e quindi del raspberry) e per permettere ai pin delle batterie di avere sufficiente margine (in fase di inserimento, rispetto al foro della vite in basso a destra) per essere collegati.
			- [ ] Aggiungere griglia di aerazione alla paratia centrale tra RasPi e Torpedo2
		- [ ] (!se necessario) Modificare GuscioEsterno, spostando il foro della vite in basso-centro-dritta per ovviare allo spostamento della paratia centrale di ScheletroInterno verso dritta
	- [ ] Stampa
		- [x] GuscioEsterno
		- [ ] ScheletroInterno
		- [ ] (!se necessario) GuscioEsterno
		- [ ] Tappo
		- [ ] Guarnizione
	- [ ] Preparazione e saldatura delle schede dei moduli sensore (IMU e GPS) con i connettori appropriati.
	- [ ] Salduatura dei sensori sui moduli (Prestando attenzione all'orientamento per IMU!!!).
	- [x] Progettare dei supporti (con viti o ad incastro) per il fitting dei vari sensori
	- [ ] Fitting
- [ ] Test di alimentazione e autonomia in scenario realistico della configurazione adottata (3,7V LiPo 2p)

---

## UC2: RasPi e sensori
È caldamente suggerito di curiosare nella [documentazione](https://audace-sailing-team.github.io/mothics/api/mothics) del codice (in particolare `comm_interface`) e di comprendere il funzionamento del protocollo di comunicazione I2C.
I filoni principali di lavoro sono:
### UART (GPS)
- [x] Effettuare le ultime considerazioni sull'approccio usato
- [x] Push del codice
- [ ] Creazione di un programmino di traduzione dal nostro tipo di salvataggio .json a .gpx
- [ ] Test di precisione, affidabilità ed accuratezza del GPS appena implementato.
### I2C (IMU e altri sensori) 
- [x]  Scrivere `I2CInterface` 
- [x] Code review del'ultimo push
- [ ] Convertire la gerarchia di configurazione di i2c da una lista di dizionari (una per sensore), ad un dizionario strutturato come nomeInterfaccia:dizionarioSensore, in modo da mantenere la struttura logica usata fino adesso
- [ ] Configurare il DEFAULT_CONFIG con solo l'imu adafruit
- [ ] Configurare il TOML con tutto quanto commentato, e solo l'imu adafruit abilitato

### WebApp
Aggiustare la webapp
- [ ] Identificare componenti superflue WebApp
	L'implementazione finale della WebApp mira a fornire l'interfaccia sufficiente ad interagire con le impostazioni dell'unità centrale, 
- [ ] Alleggerire la WebApp delle componenti superflue
- [ ] Correzione errori e prima run della nuova WebApp

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


