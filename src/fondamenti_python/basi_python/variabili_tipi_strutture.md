# Variabili, Tipi di Dati e Strutture Dati
In Python le variabili non hanno bisogno di essere tipizzate(si può ma è facoltativo) e presenta svariati tipi di dati.
I tipi più comunemente usati sono:
- numeri interi (int)
- numeri decimali (float)
- stringhe (str)
- booleani (bool)

```python
x: int = 34
y: float = 2.675
word: str = "Hello"
f_string: str = f"Number x = {x}"
is_animal_a_dog: bool = True
```

Questi tipi di dati base vengono sfruttati per utilizzare strutture dati più complesse, come:
- Liste: sequenze mutabili di dati che possono essere di natura omogenea o eterogenea
```python
fruits: list[str] = ['apple', 'banana', 'cherry']
```
- Tuples: sequenze immutabili sequences, spesso usate per contenere dati eterogenei.
```python
person_info: tuple[str, str, int] = ('John', 'Doe', 30)
```
- Sets: collezioni disordinate di elementi unici, utilizzate per contenere dati dove l'ordine è indifferente
```python
unique_numbers: set[int] = {3, 2, 3, 1}
```
- Dizionari: collezioni di coppie chiave-valore, le chiavi non possono ripetersi
```python
voti_studenti: dict[str, str] = {'Pippo': '8', 'Sara': '9.5', 'Gino': '6'}
```