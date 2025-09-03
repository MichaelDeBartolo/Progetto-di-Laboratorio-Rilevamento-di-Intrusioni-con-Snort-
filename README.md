# Obiettivi del Laboratorio

1. Configurazione dell'ambiente: _Creare una rete virtuale isolata per un'esecuzione sicura del laboratorio._

2. Implementazione di un IDS: Installare e configurare Snort per monitorare il traffico di rete.

3. Simulazione di attacco: Lanciare una scansione di rete aggressiva utilizzando Nmap.

4. Analisi dei log: Verificare il corretto funzionamento di Snort analizzando gli alert generati.

## Architettura del Laboratorio
La struttura del laboratorio è composta da tre macchine virtuali collegate a una rete isolata (Host-only) per garantire che le attività di test non interferiscano con la rete esterna.

## Macchina Virtuale - Sistema Operativo - Ruolo - Indirizzo IP 
_Attaccante ->	Kali Linux ->	Usata per lanciare la scansione di rete -> 172.16.250.128_

_Vittima ->	Metasploitable2 ->	Macchina volutamente vulnerabile da scansionare ->	172.16.250.132_

_IDS ->	Ubuntu Desktop -> Esegue Snort per monitorare e rilevare l'attacco ->	172.16.250.130_

# 1. Passaggi Eseguiti e Codice
Preparazione dell'Ambiente:

È stata creata una rete virtuale VMnet1 in modalità Host-only.

Le tre macchine virtuali sono state configurate per utilizzare questa rete.

# 2. Installazione e Configurazione di Snort

Bash

Aggiornamento del sistema

**sudo apt update && sudo apt upgrade -y**

Installa Snort

**sudo apt install snort -y**

Configurazione:

## Creazione della cartella delle regole e copia delle regole della community.

Bash

**sudo mkdir /etc/snort/rules**

## Copiare le regole scaricate dal browser in questa cartella

**sudo cp ~/Downloads/snort-community-rules.tar.gz /tmp/**

**sudo tar -xvf /tmp/snort-community-rules.tar.gz -C /etc/snort/rules/**

**sudo cp /etc/snort/rules/snort-community-rules/*.rules /etc/snort/rules/**

## Modifica del file snort.conf per definire la rete locale (HOME_NET).

Bash

**sudo nano /etc/snort/snort.conf**

## Modifica la riga per il tuo network ID

_ipvar HOME_NET 172.16.250.0/24_

## Assicurati che le regole della community siano incluse

_include $RULE_PATH/community.rules_

# 3. Rilevamento dell'Attacco
Lancio della Scansione da Kali:
Il seguente comando è stato eseguito dalla macchina Kali per avviare una scansione Nmap aggressiva.**

# Sostituisci l'IP con quello di Metasploitable2

bash

**nmap -A -sV 172.16.250.132**

Avvio di Snort e Analisi dei Log:

Dalla macchina Ubuntu, Snort è stato avviato in modalità IDS per monitorare il traffico.

## Avvia Snort in modalità IDS e mostra gli alert in tempo reale

bash

**sudo snort -A full -i ens33 -c /etc/snort/snort.conf**

# 4. L'allarme seguente è apparso nel terminale di Snort, confermando il rilevamento dell'attacco:

[1:1228:7] SCAN nmap XMAS

[Classification: Attempted Information Leak] [Priority: 2]

09/03-00:37:45.247789 172.16.250.128:44097 -> 172.16.250.130:1

# Conclusioni

Questo laboratorio ha dimostrato con successo che Snort, una volta correttamente configurato, è uno strumento efficace per rilevare attività ostili in una rete. 

L'allarme SCAN nmap XMAS ha fornito una prova inconfutabile che il sistema è operativo e in grado di identificare le tecniche di scansione avanzate. 
