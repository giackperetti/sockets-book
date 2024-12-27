# Implementazione del Server

In questa sezione esamineremo il funzionamento del server multithreaded progettato per consentire la comunicazione in una semplice applicazione di chat. Il server è implementato utilizzando socket e thread per gestire simultaneamente l'invio e la ricezione dei messaggi.

## Codice del Server con Spiegazioni

```python
import socket, threading

HOST = ''  # Il server ascolta su tutti gli indirizzi IP della rete locale
PORT = 12345  # La porta su cui il server accetterà le connessioni

# Funzione per gestire l'invio dei messaggi dal server al client
def send_messages(conn, server_name, client_name):
    while True:
        try:
            # Legge l'input dell'utente (server) per inviarlo al client
            message = input("> ")
            conn.sendall(message.encode())  # Invia il messaggio codificato al client

            # Se il server invia il comando '/close', chiude la connessione
            if message.strip().lower() == "/close":
                print("Closing connection...")
                break
        except ConnectionResetError:
            # Gestisce il caso in cui il client chiuda la connessione inaspettatamente
            print("\nConnection closed abruptly by the client.")
            break

# Funzione per ricevere i messaggi dal client
def receive_messages(conn, client_name):
    while True:
        try:
            # Riceve i dati dal client (massimo 1024 byte per volta)
            data = conn.recv(1024)
            if not data:
                # Se il client chiude la connessione, termina il ciclo
                print("\nConnection closed by the client.")
                break

            # Stampa il messaggio ricevuto dal client
            print(f"\n\033[1m{client_name}\033[0m: {data.decode()}\n> ", end="")
        except ConnectionResetError:
            # Gestisce la chiusura improvvisa della connessione da parte del client
            print("\nConnection closed abruptly by the client.")
            break

# Creazione del socket del server
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind((HOST, PORT))  # Associa il socket all'indirizzo e alla porta specificati
server_socket.listen(1)  # Ascolta una connessione alla volta

print("Server started. Waiting for connection...")

# Accetta una connessione da parte di un client
conn, addr = server_socket.accept()
print(f"Connected by {addr}")

# Inizializza lo scambio di nomi tra server e client
server_name = input("Enter your name: ")  # Nome del server
conn.sendall(server_name.encode())  # Invia il nome del server al client
client_name = conn.recv(1024).decode()  # Riceve il nome del client
print(f"\nYou are chatting with \033[1m{client_name}\033[0m")

# Creazione dei thread per gestire invio e ricezione dei messaggi
send_thread = threading.Thread(target=send_messages, args=(conn, server_name, client_name), daemon=True)
receive_thread = threading.Thread(target=receive_messages, args=(conn, client_name), daemon=True)

# Avvio dei thread
send_thread.start()
receive_thread.start()

# Il thread principale aspetta la fine del thread di invio prima di chiudere il server
send_thread.join()

# Chiusura della connessione e del socket del server
conn.close()
server_socket.close()
print("Connection closed.")
```

## Funzionamento del Server

1. **Ascolto e Accettazione della Connessione**:

   - Il server ascolta sulla porta specificata e attende che un client si connetta.
   - Quando un client si connette, il server accetta la connessione e registra l'indirizzo del client.

2. **Inizializzazione della Chat**:

   - Viene richiesto al server di inserire il proprio nome, che viene inviato al client.
   - Il client risponde inviando il proprio nome, visualizzato sul server.

3. **Gestione dei Messaggi**:

   - Due thread gestiscono la comunicazione:
     - Il primo thread invia messaggi dal server al client.
     - Il secondo thread riceve e stampa i messaggi dal client.

4. **Terminazione della Connessione**:
   - Se il server invia il comando `/close`, la connessione viene terminata.
   - In caso di chiusura da parte del client, entrambi i thread gestiscono l'evento senza blocchi o crash.
