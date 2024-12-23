# Variabili, Tipi di Dati e Strutture Dati in Python

In Python, le variabili non richiedono una dichiarazione esplicita del tipo (grazie alla tipizzazione dinamica), ma è possibile specificare i tipi di dati tramite annotazioni opzionali.
Le annotazioni come `list[str]` o `dict[str, str]` non sono obbligatorie per il funzionamento del codice, ma aiutano gli sviluppatori a comprendere meglio l'intento del codice e gli permettono di utilizzare le variabili in maniera più sicura.
Python offre numerosi tipi di dati, tra cui i più comuni sono:
- **Numeri interi** (`int`): per rappresentare numeri senza parte decimale.
- **Numeri decimali** (`float`): per rappresentare numeri con parte decimale.
- **Stringhe** (`str`): per rappresentare sequenze di caratteri.
- **Booleani** (`bool`): per rappresentare valori di `True` o `False`.

Esempio di utilizzo:
```python
x: int = 34                 # Variabile intera
y: float = 2.675            # Variabile decimale
word: str = "Hello"         # Stringa
f_string: str = f"Number x = {x}"  # Stringa formattata
is_animal_a_dog: bool = True  # Variabile booleana
```

<br/><br/>

Questi tipi di dati base sono il fondamento per creare e lavorare con **strutture dati** più complesse:
1. **Liste** (`list`): Sequenze ordinate e mutabili, che possono contenere dati omogenei o eterogenei.
    - Utilizzate quando è necessario accedere, modificare o aggiungere elementi dinamicamente.
    ```python
    fruits: list[str] = ['apple', 'banana', 'cherry']
    ```

2. **Tuples** (`tuple`): Sequenze ordinate e immutabili, spesso usate per raggruppare dati eterogenei.
    - Ideali per rappresentare dati che non devono essere modificati.
    ```python
    person_info: tuple[str, str, int] = ('John', 'Doe', 30)
    ```

3. **Set** (`set`): Collezioni non ordinate di elementi unici.
    - Utili quando l'ordine è irrilevante e occorre garantire l'unicità degli elementi.
    ```python
    unique_numbers: set[int] = {3, 2, 3, 1}  # Risultato: {1, 2, 3}
    ```

4. **Dizionari** (`dict`): Collezioni di coppie chiave-valore, in cui le chiavi sono uniche.
    - Ideali per rappresentare associazioni tra dati (ad esempio, mappature o lookup tables).
    ```python
    voti_studenti: dict[str, str] = {'Pippo': '8', 'Sara': '9.5', 'Gino': '6'}
    ```

<br/><br/>
Queste strutture permettono di rappresentare e manipolare i dati in maniera flessibile ed efficiente, offrendo al programmatore gli strumenti necessari per risolvere problemi di diversa complessità.