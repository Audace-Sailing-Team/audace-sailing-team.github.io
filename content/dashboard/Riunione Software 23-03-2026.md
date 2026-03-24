---
title: Riunione Software 23-03-2026
date: 2026-03-23
weight: 4
---
# I2C 
Effettuate valutazioni sul metodo di approccio alla scrittura dell'interfaccia I2C.
## Considerazioni preliminari
- Il bus I2C è **uno solo** con multipli sensori collegati.
- Ogni sensore ha il suo modo di comunicare lungo il bus i2c, le alternative variano dalla comunicazione a stringhe di testo di "Stile-Seriale" alla lettura diretta di registri del sensore in questione.
- Ogni sensore I2C ha il suo metodo preferito di interfacciarsi con il bus, alcune librerie prendono in inizializzazione l'indirizzo e il bus e fanno tutto autonomamente, mentre altre prevedono che venga creato un oggetto "bus I2C" e che gli venga passato come parametro.
- Potrebbero esserci eventuali problemi nel caso in cui le interfacce di molteplici sensori I2C (ognuna gestita da un thread dedicato) vogliano accedere al bus contemporaneamente. Un'eventuale soluzione sarebbe l'utilizzo del meccanismo dei "mutex" già presenti nel modulo "threading" di python.
  Verrà svolto un breve test per verificare che questo tipo di esclusività non avvenga già a livello del kernel linux nei termini di accesso al bus hardware. 

## Design dell'interfaccia:
Le alternative per definire l'architettura del codice dell'interfaccia I2C sarebbero due:
### Approccio "dirty" (e forse "quick")
Scrittura e aggiunta di una classe dedicata per ogni sensore che si voglia usare, allo stesso livello gerarchico delle classi `SerialInterface`, `MQTTInterface`, ecc...
Ogni sensore ha la propria implementazione dei metodi di accesso al bus e si mantiene la chiamata a una funzione tipo `on_message_callback()` per l'aggiunta in lista dei punti dato.

Pro:
- La scrittura del codice è veloce e diretta, e può essere effettuata anche copiando parti del codice già scritto (e.g. `SerialInterface`).
Contro:
- La soluzione andrebbe in "conflitto" con il paradigma usato fino adesso di definire una classe per ogni *tipo di comunicazione*, ma piuttosto di definire una classe per ogni *tipo di sensore*.
- Questo approccio risulta particolarmente disordinato all'aumentare dei sensori compatibili con mothics. Sarebbe infatti necessario inserire tutto il codice in `comm_interface.py` rendendolo di ancor più difficile navigazione.

### Approccio "clean"
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
---
# GPS
Dopo le considerazione fatte l'ultima riunione, è stato scritto ed implementato il codice relativo alla comunicazione con il modulo GPS.
Dopo un breve debugging il codice esegue e salva correttamente le coordinate dell'unità.

Si valuta lo stesso tipo di scelta architetturale che ci si è posti alla sezione I2C. Uniche differenze sarebbero le seguenti:
- Probabilmente avremmo bisogno di un solo sensore su interfaccia seriale che comunichi con il protocollo NMEA
- **A prescindere** probabilmente non sarà necessario più di un GPS
- Molti GPS che comunicano tramite porta seriale usano lo standard di comunicazione NMEA, il codice è perciò altamente flessibile ed adattabile a diversi modelli di GPS.
