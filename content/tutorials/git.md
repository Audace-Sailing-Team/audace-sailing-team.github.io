---
title: 'Guida rapida a Git'
date: 2025-01-23
author: 'Iacopo Ricci, Claude, Zio GPT'
weight: 1
---


<!--more-->

# Introduzione
## Premesse 
Allo stato attuale, la guida è ancora in grande parte incompleta. Le
sezioni ancora in procinto di essere scritte o mancanti sono
contrassegnate da **WIP** nel titolo e dall'indicazione **Work in
progress** nel corpo del testo.

## Come usare questa guida
Qui, cerchiamo di dare tutte le informazioni di base necessarie per
iniziare a usare *git* e per capire il perchè di questa scelta. Per
far ciò, violeremo tutti i precetti delle guide rapide all'uso: ci
saranno tante, troppe parole ad accompagnare i comandi più pratici.

Suggeriamo agli utenti novizi particolarmente zelanti di leggere
l'intera guida (con pazienza). Invece, chi ha già idea di cosa sia un
sistema di controllo versione e ha già *git* installato, può saltare
l'intera introduzione.

## Perchè usare Git?
Prima di approfondire Git nello specifico, è fondamentale capire
perché esiste il **controllo versione**. 

Immagina di scrivere una tesi di laurea. Potresti salvare diverse
versioni del tuo lavoro: *"tesi-bozza1.doc"*, *"tesi-bozza2.doc"*,
*"tesi-finale.doc"*, *"tesi-finale-DAVVERO.doc"*. Questo controllo
versione manuale diventa rapidamente ingestibile, e diventa facile
inviare al proprio relatore la versione sbagliata della tesi. Ora
immagina di coordinare questo lavoro con altri dieci autori, ognuno
che lavora su capitoli diversi. Questo è il problema che i sistemi di
controllo versione risolvono.

I sistemi di controllo versione automatizzano questo processo di
tracciamento delle modifiche. Infatti, essi mantengono una cronologia
completa di chi ha modificato cosa e quando, permettono a più persone
di lavorare simultaneamente senza interferire l'uno con l'altro e
forniscono meccanismi per unire il lavoro di tutti.

Ovviamente, la stesura di una tesi di laurea è solo un possibile
utilizzo (particolarmente di nicchia) di Git. L'utilizzo più comune è
la gestione di progetti informatici complessi, ed è stata proprio
questa la motivazione dietro alla sua nascita. 

## Git, Github, Gitlab, ...
Più che di *git*, è facile sentire parlare di *Github* e di
*Gitlab*. Qual è la differenza fra i tre?

Git è un sistema di controllo versione distribuito, *i.e.* uno
strumento che tiene traccia delle modifiche nel codice sorgente
compiute durante lo sviluppo del software; esso opera
localmente sul tuo computer. 

GitHub e GitLab sono piattaforme web che forniscono hosting per
repository (*i.e.* progetti) Git insieme a funzionalità aggiuntive.

In breve, Git è lo strumento che gestisce il codice, mentre GitHub e
GitLab sono strumenti web costruiti attorno a Git che permettono di
salvare online i propri progetti e di collaborare da remoto con altri
utenti.

## Configurazione
Git è uno strumento da linea di comando. Esistono interfacce grafiche
per agevolare gli utenti occasionali, ma spesso esse impediscono di
avere il grado di controllo che garantisce la linea di comando.

### Installazione
Su **Linux**, Git è spesso pre-installato. Per verificare se Git è
presente, è sufficiente aprire il terminale e scrivere 

```
git --version
```

