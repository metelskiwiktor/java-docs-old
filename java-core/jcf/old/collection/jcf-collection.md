# Java Collections Framework: Interfejs `Collection`

## Spis treści
1. [Wprowadzenie](#wprowadzenie)
2. [Ogólny opis interfejsu `Collection`](#ogólny-opis-interfejsu-collection)
3. [Szczegółowe omówienie metod](#szczegółowe-omówienie-metod)
    - [Podstawowe operacje](#podstawowe-operacje)
    - [Operacje grupowe](#operacje-grupowe)
    - [Iteracja po elementach](#iteracja-po-elementach)
4. [Rozszerzenia interfejsu `Collection`](#rozszerzenia-interfejsu-collection)
    - [`List`](#list)
    - [`Set`](#set)
    - [`Queue` i `Deque`](#queue-i-deque)
5. [Implementacje interfejsu `Collection`](#implementacje-interfejsu-collection)
    - [`ArrayList`](#arraylist)
    - [`HashSet`](#hashset)
    - [`LinkedList`](#linkedlist)
6. [Uwzględnienie wersji Javy](#uwzględnienie-wersji-javy)
7. [Najlepsze praktyki](#najlepsze-praktyki)
8. [Przykłady kodu](#przykłady-kodu)
9. [Pytania rekrutacyjne](#pytania-rekrutacyjne)
    - [Pytania podstawowe](#pytania-podstawowe)
    - [Pytania zaawansowane](#pytania-zaawansowane)

---

## Wprowadzenie

Interfejs `Collection` jest centralnym elementem **Java Collections Framework**. Służy jako korzeń hierarchii kolekcji i definiuje podstawowe operacje dostępne dla wszystkich kolekcji w Javie. Zrozumienie interfejsu `Collection` jest kluczowe dla efektywnego korzystania z kolekcji i pisania elastycznego, modularnego kodu.

---

## Ogólny opis interfejsu `Collection`

Interfejs `Collection` znajduje się w pakiecie `java.util` i jest podstawowym interfejsem, z którego wywodzą się bardziej wyspecjalizowane interfejsy, takie jak `List`, `Set` czy `Queue`.

**Hierarchia klas:**

```
java.lang.Object
   ↳ java.util.Collection<E>
       ↳ java.util.List<E>
       ↳ java.util.Set<E>
       ↳ java.util.Queue<E>
```

**Główne cechy:**

- **Uniwersalność:** Definiuje podstawowy zestaw operacji dla kolekcji.
- **Elastyczność:** Może być używany z różnymi typami kolekcji poprzez polimorfizm.
- **Podstawowe operacje:** Dodawanie, usuwanie, sprawdzanie zawartości, iteracja.

**Kluczowe punkty:**

- **Nie pozwala na dostęp pozycyjny:** W przeciwieństwie do `List`, `Collection` nie definiuje metod opartych na indeksach.
- **Nie gwarantuje kolejności:** Kolejność elementów może być nieokreślona, chyba że implementacja stanowi inaczej.

---

## Szczegółowe omówienie metod

### Podstawowe operacje

- **`int size()`**
    - Zwraca liczbę elementów w kolekcji.

- **`boolean isEmpty()`**
    - Sprawdza, czy kolekcja jest pusta.

- **`boolean contains(Object o)`**
    - Sprawdza, czy kolekcja zawiera określony element.

- **`boolean add(E e)`**
    - Dodaje element do kolekcji. W niektórych kolekcjach (np. `Set`) może nie dodać duplikatu.

- **`boolean remove(Object o)`**
    - Usuwa pojedyncze wystąpienie określonego elementu.

### Operacje grupowe

- **`boolean containsAll(Collection<?> c)`**
    - Sprawdza, czy kolekcja zawiera wszystkie elementy z innej kolekcji.

- **`boolean addAll(Collection<? extends E> c)`**
    - Dodaje wszystkie elementy z innej kolekcji.

- **`boolean removeAll(Collection<?> c)`**
    - Usuwa wszystkie elementy, które są zawarte w innej kolekcji.

- **`boolean retainAll(Collection<?> c)`**
    - Zachowuje tylko te elementy, które są zawarte w innej kolekcji.

- **`void clear()`**
    - Usuwa wszystkie elementy z kolekcji.

### Iteracja po elementach

- **`Iterator<E> iterator()`**
    - Zwraca iterator pozwalający na przeglądanie elementów kolekcji.

- **Od Javy 8:**

    - **`default Stream<E> stream()`**
        - Zwraca strumień sekwencyjny elementów.

    - **`default Stream<E> parallelStream()`**
        - Zwraca strumień równoległy elementów.

---

## Rozszerzenia interfejsu `Collection`

Interfejs `Collection` jest rozszerzany przez kilka bardziej wyspecjalizowanych interfejsów.

### `List`

- **Opis:** Uporządkowana kolekcja pozwalająca na dostęp pozycyjny.
- **Cechy:**
    - Dopuszcza duplikaty.
    - Elementy są indeksowane.

### `Set`

- **Opis:** Kolekcja, która nie pozwala na duplikaty.
- **Cechy:**
    - Brak gwarancji co do kolejności elementów (chyba że użyje się `LinkedHashSet` lub `TreeSet`).

### `Queue` i `Deque`

- **`Queue`:**
    - Kolekcja reprezentująca kolejkę (FIFO).
- **`Deque`:**
    - Dwustronna kolejka, pozwala na operacje na obu końcach.

---

## Implementacje interfejsu `Collection`

### `ArrayList`

- **Implementuje:** `List`
- **Opis:** Dynamiczna tablica pozwalająca na szybki dostęp pozycyjny.
- **Kiedy używać:** Gdy potrzebny jest szybki dostęp do elementów i częste dodawanie na końcu.

### `HashSet`

- **Implementuje:** `Set`
- **Opis:** Zbiór oparty na tablicy haszującej.
- **Cechy:**
    - Nie gwarantuje kolejności elementów.
    - Szybkie operacje dodawania, usuwania i sprawdzania zawartości.

### `LinkedList`

- **Implementuje:** `List`, `Deque`
- **Opis:** Dwukierunkowa lista powiązana.
- **Cechy:**
    - Szybkie wstawianie i usuwanie na początku i końcu.
    - Może być używana jako stos lub kolejka.

---

## Uwzględnienie wersji Javy

- **Java 1.2:** Wprowadzenie interfejsu `Collection` i podstawowych implementacji.
- **Java 1.5:**
    - Wprowadzenie **generyków**, umożliwiających typowanie kolekcji.
    - Dodanie pętli **for-each**.
- **Java 8:**
    - Dodanie metod domyślnych, takich jak `stream()` i `parallelStream()`.
- **Java 9:**
    - Wprowadzenie metod statycznych do tworzenia niemodyfikowalnych kolekcji: `List.of()`, `Set.of()`, `Map.of()`.

**Przykład (Java 9 i późniejsze):**

```java
Collection<String> immutableCollection = List.of("A", "B", "C");
```

---

## Najlepsze praktyki

- **Typuj do interfejsu:**

  ```java
  Collection<String> collection = new ArrayList<>();
  ```

- **Używaj odpowiedniej implementacji:**
    - Wybierz implementację w zależności od potrzeb (np. `HashSet` dla unikalnych elementów, `ArrayList` dla szybkiego dostępu).

- **Unikaj modyfikacji kolekcji podczas iteracji:**
    - Może to prowadzić do `ConcurrentModificationException`.
    - Jeśli musisz modyfikować kolekcję podczas iteracji, użyj iteratora i jego metod `remove()`.

- **Korzystaj z metod z Javy 8 i nowszych:**
    - Używaj `stream()` do przetwarzania danych w sposób deklaratywny.

- **Twórz niemodyfikowalne kolekcje, gdy to możliwe:**
    - Zapewnia to bezpieczeństwo danych i może zapobiec błędom.

---

## Przykłady kodu

### Przykład 1: Użycie `Collection` z `ArrayList`

```java
Collection<String> fruits = new ArrayList<>();
fruits.add("Apple");
fruits.add("Banana");
fruits.add("Orange");

// Sprawdzanie zawartości
if (fruits.contains("Apple")) {
    System.out.println("Collection contains Apple");
}

// Iteracja po elementach
for (String fruit : fruits) {
    System.out.println(fruit);
}
```

### Przykład 2: Operacje grupowe

```java
Collection<String> collection1 = new HashSet<>();
collection1.add("A");
collection1.add("B");
collection1.add("C");

Collection<String> collection2 = new HashSet<>();
collection2.add("B");
collection2.add("C");
collection2.add("D");

// Różnica zbiorów
collection1.removeAll(collection2); // collection1 zawiera teraz tylko "A"
```

### Przykład 3: Korzystanie z `stream()`

```java
Collection<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Sumowanie liczb parzystych
int sum = numbers.stream()
                 .filter(n -> n % 2 == 0)
                 .mapToInt(Integer::intValue)
                 .sum();

System.out.println("Sum of even numbers: " + sum); // 6
```

---

## Pytania rekrutacyjne

### Pytania podstawowe

1. **Co to jest interfejs `Collection` i jakie jest jego miejsce w hierarchii kolekcji Javy?**
    - `Collection` to podstawowy interfejs w Java Collections Framework, definiujący podstawowe operacje dla wszystkich kolekcji. Jest korzeniem hierarchii, z którego dziedziczą bardziej wyspecjalizowane interfejsy, takie jak `List`, `Set` i `Queue`.

2. **Jakie są podstawowe metody dostępne w interfejsie `Collection`?**
    - Podstawowe metody to: `add(E e)`, `remove(Object o)`, `size()`, `isEmpty()`, `contains(Object o)`, `iterator()`, `addAll(Collection<? extends E> c)`, `removeAll(Collection<?> c)`, `retainAll(Collection<?> c)`, `clear()`.

3. **Czym różni się interfejs `Collection` od `Map`?**
    - `Collection` reprezentuje grupę elementów, podczas gdy `Map` reprezentuje zbiór par klucz-wartość. `Map` nie dziedziczy po `Collection` i nie jest bezpośrednio częścią hierarchii `Collection Framework`.

4. **Czy `Collection` może przechowywać duplikaty elementów?**
    - Tak, większość implementacji `Collection`, takich jak `List`, pozwala na przechowywanie duplikatów. Wyjątkiem są implementacje takie jak `Set`, które nie dopuszczają duplikatów.

5. **Jakie są różnice między `Collection` a jej rozszerzeniami, takimi jak `List`, `Set`, `Queue`?**
    - `List` jest uporządkowaną kolekcją z dostępem przez indeks, `Set` jest nieuporządkowaną kolekcją bez duplikatów, a `Queue` jest kolekcją działającą na zasadzie FIFO. Każdy z tych interfejsów dodaje specyficzne funkcje do podstawowych operacji `Collection`.

### Pytania zaawansowane

1. **W jaki sposób interfejs `Collection` wpływa na polimorfizm w Javie?**
    - `Collection` umożliwia pisanie kodu niezależnego od konkretnej implementacji kolekcji, co pozwala na łatwe podmiany implementacji dzięki polimorfizmowi.

2. **Jak działają metody domyślne w interfejsie `Collection` wprowadzone w Javie 8 i jakie są ich zalety?**
    - Metody domyślne pozwalają na dodawanie nowych metod do interfejsu bez łamania istniejących implementacji. Ułatwiają rozszerzanie funkcjonalności interfejsów i promują użycie strumieni.

3. **Wyjaśnij, jak działa metoda `removeIf(Predicate<? super E> filter)` i podaj przykład jej użycia.**
    - `removeIf` usuwa wszystkie elementy spełniające warunek określony przez predykat. Przykład:
      ```java
      collection.removeIf(e -> e.isEmpty());
      ```

4. **Jakie są konsekwencje modyfikacji kolekcji podczas iteracji i jak można tego bezpiecznie dokonać?**
    - Modyfikacja kolekcji podczas iteracji może spowodować `ConcurrentModificationException`. Bezpieczne modyfikacje można przeprowadzić używając `Iterator` i jego metod `remove()`, lub używając `ListIterator`.

5. **Omów różnice między metodami `removeAll()`, `retainAll()` i `containsAll()`.**
    - `removeAll` usuwa wszystkie elementy kolekcji, które są również w innej kolekcji. `retainAll` zachowuje tylko te elementy, które są w innej kolekcji. `containsAll` sprawdza, czy kolekcja zawiera wszystkie elementy z innej kolekcji.

---
