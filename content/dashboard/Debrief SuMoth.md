---
title: Debrief SuMoth
date: 2026-05-12
weight: 4
---

Debrief SuMoth
<!--more-->

# Elettronica alla SuMoth del 2026
Questa SuMoth, per varie sviste, sfortune ed imprevisti, non è stato possibile montare l'elettronica a bordo di Neverina.
I problemi riscontrati possono essere tutti inseriti nelle seguenti macro-problematiche:
- Problemi di impermeabilità
- Problemi di collocazione a bordo
Per questo, le uniche tracce registrate provengono da due uscite in gommone, durante le quali è stata portata la scatola di elettronica, che hanno comunque prodotto informazioni utili.

# Software
## mothics
Alla SuMoth è stato usato mothics con la vecchia interfaccia, in quanto semplice da navigare e reattiva dinamicamente ai cambiamenti di setup Hardware che sono stati necessari. 
Il software inoltre si è dimostrato stabile con il minimo intervento, permettendo al team di concentrarsi sugli altri problemi, protagonisti di questa edizione.
Non è stata rimossa la mappa (vedi considerazioni hardware/batteria in seguito).
Si è notato che ogni traccia registrata conteneva una traccia gps "extra" vuota, associata ad un'unità "rm1", a prescindere sia dalla configurazione software che hardware del sistema.
La problematica non è stata estensivamente indagata, ma potrebbe essere di semplice risoluzione.
Si conclude decretando che mothics sia "pronta all'uso" sebbene abbia ancora qualche problema.
## mothics-analysyis
E' stato effettuato un tentativo di lettura delle tracce registrate utilizzando l'applicativo sperimentale e poco testato mothics-analisys.
Sebbene sia stato possibile l'avvio del programma, con minimo intervento al software, rimangono presenti problemi di lettura delle tracce, nello specifico gps. Difatti ognuna di queste falliva a mostrare la traccia gps nel riquadro apposito, mostrando il messaggio di errore "gps unavailable"
Sebbene sia stata leggermente indagata questa problematica, non è stata trovata nessuna soluzione accertata.
mothics-analisys rimane dunque ancora "non pronta all'uso", sebbene funzionante
# Hardware
## Unità di elaborazione
E' stato utilizzato un Raspberry Pi 4 come unità di elaborazione.
Non sono stati riscontrati problemi nella capacità di elaborazione, dimensione della memoria e emissione di calore (la temperatura della scatola è rimasta tutto il tempo ben al di sotto dei 40 gradi).
Si è riscontrato che le dimensioni della scheda sono proibitive in termine di spazi e libertà di posizionamento.
La connettività si è rivelata adeguata, sebbene il bus I2C non fosse completamente robusto (vedi I2C clock streching)
Si considera l'attuale soluzione "Funzionante" sebbene eccessiva per alcuni aspetti.
## IMU
Dopo svariati tentativi atti a sistemare i problemi causati dall'imu BNO08X, si è deciso di cambiare sensore in favore di un BNO0855.
Questo, sebbene leggermente più facile da usare, rimane di scarsa affidabilità. 
Uno dei due campioni, infatti, continua a dare problemi intermittenti e di difficile soluzione, probabilmente dovuti a problemi di contatto/collegamento interno alla pcb.
Inoltre, ad alte frequenze, il sensore riscontra un problema che lo manda in uno stato di errore, e che interrompe dunque il campionamento.
Questo problema si è dovuto risolvere con una logica che ad ogni evenienza, cattura l'errore e resetta il sensore. 
Questo causa un campionamento intermittente e a frequenza altalenante.
Si considera quindi l'attuale soluzione "inaffidabile", sebbene funzionante.
## GPS
E' stato utilizzato un NEO-6M, con antenna attiva di dimensioni ridotte.
Sebbene lento ad iniziare a fixare, si è dimostrato affidabile e non ha mai dato problemi.
Si nota che 1hz non è una frequenza di campionamento molto elevata.
Si pianificava anche l'utilizzo di un Adafruit Ultimate GPS, il quale però possiede solo interfaccia seriale via USB, e che quindi è stato soltanto testato.
Si considera quindi l'attuale soluzione "Affidabile" sebbene dalle basse prestazioni.
## RTC
Non configurato
Si osserva che il formato è scomodo e il tipo di batterie usato è poco comune e quindi difficile da reperire.
## ADC
E' stato utilizzato un ADC AS5111 per la lettura della tensione delle batterie.
L'implementazione è stata semplice e i risultati si sono dimostrati utili.
I dati registrati tuttavia potrebbero essere lievemente imprecisi (di 0.1v circa), la causa non è stata ancora confermata
Si nota che molti dei moduli comprati si sono rivelati non funzionanti o inaffidabili.
Il modulo, inoltre, è anche leggermente ingombrante.
Si considera l'attuale soluzione "Utilizzabile" sebbene non certamente affidabile, e dalle prestazioni migliorabili.
## Temperatura e umidità
Si è usato un DHT22
funzionava
occupa tanto spazio
## Alimentazione
Si è utilizzata una scheda di alimentazione "Torpedo2" per stabilizzare la tensione da 3.7v delle batterie a 5v, e per gestire la ricarica.
Funziona egregiamente, le uniche note di demerito sono:
- Il led di "ricarica completata" non si accende sempre, anche se le celle hanno raggiunto la tensione adeguata
- Il formato è ingombrante
Le celle LiPo da 3.7V hanno dimostrato di avere densità energetica sufficiente e un formato utilizzabile.
La durata si è rivelata sufficiente.
Si considera l'attuale soluzione "Affidabile", sebbene ingombrante.
## Collegamenti
I collegamenti sono stati effettuati tramite un mix di connettori crimpati JST, terminali di jumper, e saldature. 
Pur essendo i primi di scomoda manifattura, essi si sono rivelati estremamente utili nelle fasi di smontaggio e rimontaggio della scatola
I secondi si sono rivelati affidabili nel collegamento, ma eccessivamente ingombranti. La loro manifattura inoltre causava difetti nei connettori creati.
Delle saldature non ne parliamo
Tutti sensori sono stati alloggiati su schede preforate da prototipazione: fanculo, l'anno prossimo facciamo le PCB
# Enclosure
L'enclosure è stato il problema principale affrontato dal reparto di elettronica, sia durante l'anno che durante la SuMoth, e che ha causato poi il fallimento del progetto.
## Ubicazione
Il punto assegnato alla posizione della scatola ha un'ubicazione altamente vantaggiosa per la raccolta dati di sensori come IMU e GPS.
Tuttavia, alcuni problemi costruttivi hanno costretto il reparto a riprogettarla molteplici volte, e infine ad arrendere i tentativi di posizionamento sull'imbarcazione.
La scatola, sebbene ridisegnata per aggirare la scassa di deriva, si è rivelata delle dimensioni sbagliate, andando a collidere con la stessa, e con la wingbar delle terrazze.
Si è osservato che comunque, l'ubicazione della scatola in quel punto dell'imbarcazione la mette a rischio di calpestamenti frequenti.
## Geometria e dimensioni
La geometria complessa del piano di coperta ha fatto in modo che il volume a disposizione per l'alloggiamento dei sensori non fosse solo limitato, ma anche di scarsa utilizzabilità. 
La disposizione interna è stata modificata molteplici volte, per adeguarsi agli errori di misurazione e di manifattura della barca.
## Resistenza all'acqua
La problematica protagonista di quest'anno è stata l'impermeabilità dell'enclosure.
L'idea iniziale è stata quella di chiudere la scatola con un tappo avvitato, il quale comprimeva una guarnizione di tpu. Questa soluzione si è rivelata comicamente inadeguata, in quanto il tpu non è stato in grado di sigillare efficacemente la fessura, e la distanza tra le viti, dovuta alla disposizione delle componenti interne, si è rivelata troppo elevata, e quindi causa di ingressi di acqua.
Ulteriori test successivi al sigillo dell'interfaccia, suggeriscono la poca efficacia del coating Marlin come sigillante, in quanto si suppongono molti ingressi provenienti da imperfezioni nel coating e nei layer della scatola.
Il tentativo di sigillare la scatola con un coating di resina ha decretato il fallimento ultimo del progetto.

---
# Problemi da risolvere:
- Impermeabilità della scatola
- Collocazione della scatola
- Affidabilità di alcuni sensori