# Metodi Principali

Il modulo `socket` di Python fornisce una serie di metodi che consentono di creare, configurare e gestire la comunicazione tra client e server. Di seguito esploreremo i metodi principali che puoi utilizzare per lavorare con i socket, illustrando le operazioni di base come la creazione di un socket, l'assegnazione di un indirizzo e porta, l'invio e la ricezione di dati.

## Creazione di un Socket

Per creare un socket, si utilizza il metodo `socket.socket()`. Questo metodo accetta due argomenti principali:
- **family**: la famiglia di indirizzi (IPv4 o IPv6).
- **type**: il tipo di socket (UDP o TCP).

Un esempio di creazione di un socket TCP (usato per una connessione affidabile) con IPv4 è:
```python
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```
- `AF_INET` indica l'uso di IPv4.
- `SOCK_STREAM` è il tipo di socket per TCP.

## Connessione a un Server

Il metodo `connect()` viene utilizzato dal lato client per stabilire una connessione con il server. Accetta come argomento un **tupla (indirizzo, porta)** che identifica il server a cui ci si vuole connettere.

Esempio:
```python
s.connect(('localhost', 8080))
```
Questo tentativo di connessione cerca di stabilire una connessione al server che ascolta sulla porta `8080` dell'indirizzo `localhost`.

## Binding di un Socket

Il metodo `bind()` è utilizzato dal lato server per associare un indirizzo e una porta al socket, permettendo al server di ascoltare le connessioni in arrivo.

Esempio:
```python
s.bind(('localhost', 8080))
```
Questo lega il socket all'indirizzo IP `localhost` e alla porta `8080`, preparando il server ad accettare le connessioni in entrata.

## Ascolto e Accettazione delle Connessioni

Il metodo `listen()` è utilizzato dal server per iniziare ad ascoltare le connessioni in arrivo. Si fornisce il numero massimo di connessioni in coda che il server è disposto a gestire.
```python
s.listen(5)
```
Il server ora è pronto a ricevere fino a 5 connessioni simultanee in coda.

Il metodo `accept()` permette di accettare una connessione in arrivo, restituendo un nuovo socket per la comunicazione con il client e l'indirizzo del client.
```python
client_socket, client_address = s.accept()
```

## Invio e Ricezione di Dati

Una volta che la connessione è stabilita, il client e il server possono scambiarsi dati. Per inviare dati si utilizza il metodo `send()`, mentre per ricevere dati si utilizza `recv()`.

Esempio di invio di dati:
```python
s.send(b'Hello, Server')
```
Esempio di ricezione di dati:
```python
data = s.recv(1024)
```
Il metodo `recv()` accetta come parametro la dimensione del buffer (in byte) per la lettura dei dati.

## Chiusura del Socket

Una volta che la comunicazione è terminata, è importante chiudere il socket per liberare le risorse. Il metodo `close()` viene utilizzato per chiudere il socket sia lato client che server.
```python
s.close()
```

## Riepilogo dei Metodi Principali

1. **socket()**: crea un nuovo socket.
2. **connect()**: stabilisce una connessione con un server.
3. **bind()**: associa un indirizzo e una porta a un socket.
4. **listen()**: avvia l'ascolto delle connessioni in arrivo (solo lato server).
5. **accept()**: accetta una connessione in entrata (solo lato server).
6. **send()**: invia dati attraverso il socket.
7. **recv()**: riceve dati dal socket.
8. **close()**: chiude il socket.

Questi sono i metodi fondamentali per la gestione dei socket in Python e ti permetteranno di implementare facilmente una comunicazione di rete tra client e server. Nella sezione successiva esploreremo in dettaglio come le famiglie di indirizzi influenzano la creazione e gestione dei socket, con particolare attenzione alle differenze tra IPv4 e IPv6.
