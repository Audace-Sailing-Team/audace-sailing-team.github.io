---
title: 'Analisi post-mortem'
date: 2025-01-23
weight: 4
---

L'**analisi post-mortem** include tutte le procedure di
visualizzazione, analisi e archiviazione dei dati relativi a uscite in
mare già concluse. Il progetto (**Audace Mothics**) include un'interfaccia
web, basata su un backend in *Python*.

<!--more-->

## Nome
Di seguito, **Audace Mothics** (*Audace Moth* analyt*ics*) verrà usato
al posto del meno elegante "software di analisi dati post-mortem". Si
accettano proposte migliori!

## Struttura generale
Essenzialmente, il software dell'UC è una versione ridotta e
specializzata di Mothics; dunque, la *codebase* è essenzialmente
analoga, così come la struttura essenziale dell'interfaccia.

### Deployment
Essendo la *codebase* sostanzialmente identica fra UC e analisi
post-mortem, si può pensare a Mothics come a un pacchetto Python
disponibile in due versioni
 - `full`: il software completo, usato per l'analisi post-mortem
 - `lite`: tutto ciò che occorre per visualizzare i dati in tempo
   reale dall'UC.
   
Dividere i due pacchetti sarebbe abbastanza facile; occorre un
*Makefile* con due diverse procedure di installazione, accessibili con
comandi diversi

```
	make install-full    # full Mothics installation for post-mortem analysis
	make install-lite    # light Mothics installation for live analysis on moth
```

Nel caso si volesse inserire il pacchetto su PyPI, è comunque
possibile differenziare le due installazioni, *e.g.*

```
	pip install mothics          # full Mothics installation for post-mortem analysis
	pip install mothics[lite]    # light Mothics installation for live analysis on moth
```


### Disaccoppiamento e stream di dati
Qualora si dovesse procedere con l'opzione del "software unico con due
installazioni diverse", sarebbe necessario effettuare un disaccoppiamento fra la
fonte dei dati e la visualizzazione web.  

Allo stato attuale, `Aggregator` è a tutti gli effetti la fonte dei
dati; con l'aggiunta di un metodo o attributo per la lettura di track
già esistenti e di un "orologio interno" (*i.e.* un loop di durata
pari a quella della track, o a suoi multipli), è possibile emulare il
"comportamento dei dati" in tempo reale e di visualizzarli in maniera analoga. 

## Funzionalità
Al minimo, Mothics deve:
 - gestire un database locale (e potenzialmente remoto) di *tracks*,
   *i.e.* dati raccolti durante un'uscita in mare
 - permettere la selezione di una singola track e di visualizzarne i
   dati in maniera sintetica e interattiva

- confrontare i dati (perlomeno medi) fra varie tracks (obiettivo
   successivo).
 
### Visualizzazione
Quali dati devono essere visualizzati nelle due versioni del codice?
(*...inserire note da lavagne delle riunioni di novembre/dicembre 2024*)

## Database
I vari dataset possono essere esportati in `.json` ed essere gestiti
con *TinyDB* o un database relazionale più serio. Pare che *TinyDB*
sia abbastanza versatile per giocare coi dati.

Alcuni riferimenti:
 - [TinyDB docs](https://tinydb.readthedocs.io/en/latest/getting-started.html)

## Interfaccia web
Si mantiene l'interfaccia basata su `flask` e HTML
jinja-templating. Le due versioni del software differiscono sia in
funzionalità proposte, che in logica.

### Versione lite
L'interfaccia `lite` presenta
 - **dashboard**: dati istantanei dei sensori, time-series delle grandezze
   di interesse
 - **log**: visualizzazione in tempo reale dei log dei sensori e delle
   routine di aggregazione e trattamento dati
 - **impostazioni**: gestione di tempi di campionamento e funzionalità
   dei sensori

### Versione full
L'interfaccia `full` richiede diversi adattamenti. Occorre modificare
 - **impostazioni**: il tuning dei sensori e delle comunicazioni non è
   necessario, così come non è possibile variare la frequenza di
   campionamento al di sotto di quella minima fornita dai dati
 - (...)
 
inoltre, occorre aggiungere
 - **selezione della track**: manipolazione delle track disponibili
 nel database, sincronizzazione con server remoto, selezione della
 track di interesse, eventuali opzioni di esportazione.

