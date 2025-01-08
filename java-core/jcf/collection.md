## **Interfejs Collection**

### **1. Wprowadzenie do interfejsu Collection**

Interfejs `Collection` jest podstawowym elementem Java Collection Framework (JCF) i służy do reprezentowania grup
obiektów jako pojedynczej jednostki. Jest bazowym interfejsem, z którego wywodzą się bardziej wyspecjalizowane
interfejsy, takie jak `List`, `Set` i `Queue`. `Collection` definiuje ogólne operacje na danych, takie jak dodawanie,
usuwanie i przeglądanie elementów, a także dostarcza spójne API dla różnych struktur danych.

### **2. Kluczowe cechy interfejsu Collection**

- **Spójność API**: Wszystkie kolekcje w JCF korzystają z ujednoliconego zestawu metod do zarządzania danymi.
- **Obsługa typów generycznych**: `Collection` umożliwia określenie typu elementów przechowywanych w kolekcji, co
  zwiększa bezpieczeństwo typów.
- **Operacje na danych**: Wspiera podstawowe operacje, takie jak dodawanie, usuwanie, przeglądanie oraz sprawdzanie
  obecności elementów w kolekcji.
- **Rozszerza `Iterable`**: Interfejs `Collection` rozszerza `Iterable`, co pozwala na iterację po jego elementach za
  pomocą pętli `for-each`. Interfejs `Iterable` definiuje metodę `iterator()`, która zwraca obiekt `Iterator`
  umożliwiający przeglądanie elementów kolekcji w uporządkowany sposób.

### **3. Metody dostępne w interfejsie Collection**

Poniżej znajduje się kod źródłowy interfejsu `Collection` z opisem każdej metody:

