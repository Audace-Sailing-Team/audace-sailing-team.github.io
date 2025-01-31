---
title: 'Analisi dati'
date: 2025-01-23
weight: 4
---

L'**analisi dati** include tutte le procedure di
visualizzazione, analisi e archiviazione dei dati relativi a uscite in
mare. Il progetto (**Audace Mothics** `full`) include un'interfaccia
web, basata su un backend in *Python*.

<!--more-->

## Nome
Di seguito, **Audace Mothics** (*Audace Moth* analyt*ics*) verrà usato
al posto del meno elegante "software di analisi dati post-mortem". Si
accettano proposte migliori!

Con Mothics, si fa riferimento all'insieme di
 - `Aggregator` (gestione di dati *raw* da `Communicator` e
   salvataggio, o gestione di track da file)
 - `WebApp` (interfaccia web)
 - API e script di avvio e spegnimento.

## Struttura generale
Il software dell'UC è una versione ridotta e specializzata di Mothics;
dunque, la *codebase* è essenzialmente analoga, così come la struttura
dell'interfaccia.

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

Un altro elemento da disaccoppiare è la verifica dello stato dei
sensori.

#### Stato delle UR
I tre possibili stati delle unità remote sono:
 - **online** (verde): l'UR è attiva e prende attivamente dati
 - **non-communicating** (giallo): l'UR non ha inviato dati negli
   ultimi 30s, o c'è un problema di comunicazione con l'UC
 - **offline** (rosso): l'UR non comunica con l'UC. 

In una certa misura, lo stato dei sensori è un'informazione *derivata*
dai dati ottenuti dai sensori stessi, ergo può essere ricavata in
maniera indipendente, e non necessariamente in fase di presa dati.
Dunque, è necessario memorizzare in fase di aggregazione
(`Aggregator.aggregate`) il timestamp dell'ultimo dato raccolto da un
dato sensore; l'informazione è recuperata in `WebApp` e, attraverso
una funzione *helper*, convertita nello stato del sensore.

Questo disaccoppiamento permette di implementare più facilmente le
routine necessarie per caricare un file da analizzare *post-mortem*.

## Funzionalità
Al minimo, Mothics deve:
 - gestire un database locale (e potenzialmente remoto) di *tracks*,
   *i.e.* dati raccolti durante un'uscita in mare
 - permettere la selezione di una singola track e di visualizzarne i
   dati in maniera sintetica e interattiva

- confrontare i dati (perlomeno medi) fra varie tracks (obiettivo
   successivo).

## Database
Le varie tracks possono essere esportati in `.json` ed essere gestiti
con *TinyDB* o un database relazionale più serio. Pare che *TinyDB*
sia abbastanza versatile per giocare coi dati.

Alcuni riferimenti:
 - [TinyDB docs](https://tinydb.readthedocs.io/en/latest/getting-started.html)

### Salvataggio
Il salvataggio dei dati concerne esclusivamente la versione
`lite`. Esso deve avvenire in diversi momenti
 - periodicamente, con una certa frequenza (*checkpoint*) per
   tutelarsi da eventuali avarie dell'UC (batterie scariche, danni,
   ...)
 - al termine del ciclo di `Aggregator`
 - su segnale fornito dall'utente.
 
Queste necessità suggeriscono l'introduzione di due modalità operative
per la versione `lite`
 1. campionamento **continuo** dall'attivazione allo spegnimento dell'UC
 2. campionamento **on demand**, iniziato e terminato dall'utente a
    piacimento.

#### Campionamento continuo
Questa è la modalita "di default" in fase di testing di Mothics e
dell'hardware, in particolar modo in assenza della pulsantiera di
bordo per comunicare direttamente con l'UC. 

Si divide in due fasi:
 1. presa di checkpoint a un intervallo di tempo deciso dall'utente
 2. salvataggio finale al termine del ciclo di `Aggregator`.

#### Campionamento on demand
Questa è la modalità *finale*, prevista sull'implementazione di
produzione dell'UC. 

Di default, `Aggregator` non registra i dati fino alla pressione di un
pulsante di avvio nell'interfaccia grafica (e fisica, attraverso una
pulsantiera); essa termina in maniera analoga alla pressione di un
pulsante di interruzione. Per far ciò, occorrono due metodi di
`Aggregator` (*e.g.* `start_save`, `stop_save`) che
 1. all'avvio del campionamento, attivano la procedura di
    checkpointing
 2. all'interruzione, salvano la track finale e disattivano il
    checkpointing.
 
#### Artefatti
I checkpoint sono artefatti del campionamento associati a ogni track,
utili in caso di spegnimento improvviso dell'UC. 
Per entrambe le procedure di salvataggio, essi vanno rimossi solamente
al termine del ciclo di `Aggregator`, ovvero dopo che la track è stata
registrata.

## Interfaccia web
Si mantiene l'interfaccia basata su `flask` e HTML
jinja-templating. Le due versioni del software differiscono sia in
funzionalità proposte, che in logica.

### Versioni
L'interfaccia `lite` presenta
 - **dashboard**: dati istantanei dei sensori, time-series delle grandezze
   di interesse
 - **log**: visualizzazione in tempo reale dei log dei sensori e delle
   routine di aggregazione e trattamento dati
 - **impostazioni**: gestione di tempi di campionamento e funzionalità
   dei sensori

L'interfaccia `full` richiede diversi adattamenti. Occorre modificare
 - **impostazioni**: il tuning dei sensori e delle comunicazioni non è
   necessario, così come non è possibile variare la frequenza di
   campionamento al di sotto di quella minima fornita dai dati
 - (...)
 
inoltre, occorre aggiungere
 - **selezione della track**: manipolazione delle track disponibili
 nel database, sincronizzazione con server remoto, selezione della
 track di interesse, eventuali opzioni di esportazione.

### Visualizzazione
Quali dati devono essere visualizzati nelle due versioni del codice?
(*...inserire note da lavagne delle riunioni di novembre/dicembre 2024*)

#### Flask: applicazioni web con Python
Alcuni riferimenti:
 - [Python Web Applications: Deploy Your Script as a Flask
   App](https://realpython.com/python-web-applications/#test-locally)

##### Grafici interattivi: Flask e Bokeh
In linea di massima è possibile mostrare grafici interattivi su pagine
web con *Bokeh*. Sono disponibili due modalità principali:
 - *standalone documents*: script indipendenti, che non richiedono un
   server *Bokeh* attivo e garantiscono un livello limitato di
   interazione; non si aggiornano dinamicamente, *e.g.* all'arrivo di
   nuovi dati
 - *Bokeh applications*: applicazioni totalmente interattive, che si
   appoggiano a un server *Bokeh* in background; possono aggiornarsi
   dinamicamente.
 
Alcuni riferimenti:
 - [Interactive Data Visualization in Python With Bokeh](https://realpython.com/python-data-visualization-bokeh/)
 - [Embedding a bokeh app in
   flask](https://stackoverflow.com/questions/29949712/embedding-a-bokeh-app-in-flask)
 - [Bokeh in web
   pages](https://docs.bokeh.org/en/latest/docs/user_guide/output/embed.html#ug-output-embed)
 - [Bokeh server APIs](https://docs.bokeh.org/en/latest/docs/user_guide/server/library.html)
 - [Bokeh demo](https://demo.bokeh.org/)
