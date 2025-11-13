---
title: 'Guida rapida a Git'
date: 2025-11-13
author: 'Iacopo Ricci, Zio GPT'
weight: 1
---


<!--more-->

# Introduzione
## Premesse 
Allo stato attuale, la guida è ancora in grande parte incompleta. Le sezioni ancora in procinto di essere scritte o mancanti sono contrassegnate da **WIP** nel titolo e dall'indicazione **Work in progress** nel corpo del testo.

## Come usare questa guida
Qui, cerchiamo di dare tutte le informazioni di base necessarie per iniziare a usare *git* e per capire il perchè di questa scelta. Per far ciò, violeremo tutti i precetti delle guide rapide all'uso: ci saranno tante, troppe parole ad accompagnare i comandi più pratici.

Suggeriamo agli utenti novizi particolarmente zelanti di leggere l'intera guida (con pazienza). Invece, chi ha già idea di cosa sia un sistema di controllo versione e ha già *git* installato, può saltare l'intera introduzione.

## Perchè usare Git?
Prima di approfondire Git nello specifico, è fondamentale capire perché esiste il **controllo versione**.

Immagina di scrivere una tesi di laurea. Potresti salvare diverse versioni del tuo lavoro: *"tesi-bozza1.doc"*, *"tesi-bozza2.doc"*, *"tesi-finale.doc"*, *"tesi-finale-DAVVERO.doc"*. Questo controllo versione manuale diventa rapidamente ingestibile, e diventa facile inviare al proprio relatore la versione sbagliata della tesi. Ora immagina di coordinare questo lavoro con altri dieci autori, ognuno che lavora su capitoli diversi. Questo è il problema che i sistemi di controllo versione risolvono.

I sistemi di controllo versione automatizzano questo processo di tracciamento delle modifiche. Infatti, essi mantengono una cronologia completa di chi ha modificato cosa e quando, permettono a più persone di lavorare simultaneamente senza interferire l'uno con l'altro e forniscono meccanismi per unire il lavoro di tutti.

Ovviamente, la stesura di una tesi di laurea è solo un possibile utilizzo (particolarmente di nicchia) di Git. L'utilizzo più comune è la gestione di progetti informatici complessi, ed è stata proprio questa la motivazione dietro alla sua nascita.

## Git, Github, Gitlab, ...
Più che di *git*, è facile sentire parlare di *GitHub* e di *GitLab*. Qual è la differenza fra i tre?

*Git* è un sistema di controllo versione distribuito, *i.e.* uno strumento che tiene traccia delle modifiche nel codice sorgente compiute durante lo sviluppo del software; esso opera localmente sul tuo computer.

*GitHub* e *GitLab* sono piattaforme web che forniscono hosting per repository (*i.e.* progetti) Git insieme a funzionalità aggiuntive.

In breve, *Git* è lo strumento che gestisce il codice, mentre *GitHub* e *GitLab* sono strumenti web costruiti attorno a *Git* che permettono di salvare online i propri progetti e di collaborare da remoto con altri utenti.

## Configurazione
*Git* è uno strumento da linea di comando. Esistono interfacce grafiche per agevolare gli utenti occasionali, ma spesso esse impediscono di avere il grado di controllo che garantisce la linea di comando.

### Installazione
Su **Linux**, Git è spesso pre-installato. Per verificare se Git è presente, è sufficiente aprire il terminale e scrivere

```
git --version
```

