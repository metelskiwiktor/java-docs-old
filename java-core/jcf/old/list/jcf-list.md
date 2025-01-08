# Java Collections Framework: Interfejs `List`

## Spis treści
1. [Wprowadzenie](#wprowadzenie)
2. [Ogólny opis interfejsu `List`](#ogólny-opis-interfejsu-list)
3. [Szczegółowe omówienie metod](#szczegółowe-omówienie-metod)
    - [Metody dostępu](#metody-dostępu)
    - [Metody modyfikujące](#metody-modyfikujące)
    - [Metody wyszukiwania](#metody-wyszukiwania)
    - [Metody iteracji](#metody-iteracji)
4. [Porównanie implementacji `List`](#porównanie-implementacji-list)
    - [`ArrayList`](#arraylist)
    - [`LinkedList`](#linkedlist)
    - [`Vector` i `Stack`](#vector-i-stack)
5. [Porównanie wydajności](#porównanie-wydajności)
6. [Uwzględnienie wersji Javy](#uwzględnienie-wersji-javy)
7. [Najlepsze praktyki](#najlepsze-praktyki)
8. [Przykłady kodu](#przykłady-kodu)
9. [Pytania rekrutacyjne](#pytania-rekrutacyjne)
    - [Pytania podstawowe](#pytania-podstawowe)
    - [Pytania zaawansowane](#pytania-zaawansowane)

---

## Wprowadzenie

Interfejs `List` jest kluczowym elementem **Java Collections Framework**. Reprezentuje uporządkowaną kolekcję, która pozwala na przechowywanie duplikatów i zapewnia kontrolę nad pozycją, w której elementy są wstawiane lub pobierane. Zrozumienie interfejsu `List` jest niezbędne dla każdego programisty Java, ponieważ jest on podstawą wielu powszechnie używanych struktur danych.

---

## Ogólny opis interfejsu `List`

Interfejs `List` znajduje się w pakiecie `java.util` i rozszerza interfejs [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html). Definiuje **sekwencję elementów**, w której:

- Elementy są uporządkowane według ich pozycji (indeksu) w liście.
- Dozwolone są duplikaty elementów.
- Użytkownik ma pełną kontrolę nad tym, gdzie wstawia lub pobiera elementy.

**Hierarchia klas:**

```
java.lang.Object
   ↳ java.util.Collection<E>
       ↳ java.util.List<E>
```

**Główne cechy:**

- **Uporządkowanie:** Elementy są przechowywane w określonej kolejności.
- **Indeksy:** Każdy element ma przypisany indeks, zaczynając od 0.
- **Duplikaty:** Można przechowywać wiele takich samych elementów.

---

## Szczegółowe omówienie metod

Interfejs `List` definiuje szereg metod, które pozwalają na manipulowanie elementami na podstawie ich pozycji w liście.

### Metody dostępu

- **`E get(int index)`**
    - Zwraca element na określonej pozycji.
- **`E set(int index, E element)`**
    - Zastępuje element na określonej pozycji nowym elementem.

### Metody modyfikujące

- **`boolean add(E element)`**
    - Dodaje element na koniec listy.
- **`void add(int index, E element)`**
    - Wstawia element na określoną pozycję.
- **`E remove(int index)`**
    - Usuwa element z określonej pozycji.
- **`boolean remove(Object o)`**
    - Usuwa pierwsze wystąpienie określonego elementu.

### Metody wyszukiwania

- **`int indexOf(Object o)`**
    - Zwraca indeks pierwszego wystąpienia elementu lub -1.
- **`int lastIndexOf(Object o)`**
    - Zwraca indeks ostatniego wystąpienia elementu lub -1.

### Metody iteracji

- **`ListIterator<E> listIterator()`**
    - Zwraca iterator, który pozwala na iterację w obu kierunkach.
- **`ListIterator<E> listIterator(int index)`**
    - Zwraca iterator zaczynający od określonej pozycji.
- **`List<E> subList(int fromIndex, int toIndex)`**
    - Zwraca widok (podlistę) części listy między `fromIndex` a `toIndex`.

---

## Porównanie implementacji `List`

### `ArrayList`

- **Opis:** Implementacja oparta na tablicy dynamicznej.
- **Cechy:**
    - Szybki dostęp do elementów (`get`, `set`) O(1).
    - Wolne operacje wstawiania/usuwania wewnątrz listy O(n).
- **Kiedy używać:**
    - Gdy dominują operacje odczytu i dodawania na końcu listy.

### `LinkedList`

- **Opis:** Implementacja oparta na dwukierunkowej liście powiązanej.
- **Cechy:**
    - Wolniejszy dostęp do elementów (`get`, `set`) O(n).
    - Szybsze operacje wstawiania/usuwania na początku i wewnątrz listy O(1).
- **Kiedy używać:**
    - Gdy dominują operacje wstawiania/usuwania wewnątrz listy.

### `Vector` i `Stack`

- **`Vector`:**
    - Podobny do `ArrayList`, ale synchronizowany.
    - Wolniejszy w środowiskach jednowątkowych.
- **`Stack`:**
    - Rozszerza `Vector`, realizuje stos (LIFO).
    - **Od Javy 1.6:** Zalecane jest użycie `Deque` zamiast `Stack`.

---

## Porównanie wydajności

| Operacja                | `ArrayList`         | `LinkedList`                                |
|-------------------------|---------------------|---------------------------------------------|
| **Dostęp (`get`)**      | O(1)                | O(n)                                        |
| **Wstawianie na końcu** | O(1) amortyzowane   | O(1)                                        |
| **Wstawianie wewnątrz** | O(n)                | O(1)                                        |
| **Usuwanie**            | O(n)                | O(1)                                        |
| **Przechowywanie**      | Mniej pamięciożerne | Więcej pamięciożerne (dodatkowe referencje) |

**Uwagi:**

- `ArrayList` jest lepszy w przypadkach, gdy częste są operacje dostępu losowego.
- `LinkedList` sprawdza się, gdy operacje wstawiania/usuwania są częste na początku lub wewnątrz listy.

---

## Uwzględnienie wersji Javy

- **Java 1.2:** Wprowadzenie `Collection Framework`, w tym interfejsu `List`, `ArrayList`, `LinkedList`.
- **Java 1.5:**
    - Wprowadzenie **generyków**, co pozwoliło na typowanie kolekcji, np. `List<String>`.
    - Dodano pętlę **for-each** do łatwej iteracji.
- **Java 1.6 i późniejsze:**
    - Optymalizacje wydajnościowe.
- **Java 8:**
    - Dodano metody domyślne w interfejsach.
    - Wprowadzenie **strumieni** (`stream()`), co umożliwia operacje funkcyjne na kolekcjach.
- **Java 9:**
    - Metody statyczne do tworzenia niemodyfikowalnych list: `List.of()`.

**Przykład (Java 9 i późniejsze):**

```java
List<String> immutableList = List.of("A", "B", "C");
```

---

## Najlepsze praktyki

- **Typuj do interfejsu:**

  ```java
  List<String> list = new ArrayList<>();
  ```

- **Unikaj `Vector` i `Stack`:**

    - Są synchronizowane i zazwyczaj niepotrzebnie spowalniają działanie.
    - Użyj `ArrayList` lub `Deque` w zależności od potrzeb.

- **Wybierz odpowiednią implementację:**

    - **`ArrayList`:** Gdy potrzebujesz szybkiego dostępu do elementów.
    - **`LinkedList`:** Gdy często wstawiasz/usuwasz elementy w środku listy.

- **Unikaj modyfikacji listy podczas iteracji:**

    - Może to spowodować `ConcurrentModificationException`.
    - Jeśli musisz modyfikować listę podczas iteracji, użyj `ListIterator`.

- **Korzystaj z metod z Javy 8 i nowszych:**

    - Używaj `stream()` do przetwarzania danych w sposób deklaratywny.

---

## Przykłady kodu

### Przykład 1: Użycie `ArrayList`

```java
List<String> names = new ArrayList<>();
names.add("Anna");
names.add("Bob");
names.add("Charlie");

// Dostęp do elementu
String secondName = names.get(1); // "Bob"

// Iteracja po elementach
for (String name : names) {
    System.out.println(name);
}
```

### Przykład 2: Użycie `LinkedList` jako kolejki

```java
LinkedList<String> queue = new LinkedList<>();
queue.add("First");
queue.add("Second");
queue.add("Third");

// Usuwanie z początku kolejki
String first = queue.removeFirst(); // "First"
```

### Przykład 3: Iteracja z modyfikacją listy

```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
ListIterator<Integer> iterator = numbers.listIterator();

while (iterator.hasNext()) {
    Integer number = iterator.next();
    if (number % 2 == 0) {
        iterator.remove(); // Bezpieczne usuwanie podczas iteracji
    }
}

System.out.println(numbers); // [1, 3, 5]
```

---

## Pytania rekrutacyjne

### Pytania podstawowe

1. **Jakie są główne cechy interfejsu `List` w Javie?**
    - Uporządkowana kolekcja, przechowuje duplikaty, zapewnia dostęp przez indeks i pozwala na dynamiczne modyfikacje
      elementów.

2. **Jaka jest różnica między `ArrayList` a `LinkedList`?**
    - `ArrayList`: szybki dostęp losowy, wolniejsze wstawianie/usuwanie w środku.
    - `LinkedList`: szybsze wstawianie/usuwanie na początku, wolniejszy dostęp losowy.

3. **Co to jest `ListIterator` i kiedy należy go używać?**
    - Rozszerzenie `Iterator` umożliwiające iterację w obu kierunkach i modyfikacje listy podczas iteracji.

4. **Jakie metody są unikalne dla interfejsu `List` w porównaniu z `Collection`?**
    - `get(int index)`, `set(int index, E element)`, `add(int index, E element)`, `remove(int index)`,
      `subList(int fromIndex, int toIndex)`.

5. **Czy `List` może przechowywać duplikaty?**
    - Tak, `List` pozwala na duplikaty.

### Pytania zaawansowane

1. **Kiedy użyć `ArrayList`, a kiedy `LinkedList`?**
    - `ArrayList`: szybki dostęp losowy, operacje na końcu.
    - `LinkedList`: częste wstawianie/usuwanie w środku.

2. **Jak działa `subList()` i jakie są pułapki?**
    - `subList()` zwraca widok części listy, a modyfikacje wprowadzone w `subList` wpływają na oryginalną listę. Zmiany
      dokonane w oryginalnej liście poza zakresem `subList` (np. dodawanie/usuwanie elementów bezpośrednio w oryginalnej
      liście) mogą wywołać `ConcurrentModificationException`, jeśli w tym czasie korzystasz z `subList`.

3. **Co to jest `ConcurrentModificationException`?**
    - Występuje, gdy modyfikujesz listę podczas iteracji bez użycia iteratora.

4. **Jakie korzyści dają generyki w kolekcjach?**
    - Bezpieczeństwo typów, czytelność, brak potrzeby rzutowania.

5. **Jakie są różnice między synchronizacją a niesynchronizacją w `List`?**
    - Synchronizowane listy są bezpieczne wątkowo, ale wolniejsze. Alternatywy to `Collections.synchronizedList()` i
      `CopyOnWriteArrayList`.

---
