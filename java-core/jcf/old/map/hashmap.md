# `HashMap` w Javie – Wprowadzenie i Szczegóły Działania

## 1. Wstęp

### Czym jest `HashMap`

`HashMap` to klasa w języku Java implementująca interfejs `Map`, służąca do przechowywania par klucz-wartość. Umożliwia szybki dostęp do danych na podstawie klucza dzięki wykorzystaniu mechanizmu haszowania.

### Podstawowe cechy `HashMap`

- **Szybki dostęp do danych**: Średni czas operacji `get`, `put` i `remove` wynosi O(1).
- **Nie gwarantuje kolejności**: Elementy nie są przechowywane w określonym porządku.
- **Pozwala na `null` jako klucz i wartość**: Dopuszczalny jest jeden klucz `null` oraz dowolna liczba wartości `null`.
- **Niebezpieczna w środowiskach wielowątkowych**: Nie jest synchronizowana; wymaga dodatkowej synchronizacji w aplikacjach wielowątkowych.

---

## 2. Jak działa `HashMap`

### Struktura wewnętrzna: tablica `Node`

`HashMap` wykorzystuje wewnętrzną tablicę `Node<K, V>[]`, gdzie każdy element tablicy nazywany jest kubełkiem (ang. *bucket*). Kubełek przechowuje listę wiązaną lub drzewo binarne z elementami o tym samym indeksie.

**Klasa `Node` zawiera:**

- **`K key`**: Klucz.
- **`V value`**: Wartość.
- **`Node<K, V> next`**: Referencja do następnego węzła (wykorzystywana do kolizji hashu).

### Mechanizm działania

1. **Obliczenie haszu klucza**: Za pomocą metody `hashCode()` klucza.
2. **Określenie indeksu kubełka**: Na podstawie haszu i rozmiaru tablicy.
3. **Przechowywanie elementu**:
    - Jeśli kubełek jest pusty, wstawiany jest nowy `Node`.
    - Jeśli kubełek zawiera już elementy, dochodzi do kolizji i element jest dodawany do listy wiązanej lub drzewa w tym kubełku.
4. **Wyszukiwanie elementu**:
    - Obliczany jest indeks kubełka.
    - Przeszukiwana jest lista wiązana lub drzewo w kubełku w poszukiwaniu klucza.

---

## 3. Szczegółowe omówienie hashowania

### Czym jest hashowanie

Hashowanie to proces przekształcania danych wejściowych (klucza) w wartość haszującą (liczbę całkowitą). W `HashMap` wartość ta służy do szybkiego odnalezienia miejsca przechowywania pary klucz-wartość w tablicy kubełków.

### Funkcja haszująca

**Cel funkcji haszującej:**

- **Równomierne rozproszenie kluczy**: Minimalizowanie liczby kolizji.
- **Deterministyczność**: Ten sam klucz zawsze daje ten sam hasz.
- **Efektywność**: Szybkie obliczanie wartości haszującej.

**Obliczanie indeksu kubełka:**

```java
int hash = key == null ? 0 : Math.abs(key.hashCode());
int index = hash % capacity; // capacity to rozmiar tablicy
```

### Rozwiązywanie kolizji

Kolizja występuje, gdy dwa różne klucze dają ten sam indeks kubełka. `HashMap` rozwiązuje kolizje poprzez:

1. **Łańcuchowanie (separowane)**:
    - Elementy o tym samym indeksie są przechowywane w liście wiązanej w kubełku.
    - W przypadku małej liczby kolizji.

2. **Wykorzystanie drzew binarnych (od Java 8)**:
    - Gdy liczba elementów w kubełku przekroczy próg (domyślnie 8), lista wiązana jest przekształcana w drzewo czerwono-czarne.
    - Poprawia to wydajność z O(n) do O(log n) dla operacji w kubełku.

**Mechanizm działania podczas wstawiania:**

- **Bez kolizji**: Wstawienie nowego `Node` do pustego kubełka.
- **Kolizja**:
    - **Liczba elementów < 8**: Dodanie nowego `Node` do listy wiązanej.
    - **Liczba elementów ≥ 8**: Przekształcenie listy w drzewo i dodanie nowego `TreeNode`.

---

## 4. Porównanie wydajności

### Złożoność operacji

| Operacja | Średni przypadek | Najgorszy przypadek (bez drzew) | Najgorszy przypadek (z drzewami) |
|----------|------------------|---------------------------------|----------------------------------|
| `put`    | O(1)             | O(n)                            | O(log n)                         |
| `get`    | O(1)             | O(n)                            | O(log n)                         |
| `remove` | O(1)             | O(n)                            | O(log n)                         |

- **Średni przypadek**: Dzięki dobremu haszowaniu i niskiej liczbie kolizji.
- **Najgorszy przypadek bez drzew**: Gdy wszystkie klucze trafiają do jednego kubełka (lista wiązana).
- **Najgorszy przypadek z drzewami**: Dzięki drzewom czerwono-czarnym złożoność spada do O(log n).

### Tabela porównawcza

| Właściwość              | `HashMap` z listami wiązanymi | `HashMap` z drzewami czerwono-czarnymi |
|-------------------------|-------------------------------|----------------------------------------|
| Złożoność wstawiania    | O(n)                          | O(log n)                               |
| Złożoność wyszukiwania  | O(n)                          | O(log n)                               |
| Zużycie pamięci         | Mniejsze                      | Większe                                |
| Złożoność implementacji | Prostsza                      | Bardziej złożona                       |
| Stabilność wydajności   | Niska przy dużych kolizjach   | Wysoka nawet przy dużych kolizjach     |

---

## 5. Praktyczne wskazówki

### Optymalizacja

- **Wybór odpowiedniej pojemności początkowej**:
    - Jeśli wiesz, że będziesz przechowywać dużo danych, zainicjuj `HashMap` z większą pojemnością.
- **Dostosowanie współczynnika obciążenia (`load factor`)**:
    - Domyślnie wynosi 0.75; zmniejszenie go może zmniejszyć liczbę kolizji kosztem większego zużycia pamięci.
- **Implementacja dobrych metod `hashCode()` i `equals()`**:
    - Zapewnia równomierne rozproszenie kluczy i poprawne porównywanie.

### Typowe błędy do unikania

- **Używanie mutowalnych kluczy**:
    - Nie należy zmieniać obiektów użytych jako klucze po ich wstawieniu do mapy.
- **Nieprawidłowa implementacja `hashCode()` i `equals()`**:
    - Niespójność tych metod może prowadzić do błędów w wyszukiwaniu elementów.
- **Ignorowanie bezpieczeństwa wątkowego**:
    - W aplikacjach wielowątkowych należy użyć `ConcurrentHashMap` lub synchronizacji.
- **Przekroczenie pojemności tablicy**:
    - Brak mechanizmu rozszerzania tablicy kubełków może prowadzić do spadku wydajności.
