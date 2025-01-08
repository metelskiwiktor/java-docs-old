# Java Collections Framework: Klasa `LinkedList`

## Spis treści
1. [Wprowadzenie](#wprowadzenie)
2. [Ogólny opis klasy `LinkedList`](#ogólny-opis-klasy-linkedlist)
3. [Szczegółowe omówienie działania](#szczegółowe-omówienie-działania)
    - [Struktura wewnętrzna](#struktura-wewnętrzna)
    - [Dwa rodzaje implementacji](#dwa-rodzaje-implementacji)
4. [Główne metody](#główne-metody)
    - [Dodawanie elementów](#dodawanie-elementów)
    - [Dostęp do elementów](#dostęp-do-elementów)
    - [Usuwanie elementów](#usuwanie-elementów)
    - [Operacje na obu końcach](#operacje-na-obu-końcach)
    - [Iteracja po elementach](#iteracja-po-elementach)
5. [Porównanie z innymi implementacjami `List`](#porównanie-z-innymi-implementacjami-list)
    - [`LinkedList` vs `ArrayList`](#linkedlist-vs-arraylist)
6. [Uwzględnienie wersji Javy](#uwzględnienie-wersji-javy)
7. [Najlepsze praktyki](#najlepsze-praktyki)
8. [Przykłady kodu](#przykłady-kodu)
9. [Pytania rekrutacyjne](#pytania-rekrutacyjne)
    - [Pytania podstawowe](#pytania-podstawowe)
    - [Pytania zaawansowane](#pytania-zaawansowane)

---

## Wprowadzenie

Klasa `LinkedList` jest jedną z głównych implementacji interfejsu `List` w **Java Collections Framework**. Oparta na dwukierunkowej liście powiązanej, oferuje unikalne cechy, które sprawiają, że jest odpowiednia dla specyficznych scenariuszy użycia, zwłaszcza tam, gdzie operacje wstawiania i usuwania elementów są częste. Dodatkowo, dzięki implementacji interfejsów `Deque` i `Queue`, `LinkedList` może być używana jako stos (LIFO) lub kolejka (FIFO).

---

## Ogólny opis klasy `LinkedList`

- **Pakiet:** `java.util`
- **Dziedziczenie i Implementacje:**

  ```
  java.lang.Object
     ↳ java.util.AbstractSequentialList<E>
         ↳ java.util.AbstractList<E>
             ↳ java.util.AbstractCollection<E>
                 ↳ java.util.Collection<E>
                     ↳ java.util.List<E>
                         ↳ java.util.LinkedList<E>
                             ↳ java.util.Deque<E>
                                 ↳ java.util.Queue<E>
  ```

- **Interfejsy implementowane:**
    - `List<E>`
    - `Deque<E>`
    - `Queue<E>`

**Główne cechy:**

- **Dwukierunkowa lista powiązana:** Każdy element (węzeł) zawiera odniesienia do poprzedniego i następnego elementu.
- **Uporządkowana kolekcja:** Elementy są przechowywane w kolejności ich wstawiania.
- **Duplikaty i wartości `null`:** `LinkedList` pozwala na przechowywanie duplikatów oraz elementów o wartości `null`.
- **Implementacja interfejsów `Deque` i `Queue`:** Umożliwia użycie `LinkedList` jako stosu (LIFO) lub kolejki (FIFO).

---

## Szczegółowe omówienie działania

### Struktura wewnętrzna

- **Węzeł (`Node`):**
    - Każdy węzeł zawiera:
        - **Dane (`item`):** Przechowywana wartość.
        - **Odnośnik do poprzedniego węzła (`prev`).**
        - **Odnośnik do następnego węzła (`next`).**
- **Pola klasowe:**
    - **`first`:** Referencja do pierwszego węzła listy.
    - **`last`:** Referencja do ostatniego węzła listy.
    - **`size`:** Aktualna liczba elementów w liście.
    - **`modCount`:** Licznik modyfikacji, używany do wykrywania zmian podczas iteracji.

### Rodzaje implementacji

Klasa `LinkedList` implementuje trzy interfejsy: `List`, `Deque` oraz `Queue`, co umożliwia jej wykorzystanie w różnych kontekstach. Każdy z tych interfejsów oferuje unikalne funkcjonalności:

1. **Jako `List`:**
    - **Dostęp przez indeks:**
        - Umożliwia szybki dostęp do elementów za pomocą indeksów.
        - Przykładowe metody: `get(int index)`, `set(int index, E element)`.
    - **Operacje na dowolnym miejscu:**
        - Obsługuje wstawianie, usuwanie oraz modyfikowanie elementów w dowolnym miejscu listy.
        - Przykładowe metody: `add(int index, E element)`, `remove(int index)`.

2. **Jako `Deque` (Double-Ended Queue):**
    - **Operacje na obu końcach:**
        - Umożliwia dodawanie i usuwanie elementów zarówno na początku, jak i na końcu listy.
        - Przykładowe metody: `addFirst(E e)`, `addLast(E e)`, `removeFirst()`, `removeLast()`.
    - **Elastyczność struktur danych:**
        - Może być używana jako stos (LIFO - Last-In-First-Out) lub jako kolejka dwustronna, umożliwiając bardziej zaawansowane operacje.

3. **Jako `Queue`:**
    - **Kolejka FIFO (First-In-First-Out):**
        - Implementuje standardowe operacje kolejki, gdzie elementy są dodawane na końcu i usuwane z początku.
        - Przykładowe metody: `offer(E e)`, `poll()`, `peek()`.
    - **Podstawowe operacje kolejki:**
        - Zapewnia podstawowe operacje dodawania i usuwania elementów zgodnie z zasadą pierwsze weszło, pierwsze wyszło.

#### Kluczowe Różnice między `Deque` a `Queue`:

- **Zakres operacji:**
    - **`Queue`:** Oferuje podstawowe operacje kolejki FIFO, takie jak `offer`, `poll`, `peek`.
    - **`Deque`:** Rozszerza `Queue`, dodając operacje na obu końcach listy, umożliwiając także implementację stosów (LIFO) oraz bardziej zaawansowanych kolejek dwustronnych.

- **Elastyczność:**
    - **`Queue`:** Skoncentrowana na modelu FIFO, odpowiednia do prostych kolejek.
    - **`Deque`:** Bardziej elastyczna, umożliwiająca modelowanie zarówno FIFO, jak i LIFO oraz innych zaawansowanych struktur danych.
---

## Główne metody

### Dodawanie elementów

- **`boolean add(E e)`**
    - Dodaje element na koniec listy.

- **`void add(int index, E element)`**
    - Wstawia element na określoną pozycję, przesuwając istniejące elementy.

- **`boolean offer(E e)`**
    - Dodaje element na końcu kolejki (`Queue`).

- **`void push(E e)`**
    - Dodaje element na początku stosu (`Deque`).

### Dostęp do elementów

- **`E get(int index)`**
    - Zwraca element na określonym indeksie.

- **`E peek()`**
    - Zwraca pierwszy element bez usuwania go.

- **`E peekFirst()`**
    - Zwraca pierwszy element listy.

- **`E peekLast()`**
    - Zwraca ostatni element listy.

### Usuwanie elementów

- **`E remove(int index)`**
    - Usuwa element na określonym indeksie, przesuwając pozostałe elementy.

- **`boolean remove(Object o)`**
    - Usuwa pierwsze wystąpienie określonego elementu.

- **`E poll()`**
    - Usuwa i zwraca pierwszy element kolejki.

- **`E pop()`**
    - Usuwa i zwraca pierwszy element stosu.

### Operacje na obu końcach

- **`void addFirst(E e)`**
    - Dodaje element na początku listy.

- **`void addLast(E e)`**
    - Dodaje element na końcu listy.

- **`E removeFirst()`**
    - Usuwa pierwszy element listy.

- **`E removeLast()`**
    - Usuwa ostatni element listy.

### Iteracja po elementach

- **`Iterator<E> iterator()`**
    - Zwraca iterator do iteracji po elementach od początku do końca.

- **`ListIterator<E> listIterator()`**
    - Zwraca listę iteratorów z możliwością iteracji w obu kierunkach.

- **`Iterator<E> descendingIterator()`**
    - Zwraca iterator do iteracji po elementach od końca do początku.

---

## Najlepsze praktyki

- **Typuj do interfejsu:**

  ```java
  List<String> list = new LinkedList<>();
  ```

- **Wybierz odpowiednią implementację:**
    - Używaj `LinkedList` tam, gdzie potrzebne są częste operacje wstawiania/usuwania na początku lub w środku listy.
    - Unikaj `LinkedList` tam, gdzie dominują operacje dostępu losowego (`get`), ponieważ są one wolniejsze niż w `ArrayList`.

- **Unikaj nadmiernego używania `LinkedList`:**
    - `LinkedList` ma większy narzut pamięciowy ze względu na przechowywanie referencji do poprzedniego i następnego elementu.
    - W większości przypadków `ArrayList` jest bardziej efektywną implementacją `List`.

- **Używaj metod specyficznych dla `Deque`, jeśli potrzebujesz stosu lub kolejki:**

  ```java
  Deque<String> deque = new LinkedList<>();
  deque.addFirst("First");
  deque.addLast("Last");
  ```

- **Zachowaj ostrożność podczas iteracji i modyfikacji listy:**
    - Używaj `ListIterator` do bezpiecznej modyfikacji listy podczas iteracji.

---

## Przykłady kodu

### Przykład 1: Podstawowe operacje

```java
List<String> animals = new LinkedList<>();

// Dodawanie elementów
animals.add("Dog");
animals.add("Cat");
animals.add("Elephant");

// Dostęp do elementu
String firstAnimal = animals.get(0); // "Dog"

// Modyfikacja elementu
animals.set(1, "Lion"); // Zmienia "Cat" na "Lion"

// Usuwanie elementu
animals.remove(2); // Usuwa "Elephant"

// Iteracja po elementach
for (String animal : animals) {
    System.out.println(animal);
}
```

### Przykład 2: Użycie `Deque` jako stosu

```java
Deque<Integer> stack = new LinkedList<>();

// Dodawanie elementów na stos
stack.push(1);
stack.push(2);
stack.push(3);

// Usuwanie elementów ze stosu
while (!stack.isEmpty()) {
    System.out.println(stack.pop()); // Wydrukuje 3, 2, 1
}
```

### Przykład 3: Użycie `ListIterator` do modyfikacji listy podczas iteracji

```java
List<String> colors = new LinkedList<>(Arrays.asList("Red", "Green", "Blue", "Yellow"));
ListIterator<String> iterator = colors.listIterator();

while (iterator.hasNext()) {
    String color = iterator.next();
    if (color.equals("Green")) {
        iterator.set("Purple"); // Zastępuje "Green" na "Purple"
    }
    if (color.equals("Blue")) {
        iterator.remove(); // Usuwa "Blue"
    }
}

System.out.println(colors); // [Red, Purple, Yellow]
```

### Przykład 4: Użycie `LinkedList` jako kolejki

```java
Queue<String> queue = new LinkedList<>();

// Dodawanie elementów do kolejki
queue.offer("First");
queue.offer("Second");
queue.offer("Third");

// Usuwanie elementów z kolejki
while (!queue.isEmpty()) {
    System.out.println(queue.poll()); // Wydrukuje "First", "Second", "Third"
}
```

---

## Pytania rekrutacyjne

### Pytania podstawowe

1. **Co to jest `LinkedList` i jakie są jej główne cechy?**
    - `LinkedList` to implementacja interfejsu `List` oparta na dwukierunkowej liście powiązanej. Umożliwia szybkie wstawianie i usuwanie elementów na początku lub w środku listy, a także może być używana jako stos lub kolejka dzięki implementacji interfejsów `Deque` i `Queue`.

2. **Jakie są różnice między `LinkedList` a `ArrayList`?**
    - `LinkedList` jest oparta na liście powiązanej, oferuje szybkie wstawianie/usuwanie elementów na początku i w środku listy, ale ma wolniejszy dostęp losowy (`get`). `ArrayList` jest oparta na dynamicznej tablicy, oferuje szybki dostęp losowy, ale ma wolniejsze wstawianie/usuwanie elementów w środku listy.

3. **Czy `LinkedList` jest synchronizowana?**
    - Nie, `LinkedList` nie jest synchronizowana i nie jest bezpieczna w środowiskach wielowątkowych bez dodatkowej synchronizacji.

4. **Jakie interfejsy implementuje `LinkedList`?**
    - `LinkedList` implementuje interfejsy `List`, `Deque` i `Queue`.

5. **Jakie metody są specyficzne dla `LinkedList` dzięki implementacji `Deque`?**
    - Metody takie jak `addFirst()`, `addLast()`, `removeFirst()`, `removeLast()`, `peekFirst()`, `peekLast()` umożliwiają operacje na obu końcach listy.

### Pytania zaawansowane

1. **Jak działa `LinkedList` pod względem złożoności czasowej operacji `get(int index)` i `add(int index, E element)`?**
    - Operacja `get(int index)` ma złożoność O(n), ponieważ wymaga przejścia przez listę od początku lub końca do określonego indeksu. Operacja `add(int index, E element)` również ma złożoność O(n) ze względu na konieczność znalezienia odpowiedniego miejsca w liście, ale samo wstawianie elementu jest O(1).

2. **Jakie są konsekwencje używania `LinkedList` zamiast `ArrayList` w kontekście pamięci?**
    - `LinkedList` zużywa więcej pamięci niż `ArrayList` ze względu na dodatkowe referencje do poprzedniego i następnego węzła w każdym elemencie listy.

3. **Wyjaśnij, jak działa metoda `descendingIterator()` w `LinkedList`.**
    - `descendingIterator()` zwraca iterator, który umożliwia iterację po elementach listy od końca do początku, co jest możliwe dzięki dwukierunkowej strukturze listy powiązanej.

4. **Jak `LinkedList` obsługuje modyfikacje listy podczas iteracji?**
    - Podobnie jak inne kolekcje, `LinkedList` używa pola `modCount` do śledzenia modyfikacji struktury listy. Jeśli lista jest modyfikowana poza iteratorem podczas iteracji, zostanie rzucony `ConcurrentModificationException`. Aby bezpiecznie modyfikować listę podczas iteracji, należy używać `ListIterator` i jego metod `add()`, `remove()`, `set()`.

5. **Czy `LinkedList` jest lepszym wyborem niż `ArrayList` do implementacji stosu? Dlaczego?**
    - `LinkedList` może być dobrym wyborem do implementacji stosu ze względu na efektywne operacje `push` i `pop` (dodawanie i usuwanie elementów na początku listy, co jest O(1)). Jednak `ArrayList` również może być używana do implementacji stosu i oferuje szybki dostęp losowy, co może być przydatne w niektórych scenariuszach. Wybór zależy od specyficznych wymagań aplikacji.

---
