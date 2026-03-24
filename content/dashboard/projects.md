---
title: 'Progetti in corso'
date: 2026-03-19
weight: 4
---

Ogni progetto in corso richiede un luogo nel quale annotare idee, progetti, proposte e riflessioni. Chiunque prenda parte al processo di sviluppo di un progetto del reparto con qualcosa da dire è il benvenuto!
<!--more-->

# UC1: alimentazione
...WIP...

---

# UC2: RasPi e sensori IMU, GPS
L'obiettivo del gruppo di lavoro è studiare la possibile rimozione dei microcontrollori dall'Unità Centrale, connettendo direttamente i sensori al Raspberry Pi. 
Un possibile approccio richiede la costruzione di un bus I2C (o una qualunque alternativa equivalente) che connetta i sensori ai GPIO del RasPi. A esso, va affiancata una interfaccia di comunicazione (*e.g.* `I2CInterface`) per `Communicator`, in Mothics; il suo scopo è l'acquisizione e decodifica dei dati ricevuti.


## I2C (e IMU nello specifico)

### Generalità
- I2C è un protocollo seriale, multi-master, multi-slave, che richiede due linee dati:
	 - SCL (serial clock): linea di clock, gestita dal master
	 - SDA (serial data): linea dati, bidirezionale (master <-> slave)
- Ogni dispositivo/sensore slave è dotato di un proprio indirizzo a 7 o 10 bit, non noto a priori al master.
- Ogni sensore ha il suo modo di comunicare lungo il bus i2c, le alternative variano dalla comunicazione a stringhe di testo di "Stile-Seriale" alla lettura diretta di registri del sensore in questione.
- Ogni sensore I2C è inizializzato in un modo quasi-unico, alcune librerie prendono in inizializzazione l'indirizzo e il bus e fanno tutto autonomamente, mentre altre prevedono che venga creato un oggetto "bus I2C" e che gli venga passato come parametro.
- Potrebbero esserci eventuali problemi nel caso in cui le interfacce di molteplici sensori I2C (ognuna gestita da un thread dedicato) vogliano accedere al bus contemporaneamente. Un'eventuale soluzione sarebbe l'utilizzo del meccanismo dei "mutex" già presenti nel modulo "threading" di python.
  Verrà svolto un breve test per verificare che questo tipo di esclusività non avvenga già a livello del kernel linux nei termini di accesso al bus hardware. 


