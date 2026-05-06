---
title: 'To do'
weight: 2
date: 2026-03-19
---

<!--more-->
# Progetti in corso  

## UC_HW

- [ ] Creazione primo prototipo di scatola
	- [x] Modellazione
		- [x] Modificare ScheletroInterno secondo le seguenti:
			- [x] Traslare verso il centro il Raspberry Pi il più possibile in modo tale da permettere ai pin interessati dalle connessioni I2C, UART e alimentazione di non interferire con il foro per la vite presente in loro corrispondenza (circa 5mm?) Per fare ciò è necessario spostare il foro della vite centrale del guscio e la paratia centrale dello scheletro il più possibile verso il lato di dritta. Inoltre è necessario traslare anche il Torpedo2 più a dritta possibile.
			- [x] Ruotare di 180 gradi il torpedo in modo che dal lato del tappo venga mostrato il lato con il jack DC. Questo per permettere una maggiore traslazione del torpedo (e quindi del raspberry) e per permettere ai pin delle batterie di avere sufficiente margine (in fase di inserimento, rispetto al foro della vite in basso a destra) per essere collegati.
			- [x] Aggiungere griglia di aerazione alla paratia centrale tra RasPi e Torpedo2
		- [x] (!se necessario) Modificare GuscioEsterno, spostando il foro della vite in basso-centro-dritta per ovviare allo spostamento della paratia centrale di ScheletroInterno verso dritta
	- [ ] Stampa
		- [x] GuscioEsterno
		- [ ] ScheletroInterno
		- [ ] (!se necessario) GuscioEsterno
		- [x] Tappo
		- [ ] Guarnizione
	- [x] Progettare dei supporti (con viti o ad incastro) per il fitting dei vari sensori
	- [ ] Fitting
- [ ] Test di alimentazione e autonomia in scenario realistico della configurazione adottata (3,7V LiPo 2p)

---

## UC_SW
È caldamente suggerito di curiosare nella [documentazione](https://audace-sailing-team.github.io/mothics/api/mothics) del codice (in particolare `comm_interface`) e di comprendere il funzionamento del protocollo di comunicazione I2C.
I filoni principali di lavoro sono:
### UART (GPS)
- [ ] Creazione di un programmino di traduzione dal nostro tipo di salvataggio .json a .gpx
- [ ] Test di precisione, affidabilità ed accuratezza del GPS appena implementato.
### I2C (IMU e altri sensori) 

### WebApp
Aggiustare la webapp
- [ ] Rimuovere la sezione di metadati relativa ad ogni traccia nella sezione "tracks" della webapp live per la seguente motivazione:
      All'apertura della pagina, nel caso di tracce particolarmente lunghe, i metadati di descrizione delle tracce vengono ricavati *caricando* efe
- [ ] Correzione errori e prima run della nuova WebApp
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


