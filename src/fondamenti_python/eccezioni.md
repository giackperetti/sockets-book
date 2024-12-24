# Gestione delle Eccezioni

Le eccezioni rappresentano eventi straordinari o imprevisti che si verificano durante l'esecuzione di un programma, interrompendo il normale flusso delle istruzioni. Piuttosto che consentire al programma di arrestarsi in modo anomalo, Python fornisce strumenti per rilevare e gestire questi eventi in modo controllato, garantendo una maggiore robustezza del codice.

Un'eccezione viene generata quando Python incontra un errore durante l'esecuzione, come il tentativo di dividere un numero per zero, accedere a una risorsa inesistente o eseguire operazioni con dati incompatibili. Quando ciò accade, viene creato un oggetto eccezione che contiene informazioni sull'errore, come il tipo e il messaggio associato.

## Il Blocco Try...Except...Finally

Python offre una struttura dedicata per gestire le eccezioni: il blocco `try...except...finally`. Questa struttura consente di eseguire del codice che potrebbe generare errori (`try`), definire come reagire a specifiche eccezioni (`except`) e garantire l'esecuzione di codice finale, indipendentemente dal risultato (`finally`).

- **try**: Il codice all'interno del blocco `try` viene eseguito normalmente, ma Python "ascolta" la possibilità che si verifichi un'eccezione.
- **except**: Se viene generata un'eccezione nel blocco `try`, il flusso si sposta immediatamente nel blocco `except`, dove è possibile gestire l'errore.
- **finally**: Questo blocco viene sempre eseguito, indipendentemente dal fatto che un'eccezione sia stata sollevata o meno, ed è utile per operazioni come la chiusura di file o il rilascio di risorse.

## Eccezioni di I/O e di Rete

Le eccezioni di I/O (Input/Output) si verificano durante l'interazione con il filesystem o dispositivi esterni, ad esempio quando si tenta di aprire un file inesistente o di scrivere su un dispositivo non accessibile. È fondamentale gestire questi errori per evitare che il programma si blocchi in modo inaspettato.

Analogamente, le eccezioni di rete possono verificarsi durante le comunicazioni con server o risorse esterne, ad esempio in caso di perdita di connessione, timeout o risposte non valide. Poiché le operazioni di rete sono intrinsecamente soggette a errori esterni, gestirle correttamente è essenziale per garantire la stabilità di un'applicazione.

Questa sezione introduce i concetti fondamentali delle eccezioni, mostrando come gestire errori comuni utilizzando `try...except...finally`. Approfondiremo anche i tipi di eccezioni più comuni legati a I/O e rete, aiutandovi a scrivere codice affidabile e resiliente.
