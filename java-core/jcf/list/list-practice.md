## **Metody List, ArrayList i LinkedList**

Podczas rozwiązywania zadań skorzystasz z poniższych metod interfejsu `List`:

1. **Dodawanie elementów**:
    - `add(element)` – dodaje element na końcu listy.
    - `add(index, element)` – dodaje element na określonym indeksie.
    - `addAll(Collection c)` – dodaje wszystkie elementy kolekcji do listy.

2. **Pobieranie i wyszukiwanie**:
    - `get(index)` – pobiera element na określonym indeksie.
    - `indexOf(element)` – zwraca pierwszy indeks danego elementu.
    - `lastIndexOf(element)` – zwraca ostatni indeks danego elementu.

3. **Usuwanie elementów**:
    - `remove(index)` – usuwa element na podanym indeksie.
    - `remove(Object o)` – usuwa pierwsze wystąpienie danego elementu.
    - `clear()` – usuwa wszystkie elementy.

4. **Modyfikacja**:
    - `set(index, element)` – zamienia element na określonym indeksie.

5. **Rozmiar i zawartość**:
    - `size()` – zwraca liczbę elementów.
    - `contains(element)` – sprawdza, czy element znajduje się na liście.

---

## **Zadania praktyczne**

### **1. Dodawanie elementów**

**Cel zadania**: Poznaj różne sposoby dodawania elementów do listy.

1. Utwórz listę typu `ArrayList` i dodaj następujące elementy: `["Anna", "Jan"]`.

2. Dodaj element "Maria" na indeksie `1`.

3. Spróbuj dodać element `null` dwukrotnie.

4. Spróbuj dodać element na indeksie większym niż rozmiar listy (np. indeks `5`).

**Oczekiwane wyniki**:
```
Lista początkowa: [Anna, Jan]
Po dodaniu "Maria" na indeksie 1: [Anna, Maria, Jan]
Po dodaniu dwóch wartości null: [Anna, Maria, Jan, null, null]
Błąd przy próbie dodania elementu na indeksie 5: IndexOutOfBoundsException
```

---

### **2. Usuwanie elementów**

**Cel zadania**: Poznaj różne metody usuwania elementów z listy.

1. Utwórz listę: `["Anna", "Maria", "Jan", null, null]`.

2. Usuń element na indeksie `2`.

3. Usuń pierwsze wystąpienie wartości `null`.

4. Spróbuj usunąć element, który nie istnieje w liście, np. "Krzysztof".

**Oczekiwane wyniki**:
```
Lista początkowa: [Anna, Maria, Jan, null, null]
Po usunięciu elementu na indeksie 2: [Anna, Maria, null, null]
Po usunięciu pierwszego wystąpienia null: [Anna, Maria, null]
Przy próbie usunięcia "Krzysztof": Lista pozostaje bez zmian: [Anna, Maria, null]
```

---

### **3. Pobieranie elementów**

**Cel zadania**: Sprawdź, jak działa pobieranie elementów za pomocą indeksu.

1. Utwórz listę: `["Anna", "Maria", "Jan"]`.

2. Pobierz element na indeksie `1`.

3. Spróbuj pobrać element na indeksie poza zakresem (np. `5`).

**Oczekiwane wyniki**:
```
Lista początkowa: [Anna, Maria, Jan]
Element na indeksie 1: Maria
Błąd przy próbie pobrania elementu na indeksie 5: IndexOutOfBoundsException
```

---

### **4. Wyszukiwanie elementów**

**Cel zadania**: Sprawdź działanie metod wyszukiwania w liście.

1. Utwórz listę: `["Anna", "Maria", "Jan", "Anna"]`.

2. Znajdź pierwszy indeks "Anna".

3. Znajdź ostatni indeks "Anna".

4. Sprawdź, czy lista zawiera element "Krzysztof".

**Oczekiwane wyniki**:
```
Lista początkowa: [Anna, Maria, Jan, Anna]
Pierwszy indeks "Anna": 0
Ostatni indeks "Anna": 3
Czy lista zawiera "Krzysztof": false
```

---

### **5. Modyfikowanie listy**

**Cel zadania**: Zrozum, jak można zmieniać elementy w liście.

1. Utwórz listę: `["Anna", "Maria", "Jan"]`.

2. Zmień element na indeksie `1` na "Marta".

3. Spróbuj zmienić element na indeksie poza zakresem (np. `5`).

**Oczekiwane wyniki**:
```
Lista początkowa: [Anna, Maria, Jan]
Po zmianie elementu na indeksie 1 na "Marta": [Anna, Marta, Jan]
Błąd przy próbie zmiany elementu na indeksie 5: IndexOutOfBoundsException
```

---

### **6. Kopiowanie i konwersja**

**Cel zadania**: Naucz się kopiować elementy między listami.

1. Utwórz `ArrayList` z elementami `["A", "B", "C"]`.

2. Skopiuj elementy tej listy do nowej listy typu `LinkedList`.

3. Dodaj do nowej listy element "D".

**Oczekiwane wyniki**:
```
Lista ArrayList: [A, B, C]
Lista LinkedList po kopiowaniu: [A, B, C]
Lista LinkedList po dodaniu "D": [A, B, C, D]
```

---

### **7. Łączenie list**

1. Utwórz dwie listy: `List1: [A, B, C]`, `List2: [D, E, F]`.

2. Połącz obie listy w jedną.

**Oczekiwane wyniki**:
```
Połączona lista: [A, B, C, D, E, F]
```

---

### **8. Niemutowalne listy**

**Cel zadania**: Sprawdź, jak działają listy niemutowalne.

1. Utwórz niemutowalną listę za pomocą `List.of("X", "Y", "Z")`.

2. Wyświetl element na indeksie `1`.

3. Spróbuj dodać element "W" do tej listy.

4. Spróbuj usunąć element z tej listy.

**Oczekiwane wyniki**:
```
Lista początkowa: [X, Y, Z]
Element na indeksie 1: Y
Błąd przy próbie dodania elementu: UnsupportedOperationException
Błąd przy próbie usunięcia elementu: UnsupportedOperationException
```

### **9. Porównanie wydajności `ArrayList` i `LinkedList`**

**Cel zadania**:
1. Utwórz dużą listę (`ArrayList`) liczb od 1 do 1_000_000.
2. Dodaj element na początku listy.
3. Powtórz to samo dla `LinkedList`.
4. Zmierz czas wykonywania obu operacji.

> Wykorzystaj `System.currentTimeMillis()` do odmierzania czasu lub inny, znany Ci sposób.