In linea di massima, è possibile costruire una dorsale di comunicazione (bus) che connette tutti i sensori con il master. Esso dovrà procedere a 
 - ottenere gli indirizzi di tutti gli slave manualmente (inseriti dall'utente), o automaticamente con strumenti come `i2cdetect`
 - emettere la condizione di start e inviare a tutti i dispositivi la richiesta di lettura/scrittura corredata dell'indirizzo dello slave di interesse
 - ottenere l'`ACK` dal dispositivo con l'indirizzo corretto e leggere/scrivere i registri dello stesso
 - emettere la condizione di stop.

### Design del bus
Le alternative per definire l'architettura del codice dell'interfaccia I2C sarebbero due:
#### Approccio "dirty" (e forse "quick")
Scrittura e aggiunta di una classe dedicata per ogni sensore che si voglia usare, allo stesso livello gerarchico delle classi `SerialInterface`, `MQTTInterface`, ecc...
Ogni sensore ha la propria implementazione dei metodi di accesso al bus e si mantiene la chiamata a una funzione tipo `on_message_callback()` per l'aggiunta in lista dei punti dato.

Pro:
- La scrittura del codice è veloce e diretta, e può essere effettuata anche copiando parti del codice già scritto (e.g. `SerialInterface`).
Contro:
- La soluzione andrebbe in "conflitto" con il paradigma usato fino adesso di definire una classe per ogni *tipo di comunicazione*, ma piuttosto di definire una classe per ogni *tipo di sensore*.
- Questo approccio risulta particolarmente disordinato all'aumentare dei sensori compatibili con mothics. Sarebbe infatti necessario inserire tutto il codice in `comm_interface.py` rendendolo di ancor più difficile navigazione.

#### Approccio "clean"
Scrittura di un'unica interfaccia `I2CInterface`, sottoclasse di `BaseInterface`, e che nell'inizializzazione si *adatti* al tipo di sensore dichiarato nei file di configurazione.
Questo verrebbe ottenuto tramite la creazione di un nuovo modulo contenente: 
- Una nuova classe base: `I2CSensor`, incaricata di modellare un "oggetto I2C". Essa possiederà i campi e metodi (e.g. getSensorData() o initalizeSensor()) necessari alla manipolazione di un generico sensore I2C.
	- Un'insieme di sottoclassi di `I2CSensor`, ognuna delle quali gestisce con del codice ad-hoc l'interazione con il sensore specifico.
L'adattabilità a molteplici sensori può essere gestita con un meccanismo simile (se non identico) a quello usato per distinguere le varie interfacce di comunicazione: in base alle opzioni dichiarate nel file di configurazione (nelle quali sarà indicato il modello del sensore I2C interessato) viene istanziata la classe dedicata al tipo di sensore. L'interazione con questa classe avviene successivamente con i metodi di accesso al sensore standard dichiarati in `I2CSensor`.

Pro:
- Maggior ordine nella scrittura del codice: viene mantenuto il paradigma di Communicator, dove risiedono le descrizioni delle "interfacce di comunicazione".
- Minor confusione e sovraccarico del file `comm_interface.py`, il quale vedrà aggiunta solo una classe `I2CInterface`, mentre il codice dedicato alla gestione dei vari tipi di sensore I2C avverrà in un modulo (file) separato.
- Maggiore modularità e sviluppabilità del codice, nel caso di sviluppo e utilizzo di nuovi e svariati sensori I2C.
Contro:
- Aumento del carico di scrittura del codice con la creazione di un nuovo modulo con i meccanismi di gestione di vari sensori I2C.

### Interfaccia per Communicator
L'approccio più congruo alle scelte di design del codice richiede la creazione di un'interfaccia unica, `I2CInterface`. Essa agirà da *master*, con i vari sensori I2C del bus come *slaves*. I suoi compiti sono
 1. interfacciarsi *con il metallo*, *i.e.* con i pin I2C-enabled dei GPIO del RasPi
 2. acquisire gli indirizzi dei sensori sul bus
 3. interrogare periodicamente i vari sensori connessi al bus per acquisirne i dati
 4. tradurre i dati per renderli utili e fruibili ai livelli superiori di Mothics (`Aggregator`, ..., utente finale)
 
### Interazione con i GPIO
Il RasPi offre i pin
 - GPIO 2 (SDA), GPIO 3 (SCL) come principali pin I2C
 - GPIO 0 (SDA), GPIO 1 (SCL) come pin I2C abilitabili via software.

## UART (e GPS nello specifico)

### Generalità
La porta UART del Raspberry Pi è accessibile come qualsiasi altra porta seriale di sistema, all'indirizzo `/dev/ttyS0`; è quindi sufficiente riutilizzare il codice di `SerialInterface` per collegarsi a tale porta.  
Da notare che il suddetto canale di comunicazione, salvo nostra ignoranza, non è un "bus" inteso nel senso comune (un canale di comunicazione al quale possono "agganciarsi" molteplici sensori), ma è semplicemente un collegamento tra raspberry ed una sola periferica. 

### GPS

>Fra i sensori GPS a disposizione è presente l'Arduino MKR GPS, basato su u-blox SAM-M8Q GNSS. Fortunatamente, è disponibile la [schematica](https://docs.arduino.cc/resources/schematics/ASX00017-schematics.pdf). Il sensore verrà usato quasi esclusivamente per la prototipazione e il debugging; è ideale se utilizzato con un Arduino MKR, mentre non è la soluzione ottimale in qualunque altro caso.
>
>Da quanto emerge dalla [documentazione](https://docs.arduino.cc/resources/pinouts/ABX00023-full-pinout.pdf) del microcontrollore associato (famiglia Arduino MKR), la porta di comunicazione alternativa alla modalita HAT è un connettore JST-PH a 5 pin che fornisce alimentazione a 5V e GND, i due pin I2C canonici e un terzo pin corrispondente al pin D7 dell'header.
>
>L'altro sensore attualmente disponibile è il GY-NEO6MV2, basato su u-blox NEO-6M.  
Il testo sopra è superfluo alla attuale attività di progettazione, verrà tenuto in questa forma ai puri fini della preservazione, e in futuro rimosso  

### Approccio:  
Creare una nuova classe, (e.g. "SerialUartGPSInterface") che erediti tutti i campi e metodi da `SerialInterface` ma che sovrascriva il metodo `_run_loop()` con il codice necessario ad effettuare il parsing dei dati NMEA dal GPS, per poi chiamare il metodo `on_message_callback()` e lasciar proseguire il flusso dei dati come normale.  

### Nota:
Per il futuro, si può pensare ad implementare un modulo del software, con lo stesso identico comportamento degli attuali `preprocessor`, ma dedicato invece a fare il parsing dei dati da varie interfacce particolari. Questo renderebbe superflua la creazione di una nuova classe ad-hoc per ogni sensore che intendiamo collegare all'unità centrale, permettendoci invece di usare le classi già presenti (serial, mqtt, gpio, ecc..) con un "parser" dedicato associato ad ogni interfaccia.  
Questo approccio tuttavia comporta un dispendio di risorse che il nostro reparto in questo momento non può permettersi.  
Verrà quindi studiato l'approcio indicato sopra e questa alternativa verrà indicata come "To do".

### Parsing dei dati
Il parsing può essere effettuato sfruttando due librerie di Python
 - [`pynmea2`](https://github.com/Knio/pynmea2) per il parsing dei dati del GPS (protocollo NMEA 0183)
 - una libreria stile-`smbus` per il parsing dei dati degli altri sensori.

---

# AU1: Anemometro

### Obiettivo
Realizzare un anemometro a coppette e banderuola affidabile che possa comunicare con l’unità centrale tramite [inserire protocollo].  
Il design utilizza tecnologia a sensori ad effetto Hall per rilevare il movimento dei componenti mantenendo comunque la tenuta stagna.

### Stato attuale  
Effettuati i primi test con la comunicazione MQTT.  
MQTT non sembra promettente per questa applicazione, il tempo tra i messaggi è lungo e alcuni si perdono.  
Necessario valutare e approfondire le alternative a MQTT  

---

# MQTT
## Idee per migliorare la trasmissione
- Inviare i dati grezzi dal sensore e non pacchetti o Json riduce il carico in ricezione;
- Impostare la comunicazione in QoS = 0 (Quality Of Service);
- Aumentare la frequenza di comunicazione a meno di 2 secondi (1s, 500 ms): l'aumento della frequenza di trasmissione aumenta il carico sulla rete e il rischio di congestione, si può arrivare a 200 ms come margine di sicurezza;
- Diminuire la frequenza di Keep-alive (tempo in cui la comunicazione può rimanere inattiva prima che il broker chiuda il collegamento) può ridurre il traffico sulla rete e lasciare maggiori risorse alla trasmissione dati;
- Evitare di aprire/chiudere la connessione ad ogni messaggio, ma utilizzare una sessione persistente;
- Uso di MQTT v5, che supporta i topic alias, permettendo di non inviare ogni volta il nome completo del topic: meno byte, meno latenza.


## Quality of Service
La libreria PubSubClient, implementa esclusivamente QoS 0 per la comunicazione MQTT, e non supporta QoS 1 o QoS 2.  
