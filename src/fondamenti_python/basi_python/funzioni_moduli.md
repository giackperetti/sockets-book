# Funzioni e Moduli

Questa sezione copre due aspetti fondamentali della programmazione in Python: le **funzioni** e i **moduli**. Le funzioni consentono di organizzare il codice in blocchi riutilizzabili, migliorando leggibilità e manutenzione. I moduli, invece, permettono di suddividere il codice in file separati, rendendo più facile la gestione di progetti complessi.

## Funzioni

In Python, una funzione è un blocco di codice che esegue un'operazione specifica. Le funzioni possono accettare parametri e restituire valori. Esistono due tipi principali di funzioni in Python: **funzioni incorporate** (come `print()`, `len()`, ecc.) e **funzioni definite dall'utente**.

```python
def greet(name: str) -> None:
    """
    greet: Funzione che stampa un saluto personalizzato.

    :param str name: Il nome della persona da salutare.
    """
    print(f"Ciao, {name}!")

greet("Mario")  # Output: Ciao, Mario!
```
Questa funzione prende un parametro `name` e stampa un messaggio di saluto. Le funzioni possono anche restituire valori usando `return`.

### Funzioni con Restituzione di Valori
```python
def somma(a: int, b: int) -> int:
    return a + b

risultato: int = somma(5, 3)  # Risultato: 8
```
Le funzioni possono avere documentazione tramite stringhe `docstring`, permettendo di descriverne il comportamento e i parametri.

## Moduli

Un modulo è un file Python che contiene definizioni di funzioni, classi o variabili che possono essere riutilizzate in altri file. Per utilizzare un modulo, si usa l'istruzione `import`.

### Importazione di Moduli
```python
import math
print(math.sqrt(16))  # Output: 4.0
```
In questo esempio, il modulo `math` viene utilizzato per calcolare la radice quadrata. È possibile anche importare elementi specifici di un modulo:
```python
from datetime import datetime
print(datetime.now())  # Output: Data e ora corrente
```

### Creazione di Moduli Personalizzati

Un modulo personalizzato è semplicemente un file `.py` con funzioni o classi definite dall'utente. Ad esempio, un file chiamato `mio_modulo.py` può contenere:
```python
def saluta():
    print("Ciao dal modulo personalizzato!")
```
Per usarlo, basta importarlo:
```python
import mio_modulo
mio_modulo.saluta()
```

## Pacchetti
Un pacchetto è una raccolta di moduli organizzati in una struttura a directory. Una directory è considerata un pacchetto se contiene un file `__init__.py`. Questo approccio è utilizzato per organizzare progetti complessi.

Ad esempio, la struttura:
```
pacchetto/
    ├── __init__.py
    ├── modulo1.py
    └── modulo2.py
```

Può essere utilizzata con:
```python
from pacchetto import modulo1
modulo1.funzione()
```

Le funzioni e i moduli rappresentano un pilastro fondamentale per scrivere codice Python efficiente, modulare e facile da mantenere in quanto ci permettono di riutilizzare parti del codice che altrimenti dovremmo riscrivere svariate volte.