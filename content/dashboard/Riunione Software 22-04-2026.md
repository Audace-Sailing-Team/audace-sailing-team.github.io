---
title: Riunione Software 23-03-2026
date: 2026-04-22
weight: 4
---
# I2C 

Implementato il supporto di sensori I2C secondo le seguenti specifiche:

Scrittura di un'unica interfaccia `I2CInterface`, sottoclasse di `BaseInterface`, che durante l'inizializzazione si adatti al tipo di sensore dichiarato nei file di configurazione.
Questo viene ottenuto tramite la creazione del modulo `i2c_modules.py` contenente:

- Una nuova classe base: `I2CModuleBase`, incaricata di modellare un generico "oggetto I2C". Essa definisce i metodi standard necessari alla manipolazione del sensore, in particolare `setup()`, `read()` e `cleanup()`.
- Un insieme di sottoclassi di `I2CModuleBase` (come `MPU6050Module`, `DPS310Module` e `BNO08xModule`), ognuna delle quali gestisce con del codice ad-hoc l'interazione hardware con il sensore specifico.

L'adattabilità a molteplici sensori è gestita tramite un pattern di registrazione: all'interno del modulo è definito il dizionario `i2c_module_registry` che mappa i nomi dei sensori (es. "mpu6050") alle relative classi. In base al tipo di sensore indicato nella configurazione, `I2CInterface` interroga questo registro per istanziare dinamicamente la classe corretta. L'interazione successiva del ciclo di lettura avviene in maniera totalmente agnostica, utilizzando unicamente i metodi polimorfici standard (`setup()` e `read()`) esposti dalla classe base.

---
# GPS

L'implementazione della classe `GPSInterface` in `comm_interface.py` procede in modo molto pulito, sfruttando l'architettura esistente per minimizzare la duplicazione del codice.

Ecco i punti salienti dello stato attuale:

- **Ereditarietà:** La classe estende direttamente `SerialInterface`. Questa scelta ha permesso di ereditare integralmente la logica di gestione della connessione seriale e del threading in background, limitando l'intervento alla sola sovrascrittura del metodo `_run_loop()`.
- **Filtraggio e Parsing NMEA:** Il loop di lettura analizza il traffico UART in ingresso, isolando esclusivamente le stringhe che iniziano con il carattere `$`. Il parsing effettivo del protocollo NMEA è delegato in modo efficiente alla libreria esterna `pynmea2`.
- **Decodifica selettiva:** Attualmente il sistema è in grado di riconoscere e spacchettare due specifiche sentenze NMEA:
    - **GGA:** Da cui estrae latitudine, longitudine e numero di satelliti visibili.
    - **RMC:** Da cui estrae latitudine, longitudine e il flag di validità del segnale.
- **Routing dei messaggi:** I valori estratti vengono re-immessi nel flusso standard del Communicator tramite la funzione `on_message_callback()`, mappati su topic specifici e coerenti (come `gps/position/lat` o `gps/status/sats`).

Strutturalmente è un'ottima implementazione di base. La presenza di blocchi `try-except` dedicati per gli errori di decodifica `pynmea2.ParseError` garantisce inoltre che un pacchetto seriale corrotto non provochi il crash irreversibile del thread di ascolto.