```java
public interface Collection<E> extends Iterable<E> {

    /**
     * Dodaje element do kolekcji.
     *
     * @param e element do dodania.
     * @return true, jeśli kolekcja zmieniła się w wyniku tej operacji.
     *         (Niektóre kolekcje, np. niezmienne, zawsze zwrócą false).
     * @throws UnsupportedOperationException jeśli operacja dodania nie jest obsługiwana.
     * @throws ClassCastException jeśli typ elementu nie jest zgodny z kolekcją.
     * @throws NullPointerException jeśli kolekcja nie pozwala na null, a dodano null.
     * @throws IllegalArgumentException jeśli element narusza specyficzne ograniczenia kolekcji.
     * @throws IllegalStateException jeśli element nie może zostać dodany z powodów specyficznych dla kolekcji.
     */
    boolean add(E e);

    /**
     * Dodaje wszystkie elementy z podanej kolekcji do tej kolekcji.
     *
     * @param c kolekcja zawierająca elementy do dodania.
     * @return true, jeśli kolekcja zmieniła się w wyniku tej operacji.
     * @throws UnsupportedOperationException jeśli operacja dodania nie jest obsługiwana.
     * @throws ClassCastException jeśli typ elementów w kolekcji `c` nie jest zgodny z bieżącą kolekcją.
     * @throws NullPointerException jeśli kolekcja `c` lub jej elementy są null, a bieżąca kolekcja nie pozwala na null.
     * @throws IllegalArgumentException jeśli elementy z kolekcji `c` naruszają ograniczenia tej kolekcji.
     * @throws IllegalStateException jeśli elementy nie mogą zostać dodane z powodów specyficznych dla kolekcji.
     */
    boolean addAll(Collection<? extends E> c);

    /**
     * Usuwa pojedyncze wystąpienie podanego elementu z kolekcji, jeśli istnieje.
     *
     * @param o element do usunięcia.
     * @return true, jeśli element został usunięty, false w przeciwnym razie.
     * @throws UnsupportedOperationException jeśli operacja usunięcia nie jest obsługiwana.
     * @throws ClassCastException jeśli typ elementu nie jest zgodny z kolekcją.
     * @throws NullPointerException jeśli kolekcja nie pozwala na null, a próbujesz usunąć null.
     */
    boolean remove(Object o);

    /**
     * Sprawdza, czy kolekcja zawiera podany element.
     *
     * @param o element, którego obecność należy sprawdzić.
     * @return true, jeśli element jest w kolekcji, false w przeciwnym razie.
     * @throws ClassCastException jeśli typ elementu nie jest zgodny z kolekcją.
     * @throws NullPointerException jeśli kolekcja nie pozwala na null, a próbujesz sprawdzić obecność null.
     */
    boolean contains(Object o);

    /**
     * Zwraca liczbę elementów w kolekcji.
     *
     * @return liczba elementów w kolekcji. Jeśli kolekcja jest pusta, zwróci 0.
     *         Jeśli kolekcja zawiera więcej elementów niż {@code Integer.MAX_VALUE},
     *         zwraca {@code Integer.MAX_VALUE}.
     */
    int size();

    /**
     * Sprawdza, czy kolekcja jest pusta.
     *
     * @return true, jeśli kolekcja nie zawiera elementów, false w przeciwnym razie.
     */
    boolean isEmpty();

    /**
     * Usuwa wszystkie elementy z kolekcji.
     *
     * @throws UnsupportedOperationException jeśli operacja usuwania wszystkich elementów nie jest obsługiwana.
     */
    void clear();

    /**
     * Usuwa wszystkie elementy tej kolekcji, które znajdują się w podanej kolekcji.
     *
     * @param c kolekcja zawierająca elementy do usunięcia.
     * @return true, jeśli kolekcja zmieniła się w wyniku tej operacji.
     * @throws UnsupportedOperationException jeśli operacja usuwania nie jest obsługiwana.
     * @throws ClassCastException jeśli typ elementów w `c` nie jest zgodny z kolekcją.
     * @throws NullPointerException jeśli `c` lub jej elementy są null, a kolekcja nie pozwala na null.
     */
    boolean removeAll(Collection<?> c);

    /**
     * Zachowuje tylko te elementy w tej kolekcji, które znajdują się w podanej kolekcji.
     *
     * @param c kolekcja zawierająca elementy, które mają być zachowane.
     * @return true, jeśli kolekcja zmieniła się w wyniku tej operacji.
     * @throws UnsupportedOperationException jeśli operacja zachowania elementów nie jest obsługiwana.
     * @throws ClassCastException jeśli typ elementów w `c` nie jest zgodny z kolekcją.
     * @throws NullPointerException jeśli `c` lub jej elementy są null, a kolekcja nie pozwala na null.
     */
    boolean retainAll(Collection<?> c);

    /**
     * Zwraca tablicę zawierającą wszystkie elementy w tej kolekcji.
     *
     * @return tablica zawierająca wszystkie elementy w kolekcji.
     */
    Object[] toArray();

    /**
     * Zwraca tablicę zawierającą wszystkie elementy w tej kolekcji, w typie określonym przez podany argument.
     *
     * @param <T> typ elementów w zwróconej tablicy.
     * @param a tablica, która ma zostać użyta do przechowywania elementów, jeśli jej rozmiar jest wystarczający.
     *          Jeśli tablica jest za mała, zostanie utworzona nowa tablica tego samego typu co `a`, z odpowiednim rozmiarem.
     * @return tablica zawierająca elementy z kolekcji.
     * @throws ArrayStoreException jeśli typ elementów w kolekcji nie jest zgodny z typem tablicy.
     * @throws NullPointerException jeśli podana tablica jest null.
     */
    <T> T[] toArray(T[] a);

    /**
     * Zwraca tablicę zawierającą elementy w kolekcji, tworzoną za pomocą podanej funkcji generatora.
     *
     * @param generator funkcja generująca tablicę.
     * @return tablica zawierająca elementy z kolekcji.
     * @throws NullPointerException jeśli generator jest null.
     */
    default <T> T[] toArray(IntFunction<T[]> generator) {
        return toArray(generator.apply(0));
    }

    /**
     * Usuwa elementy spełniające podany warunek.
     *
     * @param filter predykat określający elementy do usunięcia.
     * @return true, jeśli usunięto co najmniej jeden element.
     * @throws UnsupportedOperationException jeśli operacja usuwania nie jest obsługiwana.
     * @throws NullPointerException jeśli predykat jest null.
     */
    default boolean removeIf(Predicate<? super E> filter) {
        Objects.requireNonNull(filter);
        boolean removed = false;
        for (Iterator<E> it = iterator(); it.hasNext(); ) {
            if (filter.test(it.next())) {
                it.remove();
                removed = true;
            }
        }
        return removed;
    }
    
    /**
     * Sprawdza, czy wszystkie elementy podanej kolekcji znajdują się w tej kolekcji.
     *
     * @param c kolekcja zawierająca elementy do sprawdzenia.
     * @return true, jeśli wszystkie elementy są obecne, false w przeciwnym razie.
     * @throws ClassCastException jeśli typ elementów w `c` nie jest zgodny z kolekcją.
     * @throws NullPointerException jeśli `c` lub jej elementy są null, a kolekcja nie pozwala na null.
     */
    boolean containsAll(Collection<?> c);

    //iterator(), spliterator() oraz forEach() nadpisane z interfejsu `Iterable`
    
    /**
     * Zwraca sekwencyjny strumień dla tej kolekcji.
     *
     * @return sekwencyjny strumień elementów w kolekcji.
     */
    default Stream<E> stream() {
        return StreamSupport.stream(spliterator(), false);
    }

    /**
     * Zwraca równoległy strumień dla tej kolekcji.
     *
     * @return równoległy strumień elementów w kolekcji.
     */
    default Stream<E> parallelStream() {
        return StreamSupport.stream(spliterator(), true);
    }
}
```
Kluczowe metody można podzielić na kilka grup:

1. **Podstawowe operacje**:
    - `add(E e)`: Dodaje element do kolekcji.
    - `remove(Object o)`: Usuwa pierwszy wystąpienie podanego elementu.
    - `size()`: Zwraca liczbę elementów w kolekcji.
    - `isEmpty()`: Sprawdza, czy kolekcja jest pusta.

2. **Operacje zbiorowe**:
    - `addAll(Collection<? extends E>)`: Dodaje wszystkie elementy z podanej kolekcji.
    - `removeAll(Collection<?>)`: Usuwa wszystkie elementy, które znajdują się w podanej kolekcji.
    - `retainAll(Collection<?>)`: Zachowuje tylko te elementy, które są wspólne z podaną kolekcją.

3. **Iteracja i przeglądanie**:
    - `iterator()`: Zwraca iterator do przeglądania elementów kolekcji. (metoda zdefiniowana w interfejsie `Iterable`)
    - `toArray()`: Zwraca tablicę zawierającą wszystkie elementy kolekcji.

4. **Operacje wspomagające**:
    - `contains(Object o)`: Sprawdza, czy podany element znajduje się w kolekcji.
    - `clear()`: Usuwa wszystkie elementy z kolekcji.

5. **Metody domyślne w Java 8**:
    - `stream()` i `parallelStream()`: Umożliwiają przetwarzanie elementów kolekcji za pomocą `Stream API`.
    - `removeIf(Predicate<? super E>)`: Usuwa elementy spełniające określony warunek.

#### **Dlaczego nie każda metoda korzysta z typu generycznego?**

Nie wszystkie metody w interfejsie `Collection` korzystają z typu generycznego, a zamiast tego używają `Object`, aby
zapewnić większą elastyczność i zgodność w różnych scenariuszach programistycznych. Metody takie jak `remove(Object o)`
pozwalają na operacje między kolekcjami różnych, ale logicznie kompatybilnych typów. Na przykład, umożliwia to usunięcie
elementu z `List<Number>` na podstawie elementu z `List<Long>`.

