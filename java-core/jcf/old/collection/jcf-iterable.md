# Java Collections Framework: Interfejsy `Iterator` i `Iterable`

## Wprowadzenie

Interfejsy `Iterator` i `Iterable` są kluczowymi elementami **Java Collections Framework**. Umożliwiają iterację po elementach kolekcji w sposób ujednolicony i niezależny od konkretnej implementacji kolekcji. Dzięki nim możemy łatwo przeglądać, modyfikować i usuwać elementy z kolekcji podczas iteracji.

---

## Interfejs `Iterable`

- **Pakiet:** `java.lang`

**Opis:**

Interfejs `Iterable<T>` reprezentuje kolekcję, której elementy mogą być iterowane. Jest to interfejs funkcyjny zawierający jedną abstrakcyjną metodę:

- **`Iterator<T> iterator()`**

Implementacja tej metody pozwala na użycie obiektu w pętli **for-each**, co znacznie upraszcza kod.

**Przykład:**

```java
public interface Iterable<T> {
    Iterator<T> iterator();
}
```

**Zastosowanie:**

Każda klasa implementująca `Iterable` może być użyta w pętli for-each:

```java
List<String> names = Arrays.asList("Anna", "Bob", "Charlie");
for (String name : names) {
    System.out.println(name);
}
```

---

## Interfejs `Iterator`

- **Pakiet:** `java.util`

**Opis:**

Interfejs `Iterator<T>` zapewnia metody do sekwencyjnego przeglądania elementów kolekcji.

**Główne metody:**

1. **`boolean hasNext()`**
    - Sprawdza, czy są jeszcze elementy do iteracji.
2. **`T next()`**
    - Zwraca następny element w iteracji.
3. **`default void remove()`**
    - Usuwa ostatni zwrócony element z kolekcji (opcjonalna operacja).

**Przykład:**

```java
public interface Iterator<T> {
    boolean hasNext();
    T next();
    default void remove();
}
```

---

## Implementacja `Iterator` w różnych kolekcjach

Każda kolekcja w Java Collections Framework dostarcza własną implementację `Iterator`, dostosowaną do swojej struktury danych.

1. **`List` (np. `ArrayList`, `LinkedList`):**
    - Iteruje po elementach w kolejności ich indeksów.
    - Można użyć `ListIterator`, który rozszerza `Iterator` o dodatkowe metody.

2. **`Set` (np. `HashSet`, `TreeSet`):**
    - Iteracja nie gwarantuje określonej kolejności (chyba że używamy `LinkedHashSet` lub `TreeSet`).

3. **`Map`:**
    - Nie implementuje `Iterable` bezpośrednio, ale możemy iterować po jej kluczach, wartościach lub wpisach (`Map.Entry<K, V>`).

**Przykład iteracji po kluczach mapy:**

```java
Map<Integer, String> map = new HashMap<>();
map.put(1, "One");
map.put(2, "Two");

for (Integer key : map.keySet()) {
    System.out.println(key);
}
```

---

### Operacje `Iterator`

#### Metoda `hasNext()`

- **Opis:** Sprawdza, czy są jeszcze elementy do przeglądania.
- **Zastosowanie:** Używana w warunku pętli, aby kontrolować jej wykonanie.

**Przykład:**

```java
Iterator<String> iterator = names.iterator();
while (iterator.hasNext()) {
    // ...
}
```

#### Metoda `next()`

- **Opis:** Zwraca następny element w iteracji.
- **Zastosowanie:** Używana do pobierania kolejnych elementów podczas iteracji.

**Przykład:**

```java
while (iterator.hasNext()) {
    String name = iterator.next();
    System.out.println(name);
}
```

#### Metoda `remove()`

- **Opis:** Usuwa ostatni element zwrócony przez `next()` z kolekcji.
- **Zastosowanie:** Pozwala na bezpieczne usuwanie elementów podczas iteracji.

**Przykład:**

```java
while (iterator.hasNext()) {
    String name = iterator.next();
    if (name.startsWith("A")) {
        iterator.remove(); // Bezpieczne usuwanie podczas iteracji
    }
}
```

#### Metoda `forEachRemaining()`

- **Opis:** Wykonuje podaną akcję dla każdego pozostałego elementu w iteracji.
- **Zastosowanie:** Ułatwia przetwarzanie elementów bez konieczności ręcznego sprawdzania `hasNext()` i wywoływania `next()`.

**Przykład:**

```java
Iterator<String> iterator = names.iterator();
iterator.forEachRemaining(name -> System.out.println(name));
```

---

## Tworzenie własnego iteratora

Jeśli tworzymy własną kolekcję lub strukturę danych, możemy zaimplementować interfejs `Iterable` i dostarczyć własną implementację `Iterator`.

**Przykład:**

```java
public class MyCollection implements Iterable<String> {
    private String[] data = {"A", "B", "C"};

    @Override
    public Iterator<String> iterator() {
        return new MyIterator();
    }

    private class MyIterator implements Iterator<String> {
        private int index = 0;

        @Override
        public boolean hasNext() {
            return index < data.length;
        }

        @Override
        public String next() {
            if (!hasNext()) throw new NoSuchElementException();
            return data[index++];
        }
    }
}
```

**Użycie:**

```java
MyCollection collection = new MyCollection();
for (String item : collection) {
    System.out.println(item);
}
```

---

## Pułapki i błędy związane z iteracją

### `ConcurrentModificationException`

