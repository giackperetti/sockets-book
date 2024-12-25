# Creare un Client TCP

In questa sezione, vedremo come creare un client TCP che si connette a un server, invia un messaggio e riceve una risposta. Utilizzeremo il modulo `socket` di Python, che offre le funzionalità necessarie per la comunicazione di rete tramite i protocolli TCP/IP. Esploreremo il codice di un client che si connette a un server, invia un messaggio e riceve la risposta.

### Codice del Client TCP

```python
import socket

HOST = '127.0.0.1'  # Indirizzo IP del server (localhost)
PORT = 12345  # Porta del server

# Creazione del socket
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    # Connessione al server
    client_socket.connect((HOST, PORT))
    print(f"Connected to server at ({HOST}:{PORT})")

    # Messaggio da inviare al server
    message = "Hello, server!"
    print(f"Sending: {message}")
    client_socket.sendall(message.encode())  # Invio del messaggio

    # Ricezione della risposta dal server
    response = client_socket.recv(1024)
    print(f"Received: {response.decode()}")
except ConnectionError as e:
    print(f"Connection error: {e}")
finally:
    # Chiusura della connessione
    client_socket.close()
    print("Connection closed.")
```

### Considerazioni sul Funzionamento del Client

- **Indirizzo e Porta**: In questo esempio, il client si connette a un server locale, usando l'indirizzo IP `127.0.0.1`, che rappresenta il proprio computer (localhost). La porta `12345` deve essere la stessa su cui il server sta ascoltando le connessioni in entrata.
- **Blocco durante la Connessione**: I metodi `connect()` e `recv()` sono bloccanti, il che significa che il programma aspetterà il completamento di queste operazioni prima di procedere. Ad esempio, il client aspetta di ricevere una risposta dal server prima di continuare.
- **Gestione degli Errori**: L'uso del blocco `try`...`except` garantisce che il client non termini inaspettatamente in caso di errore di connessione, e permette di gestire eventuali problemi con un messaggio di errore chiaro.

### Conclusione

In questo esempio, abbiamo creato un client TCP che si connette a un server, invia un messaggio e riceve una risposta. La gestione delle connessioni tramite socket è una delle basi della comunicazione di rete in Python, ed è particolarmente utile per costruire applicazioni distribuite, come chat, server web, o altre applicazioni che necessitano di scambi di dati tra client e server.