Jak wyjaśnił [Josh Bloch](https://www.youtube.com/watch?v=wDN_EYUvUq0) (autor kluczowych bibliotek Javy i książki
*Effective Java*), próby pełnej "generyfikacji" metod takich jak `remove` podczas tworzenia Java Collection
Framework nie powiodły się, ponieważ ograniczenie
parametrów tylko do generycznego typu kolekcji uniemożliwiłoby wiele sensownych operacji. Przykład przecięcia
`List<Number>` z `List<Long>` ilustruje ten problem. Użycie `Object` zachowuje kompatybilność z metodami takimi jak
`equals` i `hashCode`, które są definiowane w `Object`, i pozwala na porównywanie oraz usuwanie elementów bez ścisłych
ograniczeń typów.

### **4. Wyjątki w interfejsie Collection i ich przyczyny**

W interfejsie `Collection` różne metody mogą generować wyjątki w określonych sytuacjach. Poniżej znajduje się lista
najczęściej występujących wyjątków, ich znaczenie oraz przykładowe sytuacje, w których mogą się pojawić:

#### **1. `UnsupportedOperationException`**
- **Przyczyna**: Metoda, której próbujesz użyć, nie jest obsługiwana przez implementację kolekcji.
- **Przykładowe sytuacje**:
    - Próba modyfikacji kolekcji niemodyfikowalnej (np. utworzonej przez `List.of(...)`).
    - Operacja usuwania na iteratorze, który tego nie obsługuje.
- **Przykład kodu**:
  ```java
  List<String> immutableList = List.of("A", "B");
  immutableList.add("C"); // UnsupportedOperationException
  ```

---

#### **2. `ClassCastException`**
- **Przyczyna**: Element, który próbujesz dodać lub usunąć, nie jest zgodny z typem kolekcji.
- **Przykładowe sytuacje**:
    - Dodawanie elementu o niewłaściwym typie do kolekcji sparametryzowanej typami generycznymi.
---

#### **3. `NullPointerException`**
- **Przyczyna**: Operacja dotyczy wartości `null`, a kolekcja nie obsługuje `null`.
- **Przykładowe sytuacje**:
    - Próba dodania `null` do listy utworzonej za pomocą `List.of(...)`.
    - Wywołanie metody na `null` jako parametr (np. w `addAll` lub `removeAll`).
- **Przykład kodu**:
  ```java
  List<String> list = List.of("A", "B");
  list.add(null); // NullPointerException
  ```

---

#### **4. `IllegalArgumentException`**
- **Przyczyna**: Element dodawany do kolekcji narusza specyficzne reguły implementacji.
- **Przykładowe sytuacje**:
    - Próba dodania elementu, który nie spełnia wymagań kolekcji (np. ograniczenie na typ danych w implementacjach specjalnych).

---

#### **5. `IllegalStateException`**
- **Przyczyna**: Operacja nie jest dozwolona w bieżącym stanie kolekcji.
- **Przykładowe sytuacje**:
    - Próba dodania elementu do pełnej kolekcji o ograniczonej pojemności.
- **Przykład kodu**:
  ```java
  Queue<Integer> queue = new ArrayBlockingQueue<>(1);
  queue.add(1);
  queue.add(2); // IllegalStateException, ponieważ kolejka jest pełna
  ```

---

#### **6. `IndexOutOfBoundsException`**
- **Przyczyna**: Operacja odnosi się do indeksu spoza zakresu listy.
- **Przykładowe sytuacje**:
    - Próba dostępu do elementu na indeksie, który nie istnieje w liście.
- **Przykład kodu**:
  ```java
  List<String> list = new ArrayList<>(List.of("A", "B"));
  list.get(3); // IndexOutOfBoundsException
  ```

---

