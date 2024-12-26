# Implementazione del Client

In questa sezione analizzeremo il funzionamento del client multithreaded che consente la comunicazione con un server in una semplice applicazione di chat. Il client utilizza socket per la connessione e thread per gestire contemporaneamente l'invio e la ricezione dei messaggi.

## Codice del Client con Spiegazioni

```python
import socket, threading

HOST = '127.0.0.1'  # Indirizzo IP del server
PORT = 12345        # Porta del server a cui connettersi

# Funzione per ricevere i messaggi dal server
def receive_messages(sock, server_name):
    while True:
        try:
            # Riceve i dati dal server
            data = sock.recv(1024)
            if not data:
                # Se il server chiude la connessione, termina il ciclo
                print("\nConnection closed by the server.")
                break

            # Stampa il messaggio ricevuto dal server
            print(f"\n\033[1m{server_name}\033[0m: {data.decode()}\n> ", end="")
        except ConnectionResetError:
            # Gestisce la chiusura improvvisa della connessione da parte del server
            print("\nConnection closed abruptly by the server.")
            break

# Creazione del socket del client
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect((HOST, PORT))  # Connessione al server
print(f"Connected to server at {HOST}:{PORT}")

# Scambio iniziale dei nomi tra client e server
client_name = input("Enter your name: ")  # Nome del client
client_socket.sendall(client_name.encode())  # Invia il nome al server
server_name = client_socket.recv(1024).decode()  # Riceve il nome del server
print(f"\nYou are chatting with \033[1m{server_name}\033[0m")

# Creazione di un thread per ricevere messaggi dal server
thread = threading.Thread(target=receive_messages, args=(client_socket, server_name), daemon=True)
thread.start()

try:
    # Ciclo per inviare messaggi al server
    while True:
        message = input("> ")  # Legge l'input dell'utente
        client_socket.sendall(message.encode())  # Invia il messaggio al server

        # Se il client invia il comando '/close', chiude la connessione
        if message.strip().lower() == "/close":
            print("Closing connection...")
            break
finally:
    # Chiude il socket e termina la connessione
    client_socket.close()
    print("Connection closed.")
```

## Funzionamento del Client

1. **Connessione al Server**:

   - Il client crea un socket e si connette al server specificando il suo indirizzo IP (`127.0.0.1`) e la porta (`12345`).
   - Una volta connesso, il client stampa un messaggio di conferma.

2. **Scambio dei Nomi**:

   - Il client invia il proprio nome al server.
   - Riceve e stampa il nome del server, indicando con chi sta chattando.

3. **Gestione della Comunicazione**:

   - Un thread separato è dedicato alla ricezione dei messaggi dal server. Questo garantisce che i messaggi possano essere ricevuti in tempo reale mentre l'utente digita o invia messaggi.
   - Nel thread principale, il client legge l'input dell'utente e invia i messaggi al server.

4. **Terminazione della Connessione**:
   - Se l'utente invia il comando `/close`, la connessione viene chiusa in modo ordinato.
   - Il client gestisce situazioni in cui il server chiude la connessione, sia in modo previsto che improvviso.
