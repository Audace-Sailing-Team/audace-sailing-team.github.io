---
title: 'Unità Centrale'
date: 2024-11-28
weight: 2
---

L'**unità centrale (UC)** è costituita da un Raspberry Pi 4 B+, sul quale
gira un software *in-house* scritto in Python, un broker MQTT e un
server Flask. 


<!--more-->


I compiti dell'UC sono
 - acquisizione dati dalle unità remote mediante protocollo MQTT, seriale (e
   TCP/IP)
 - aggregazione e processing dei dati dai sensori
 - hosting di un server locale con Flask per visualizzare i dati
   acquisiti
 - hosting di un broker MQTT
 - access point Wi-Fi per la connessione delle unità remote.

## Software
Ecco alcune note, riflessioni e opinioni sulla struttura del progetto.

### Struttura generale 

![alternative text](//www.plantuml.com/plantuml/png/hLLDJnin4Btlht1leMNp0n2geFI2r8zKHUe18MlYdImkNdjbpz8YslzUZwtW08hYIdk9vetNRvxVU9opGE1f6appPlJG3o2cW9UMq0_OQI1SWRg37eFOW0dODCf024AX9Gz6JbRJKhbXa9htR43XyYB2c5T0pZysuiVbgX7lX8TViWE24z5fHStV6gOFaBHa91vVqt16EMjHqUA8GQV3CpnEQwF3RGHL1Jz_kxfwKV3M1R8mu8duuYmcJkUfe3l4KOBHzrXov4QjGi55z3QZZ4PB1HlNz-WBhZoKQ8LqLkwivaJPKN83XW0zvaJ1Ke4nwzZaMe-kBbR5ejBVETlm7Rq6irEbs1x8CrZ3r7cWw7dSnOcGjYGd_jT51Z4Yj7lrGTzxxFb2By-VmYphTM_JEUx1b0olho5WoVZ9Mctx_HUWRFcO8qnzt3W_0CbqmHRfD_StyadZtBWu4hnxgnVpwTACRqCtvWX_iwD9-zvpRrKiMSJAQKlvqAG7ZAJ3UC92aU5o6sGBnYbJH_rQqNKWVopExSDDJI4ESXrEov7hncdwf0DTFmcvuVMDHjD97i-u_dKaxIoOQ_wGAzwaRw3hCiNYIVLn8jvrWPnKuNwnqGQPxSNIp-3iyxK82BrSFqWcbHviZujaSqeS2xHTYd9xP5GZvLwRWoO-C4zI_E20CM_Rzb2Jclf7zVHFExNrR-Aip6NCv-VUVQMQsdyhuTeAB-QbBKRCGpo32wUjRolUlmCNDTAOAfgAbxRjslUlHwOvGzjDW_a7)

Classi di comunicazione:
 - `Communicator`: classe che aggrega i vari protocolli di
   comunicazione usati, interroga *e.g.* porta seriale e broker MQTT,
   aggrega i dati 'raw' (in un dizionario, con i topics come chiavi e
   liste di dizionari `{timestamp: dato}` come valori); fornisce un
   loop principale che itera su tutti i protocolli di comunicazione
   forniti
 - `BaseInterface`: ABC per le classi specifiche di comunicazione
   `MQTTInterface`, `SerialInterface`

In via più semplice, si può scrivere un'unica classe `Communicator`
che contiene tutti i possibili metodi per tutti i protocolli di
comunicazione. Rigido, poco estendibile.

Classi di analisi dati:
 - `Aggregator`: classe che campiona a intervalli regolari e aggrega
   tutti i dati 'raw' forniti da `Communicator`, ne permette una prima
   analisi dati e li immagazzina in *dataclasses* apposite che ne
   permettono una manipolazione, salvataggio e conversione più
   approfondita.
   
*Dataclasses*:
 - `DataPoint`: singolo punto dati generato da `Aggregator` a
   intervalli regolari
 - `Database`: collezione di `DataPoint` (ed eventuali metadati) che
   permette salvataggio su file, conversione e manipolazione dei dati;
   contiene i dati da visualizzare sulla pagina intranet e da
   preservare su un eventuale database esterno
   
Server Flask:
 - WIP

### Protocollo MQTT
#### Quality of Service (QoS)
Il protocollo MQTT fornisce differenti livelli di QoS, *i.e.*
differenti metodi di consegna dei broadcasts e diversi livelli di
reliability. In particolare, individuiamo
 - **QoS 0** (*best effort*): il protocollo cerca di recapitare i
   messaggi senza restrizioni di alcun genere; essi potrebbero essere
   recapitati più volte, o affatto
 - **QoS 1** (*at least once*): è garantito il recapito del messaggio
   almeno una volta, con potenziali duplicati
 - **QoS 2** (*exactly effort*): è garantito il recapito del messaggio
   una e una sola volta, con un *handshake* fra broker e destinatario
   per garantire la consegna effettiva del broadcast
   
Ai nostri scopi, il minimo livello accettabile è QoS 1; QoS 2 è ideale
ma potrebbe introdurre ritardi e latenze insostenibili. 
   
Alcuni riferimenti:
 - [Mosquitto test broker](https://test.mosquitto.org/)
 - [paho-mqtt
   documentation](https://eclipse.dev/paho/files/paho.mqtt.python/html/index.html)
 - [Sending Data over
   MQTT](https://docs.arduino.cc/tutorials/uno-wifi-rev2/uno-wifi-r2-mqtt-device-to-device/)
 - [A Beginner’s Guide to MQTT: Understanding MQTT, Mosquitto Broker, and Paho Python MQTT Client](https://medium.com/@potekh.anastasia/a-beginners-guide-to-mqtt-understanding-mqtt-mosquitto-broker-and-paho-python-mqtt-client-990822274923)

### Comunicazione via cavo RasPi/Arduino
Non ci dovrebbero essere problemi: i due possono interagire via
seriale con cavo USB, senza ulteriori complicazioni. Via Python, si
può definire una classe opportuna che sfrutta `pyserial`, *e.g.*
`SerialInterface`, che stia in ascolto di dati dall'Arduino.

Alcuni riferimenti:
 - [Serial Communication between Python and
   Arduino](https://projecthub.arduino.cc/ansh2919/serial-communication-between-python-and-arduino-663756)
 - [pySerial short intro](https://pyserial.readthedocs.io/en/stable/shortintro.html)

### Telnet, TCP/IP e Python
Non è taboo implementare anche l'uso del protocollo TCP/IP per
comunicare a basso livello con i moduli remoti. I motivi di una scelta
simile sono vari,
 - sfruttare il lavoro già fatto in questo senso lato Arduino
 - avere un metodo di backup di comunicazione in caso di problemi con
   il broker MQTT, o di eccessiva latenza con invio e ricezione di
   broadcasts MQTT

Alcuni riferimenti:
 - [Socket Programming in Python](https://realpython.com/python-sockets/)

### AsyncIO: asincronia fra `Communicator` e `Aggregator`
`Communicator`, per quanto concerne il protocollo MQTT (via
`MQTTInterface`) può sfruttare le funzioni di libreria `loop_start` e
`loop_stop`, che forniscono un loop asincrono non bloccante, *i.e.*
permettono il pinging del broker MQTT e la contemporanea esecuzione di
altri processi, fino a interruzione (con `loop_stop`) o disconnessione
(con `disconnect`).

Perciò, è possibile avviare il loop via `Communicator` e, a intervalli
regolari, campionare i dati chiamando opportuni metodi di `Aggregator`
senza necessità di usare [`asyncio`](https://realpython.com/async-io-python/).
Tuttavia, l'asincronia può essere implementata se si vuole svolgere
qualche task in quel secondo di attesa fra un campionamento e il
successivo; in tal caso, basta trasformare tutto in funzioni asincrone
e usare `await` sugli `asyncio.sleep`.

Un'accortezza adottata per garantire la sincronia dei dati passati da
`Communicator` a `Aggregator` è un metodo *getter* esterno invece di
un dizionario aggiornato *in place*.  Passare il dizionario `raw_data`
di `Communicator` nell'inizializzatore di `Aggregator` non è una
strategia valida: esso non viene aggiornato *in place* durante il
ciclo non bloccante di raccolta dati di `Aggregator`. Il *getter* è
utile per recuperate in *runtime* il dizionario aggiornato di
`Communicator`.

### Gestione dei dati
I vari dataset possono essere esportati in `.json` ed essere gestiti
remotamente con *TinyDB* o un database relazionale più serio. Pare che
*TinyDB* sia abbastanza versatile per giocare coi dati.

Alcuni riferimenti:
 - [TinyDB docs](https://tinydb.readthedocs.io/en/latest/getting-started.html)

### Flask: applicazioni web con Python
Alcuni riferimenti:
 - [Python Web Applications: Deploy Your Script as a Flask App](https://realpython.com/python-web-applications/#test-locally)
 - [Interactive Data Visualization in Python With Bokeh](https://realpython.com/python-data-visualization-bokeh/)
 - [Embedding a bokeh app in
   flask](https://stackoverflow.com/questions/29949712/embedding-a-bokeh-app-in-flask)
 - [Bokeh documentation](https://docs.bokeh.org/en/latest/index.html)
 - [Bokeh demo](https://demo.bokeh.org/)

