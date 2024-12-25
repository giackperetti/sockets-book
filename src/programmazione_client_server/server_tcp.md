# Creare un Server TCP

In questo capitolo vedremo come creare un server TCP utilizzando i socket in Python, prendendo come esempio un semplice "echo server". Un echo server è un programma che riceve dei messaggi dai client, li rimanda indietro senza modificarli, ed è utile per testare la connessione di rete e il funzionamento dei socket.

### Cos'è un Server TCP?

Un server TCP (Transmission Control Protocol) è un'applicazione che accetta connessioni da client attraverso una rete, comunemente utilizzato per comunicare in modo affidabile tra dispositivi. Un server TCP ascolta una porta su un determinato indirizzo IP per le richieste in arrivo e stabilisce una connessione quando un client tenta di connettersi.

### Creare un Server TCP in Python

Di seguito viene presentato il codice di un semplice server TCP, che ascolta su una porta specifica e restituisce indietro tutto ciò che riceve dai client:

```python
import socket

HOST = ''  # Accetta connessioni su tutti gli indirizzi IP locali
PORT = 12345  # Numero di porta su cui il server ascolterà

# Crea un socket per la comunicazione tramite TCP (stream di dati)
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Associa il socket a un indirizzo IP e una porta
s.bind((HOST, PORT))

# Inizia ad ascoltare le connessioni, il parametro 1 significa che può esserci solo una connessione in coda
s.listen(1)

# Accetta una connessione da un client
conn, addr = s.accept()
print(f"Connected by {addr}")

# Ciclo infinito per ricevere e inviare i dati
while True:
  # Riceve i dati inviati dal client (fino a 1024 byte)
  data = conn.recv(1024)
  print(f"Received: {data.decode()}")  # Decodifica e stampa i dati ricevuti
  if not data:  # Se il client ha chiuso la connessione, esce dal ciclo
    break
  # Invia i dati ricevuti indietro al client
  conn.sendall(data)
  print(f"Sending back: {data}")

# Chiude la connessione
conn.close()
```

### Quando Usare un Server TCP?

Un server TCP come quello descritto è utile per applicazioni dove è necessaria una comunicazione affidabile tra client e server, come:

- Sistemi di messaggistica istantanea
- Servizi di database
- Applicazioni di rete che richiedono un flusso continuo di dati (streaming)

Il server TCP è ideale quando si vuole garantire che tutti i dati inviati arrivino correttamente al destinatario e in ordine, come avviene nel caso di un "echo server" che rimanda indietro i dati ricevuti.

### Conclusione

Abbiamo appena visto come creare un semplice server TCP utilizzando i socket in Python. Questo server può ricevere richieste da un client, processarle e restituirle indietro, il che è la base per costruire applicazioni di rete più complesse. Sfruttando la modularità e la potenza di Python, è possibile creare facilmente server robusti per una vasta gamma di applicazioni di rete.
