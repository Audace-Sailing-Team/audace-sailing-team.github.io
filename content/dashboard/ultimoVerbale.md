---
title: 'Verbale 18/03/2026'
date: 2026-03-19
weight: 4
---

Riassunto dell'ultima riunione in data 2026-03-19
<!--more-->

# Software

## Novità  
- Trovato il modo di far comunicare il gps in seriale con communicator, è sufficiente inserire l'entrata sul file `config.toml` come segue:
```
[serial]
[serial.GPS]
name = "GPS"
port = "/dev/ttyS0"
baudrate = 9600 #per il gps va bene 9600
# Topics can be a single string or a comma‐separated list
topics = "rm2/wind/speed" #ecc...
```
A quel punto l'inizializzazione della porta seriale avviene con successo, tuttavia i dati non sono (ovviamente) leggibili da Mothics, in quanto lui si aspetta una struttura stile-json del tipo "dizionario di pithon" (coppie topic:valore), ma questo non è un problema  

Per riprodurre il comportamento, editare il file di configurazione `config.toml` con l'entrata sopracitata, eseguire `start live`, eseguire `stop` e per vedere che tutto sia accaduto come previsto eseguire `log show`  
Verranno mostrati errori di parsing dei dati ma verrà anche mostrato il dato NMEA

## Funzionamento attuale:
Ogni interfaccia, nello specifico ogni interfaccia seriale, avvia un loop su un thread dedicato, per il quale, ad ogni esecuzione del loop, esegue le seguenti:
- Legge il buffer in arrivo dalla comunicazione seriale (riga 204 di `comm_interface.py`)
- Esegue il parsing dei messaggi, ogni messaggio identificato come una sequenza di caratteri nel formato `.json` (a.k.a. una serie di coppie `topic: valore` separate da virgole e contenute tra due parentesi graffe)  
- Per ogni coppia `topic`:`valore` individuata, chiama il metodo `on_message_callback(topic, valore)` che si occupa di appendere il dato individuato alla lista di dati relativa al `topic` in questione nel campo dizionario `raw_data` dell'interfaccia stessa  

Questo parsing avviene **prima** che venga applicato il preprocessor dedicato all'interfaccia, perciò non è possibile usare questa feature per eseguire il parsing NMEA dei dati gps.  
Sarà quindi necessario alterare il codice dell'interfaccia per eseguire questa operazione.

## Approccio:  
Creare una nuova classe, (e.g. "SerialUartGPSInterface") che erediti tutti i campi e metodi da `SerialInterface` ma che sovrascriva il metodo `_run_loop()` con il codice necessario ad effettuare il parsing dei dati NMEA dal GPS, per poi chiamare il metodo `on_message_callback()` e lasciar proseguire il flusso dei dati come normale.  

## Nota:
Per il futuro, si può pensare ad implementare un modulo del software, con lo stesso identico comportamento degli attuali `preprocessor`, ma dedicato invece a fare il parsing dei dati da varie interfacce particolari. Questo renderebbe superflua la creazione di una nuova classe ad-hoc per ogni sensore che intendiamo collegare all'unità centrale, permettendoci invece di usare le classi già presenti (serial, mqtt, gpio, ecc..) con un "parser" dedicato associato ad ogni interfaccia.  
Questo approccio tuttavia comporta un dispendio di risorse che il nostro reparto in questo momento non può permettersi.  
Verrà quindi studiato l'approcio indicato sopra e questa alternativa verrà indicata come "To do".

---

# Hardware

## Novità  
Ufficializzate le specifiche dell'ubicazione dell'unità centrale:
- Piano A: Tra la banana di prua e la scassa del main foil, posizione ottimale per i sensori ma disastrosa a livelli di spazio  
- Piano B: Sotto la tartaruga, schiacciandoci e spalmandoci sotto i rinvii delle manovre correnti. Buono a livelli di spazio ma pessima posizione per i sensori e accessibilità disastrosa (nonchè esposizione agli agenti atmosferici che avrebbero aiutato la dissipazione di un pelo di calore)  

## Progressi
Iniziata la modellazione e lo studio delle disposizioni dei componenti nel poco spazio a noi dedicato a prua.