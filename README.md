# Progetto di Sistemi Embedded A.A 2024/2025

**Università:** Università degli Studi di Salerno

**Corso di Laurea:** Magistrale in Ingegneria Informatica

**Gruppo:** n. 28

**Relazione:** [Relazione Project Work SISTEMI EMBEDDED - Gruppo n.28 - 2024/2025](https://docs.google.com/document/d/1zWR1HbPM4Rj4tx8CWz6kViwjbDuj_L3I6IeZw9DyaNM/edit?usp=sharing)

**Presentazione:** [Presentazione Project Work SISTEMI EMBEDDED - Gruppo n.28 - 2024/2025](https://docs.google.com/presentation/d/18FIxoS25EJeHLxz5_yFJY-8rVTJehiribgtOWQzXMGY/edit?usp=sharing)

## Utilizzo

### aprire il progetto dal file: 
#### **`index_simulink.slx`** per provare il progetto in ambiente matlab simulink (Model in the Loop) 
#### **`index_stm32g4.slx`** per provare il progetto sulla scheda stm32g4 (Hardware in loop)

## Descrizione

Realizzare un sistema il firmware di un sistema integrato di domotica che consente la gestione delle seguenti funzionalità:

1. Regolazione apertura delle veneziane  
2. Accensione luci  
3. Riscaldamento

Il sistema è dotato dei seguenti componenti hardware:

* Servo motore per regolare l’apertura delle veneziane  
* Relè per l’accensione e lo spegnimento delle luci  
* Led RGB per indicare la temperatura impostata sul termostato  
* Led utente per indicare lo stato di attivazione del dispositivo.   
* Pulsanti:  
  * CONFIG: il pulsante consente di entrare o uscire dalla modalità di configurazione  
  * ATTIVA: il pulsante consente di attivare o disattivare il dispositivo  
  * LUCI: pulsante per accendere e spegnere le luci  
  * LUCI TMP: pulsante per l’avvio dell’accensione temporizzata

## Configurazione del dispositivo

Quando è attiva la modalità di configurazione, il dispositivo trasmette tramite interfaccia UART un menù per selezionare la funzionalità da configurare ed impostare il valore desiderato.

La configurazione può avvenire esclusivamente quando il dispositivo è attivo.

Quando avviato la prima volta il dispositivo non dispone di alcuna configurazione, pertanto deve trovarsi in stato di configurazione. Se non è presente alcuna configurazione valida, il dispositivo non può uscire dallo stato di configurazione.

Le configurazioni applicabili per le tre funzionalità gestita dal sistema sono le seguenti

* Veneziane:  
  * 0° completamente chiuse  
  * 180° completamente aperte  
  * Valori da 0° a 180° selezionabili con la risoluzione di 20°  
* Luci:  
  * Accese  
  * Spente  
  * Durata dell’accensione temporizzata nel range \[5 \- 30\] secondi, quando questa è attiva.  
    * Una durata pari a 0 indica che la temporizzazione è inattiva.  
* Temperatura:  
  * Range 16° \- 30° configurabile con risoluzione di 1°

L’inserimento di valori errati comporta il rigetto della configurazione, pertanto il sistema continua ad utilizzare l’ultima configurazione considerata valida.

## Notifica dello stato

Quando il dispositivo è attivo e non è in modalità configurazione il dispositivo trasmette sull’interfaccia UART, ogni 10 secondi, il proprio stato indicato per ogni funzionalità il valore attualmente impostato.

## Attivazione del dispositivo

All’avvio il dispositivo non è attivo.

Quando il dispositivo è attivo il led utente lampeggia ad un frequenza di 2Hz e per tutti i dispositivi inerenti alle funzioni presenti sono applicati i valori di configurazione.

Quando il dispositivo è inattivo il led utente è spento e le uscite devono avere i seguenti valori:

- Veneziane chiuse  
- Luce spenta / Temporizzazione inattiva  
- Temperatura al minimo

## Indicazione della temperatura

Il dispositivo fornisce un feedback circa la temperatura selezionata tramite il led RGB in accordo alla seguente semantica:

- Da 16 a 18: colore blu  
- Da 19 a 21: colore viola  
- Da 22 a 24: colore giallo  
- Da 25 a 27: colore arancione  
- Da 28 a 30: colore rosso

## Accensione della luci

L’accensione delle luci avviene tramite l’attivazione del relè. 

Quando le luci sono impostate come “accese” lo stato permane fino a quando non sono nuovamente impostate come “spente”.

Le luci possono essere accese o spente tramite la configurazione su UART oppure tramite il pulsante LUCI.

La pressione del pulsante LUCI ha il seguente effetto:

- Se le luci sono spente, vengono accese  
- Se le luci sono accese, vengono spente  
- Se le luci sono accese con l’accensione temporizzata, vengono spente in anticipo

La pressione del pulsante LUCI TMP avvia l’accensione temporizzata, pertanto:

- Se le luci sono spente, vengono accese per il periodo impostato e poi spente  
- Se le luci sono accese in modalità non temporizzata, rimangono accese per il periodo imposto e poi spente  
- Se le luci sono già state avviate in accensione temporizzata, la pressione non ha alcun effetto.

## PIN setup

In questa sezione sono riportati i pin da utilizzare per la connessione dei dispositivi di input ed output del sistema.

| Dispositivo | PIN | Nota |
| :---- | :---- | :---- |
| LDG RGB colore rosso | PA6 | TIM 3 PWM CH1 |
| LDG RGB colore verde | PC7 | TIM 3 PWM CH2 |
| LDG RGB colore blu | PC9 | TIM 3 PWM CH4 |
| LED utente | PA5 |  |
| Controllo servomotore | PB6 | TIM4 PWM CH1 |
| Controllo relè | PC2 |  |
| Pulsante CONFIG | PB0 |  |
| Pulsante ATTIVA | PC13 | User button |
| Pulsante LUCI | PA14 |  |
| Pulsante LUCI TMP | PA15 |  |
| RX | PA3 | LPUART RX |
| TX | PA2  | LPUART TX |

