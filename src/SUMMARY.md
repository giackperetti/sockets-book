# Summary

# Fondamenti di Python per la Programmazione con i Sockets
  - [Basi di Python](fondamenti_python/basi_python.md)
    - [Variabili, Tipi di Dati e Strutture Dati](fondamenti_python/basi_python/variabili_tipi_strutture.md)
    - [Strutture di Controllo](fondamenti_python/basi_python/strutture_controllo.md)
    - [Funzioni e Moduli](fondamenti_python/basi_python/funzioni_moduli.md)
  - [Lavorare con le Librerie di Python](fondamenti_python/librerie_python.md)
    - [Importare e Utilizzare Moduli](fondamenti_python/librerie_python/importare_utilizzare_moduli.md)
    - [I Moduli `sys` e `os` per Operazioni a Livello di Sistema](fondamenti_python/librerie_python/moduli_sys_os.md)
  - [Gestione delle Eccezioni](fondamenti_python/eccezioni.md)
    - [Utilizzo di `try`, `except` e `finally`](fondamenti_python/eccezioni/try_except_finally.md)
    - [Gestione di Eccezioni I/O e di Networking](fondamenti_python/eccezioni/eccezioni_io_networking.md)
  - [Introduzione al Multithreading](fondamenti_python/multithreading.md)
    - [Cosa Sono i Thread? Casi d’Uso nel Networking](fondamenti_python/multithreading/cosa_sono_thread.md)
    - [Basi del Modulo `threading`](fondamenti_python/multithreading/modulo_threading.md)
    - [Sincronizzazione dei Thread con i Locks](fondamenti_python/multithreading/sincronizzazione_locks.md)

# Networking e Sockets
  - [Cosa Sono i Sockets?](networking_sockets/cosa_sono_sockets.md)
    - [Fondamenti di Networking e Comunicazione](networking_sockets/cosa_sono_sockets/fondamenti_networking.md)
    - [Come i Sockets Abilitano il Trasferimento di Dati](networking_sockets/cosa_sono_sockets/trasferimento_dati.md)
  - [Tipi di Sockets](networking_sockets/tipi_sockets.md)
    - [Sockets TCP vs UDP: Differenze Principali](networking_sockets/tipi_sockets/tcp_vs_udp.md)
    - [Introduzione ai Raw Sockets e Casi d’Uso](networking_sockets/tipi_sockets/raw_sockets.md)
  - [Comprendere i Protocolli di Rete](networking_sockets/protocolli_rete.md)
    - [Basi di TCP/IP: Porte, IP e Livelli di Protocollo](networking_sockets/protocolli_rete/tcp_ip.md)
    - [Panoramica del Modello OSI](networking_sockets/protocolli_rete/modello_osi.md)
  - [Configurare Python per i Sockets](networking_sockets/configurare_python.md)
    - [Installazione di Python e dei Moduli Necessari](networking_sockets/configurare_python/installazione.md)
    - [Testare Script con i Sockets Localmente](networking_sockets/configurare_python/testare_script.md)
  - [Introduzione al Modulo Sockets](networking_sockets/modulo_sockets.md)
    - [Metodi e Attributi Principali](networking_sockets/modulo_sockets/metodi_attributi.md)
    - [Comprendere le Famiglie di Indirizzi (IPv4 vs IPv6)](networking_sockets/modulo_sockets/famiglie_indirizzi.md)

# Programmazione Client-Server con i Sockets
  - [Creare un Client TCP](programmazione_client_server/client_tcp.md)
    - [Connessione a un Server](programmazione_client_server/client_tcp/connessione_server.md)
    - [Invio e Ricezione di Dati](programmazione_client_server/client_tcp/invio_ricezione_dati.md)
  - [Creare un Server TCP](programmazione_client_server/server_tcp.md)
    - [Binding a un Indirizzo e Ascolto](programmazione_client_server/server_tcp/binding_ascolto.md)
    - [Gestione di Client Multipli con i Thread](programmazione_client_server/server_tcp/client_multipli_thread.md)
  - [Uso dei Thread per la Comunicazione Asincrona](programmazione_client_server/comunicazione_asincrona.md)
    - [Creare Thread per Invio e Ricezione di Dati](programmazione_client_server/comunicazione_asincrona/creare_thread.md)
    - [Sicurezza dei Thread e Meccanismi di Blocco](programmazione_client_server/comunicazione_asincrona/sicurezza_blocco.md)

# Casi d’Uso e Progetti
  - [Creazione di un Server Echo di Base](casi_uso_progetti/server_echo_base.md)
  - [Creazione di un’Applicazione Chat Asincrona](casi_uso_progetti/chat_asincrona.md)
    - [Architettura di un Sistema di Chat](casi_uso_progetti/chat_asincrona/architettura_chat.md)
    - [Utilizzo dei Thread per la Comunicazione Client-Server](casi_uso_progetti/chat_asincrona/thread_client_server.md)
    - [Gestione di Client Multipli con i Thread](casi_uso_progetti/chat_asincrona/client_multipli_thread.md)
    - [Sincronizzazione dei Messaggi della Chat](casi_uso_progetti/chat_asincrona/sincronizzazione_messaggi.md)

# Debugging e Testing
  - [Errori Comuni nei Sockets](debugging_testing/errori_comuni.md)
    - [Errori di Connessione e Timeout](debugging_testing/errori_comuni/connessione_timeout.md)
    - [Debugging con i Codici di Errore](debugging_testing/errori_comuni/codici_errore.md)
  - [Testing delle Applicazioni con i Sockets](debugging_testing/testing_applicazioni.md)
    - [Server Mock e Client Simulati](debugging_testing/testing_applicazioni/server_mock_client_simulati.md)
    - [Utilizzo di Strumenti come Wireshark](debugging_testing/testing_applicazioni/strumenti_wireshark.md)