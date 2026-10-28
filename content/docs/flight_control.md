---
title: 'Sistema di volo'
date: 2024-11-28
weight: 3
---

Il **sistema di volo (SV)** è l'insieme di componenti meccaniche ed
elettroniche che gestisce la deflessione della superficie di controllo
del foil e regola l'assetto della barca. 

<!--more-->

## 1. Obiettivo: perché il foiling è una sfida unica

Far volare un'imbarcazione foiling (come un Moth) sull'acqua è una sfida radicalmente diversa rispetto al pilotaggio di un aereo o di un drone:
- **Margine di errore millimetrico:** un aereo in quota ha decine o centinaia di metri di tolleranza; una barca foiling naviga a pochi centimetri dal pelo dell'acqua. Ha a disposizione una "finestra utile di volo" (*ride height*) di appena **50–80 cm**.
- **I due pericoli opposti:** se la barca scende troppo, lo scafo sbatte violentemente sull'acqua rallentando di colpo (*hull slam*); se sale troppo, l'ala esce dall'acqua o aspira aria dalla superficie (*ventilazione*), perdendo istantaneamente portanza e facendola precipitare.
- **La bacchetta magica tradizionale (Wand):** nelle barche tradizionali questo controllo è affidato a un tastatore meccanico (la classica "bacchetta" a prua collegata via cavi al flap del foil). Il nostro obiettivo è passare a un **controllo di volo elettronico e automatico**, più preciso, regolabile e in grado di adattarsi alle condizioni reali di navigazione.

---

## 2. Il riferimento scientifico: la tecnologia di Coppa America

Per progettare l'architettura di controllo siamo partiti dallo stato dell'arte della ricerca nautica internazionale, prendendo come riferimento il lavoro condotto per il team **Luna Rossa** e l'Università del Michigan:

