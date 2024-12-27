# Gestione di Client Multipli con i Thread

Un server TCP che accetta connessioni da un singolo client, come nel caso dell'echo server visto precedentemente, può essere utile per applicazioni semplici. Tuttavia, in scenari reali, è molto più comune avere un server che gestisce più client contemporaneamente. Per fare ciò, possiamo utilizzare i thread. I thread permettono di eseguire più operazioni in parallelo, quindi un server multithreaded può accettare e gestire più connessioni simultaneamente senza bloccare il programma principale.

In questa sezione, vedremo come modificare il nostro echo server per accettare fino a 5 connessioni simultanee utilizzando i thread. Ogni connessione sarà gestita da un thread separato, il che permetterà al server di rispondere rapidamente a ciascun client senza dover attendere che uno finisca prima di iniziare con l'altro.

### Modificare l'Echo Server per Gestire Più Client

La versione modificata dell'echo server si basa sull'utilizzo del modulo `threading` di Python. Ogni volta che il server accetta una connessione, creerà un nuovo thread che gestirà quella connessione. Questo permetterà al server di essere molto più reattivo e di gestire fino a 5 client contemporaneamente.

### Codice del Server Modificato

```python
import socket, threading

HOST = ''  # Accetta connessioni su tutti gli indirizzi IP della rete locale
PORT = 12345  # Numero di porta su cui il server ascolterà

# Funzione per gestire la comunicazione con un client
def handle_client(conn, addr):
    print(f"Connected by {addr}")
    while True:
        data = conn.recv(1024)
        if not data:
            break
        print(f"Received from {addr}: {data.decode()}")
        conn.sendall(data)
        print(f"Sending back to {addr}: {data}")
    conn.close()
    print(f"Connection with {addr} closed")

# Crea un socket TCP
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Associa il socket a un indirizzo IP e una porta
s.bind((HOST, PORT))

# Inizia ad ascoltare le connessioni
s.listen(5)  # Il server può accettare fino a 5 connessioni in coda

# Gestione delle connessioni
connections = 0
while connections < 5:
    conn, addr = s.accept()  # Accetta una connessione in ingresso
    connections += 1
    # Crea un thread per gestire la connessione
    thread = threading.Thread(target=handle_client, args=(conn, addr))
    thread.start()

s.close()
```

### Conclusione

Abbiamo visto come modificare l'echo server per gestire più client utilizzando i thread. Ogni client viene gestito da un thread separato, il che consente al server di rimanere reattivo e di servire più client simultaneamente. Questa tecnica è molto utile in scenari di networking ad alta intensità dove più client potrebbero tentare di connettersi al server nello stesso momento.