#### **7. `ArrayStoreException`**
- **Przyczyna**: Podano tablicę o niezgodnym typie przy użyciu metody `toArray(T[] a)`.
- **Przykładowe sytuacje**:
    - Przekształcanie kolekcji na tablicę typów, które nie są kompatybilne z elementami kolekcji.
- **Przykład kodu**:
  ```java
  List<String> list = List.of("A", "B");
  Integer[] array = list.toArray(new Integer[0]); // ArrayStoreException
  ```

---

#### **8. `ConcurrentModificationException`**
- **Przyczyna**: Kolekcja została zmodyfikowana podczas iteracji w sposób, który narusza jej spójność.
- **Przykładowe sytuacje**:
    - Usuwanie elementów z kolekcji bez użycia iteratora podczas iteracji.
- **Przykład kodu**:
  ```java
  List<String> list = new ArrayList<>(List.of("A", "B", "C"));
  for (String s : list) {
      list.remove(s); // ConcurrentModificationException
  }
  ```

---

### **5. Interfejsy rozszerzające Collection**

`Collection` jest rozszerzany przez bardziej wyspecjalizowane interfejsy, które definiują dodatkowe operacje i zasady przechowywania danych:

1. **`List`**:
    - Reprezentuje uporządkowaną sekwencję elementów, w której duplikaty są dozwolone.
    - Zapewnia dostęp do elementów według indeksu, np. `ArrayList` czy `LinkedList`.
    - Przykładowe metody: `get(int index)`, `set(int index, E element)`.

2. **`Set`**:
    - Reprezentuje zbiór unikalnych elementów, w którym nie są dozwolone duplikaty.
    - Implementacje takie jak `HashSet` czy `TreeSet` oferują różne sposoby przechowywania danych: bez kolejności (`HashSet`) lub z porządkiem sortowania (`TreeSet`).
    - Przykładowe metody: Zawiera te same metody, co `Collection`, ale dodatkowo gwarantuje brak duplikatów.

3. **`Queue`**:
    - Reprezentuje kolejkę FIFO (First-In-First-Out), gdzie elementy są przetwarzane w kolejności ich dodania.
    - Przykłady: `PriorityQueue`, `LinkedList` (w trybie kolejki).
    - Przykładowe metody: `offer(E e)`, `poll()`, `peek()`.

4. **`Deque` (Double-Ended Queue)**:
    - Specjalizacja `Queue`, która pozwala na dodawanie i usuwanie elementów z obu końców.
    - Może działać zarówno jako kolejka FIFO, jak i stos LIFO (Last-In-First-Out).
    - Przykłady: `ArrayDeque`, `LinkedList` (w trybie `Deque`).
    - Przykładowe metody: `addFirst(E e)`, `addLast(E e)`, `removeFirst()`, `removeLast()`.

### **6. Przykłady użycia interfejsu Collection**

Przykład dodawania i iteracji po kolekcji:

```java
Collection<String> collection = new ArrayList<>();
collection.add("A");
collection.add("B");
collection.add("C");

collection.remove("B");

for(String text: collection) {
    System.out.println(text); //A, C    
}
```

Przykład operacji zbiorowych:

```java
Collection<Integer> numbers = new ArrayList<>();
numbers.addAll(List.of(1, 2, 3, 4, 5));
numbers.removeAll(List.of(2, 4)); // Usuwa liczby 2 i 4
System.out.println(numbers); // [1, 3, 5]
```

### **7. Ograniczenia interfejsu Collection**
- Nie obsługuje par klucz-wartość (obsługiwane przez `Map`).
- Nie narzuca szczegółowych zasad dotyczących kolejności przechowywania elementów – to zależy od konkretnej implementacji.
- Nie definiuje specjalistycznych operacji (np. dostępu przez indeks), które są specyficzne dla `List` czy `Queue`.

### **Dodatkowe zasoby**

- Oficjalna dokumentacja:
    - [Oracle Collection interface](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)