Se non succede niente, [qui](https://git-scm.com/downloads) sono forniti tutti i riferimenti per l'installazione.

Su **Windows**, è caldamente consigliato l'uso del **W**indows **S**ubsystem for **L**inux, che permette di seguire la stessa identica procedura mostrata sopra. Per i più ostinati e avversi a WSL, l'[installatore](https://git-scm.com/downloads/win) include sia gli strumenti da riga di comando che un'interfaccia grafica.

Su **Mac**... dovrebbe essere sufficiente Homebrew; gli autori di questa guida nulla sanno, e nulla di più vogliono sapere a riguardo.

> **Nota**: potrebbe essere richiesto di inserire le variabili `user.name` e `user.email`: esse identificano l'utente nei messaggi dei commit. In teoria, dovrebbero corrispondere al proprio account GitHub/GitLab, ma è raccomandabile fornire altrove il *vero* indirizzo email di contatto.

> **Nota**: la variabile `core.editor` imposta l'editor di testo preferito per i messaggi di commit e le operazioni interattive da riga di comando. Per i novizi e i non masochisti, è raccomandabile usare `nano`, che non offre grandi potenzialità ma è sufficientemente intuitivo per chi è alle prime armi. Per i lettori del Marchese de Sade, suggeriamo l'uso di `vim` o `emacs` - scelte predilette dagli autori.

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

Per iniziare a usare *git*, è necessario creare un **repository locale**, cioè una struttura che conserva la storia delle modifiche del progetto. Nella cartella del progetto, il comando
```bash
git init
```
crea la cartella nascosta `.git/`, all’interno della quale Git memorizza commit, configurazioni e tutto ciò che riguarda il versionamento. Questa directory non deve essere eliminata, poiché costituisce l’intero cuore del repository!


Una volta inizializzato il progetto, è possibile preparare i file per il primo commit
```bash
git add .
git commit -m "Initial commit"
```

Con queste operazioni si stabilisce la base della storia locale del progetto: spiegheremo il significato dei vari comandi a breve.

### Collegamento a repository remoti
Il modo più semplice per connettersi a *GitHub* o *GitLab* è creare un repository sulla piattaforma scelta. Ovviamente, serve un account su una delle due piattaforme!

> **Nota:** l'interfaccia grafica fra Github e Gitlab è abbastanza diversa, ma le funzionalità alle quali faremo riferimento sono le stesse per entrambi.

Una volta creato un repository su GitHub o GitLab, sarà possibile associarlo al progetto locale con
```
git remote add origin <URL-repository>
```

`<URL-repository>` è l'URL del repository remoto; sarà di tipo HTTPS o SSH, con due diverse procedure di configurazione.

L'opzione più semplice è HTTPS, che richiede l'inserimento delle proprie credenziali a ogni operazione compiuta dal terminale, *e.g.*  per caricare il proprio codice sul repository remoto.

>**Nota:** l'uso di SSH evita di inserire le credenziali ogni volta, ma richiede la generazione di una chiave SSH; la procedura è descritta nella sezione apposita.

>**Nota:** non sempre è possibile utilizzare HTTPS senza aver comunque configurato le chiavi SSH. Molti server remoti hanno disabilitato da qualche tempo il login da terminale con le credenziali *classiche* (username, password) per motivi di sicurezza.

Infine, si può caricare il codice sul server remoto con
```bash
git push -u origin main
```

Il flag `-u` imposta il tracking, così i push futuri necessiteranno solo di `git push`.

#### Flussi di lavoro
A seconda della situazione di partenza, si possono seguire percorsi diversi per collegare o creare repository remoti.

##### Caricare un repository esistente
Questo caso si presenta quando il progetto è già un repository Git locale (la cartella `.git` è già presente) e si vuole trasferire l’intera storia su un server remoto. È il caso più frequente, e di solito si manifesta quando si è lavorato a lungo in locale e si decide di pubblicare il repository per la prima volta.
```bash
cd existing_repo
git remote rename origin old-origin  # solo se esiste già un remote chiamato `origin`
git remote add origin git@<server>:<username>/<repository-name>.git
git push -u origin --all  # carica tutti i branch
git push -u origin --tags  # carica tutti i tag
```

##### Caricare una cartella esistente
Qui il progetto esiste già, ma **non** è ancora un repository Git.

> **Nota:** se nella cartella è già presente `.git/`, significa che il repository esiste già. La sua rimozione eliminerà completamente la storia locale.

```bash
cd existing_folder
git init --initial-branch=main
git remote add origin git@<server>:<username>/<repository-name>.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

In questo modo si crea un repository locale da zero e lo si pubblica sul server remoto.

##### Creare un repository da zero sul server remoto
In questo scenario il repository è stato creato prima sul server remoto (*e.g.* con l’interfaccia web di GitHub o GitLab) e si desidera iniziare il lavoro partendo da esso.
```bash
git clone git@<server>:<username>/<repository-name>.git
cd <repository-name>
git switch -c main
touch README.md
git add README.md
git commit -m "add README"
git push -u origin main
```

#### Chiavi SSH
L’uso delle chiavi SSH consente un’autenticazione sicura e stabile verso servizi Git remoti. L’obiettivo è generare una coppia di chiavi, conservarla in modo appropriato e registrare la chiave pubblica presso GitHub o GitLab.

La creazione di una chiave SSH può essere effettuata tramite
```bash
ssh-keygen -t ed25519
```

Durante la procedura è possibile (ma non strettamente necessario) impostare una passphrase per aumentare la sicurezza; in caso contrario, è sufficiente premere Invio tre volte! 
Il comando produce due file, **id_ed25519** (contiene la chiave privata **da non condividere**) e **id_ed25519.pub** (contiene la chiave pubblica da registrare sui servizi remoti).

Per vedere la propria chiave pubblica, è sufficiente eseguire
```bash
cat ~/.ssh/id_ed25519.pub
```

> Nota: **MAI** condividere la propria chiave privata!

Sulle due piattaforme, la configurazione della chiave SSH è molto simile, sebbene non sia identica. In ogni caso, dopo aver seguito i passi successivi, non occorrerà più fare il login per effettuare `pull`, `push` e `clone` di repositories!

##### GitHub
1. Nel menù, selezionare **Settings/SSH and GPG keys**
2. Selezionare **New SSH key**
3. Incollare la chiave pubblica (*i.e.* il contenuto di `id_ed25519.pub`)
4. Salvare!

##### GitLab
1. Nel menù, selezionare **Preferences/SSH Keys**
2. Selezionare **Add new key**
3. Incollare la chiave pubblica (*i.e.* il contenuto di `id_ed25519.pub`)
4. Salvare!

## I tre stati
Immaginiamo che Git contenga tre aree di lavoro o *stati* dei file:
1. **modificato**: questo è lo spazio di lavoro principale. Quando viene modificato un file nel repository, esso acquisisce lo stato *modificato*: Git vede che sono state fatte delle modifiche al file, ma non ha ancora fatto nulla al riguardo.

2. **staged**: questa è un'area di preparazione dove vengono collocati i file di cui preservare le modifiche. Quando si effettua lo *staging* di un file, Git include le modifiche compiute sul file nel prossimo *commit*, *i.e.* un pacchetto di modifiche. Questa procedura permette di processare solo alcune delle modifiche compiute; nel mentre, si può continuare a lavorare su altre.

3. **committed**: una volta che le modifiche sono *committate*, sono memorizzate in modo sicuro nel database di Git. Pensa a questo come allo scatto dell'istantanea di cui abbiamo parlato prima - Git ha ora una registrazione permanente di queste modifiche.

### Il workflow
Nella pratica, la prima fase è quella di modifica dei file; immaginiamo di lavorare su `index.html` e `style.css`. È possibile vedere lo stato del proprio progetto, *e.g.* quali file sono stati modificati con
```
git status
```

#### Aggiungere file (*staging*)
Per far riconoscere a Git le modifiche compiute, occorre compiere lo staging (seconda fase)
```
git add index.html style.css 
```

Con `git add`, non si aggiungono solamente file a Git - le modifiche vengono messe in *stage*. Questa è una distinzione cruciale! Si possono *mettere in stage*:
- file interi
- parti di file (*hunks*)
- file eliminati
- file rinominati

#### Aggiornare lo storico (*committing*)
Per includere le modifiche nello storico di git, bisogna fare un commit (meglio ancora con l'aggiunta di un breve messaggio)
```
git commit -m "Add button to homepage"
```

> **Nota:** è possibile scrivere messaggi di commit più lunghi e descrittivi. In generale, un messaggio è suddiviso nel titolo (*subject*) di lunghezza non superiore ai 50 caratteri, e nel corpo (*body*) di lunghezza non superiore ai 72 caratteri. Nell'esempio proposto, la flag `-m` permette di inserire il titolo rapidamente, senza aprire un editor di testo.

> **Nota:** esistono i [sette comandamenti](https://cbea.ms/git-commit/) dei buoni messaggi di commit; violare i comandamenti implica il dovere di confessarsi e di espiare le proprie colpe col parroco del Reparto.

Questo è il cuore del workflow dei progetti gestiti con git:
 1. modifica dei file
 2. staging
 3. commit

## Operazioni di base
### Visualizzare la storia
I vari comandi `git log` offrono modi diversi per visualizzare la storia del progetto. Le variazioni più utili sono:

- `--oneline`: per una panoramica rapida
- `--graph`: per visualizzare le relazioni tra i branch
- `--patch` o `-p`: per vedere le modifiche effettive in ogni commit
- `--stat`: per un riepilogo delle modifiche senza dettagli completi

Ad esempio, il modo più veloce per vedere lo storico dei messaggi di commit del progetto è
```
git log --pretty=oneline
```

Un altro comando utile per visualizzare le ultime modifiche effettuate su un file è `git diff`; esso mostra la *diff*erenza fra l'ultima versione del file presente nello storico di Git e la versione corrente
```
git diff <file>
```

### Rimuovere file dal repository
Quando si rimuovono file da Git, ci sono diversi scenari da considerare. Il comando base è `git rm`, ma il suo utilizzo dipende da ciò che si vuole ottenere!

Per rimuovere un file completamente (sia da Git che dal filesystem locale)
```
git rm filename.txt
```

Per smettere di tracciare un file ma mantenerlo localmente (utile per file committati per errore)
```
git rm --cached filename.txt
```

### Sincronizzare repository locale e remoto
La sincronizzazione tra repository locale e remoto si basa su due operazioni fondamentali: *pull* e *push*. Queste azioni permettono di mantenere allineato il lavoro tra la macchina locale e il server Git.

Il *pull* rappresenta il modo più diretto per ottenere dal server la versione più recente del progetto. Nella pratica quotidiana, quando il repository remoto ha subito aggiornamenti, è sufficiente utilizzare
```bash
git pull
```

Questo comando scarica le modifiche e le integra nella copia locale. Nei casi più elementari, l'operazione avviene senza alcun intervento aggiuntivo; quando invece il server contiene versioni che entrano in conflitto con modifiche locali, Git richiede una risoluzione manuale - di cui parleremo in seguito.

Il *push*, invece, consente di inviare al server il lavoro prodotto in locale. Dopo aver creato i commit necessari, la pubblicazione può avvenire tramite
```bash
git push
```

Quando la copia locale è perfettamente allineata con quella remota, il comando procede senza ostacoli. Nel caso in cui il remoto presenti modifiche non ancora integrate localmente, Git interrompe l’operazione per evitare sovrascritture, mostrando un avviso simile a
```bash
! [rejected] main -> main (non-fast-forward)
error: failed to push some refs
hint: Updates were rejected because the remote contains work that is not present locally.
```

In una situazione di questo tipo, è sufficiente un pull preliminare per prevenire perdite di dati e, successivamente, procedere con la pubblicazione.

Per i casi d’uso più semplici, il **flusso operativo** può essere riassunto così: 
 1. eseguire un *pull* per assicurarsi che la base di lavoro sia aggiornata
 2. modificare file, staging, creazione di commit 
 3. eseguire *push* per rendere disponibili le modifiche sul server. 
 
Questa sequenza costituisce la routine fondamentale per qualunque progetto collaborativo basato su Git.

### Archiviare le modifiche locali
Quando si lavora su un repository Git, può capitare di aver bisogno di mettere temporaneamente da parte alcune modifiche senza però volerle includere in un commit. Lo *stashing* offre un modo semplice e pulito per archiviare il lavoro in corso, così da poter tornare rapidamente a uno stato pulito del progetto.

Il comando di base è estremamente immediato
```bash
git stash
```

Con questa operazione, Git salva le modifiche non ancora committate e ripristina la directory di lavoro allo stato dell’ultimo commit. È un meccanismo utile quando serve cambiare ramo per intervenire su un altro problema urgente o per aggiornare il proprio ambiente tramite un pull.

> Nota: le modifiche vengono archiviate localmente.

Per una maggiore chiarezza, può essere utile associare un messaggio allo stash, così da identificarne il contenuto
```bash
git stash push -m "lavoro in corso sulla nuova funzione"
```

Il recupero delle modifiche archiviate avviene tramite
```bash
git stash apply
```

Questo comando ripristina il contenuto dell’ultimo stash senza rimuoverlo dalla pila. Qualora si volesse recuperare e contemporaneamente eliminare lo stash, l’opzione più diretta è
```bash
git stash pop
```

Nei casi più semplici, l’applicazione dello stash avviene senza complicazioni. Tuttavia, come accade anche in altri scenari Git, è possibile che emergano conflitti qualora il codice recuperato non si integri perfettamente con lo stato attuale del ramo. Un messaggio tipico potrebbe essere
```bash
CONFLICT (content): Merge conflict in script.py
```

In una situazione simile, è sufficiente modificare manualmente il file segnalato, risolvere il conflitto ed eseguire il commit necessario.

Per avere una panoramica degli stash archiviati, Git fornisce un elenco ordinato
```bash
git stash list
```

Ogni elemento può essere applicato selettivamente indicando il suo indice
```bash
git stash apply stash@{2}
```

## Funzionalità avanzate
Git agevola la vita non solo per il controllo di progetti gestiti da una singola persona, ma soprattutto da interi team. Qui di seguito elencheremo alcune delle funzionalità meno banali ma più potenti di Git.

### Comprendere i *branch*
In Git, il concetto di *branch* costituisce uno dei pilastri fondamentali dell’intero modello di sviluppo. 
Un branch può essere immaginato come una linea temporale separata del progetto: una copia dello stato corrente che permette di lavorare in modo indipendente, senza influenzare la versione principale. Questa idea semplice apre le porte a un flusso di lavoro estremamente potente, perché consente di sperimentare, aggiungere funzionalità o correggere errori senza mettere a rischio la stabilità del codice esistente.

#### Le basi
Ogni repository Git parte con un unico branch predefinito: tradizionalmente *master*, oggi più comunemente *main*. Questo ramo rappresenta il cuore del progetto, la versione stabile, testata e pronta per essere distribuita o rilasciata in produzione. Tutto ciò che finisce su *main* dovrebbe avere superato un ciclo di revisione e test adeguato.

Accanto al branch principale è frequente trovare altri rami. Un esempio comune è la presenza di un branch *develop*, pensato come zona di integrazione continua, dove diverse funzionalità possono coesistere e venire testate prima di finire sul *main*. A questo possono affiancarsi branch tematici, ad esempio
 - *mothics/fix-kalman-filter*
 - *anemometer-ur/rpm-averaging*
 - ...

Ogni branch ha uno scopo chiaro e delimitato. Questo approccio riduce il rischio di interferenze tra modifiche diverse e migliora l’ordine complessivo del progetto.

#### Perché usare un branch
È fortemente consigliato creare sempre un branch dedicato quando si lavora a una nuova funzionalità o si interviene su una funzionalità esistente. Lavorare direttamente sul ramo principale significa esporre il progetto a possibili problemi, soprattutto quando le modifiche non sono ancora mature. Con un branch separato, invece, il lavoro rimane isolato fino a quando non è pronto per essere reintegrato.

Un esempio banale di utilizzo dei branch per l'introduzione di una nuona feature è il seguente
```bash
git checkout -b kalman-filter
```

A questo punto l’ambiente di lavoro è completamente separato e le modifiche potranno essere committate senza toccare il ramo principale. Una volta completato il lavoro, si potrà procedere al merge.

Per vedere i vari branch disponibili, basta eseguire
```bash
git branch -va
```


#### Il merge e i suoi rischi
Quando un branch è pronto, può essere reintegrato nella linea principale tramite un *merge*. Questa è l'operazione più temuta da buona parte degli utilizzatori di Git sull'intero globo terracqueo. 
Qui,  Git dovrà combinare due storie diverse: quella del branch e quella del ramo nel quale si sta effettuando il merge. Se i due percorsi hanno modificato le stesse parti di uno o più file, Git non può decidere automaticamente quale versione sia corretta e richiederà un intervento manuale - con conseguenti sofferenze degli autori delle modifiche.

Un conflitto tipico potrebbe avere questa forma
```bash
CONFLICT (content): Merge conflict in utils/helpers.py
```

Ogni conflitto richiede una revisione attenta e un ulteriore commit di risoluzione. Approfondiremo i meccanismi del merge in seguito, poiché costituiscono una parte cruciale e talvolta delicata del flusso.

#### Quando è troppo è troppo
Per quanto i branch siano uno strumento eccezionalmente utile, la loro proliferazione incontrollata può generare caos: branch vecchi, mai integrati, abbandonati senza motivo o mantenuti per mesi senza aggiornamenti rispetto al *main*. Un tale scenario crea confusione e rende più difficile eseguire merge puliti in futuro.

È buona norma:
 - mantenere solo branch realmente attivi;
 - dare nomi chiari e coerenti;
 - aggiornare frequentemente i branch lunghi fondendo i cambiamenti dal ramo principale;
 - eliminare i branch appena conclusi tramite merge.

Per eliminare un branch locale (*e.g.* `kalman-filter`)
```bash
git branch -d kalman-filter
```

Per eliminare un branch remoto
```bash
git push origin --delete kalman-filter
```

#### Flusso di lavoro (ovvero come salvare la pelle)
Immaginiamo la necessità di aggiungere una nuova funzionalità senza causare problemi al contenuto del *main* branch (e quindi all'utente finale). 

Il primo passo è **aggiornare il branch principale**
```bash
git switch main
git pull
```

Poi, occorre **creare il branch** dedicato (al solito, *e.g.* `kalman-filter`)
```bash
git checkout -b kalman-filter
```

A questo punto è possibile lavorare alle variazioni al codice, fare commit, e testare! Una volta soddisfatti del risultato, occorre tornare su *main*
```bash
git switch main
git pull
```

Ora è possibile **unire il branch**
```bash
git merge kalman-filter
```

Se non sono avvenuti conflitti e tutto è filato liscio, si può **eliminare il branch** ormai inutile
```bash
git branch -d kalman-filter
```

### Risoluzione dei conflitti e *merging* (WIP)
**Work in progress**

Argomenti: diverging branches, merging

#### Strategie alternative (WIP)
**Work in progress**

Argomenti: rebasing

### Cambiare la storia (WIP)
**Work in progress**

Argomenti: rimuovere commit (`git revert`, `git reset`), squashing (`git squash`)
