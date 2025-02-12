# Implementazione della Logica di Gioco

In questa sezione esamineremo il funzionamento del programma del Tris progettato per consentire a 2 persone nella stessa LAN di giocare insieme.

Il codice è tenuto in un singolo file e in base agli argomenti ricevuti dalla CLI esegue un client o un server.

## Comani per eseguire un server e dei client

Per eseguire un server si usa il sottocomando `server` e si passano IP(tendenzialmente localhost) e porta dove si vogliono far partire il client e il server. Per esempio si può eseguire questo comando:

```bash
python3 tris.py server --host 127.0.0.1 --port 5000
```

Per eseguire un server si usa il sottocomando `client` e si passano IP(tendenzialmente localhost) e porta del server al quale ci si vuole connettere. Per esempio si può eseguire questo comando:

```bash
python3 tris.py client --host 172.20.22.123 --port 5000
```

## Codice del programma commentato

```python
import socket
import argparse
import hashlib
import threading
import random
import re


# Genera un token univoco per il gioco
TOKEN = hashlib.sha256("This is a tris game".encode()).hexdigest()


# Classe per gestire il gioco del Tris
class TicTacToe:
    def __init__(self):
        # Inizializza il tabellone con i numeri da 1 a 9
        self.board = [[str(i * 3 + j + 1) for j in range(3)] for i in range(3)]

    def make_move(self, position, symbol):
        # Converte la posizione inserita dall'utente in coordinate di riga e colonna
        POSITION_TO_COORDINATE = {
            1: (0, 0), 2: (0, 1), 3: (0, 2),
            4: (1, 0), 5: (1, 1), 6: (1, 2),
            7: (2, 0), 8: (2, 1), 9: (2, 2)
        }
        row, col = POSITION_TO_COORDINATE[int(position)]
        # Verifica se la posizione è disponibile e inserisce il simbolo del giocatore
        if self.board[row][col] not in ["X", "O"]:
            self.board[row][col] = symbol
            return True
        return False

    def check_win(self):
        # Controlla le righe e le colonne
        for i in range(3):
            if (self.board[i][0] == self.board[i][1]) and (
                self.board[i][1] == self.board[i][2]
            ):
                return True
            if (self.board[0][i] == self.board[1][i]) and (
                self.board[1][i] == self.board[2][i]
            ):
                return True
        # Controlla le diagonali
        if (self.board[0][0] == self.board[1][1]) and (
            self.board[1][1] == self.board[2][2]
        ):
            return True
        if (self.board[0][2] == self.board[1][1]) and (
            self.board[1][1] == self.board[2][0]
        ):
            return True
        return False

    def is_draw(self):
        # Controlla se il tabellone è pieno e non c'è un vincitore
        return all((cell in ["X", "O"]) for row in self.board for cell in row)

    def get_board_string(self):
        # Restituisce una rappresentazione testuale del tabellone
        return "\n".join([" | ".join(row) for row in self.board])


# Classe per gestire il server
class Server:
    def __init__(self, host, port):
        self.host = host
        self.port = port

        # Crea il socket del server
        self.server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.server.bind((host, port))
        self.server.listen(2)  # Il server può gestire due giocatori

    # Metodo che gestisce il flow del gioco del Tris
    def handle_game(self, player1, player2):
        game = TicTacToe()
        board_state = game.get_board_string()

        # Invia ai giocatori il loro simbolo e lo stato iniziale del tabellone
        player1.sendall(f"X\n{board_state}".encode())
        player2.sendall(f"O\n{board_state}".encode())

        # Seleziona casualmente il giocatore che inizia
        current_player = random.choice([(player1, "X"), (player2, "O")])
        other_player = (
            (player2, "O") if (current_player[0] == player1) else (player1, "X")
        )

        current_player[0].sendall("TURN".encode())
        other_player[0].sendall("WAIT".encode())

        # Game Loop
        while True:
            try:
                # Ricevi la mossa del giocatore
                move = current_player[0].recv(1024).decode()
                if game.make_move(move, current_player[1]):
                    board_state = game.get_board_string()

                    # Informa i giocatori dei cambiamenti avvenuti alla tavola di gioco
                    for player in [player1, player2]:
                        player.sendall(f"BOARD\n{board_state}".encode())

                    # Controlla se qualcuno ha vinto...
                    if game.check_win():
                        winner = current_player[1]
                        player1.sendall(f"END\nWinner: {winner}".encode())
                        player2.sendall(f"END\nWinner: {winner}".encode())
                        break
                    elif game.is_draw():  # ...O se c'è un pareggio
                        player1.sendall("END\nDraw".encode())
                        player2.sendall("END\nDraw".encode())
                        break

                    # Cambia il giocatore che deve giocare
                    current_player, other_player = other_player, current_player
                    # Informa il giocatore a cui tocca che deve giocare
                    current_player[0].sendall("TURN".encode())
                    # Informa l'altro giocatore che deve attendere
                    other_player[0].sendall("WAIT".encode())

            except Exception as e:
                print(f"Error in game: {str(e)}")
                break

        # A partita finita si chiudono le connessioni dei client
        player1.close()
        player2.close()

    # Metodo che esegue il Server
    def run(self):
        print(f"Server started on port {self.port}")
        # Loop delle connessioni
        while True:
            try:
                print("Waiting for players...")

                # Connessione dal primo giocatore
                player1, addr1 = self.server.accept()
                print(f"Player 1 connected from {addr1}")

                # Verifichiamo che il player1 sia autorizzato attraverso il controllo di un token
                player1.sendall(TOKEN.encode())
                if player1.recv(1024).decode() != "OK":
                    print("Player 1 verification failed")
                    player1.close()
                    continue

                # Connessione dal primo giocatore
                player2, addr2 = self.server.accept()
                print(f"Player 2 connected from {addr2}")

                # Verifichiamo che il player2 sia autorizzato attraverso il controllo di un token
                player2.sendall(TOKEN.encode())
                if player2.recv(1024).decode() != "OK":
                    print("Player 2 verification failed")
                    player2.close()
                    player1.close()
                    continue

                print("Starting new game")

                # Inizializza il thread che fa partire il game loop
                thread = threading.Thread(
                    target=self.handle_game, args=(player1, player2)
                )
                thread.start()
            except Exception as e:
                print(f"Server error: {str(e)}")


# Classe per gestire i client
class Client:
    def __init__(self, host, port):
        self.host = host
        self.port = port

        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.socket.connect((host, port))  # Connetti il socket client al server

    def print_board(self, board):
        print("\nCurrent board:")
        print(board)
        print()

    # Metodo che esegue il client
    def run(self):
        try:
            # Controllo autenticazione del server col il TOKEN comune
            if self.socket.recv(1024).decode() == TOKEN:
                self.socket.sendall("OK".encode())
                print("Connected to server!")
            else:
                print("Invalid server!")
                return

            # Ricevi i dati iniziali:
            #   - La game board
            #   - Il simbolo che rappresenta il giocatore
            initial_data = self.socket.recv(1024).decode().split("\n")
            self.symbol = initial_data[0]
            self.print_board("\n".join(initial_data[1:]))
            print(f"You are playing as {self.symbol}")

            # Loop della connessione del client
            while True:
                # Ricevi messaggi dal server
                message = self.socket.recv(1024).decode()

                # Gesione dei vari messaggi possibili dal server:
                #   - BOARD: Fa si che il client stampi la game board
                #   - TURN: Permette al giocatore di giocare
                #   - WAIT: Fa si che il giocatore aspetti che l'altro finisca di giocare
                #   - END: Termina la partita, stampando lo stato in cui è terminata
                if message.startswith("BOARD"):
                    self.print_board(message.split("\n", 1)[1])
                elif message == "TURN":
                    while True:
                        move = input("Your turn! Enter position (1-9): ")
                        if move.isdigit() and 1 <= int(move) <= 9:
                            self.socket.sendall(move.encode())
                            break
                        print("Invalid move! Please enter a number between 1-9.")
                elif message == "WAIT":
                    print("Waiting for opponent's move...")
                elif message.startswith("END"):
                    result = message.split("\n")[1]
                    print(f"\nGame Over! {result}")
                    break
        except Exception as e:
            print(f"Error: {str(e)}")
        finally:
            self.socket.close()


# Funzione che controlla se una stringa è un IP valido
def valid_ip(ip):
    # Pattern RegEx di controllo
    pattern = r"^(\d{1,3}\.){3}\d{1,3}$"
    if not re.fullmatch(pattern, ip):
        raise argparse.ArgumentTypeError(f"Invalid IP address: {ip}")
    return ip


# Funzione che controlla se una stringa è un PORTA valida
def valid_port(port):
    try:
        port_num = int(port)
        if 0 <= port_num <= 65535:
            return port_num
        raise ValueError
    except ValueError:
        raise argparse.ArgumentTypeError(f"Invalid port number: {port}")


# Crea un parser di argomenti per gestire gli argomenti ricevuti nella CLI
def parse_arguments():
    parser = argparse.ArgumentParser(
        description="Sockets-based TicTacToe game",
        formatter_class=argparse.RawDescriptionHelpFormatter,
        epilog="""
examples:
  Start server:
    python3 %(prog)s server --host 0.0.0.0 --port 5000

  Start client:
    python3 %(prog)s client --host 127.0.0.1 --port 5000
""",
    )

    # Crea tanti subparser quanti i sotto-comandi
    subparsers = parser.add_subparsers(dest="mode", help="Mode of operation")
    subparsers.required = True

    # Gestisci i sotto-comandi del comando server
    server_parser = subparsers.add_parser("server", help="Start server")
    server_parser.add_argument(
        "--host", type=valid_ip, required=True, help="Server IP address to bind to"
    )
    server_parser.add_argument(
        "--port", type=valid_port, required=True, help="Port number"
    )

    # Gestisci i sotto-comandi del comando client
    client_parser = subparsers.add_parser("client", help="Start client")
    client_parser.add_argument(
        "--host", type=valid_ip, required=True, help="Server IP address to connect to"
    )
    client_parser.add_argument(
        "--port", type=valid_port, required=True, help="Port number"
    )

    return parser.parse_args()


# Funzione per eseguire il programma
def main():
    args = parse_arguments()

    # Se è stato utilizzato un sotto-comando di client fa partire il client altrimenti parte un server
    if args.mode == "client":
        client = Client(host=args.host, port=args.port)
        client.run()
    else:
        server = Server(host=args.host, port=args.port)
        server.run()


if __name__ == "__main__":
    main()

```
