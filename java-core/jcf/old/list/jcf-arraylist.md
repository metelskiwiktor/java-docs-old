# Java Collections Framework: Klasa `ArrayList`

## Spis treści
1. [Wprowadzenie](#wprowadzenie)
2. [Ogólny opis klasy `ArrayList`](#ogólny-opis-klasy-arraylist)
3. [Szczegółowe omówienie działania](#szczegółowe-omówienie-działania)
    - [Struktura wewnętrzna](#struktura-wewnętrzna)
    - [Pojemność i rozmiar](#pojemność-i-rozmiar)
    - [Automatyczne zwiększanie pojemności](#automatyczne-zwiększanie-pojemności)
4. [Główne metody](#główne-metody)
    - [Dodawanie elementów](#dodawanie-elementów)
    - [Dostęp do elementów](#dostęp-do-elementów)
    - [Usuwanie elementów](#usuwanie-elementów)
    - [Iteracja po elementach](#iteracja-po-elementach)
5. [Najlepsze praktyki](#najlepsze-praktyki)
6. [Przykłady kodu](#przykłady-kodu)
7. [Pytania rekrutacyjne](#pytania-rekrutacyjne)
    - [Pytania podstawowe](#pytania-podstawowe)
    - [Pytania zaawansowane](#pytania-zaawansowane)

---

## Wprowadzenie

Klasa `ArrayList` jest jedną z najczęściej używanych implementacji interfejsu `List` w Java Collections Framework.
Oparta na dynamicznej tablicy, zapewnia szybki dostęp do elementów i jest idealna dla aplikacji wymagających częstego
odczytu danych.

---

## Ogólny opis klasy `ArrayList`

- **Pakiet:** `java.util`
- **Dziedziczenie:**
    - `java.lang.Object`
        - `java.util.AbstractCollection<E>`
            - `java.util.AbstractList<E>`
                - `java.util.ArrayList<E>`

**Główne cechy:**

- **Dynamiczna tablica:** Automatycznie zarządza rozmiarem wewnętrznej tablicy.
- **Uporządkowana kolekcja:** Elementy są przechowywane w kolejności ich wstawiania.
- **Dostęp pozycyjny:** Szybki dostęp do elementów za pomocą indeksu.
- **Dopuszcza duplikaty i wartości `null`.**

---

## Szczegółowe omówienie działania

### Struktura wewnętrzna

- **Wewnętrzna tablica:** `Object[] elementData`
    - Przechowuje rzeczywiste elementy listy.
- **Pole `size`:**
    - Przechowuje aktualną liczbę elementów w `ArrayList`.
- **Pole `modCount`:** 
    - Licznik modyfikacji, używany do wykrywania zmian podczas iteracji.


### Pojemność i rozmiar

- **Pojemność (`capacity`):**
    - Rozmiar wewnętrznej tablicy `elementData`.
- **Rozmiar (`size`):**
    - Liczba rzeczywistych elementów przechowywanych w `ArrayList`.

### Automatyczne zwiększanie pojemności

- **Gdy dodawany jest nowy element i `size == capacity`:**
    - Pojemność jest zwiększana.
    - Nowa pojemność to zazwyczaj 1.5x poprzedniej
- **Proces zwiększania:**
    - Tworzona jest nowa tablica o większej pojemności.
    - Elementy są kopiowane do nowej tablicy.
    - Stara tablica jest usuwana przez Garbage Collector.

---

## Główne metody

### Dodawanie elementów

- **`boolean add(E e)`**
    - Dodaje element na koniec listy.
- **`void add(int index, E element)`**
    - Wstawia element na określoną pozycję, przesuwając istniejące elementy.

### Dostęp do elementów

- **`E get(int index)`**
    - Zwraca element na określonym indeksie.
- **`E set(int index, E element)`**
    - Zastępuje element na określonym indeksie nowym elementem.

### Usuwanie elementów

- **`E remove(int index)`**
    - Usuwa element na określonym indeksie, przesuwając pozostałe elementy.
- **`boolean remove(Object o)`**
    - Usuwa pierwsze wystąpienie określonego elementu.

### Iteracja po elementach

- **`Iterator<E> iterator()`**
    - Zwraca iterator do iteracji po elementach.
- **`ListIterator<E> listIterator()`**
    - Zwraca listę iteratorów z dodatkowymi funkcjami (np. iteracja wstecz).

---

## Najlepsze praktyki

- **Inicjowanie z początkową pojemnością:**
    - Jeśli wiesz, ile elementów będzie przechowywane, ustaw początkową pojemność, aby zminimalizować konieczność zwiększania tablicy.
  ```java
  List<Integer> numbers = new ArrayList<>(1000);
  ```

- **Typuj do interfejsu:**
    - Używaj interfejsu `List` jako typu zmiennej.
  ```java
  List<String> list = new ArrayList<>();
  ```

- **Unikaj modyfikacji podczas iteracji:**
    - Może to prowadzić do `ConcurrentModificationException`.
    - Jeśli musisz modyfikować listę podczas iteracji, użyj `ListIterator`.

---

## Przykłady kodu

### Przykład 1: Podstawowe operacje

```java
List<String> cities = new ArrayList<>();

// Dodawanie elementów
cities.add("Warszawa");
cities.add("Krakow");
cities.add("Gdansk");

// Dostęp do elementu
String firstCity = cities.get(0); // "Warsaw"

// Modyfikacja elementu
cities.set(1, "Wroclaw"); // Zmienia "Krakow" na "Wroclaw"

// Usuwanie elementu
cities.remove(2); // Usuwa "Gdansk"

// Iteracja po elementach
for (String city : cities) {
    System.out.println(city);
}
```

### Przykład 2: Inicjalizacja z początkową pojemnością

```java
List<Integer> numbers = new ArrayList<>(50); // Początkowa pojemność 50
```

### Przykład 3: Użycie `ListIterator`

```java
List<String> names = new ArrayList<>(Arrays.asList("Anna", "Bob", "Charlie"));
ListIterator<String> iterator = names.listIterator();

while (iterator.hasNext()) {
    String name = iterator.next();
    if (name.startsWith("B")) {
        iterator.remove(); // Bezpieczne usuwanie podczas iteracji
    }
}

System.out.println(names); // ["Anna", "Charlie"]
```

---

## Pytania rekrutacyjne

### Pytania podstawowe

1. **Co to jest `ArrayList` i jakie są jej główne cechy?**
    - `ArrayList` to implementacja interfejsu `List` oparta na dynamicznej tablicy. Zapewnia szybki dostęp do elementów
      za pomocą indeksu i automatycznie zarządza swoją pojemnością.

2. **Jak `ArrayList` zarządza swoją pojemnością wewnętrzną?**
    - `ArrayList` automatycznie zwiększa swoją pojemność, gdy dodawane są nowe elementy. Tworzy nową, większą tablicę i
      kopiuje do niej istniejące elementy.

3. **Jaka jest złożoność czasowa operacji `get(int index)` i `add(E element)` w `ArrayList`?**
    - Operacja `get(int index)` ma złożoność O(1). Operacja `add(E element)` na końcu listy ma złożoność amortyzowaną O(
      1).

4. **Czy `ArrayList` jest synchronizowana?**
    - Nie, `ArrayList` nie jest synchronizowana i nie jest bezpieczna w środowiskach wielowątkowych bez dodatkowej
      synchronizacji.

5. **Jak zainicjować `ArrayList` z początkową pojemnością? Dlaczego może to być użyteczne?**
    - Można użyć konstruktora `ArrayList(int initialCapacity)`. Jest to przydatne, gdy znasz z góry liczbę elementów,
      aby uniknąć kosztownych operacji zwiększania pojemności.

### Pytania zaawansowane

1. **Jak działa mechanizm automatycznego zwiększania pojemności w `ArrayList` i jakie są jego konsekwencje
   wydajnościowe?**
    - Gdy `ArrayList` osiąga swoją maksymalną pojemność, tworzy nową tablicę o większej pojemności (zazwyczaj 1.5x
      większą), kopiuje do niej istniejące elementy. Może to być kosztowne, jeśli występuje zbyt często.

2. **Jakie są różnice między `ArrayList` a `LinkedList` pod względem wydajności i zastosowań?**
    - `ArrayList`: szybki dostęp losowy (O(1)), wolniejsze wstawianie/usuwanie w środku listy (O(n)).
    - `LinkedList`: wolniejszy dostęp losowy (O(n)), szybsze wstawianie/usuwanie na początku i w środku listy (O(1)).

3. **Co to jest `modCount` w `ArrayList` i jak wpływa na iterację po liście?**
    - `modCount` śledzi modyfikacje struktury listy. Jeśli podczas iteracji `Iterator` wykryje zmianę `modCount`, rzuci
      `ConcurrentModificationException`.

4. **Czy tablica wewnątrz `ArrayList` jest typowana generycznie?**
    - Nie, tablica wewnątrz `ArrayList` nie jest typowana generycznie z powodu ograniczeń Javy dotyczących tworzenia
      tablic typowanych generycznie. Wewnętrznie używana jest tablica obiektów (`Object[]`), a rzutowanie typów
      następuje przy pobieraniu elementów.

5. **Jak zapewnić bezpieczeństwo wątkowe podczas korzystania z `ArrayList`?**
    - Można synchronizować dostęp ręcznie lub użyć `Collections.synchronizedList(new ArrayList<>())`. Alternatywnie,
      można użyć `CopyOnWriteArrayList` z pakietu `java.util.concurrent`.