- **Opis:** Rzucany, gdy modyfikujemy kolekcję podczas iteracji w sposób niebezpieczny.
- **Przyczyna:** Większość iteratorów jest **fail-fast** – wykrywają zmiany strukturalne w kolekcji i rzucają wyjątek.

**Przykład błędnego kodu:**

```java
List<String> names = new ArrayList<>(Arrays.asList("Anna", "Bob", "Charlie"));
for (String name : names) {
    if (name.equals("Bob")) {
        names.remove(name); // Spowoduje ConcurrentModificationException
    }
}
```

**Poprawne rozwiązanie:**

Użyj `Iterator` i jego metody `remove()`:

```java
Iterator<String> iterator = names.iterator();
while (iterator.hasNext()) {
    String name = iterator.next();
    if (name.equals("Bob")) {
        iterator.remove(); // Bezpieczne usuwanie podczas iteracji
    }
}
```

### Pomijanie wywołania metody `next()`

- **Opis:** Jeśli podczas iteracji z użyciem `Iterator` nie wywołamy metody `next()`, pętla może stać się nieskończona lub nie przetworzyć żadnych elementów.
- **Przyczyna:** Metoda `next()` przesuwa iterator do następnego elementu i zwraca bieżący element. Bez jej wywołania iterator pozostaje na tej samej pozycji, a `hasNext()` nadal zwraca `true`.

**Przykład błędnego kodu:**

```java
Iterator<String> iterator = names.iterator();
while (iterator.hasNext()) {
    // Brak wywołania iterator.next()
    System.out.println("Element istnieje");
    // Pętla stanie się nieskończona, ponieważ iterator nie przesuwa się dalej
}
```

**Poprawne rozwiązanie:**

Upewnij się, że w każdym przebiegu pętli wywoływana jest metoda `next()`:

```java
while (iterator.hasNext()) {
    String name = iterator.next();
    System.out.println(name);
}
```

### Bezpieczeństwo wątkowe

- **Opis:** Standardowe kolekcje i ich iteratory nie są bezpieczne wątkowo.
- **Rozwiązanie:** Użyj synchronizacji lub kolekcji z pakietu `java.util.concurrent`, np. `ConcurrentHashMap`.

---


## Przykłady kodu

### Przykład 1: Iteracja po `ArrayList`

```java
List<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));
Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

### Przykład 2: Usuwanie elementów podczas iteracji

```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
Iterator<Integer> iterator = numbers.iterator();

while (iterator.hasNext()) {
    Integer number = iterator.next();
    if (number % 2 == 0) {
        iterator.remove(); // Usuwa liczby parzyste
    }
}

System.out.println(numbers); // [1, 3, 5]
```

### Przykład 3: Tworzenie własnego iteratora

Patrz sekcja [Tworzenie własnego iteratora](#tworzenie-własnego-iteratora).

---

## Pytania rekrutacyjne

### Pytania podstawowe

1. **Co to jest interfejs `Iterator` i jakie są jego główne metody?**
    - Interfejs `Iterator` umożliwia sekwencyjne przeglądanie elementów kolekcji. Główne metody to `hasNext()`, `next()` i `remove()`.

2. **Jaka jest różnica między `Iterator` a `Iterable`?**
    - `Iterable` to interfejs, który reprezentuje kolekcję dostarczającą `Iterator` poprzez metodę `iterator()`. `Iterator` to interfejs służący do iteracji po elementach tej kolekcji.

3. **Jakie kolekcje w Javie implementują interfejs `Iterable`?**
    - Wszystkie kolekcje z Java Collections Framework implementujące `Collection`.

4. **Dlaczego nie powinno się modyfikować kolekcji podczas iteracji przy użyciu pętli for-each?**
    - Ponieważ może to spowodować `ConcurrentModificationException`. Bezpieczne modyfikacje powinny być wykonywane bezpośrednio za pomocą iteratora.

5. **Jak bezpiecznie usunąć element z kolekcji podczas iteracji?**
    - Używając metody `remove()` dostarczonej przez `Iterator`.

### Pytania zaawansowane

1. **Co to jest iterator typu fail-fast i fail-safe?**
    - **Fail-fast:** Wykrywa zmiany w kolekcji podczas iteracji i rzuca `ConcurrentModificationException`.
    - **Fail-safe:** Pozwala na modyfikacje kolekcji podczas iteracji bez rzucania wyjątku (np. iterator na `CopyOnWriteArrayList`).

2. **Jak zaimplementować własny iterator dla niestandardowej kolekcji?**
    - Implementując interfejs `Iterator` w wewnętrznej klasie i dostarczając go poprzez metodę `iterator()` w klasie implementującej `Iterable`.

3. **Czy można używać metody `remove()` iteratora na kolekcjach niemodyfikowalnych?**
    - Nie, rzuci ona wtedy `UnsupportedOperationException`.

4. **Jak działa metoda `forEachRemaining()` w interfejsie `Iterator` i jakie są jej zalety?**
   - Metoda `forEachRemaining()` wykonuje podaną akcję dla wszystkich elementów, które nie zostały jeszcze przetworzone przez iterator, począwszy od bieżącej pozycji aż do końca kolekcji (elementy, które nie zostały jeszcze zwrócone przez wcześniejsze wywołania metody `next()`). Dzięki tej metodzie możemy wydajnie przetworzyć pozostałe elementy bez potrzeby ręcznego iterowania za pomocą pętli.

5. **Czy iterator jest bezpieczny w środowisku wielowątkowym?**
    - Standardowe iteratory nie są bezpieczne wątkowo. Należy używać synchronizacji lub kolekcji współbieżnych.