> **Riferimento:**  
> R. Bencatel, S. Keerthivarman, I. Kolmanovsky, A. Girard,  
> *"Full State Feedback Foiling Control for America's Cup Catamarans"*,  
> **IEEE Transactions on Control Systems Technology**, 2019.  
> 📄 [Scarica il Paper Completo (PDF)](https://drive.google.com/file/d/1jjL-uqZtew2z2A8iXd_quuBKD1H9pEMK/view?usp=drive_link)  
> 📑 [Scarica il Riassunto Tecnico (PDF)](https://drive.google.com/file/d/1fMuixhwfQFIloDEjHiANxf9sAKXqWZrX/view?usp=drive_link)

Questo studio, sviluppato per i catamarani volanti AC45, analizza i problemi fondamentali del volo sull'acqua e dimostra scientificamente perché i controllori classici ed elementari (come un semplice PID che guarda solo l'altezza) falliscono in mare aperto:
1. **Le onde ingannano i sensori:** un sensore di distanza (es. laser o ultrasuoni) a prua non misura l'altezza media del volo, ma la distanza dalla singola onda che passa sotto. Se il controllore cercasse di inseguire ogni cresta e cavo d'onda, la barca oscillerebbe impazzita.
2. **Ritardo dei servomotori:** i componenti meccatronici hanno un piccolo ritardo intrinseco; se il sistema corregge in ritardo, rischia di innescare oscillazioni divergenti.
3. **Accoppiamento tra quota e sbandamento:** in barca a vela la spinta laterale del vento fa inclinare lo scafo; sbandamento e altezza di volo si influenzano a vicenda e non possono essere gestiti come problemi separati.

Prendendo spunto da questa ricerca, nella cartella `src/flight_system` abbiamo sviluppato e testato due modelli di simulazione via software.

---

## 3. Fase 1: Il Modello Longitudinale 3-DOF (`moth_sim.py`)

La prima fase del lavoro si è concentrata sul movimento verticale e di beccheggio della barca (in gergo, 3 Gradi di Libertà o 3-DOF: avanzamento, quota e beccheggio).

### Quale ala usare per regolare la quota?
Una barca foiling ha due ali orizzontali: il foil principale (sotto il centro della barca) e il timone orizzontale a T (a poppa).  
Il paper di Luna Rossa spiega chiaramente perché **per salire o scendere bisogna agire sul flap del foil principale e non sul timone**:
- Se usassimo il timone posteriore per alzare la prua (come l'equilibratore di un aereo), per farlo dovremmo spingere la poppa verso il basso.
- Risultato: la barca prima perde quota e solo dopo comincia a salire (comportamento a *fase non minima*). 
- Muovere il flap del foil centrale, invece, produce portanza diretta verso l'alto senza far abbassare la barca.

### Come abbiamo risolto il problema delle onde: il Filtro di Kalman
Per evitare che la barca balli sulle onde, abbiamo implementato un **Filtro di Kalman**:
- Una piattaforma inerziale (IMU) a bordo misura le accelerazioni verticali reali della barca a frequenza elevatissima.
- Il sensore di distanza misura l'altezza rispetto alla superficie.
- Il filtro fonde queste due informazioni in tempo reale: distingue i movimenti veri dello scafo dai disturbi passeggeri delle onde, fornendo una stima pulita dell'altezza e della velocità verticale.

### L'algoritmo di controllo (LQRi)
I dati ripuliti vengono passati a un algoritmo a retroazione completa di stato (**LQR con azione integrale**). In parole semplici: il computer non aspetta che la barca sia già troppo alta o troppo bassa per intervenire; monitora contemporaneamente altezza, inclinazione e velocità angolari, calcolando l'angolo perfetto del flap per mantenere la barca perfettamente livellata alla quota desiderata.

---

## 4. Fase 2: Il Modello Completo a 6 Gradi di Libertà (`moth_6dof.py`)

Anche se il modello longitudinale funziona ottimamente sulla carta, il paper e la realtà fisica pongono un avvertimento categorico: **in acqua, ignorare lo sbandamento (rollio) e lo scarroccio porta alla scuffia.**

Un Moth non vola dritto come una freccia in aria calma:
1. La vela genera centinaia di Newton di forza laterale a due metri d'altezza.
2. Per contrastarla e far lavorare la deriva verticale, la barca naviga costantemente sbandata sopravento di circa 8–12°.
3. Quando il vento aumenta o si regola la randa (*sail pumping*), la barca si inclina; inclinando i foil, l'ala sopravento sale verso la superficie e rischia di uscire dall'acqua.

Per catturare questa complessità, abbiamo sviluppato il simulatore **`moth_6dof.py`**:
- **Simulazione 3D completa:** riproduce tutti i 6 movimenti possibili del corpo rigido (avanzamento, scarroccio laterale, discesa, rollio, beccheggio e imbardata).
- **Semiali indipendenti:** i foil sono modellati come ali divise in semiala destra e sinistra. Quando la barca sbanda, il simulatore calcola quanta portanza perde l'ala più vicina alla superficie e controlla se si avvicina alla zona critica di ventilazione.
- **Vento e vele dinamiche:** la vela risponde punto per punto alle variazioni di rotta e velocità angolare.

### I risultati delle simulazioni:
Sottoponendo il modello al test del disturbo della vela (oscillazioni realistiche indotte da raffiche o regolazioni):
- **Senza controllo o con controllore solo longitudinale (3-DOF):** la barca non riesce a gestire l'accoppiamento tra rollio e portanza e scuffia in pochi secondi.
- **Con Controllo Coordinato (Flap + Spostamento del Peso):** stabilizzando contemporaneamente l'altezza (con il flap) e il momento sbandante (simulando il movimento del peso del timoniere o di una zavorra mobile), la barca naviga stabilissima, con un errore di quota medio inferiore a **0.5 cm** in acqua piatta e contenuto entro pochi centimetri in mezzo a onde formate.

---

## 5. Cosa c'è nella cartella e prossimi passi

All'interno di `src/flight_system` sono presenti tutti gli strumenti software realizzati:
- `moth_sim.py`: simulatore 3-DOF (longitudinale) con filtro di Kalman, disturbo delle onde e controllore LQRi.
- `moth_6dof.py`: simulatore non lineare completo 6-DOF con accoppiamento idro-aerodinamico e verifica di stabilità multi-asse.
- `presentazione_volo_automatico.md` e `presentazione_6dof_volo_automatico.md`: slide di presentazione e note tecniche per il team.
- `ppr/`: materiali bibliografici, contenenti il [paper completo](https://drive.google.com/file/d/1jjL-uqZtew2z2A8iXd_quuBKD1H9pEMK/view?usp=drive_link) (Bencatel et al., 2019) e il [riassunto sintetico](https://drive.google.com/file/d/1fMuixhwfQFIloDEjHiANxf9sAKXqWZrX/view?usp=drive_link).

### Ricadute pratiche per il progetto:
I modelli sviluppati definiscono le specifiche per l'hardware che andrà a bordo:
1. **Sensori:** necessità di un'unità IMU affidabile abbinata a un sensore di altezza (ultrasuoni/LiDAR) filtrato digitalmente.
2. **Attuazione:** specifica di velocità e coppia per il servomotore che muoverà il flap sull'asse del main foil.
3. **Architettura:** scelta se operare in *simbiosi con il timoniere* (l'elettronica pensa alla quota col flap, il timoniere pensa al rollio con la cinghia) oppure integrare una zavorra mobile per un volo 100% autonomo.