Se non succede niente, [qui](https://git-scm.com/downloads) sono
forniti tutti i riferimenti per l'installazione.

Su **Windows**, è caldamente consigliato l'uso del **W**indows
**S**ubsystem for **L**inux, che permette di seguire la stessa
identica procedura mostrata sopra. Per i più ostinati e avversi a WSL,
l'[installatore](https://git-scm.com/downloads/win) include sia gli
strumenti da riga di comando che un'interfaccia grafica.

Su **Mac**... dovrebbe essere sufficiente Homebrew; gli autori di
questa guida nulla sanno, e nulla di più vogliono sapere a riguardo. 

> **Nota**: potrebbe essere richiesto di inserire le variabili `user.name`
> e `user.email`: esse ti identificano nei messaggi dei commit. 
> In teoria, dovrebbero corrispondere al tuo account GitHub/GitLab, ma
> è raccomandabile fornire altrove il proprio *vero* indirizzo email di
> contatto.

> **Nota**: la variabile `core.editor` imposta l'editor di testo
> preferito per i messaggi di commit e le operazioni interattive da
> riga di comando. Per i novizi e i non masochisti, è raccomandabile
> usare `nano`, che non offre grandi potenzialità ma è
> sufficientemente intuitivo per chi è alle prime armi. Per i lettori
> del Marchese de Sade, suggeriamo l'uso di `vim` o `emacs` - scelte
> predilette dagli autori.

# Le basi
Prenderemo come esempio un progetto molto semplice

```
progetto/
  ├── index.html
  ├── style.css
  └── script.js
```

al quale faremo riferimento in tutte le sezioni successive.

## Inizializzazione del progetto
Come iniziare a usare *git*?

Nella cartella del progetto, si inizializza un repository git con

```
git init
```

Il comando creerà la cartella nascosta `.git/` che conterrà la storia
del progetto e tutte le configurazioni a esso relative. Non cancellarla!

Successivamente, prepara i tuoi file con `git add .` e crea
il tuo primo commit con `git commit -m "Initial commit"`. Questi
passaggi stabiliscono il tuo controllo versione locale... e ne
spiegheremo il significato a breve.

### Collegamento a repository remoti
Il modo più semplice per connettersi a GitHub o GitLab è creare un
repository sulla piattaforma scelta. Ovviamente, serve un account
su una delle due piattaforme!

> **Nota:** l'interfaccia grafica fra Github e Gitlab è abbastanza diversa, ma le
> funzionalità alle quali faremo riferimento sono le stesse per
> entrambi.

Una volta creato un repository su Github o Gitlab, sarà possibile collegare
il progetto locale con

```
git remote add origin <URL-repository>
```

`<URL-repository>` è l'URL del repository remoto; sarà in formato
HTTPS o SSH, con due diverse procedure di configurazione.

L'opzione più semplice è HTTPS, che richiede l'inserimento delle
proprie credenziali a ogni operazione compiuta dal terminale, *e.g.*
per caricare il proprio codice sul repository remoto. 

>**Nota:** l'uso di SSH evita di inserire le credenziali ogni volta, ma richiede
>la generazione di una chiave SSH; la procedura è descritta nella
>sezione apposita.

Infine, carica il tuo codice con 

```
git push -u origin main
```

Il flag `-u` imposta il tracking, così i push futuri necessiteranno
solo di `git push`. 

#### Chiavi SSH (WIP)
**Work in progress**

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

## I tre stati
Immaginiamo che Git contenga tre aree di lavoro o *stati* dei file:
1. **Modificato**: considera questo come il tuo spazio di
   lavoro. Quando modifichi un file nel tuo editor di testo,
   acquisisce lo stato *modificato*: Git vede che hai fatto delle
   modifiche ma non ha ancora fatto nulla al riguardo.

2. **Staged**: questa è un'area di preparazione dove
   vengono collocati i file di cui intendi preservare le
   modifiche. Quando compi lo *staging* di un file, Git include le
   modifiche compiute sul file nel prossimo *commit*, *i.e.* un
   pacchetto di modifiche. Questa procedura permette di processare
   solo alcune delle modifiche compiute mentre continui a lavorare su
   altre.

3. **Committed**: una volta che le modifiche sono
   committate, sono memorizzate in modo sicuro nel database di
   Git. Pensa a questo come allo scatto dell'istantanea di cui
   abbiamo parlato prima - Git ha ora una registrazione permanente di
   queste modifiche.

### Il workflow
Nella pratica, la prima fase è quella di modifica dei file;
immaginiamo di lavorare su `index.html` e `style.css`. È possibile
vedere lo stato del proprio progetto, *e.g.* quali file sono stati
modificati con

```
git status
```

#### Aggiungere file (*staging*)

Per far riconoscere a Git le modifiche compiute, occorre compiere lo
staging (seconda fase)

```
git add index.html style.css 
```

Con `git add`, non stai solo aggiungendo file a Git - stai
mettendo in *stage* le modifiche. Questa è una distinzione
cruciale. Puoi mettere in stage:
- File interi
- Parti di file (hunks)
- File eliminati
- File rinominati

#### Aggiornare lo storico (*committing*)

Per includere le modifiche nello storico di git, bisogna fare un
commit (meglio ancora con l'aggiunta di un breve messaggio)

```
git commit -m "Add button to homepage"
```

> **Nota:** è possibile scrivere messaggi di commit più lunghi e
> descrittivi. In generale, un messaggio è suddiviso nel titolo
> (*subject*) di lunghezza non superiore ai 50 caratteri, e nel corpo
> (*body*) di lunghezza non superiore ai 72 caratteri. Nell'esempio
> proposto, la flag `-m` permette di inserire il titolo rapidamente,
> senza aprire un editor di testo.

> **Nota:** esistono i [sette
> comandamenti](https://cbea.ms/git-commit/) dei buoni messaggi di
> commit; violare i comandamenti implica il dovere di confessarsi e di
> espiare le proprie colpe col parroco del Reparto.

Questo è il cuore del workflow dei progetti gestiti con git: 
 1. modifica dei file
 2. staging
 3. commit

## Operazioni di base
### Visualizzare la storia
I vari comandi `git log` offrono modi diversi per visualizzare la
storia del progetto. Le variazioni più utili sono:

- `--oneline`: per una panoramica rapida
- `--graph`: per visualizzare le relazioni tra i branch
- `--patch` o `-p`: per vedere le modifiche effettive in ogni commit
- `--stat`: per un riepilogo delle modifiche senza dettagli completi

Ad esempio, il modo più veloce per vedere lo storico dei messaggi di
commit del progetto è

```
git log --pretty=oneline
```

Un altro comando utile per visualizzare le ultime modifiche effettuate
su un file è `git diff`; esso mostra la *diff*erenza fra l'ultima
versione del file presente nello storico di Git e la versione corrente

```
git diff <file>
```

### Rimuovere file dal repository
Quando si rimuovono file da Git, ci sono diversi scenari da
considerare. Il comando base è git rm, ma il suo utilizzo dipende da
ciò che vuoi ottenere:

Per rimuovere un file completamente (sia da Git che dal filesystem
locale)

```
git rm filename.txt
```

Per smettere di tracciare un file ma mantenerlo localmente (utile per
file committati per errore)

```
git rm --cached filename.txt
```

### Sincronizzare repository locale e remoto (WIP)
**Work in progress**

Argomenti: push e pull

### Archiviare le modifiche locali (WIP)
**Work in progress**

Argomenti: stashing

## Funzionalità avanzate
Git agevola la vita non solo per il controllo di progetti gestiti da
una singola persona, ma soprattutto da interi team. Qui di seguito
elencheremo alcune delle funzionalità meno banali ma più potenti di Git.

### Comprendere i *branch* (WIP)
**Work in progress**

I branch sono una delle funzionalità più utili di Git. Sono in
realtà solo puntatori a specifici commit. Quando crei un branch, stai
creando un nuovo puntatore. Quando fai un commit, il puntatore si
sposta automaticamente in avanti.

### Risoluzione dei conflitti e *merging* (WIP)
**Work in progress**

Argomenti: diverging branches, merging

#### Strategie alternative (WIP)
**Work in progress**

Argomenti: rebasing

### Cambiare la storia (WIP)
**Work in progress**

Argomenti: rimuovere commit (`git revert`, `git reset`), squashing
(`git squash`)
