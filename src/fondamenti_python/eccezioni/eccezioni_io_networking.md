# Gestione di Eccezioni I/O e di Networking

Le eccezioni di I/O (Input/Output) e di rete sono tra le più comuni in Python, soprattutto quando si interagisce con file o si effettuano comunicazioni attraverso socket. Questi tipi di eccezioni si verificano quando operazioni di input o output non possono essere completate a causa di errori come file inesistenti, permessi mancanti, connessioni interrotte o timeout.

## Eccezioni di I/O

Le operazioni di I/O, come leggere o scrivere file, possono fallire per diversi motivi:

- **FileNotFoundError**: Quando si tenta di accedere a un file che non esiste.
- **PermissionError**: Quando non si dispone dei permessi necessari per accedere o modificare un file.
- **IsADirectoryError/NotADirectoryError**: Quando si tenta di accedere a un percorso come se fosse un file o viceversa.
- **IOError**: Un'eccezione generica per problemi di input/output.

Esempio:

```python
try:
    with open("dati.txt", "r") as file:
        contenuto = file.read()
except FileNotFoundError:
    print("Errore: il file non esiste.")
except PermissionError:
    print("Errore: permesso negato.")
```

In scenari critici, è buona pratica usare il blocco `finally` per chiudere il file in caso di apertura manuale, evitando risorse inutilizzate.

## Eccezioni di Networking

Le comunicazioni di rete, in particolare quelle basate sui socket, sono suscettibili a errori dovuti a fattori esterni come connettività, server inaccessibili o configurazioni errate. Python offre una serie di eccezioni specifiche per queste situazioni, tra cui:

- **ConnectionRefusedError**: Quando un server rifiuta una connessione.
- **TimeoutError**: Quando un'operazione di rete supera il tempo massimo di attesa.
- **BrokenPipeError**: Quando il lato remoto della connessione chiude il canale inaspettatamente.
- **socket.error**: Un'eccezione generica per problemi di rete, che può includere errori come indirizzi non validi o connessioni interrotte.

Esempio:

```python
import socket

try:
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect(("www.esempio.com", 80))
    s.sendall(b"GET / HTTP/1.1\r\nHost: www.esempio.com\r\n\r\n")
    risposta = s.recv(1024)
except ConnectionRefusedError:
    print("Errore: connessione rifiutata.")
except TimeoutError:
    print("Errore: l'operazione ha superato il tempo limite.")
except socket.error as e:
    print(f"Errore socket: {e}")
finally:
    s.close()
```

## Buone Pratiche

1. **Gestione Specifica**: Specificare i tipi di eccezioni che si prevede possano verificarsi, evitando di catturare errori generici inutilmente.
2. **Timeout**: Configurare sempre un timeout per le connessioni di rete, per evitare che il programma rimanga bloccato indefinitamente.
3. **Log degli Errori**: Registrare dettagli sugli errori per facilitare il debug, specialmente in applicazioni critiche.
4. **Pulizia delle Risorse**: Utilizzare `finally` o contesti gestiti (come il costrutto `with`) per garantire che file e connessioni vengano sempre chiusi.

Con una corretta gestione delle eccezioni di I/O e di rete, è possibile creare applicazioni robuste e resilienti a errori imprevisti, migliorando l'affidabilità e la qualità dell'esperienza utente.
