# Altimeter

Progetto con lo scopo di realizzare un altimetro che potesse visualizzare i dati sia in presenza di uno smartphone al'interno di una pagina web che in ambienti outdoor utilizzando un display

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
- [Architecture](#architecture)
- [Design and Implementation](#design-and-implementation)
- [Tests](#tests)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)

## Overview

Il progetto riportato nella repository ha l'obiettivo di misurare l'altitudine a cui il dispositivo si trova dandone una misura approssimativa.
La misurazione avviene mediante un sensore di pressione atmosferica e con successivi aggiustamenti si calcola la misura approssimativa dell'elevazione rispetto al livello del mare.
Poiché la misurazione si basa sulla pressione atmosferica bisogna considerare che essa può variare considerevolmente nel tempo a causa di fenomeni atmosferici,
di conseguenza per avere una misura più accurata è necessario fare una media delle misurazioni del dispositivo nel arco di più giorni con condizioni atmosferiche stabili.
Fatta questa premessa il progetto ha come obbiettivo d'incorporare sfide dal punto di vista progettuale che vanno dalla programmazione del dispositivo fino alla sua realizzazione del circuito e della scocca.

## Features

Le funzionalità che il dispositivo realizzato supporta nella sua prima versione software:

- F 1: lettura dai sensori di pressione atmosferica e temperatura
- F 2: calcolo dell'elevazione partendo dalla pressione atmosferica
- F 3: lettura delle informazioni provenienti dai sensori mediante un display 128x32
- F 4: lettura delle informazioni provenienti dai sensori in una pagina web generata dal dispositivo
- F 5: batteria per l'utilizzo del dispositivo in outdoor

## Getting Started

Per iniziare è necessario avere disponibilità di alcuni materiali descritti nel sotto paragrafo di seguito riportato: 

### Prerequisites

- Arduino ide
- inserti da 3mm
- viti da 3mm per gli inserti
- porta batterie cr2032
- cavo usb-c
- D1 mini (esp8266mod)
- display 128x32
- fili per il circuito
- breadboard
- sensore della pressione BMP280
- (opsionale) stampante 3D 
- (opsionale) saldatore a penna per stagno
- (opsionale) protoboard

### Installation
 Come prima cosa si procede con la creazione del circuito su breadboard, poiché sia il display che il sensore comunicano con il protocollo I2C si procede a collegare i pin preposti per tale comunicazione tra il controllore e i dispositivi che comunicheranno con questo bus.
 Come mostrato di seguito: 
 
<img height="400" src="photo\breadboard_circuit.jpeg" title="breadboard circuit" width="400"/>

|         | SCL | SDA | Vin  |
|---------|-----|-----|------|
| D1 mini | D1  | D2  | #    | 
| Display | SCL | SDA | 3.3v |
| BMP280  | SCL | SDA | 3.3v |
 
 Una volta completato il circuito si può proseguire all'istallazione dei componenti necessari alla creazione del codice:

- Adafruit_BMP280
- Adafruit_GFX
- Adafruit_SSD1306
- ESP8266WiFi
- https://github.com/esp8266/Arduino (è da installare nella sezione board del ide arduino)

Installati i componenti si può procedere a connettere il D1 mini al pc e proseguire con l'upload del codice nel controllore.
Il codice lo si può trovare nella cartella `altimeter_code\ `
  
Si possono stampare in 3D le parti necessarie per la scocca (sono disponibili i file stl) stampando e inserendo negli alloggiamenti predisposti gli inserti si completa la scocca.
é stato predisposto un alloggiamento per la protoboard del circuito (in verde nella foto) per essere inserita sopra l'alloggiamento delle batterie (scatola nera collegata alla protoboard):

<img height="auto" src="photo\final_circuit_and_3d.jpeg" width="400"/>
<img height="auto" src="photo\circuit.jpeg" width="400"/>

## Usage

Per utilizzare il dispositivo si può collegare a una fonte esterna di alimentazione: da spento collegandolo a uno powerbank oppure utilizzare le batterie del dispositivo due cr2032.

Una volta acceso comparirà da prima la schermata di avvio con il logo "125ade lab" successivamente l'indirizzo ip della pagina per visualizzare da smartphone i dati dei sensori ed in seguito comparirà nel display i dati provenienti dai sensori.

Per spegnere il dispositivo è necessario staccare il cavo di alimentazione o spegnere l'interruttore del pacco batteria.

## Architecture

Concettualmente il dispositivo è formato da una batteria collegata a un D1 mini dal quale parte un bus I2C che collega il display 128x32 e il sensore di pressione.
Per quanto riguarda il software è diviso in due file uno con il codice del progetto e un altro il file secret.h che contiene l' ssid e la password della wifi generata dal dispositivo.
Il codice principale è suddiviso in 3 parti una dichiarativa della maggior parte delle variabili una seconda parte di setup dei vari sistemi che si utilizzeranno per poi concludere con la terza ed ultima parte e cioè il loop dentro il quale risiede tutta la logica che gestisce le funzionalità del dispositivo.

## Design and Implementation

In questo progetto ci sono due filoni ben distinti di sviluppo e prototipazione che lo rendono interessante: e si tratta dello sviluppo che concerne la protoboard e il software nonche lo sviluppo del hardware della scocca del dispositivo. 
Per entrambi i filoni di sviluppo sono stati raggiunti dei compromessi per mantenere le aspettative di prestazionali che questo progetto richiede.  
C'è da sottolineare che l'imperativo è stato la velocità di realizzazione nel suo complesso.

### Protoboard & software

Le scelte che hanno caratterizzato lo sviluppo sono state la reperibilità dei materiali utilizzati poiche a differenza di altri progetti questo è stato realizzato con l'uso esclusivo di rimanenze da precedenti progetti dai sensori fino ai fili utilizzati per la protoboard.
Si è utilizzata una protoboard con degli alloggi che permetto la rimozione dei piedini del controller e del display nonche del sensore poiche questa è la prima versione del circuito e non è stata sviluppata ancora un PCB specifico per questo progetto di qui la necessità di rimuovere i dispositivi per inserirli nella loro posizione definitiva una volta progettata e realizzata la versione definitiva.
La batteria nonostante non mantenga la carica per periodi prolungati nel tempo è stata selezionata poiche da al dispositivo un autonomia di alcuni giorni abbastanza per permettere di testare le componenti in diversi contesti.

Il software sviluppato è stato strutturato nella prima versione per offrire tutte le funzionalità che questo progetto deve fornire senza particolari accorgimenti lasciando ad aggiornamenti futuri la possibilità di migliorare la sensibilità complessiva dei sensori e dell'esperienza utente.

Il tempo impiegato dalla progettazione del circuito, alla realizzazione della protoboard, nonche alla primissima versione del software (con solamente le funzionalità 1-2-3) sono state impiegate meno di tre ore. 

### Hardware

Lo sviluppo di quest'ultimo ha seguito un approccio molto pragmatico rispetto al ergomicità del dispositivo poiche, la necessità categorica era di proteggere il sensore e il display da accidentali cadute in ambiente autdoor e mantenere all'interno del dispositivo la batteria e la protoboard. 
Per la realizzazione si è optato per la stampa 3D e per ridurre i tempi di utilizzo della stampante stessa si è optato per lo sviluppo di una scocca molto massiccia nel aspetto e con linee facilmente implementate dal software della stampante garantendo una rapida esecuzione.
Poiche oggettivamente il risultato finale è da aggiornare e migliorare è stato deciso che la prima versione della scocca sarà chiamata V1 la quale si compone di tre pezzi realizzati in stampa 3D, due inserti da 3mm e due viti da 3mm che garantiscono la corretta chiusura della scocca del dispositivo.

## Tests

Per testare il dispositivo ci sono due modi utilizzare una capsula pressurizzata e leggere la pressione che varia e rapportarla alla pressione imposta alla casula o portare il dispositivi in punti che hanno una diversa altitudine e vedere la differenza tra l'altezza calcolata e l'altezza effettiva del punto in cui ci si trova per i test svolti il dispositivo creato ha un approssimazione di [ +- 50 m ]

## Contributing

Chiunque è libero di aiutare a migliorare il codice o il design del dispositivo e non esiti a contattarmi per qualsiasi dubbio inerente al progetto

## [MIT License](LICENSE)

## Acknowledgements

Per questo progetto sono partito utilizzando gli esempi forniti dalle librerie di riferimento citate in precedenza e cercando sul web i vari approcci seguiti da altri prima di me.