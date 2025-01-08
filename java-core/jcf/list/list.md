### **1. Wprowadzenie do interfejsu List**

Interfejs `List` jest częścią Java Collection Framework i rozszerza interfejs `Collection`. Służy do reprezentowania
uporządkowanej sekwencji elementów, gdzie każdy element ma przypisany indeks. W `List` można przechowywać duplikaty, a
kolejność elementów jest zawsze zachowywana.

Od wersji JDK 21, interfejs `List` również rozszerza nowy interfejs **`SequencedCollection`**, który dodaje metody do
zarządzania elementami w kolejności ich wstawiania. Dzięki temu `List` zyskuje dodatkowe możliwości manipulacji na
początku i końcu listy.

---

### **2. Kluczowe cechy interfejsu List**

- **Uporządkowanie elementów**: Elementy w `List` mają stałą kolejność, która jest taka sama jak kolejność ich dodania.
- **Obsługa duplikatów**: W `List` można przechowywać wiele kopii tego samego elementu.
- **Dostęp przez indeksy**: Każdy element ma przypisany indeks, umożliwiający szybki dostęp, modyfikację i usuwanie.

---

### **3. Metody dostępne w interfejsie List**

Poniżej znajduje się kod źródłowy interfejsu `List` wraz z opisem kluczowych metod:

```java
public interface List<E> extends Collection<E>, SequencedCollection<E> {

    /**
     * Dodaje element do listy na określonym indeksie.
     *
     * @param index indeks, na którym element ma zostać dodany.
     * @param element element do dodania.
     * @throws UnsupportedOperationException jeśli operacja dodawania nie jest obsługiwana.
     * @throws ClassCastException jeśli typ elementu nie jest zgodny z typem listy.
     * @throws NullPointerException jeśli lista nie pozwala na null, a element jest null.
     * @throws IllegalArgumentException jeśli element narusza specyficzne ograniczenia listy.
     * @throws IndexOutOfBoundsException jeśli indeks jest poza zakresem.
     */
    void add(int index, E element);

    /**
     * Usuwa element z listy na określonym indeksie.
     *
     * @param index indeks elementu do usunięcia.
     * @return element, który został usunięty.
     * @throws UnsupportedOperationException jeśli operacja usuwania nie jest obsługiwana.
     * @throws IndexOutOfBoundsException jeśli indeks jest poza zakresem.
     */
    E remove(int index);

    /**
     * Zwraca element znajdujący się pod określonym indeksem.
     *
     * @param index indeks elementu do zwrócenia.
     * @return element znajdujący się na podanym indeksie.
     * @throws IndexOutOfBoundsException jeśli indeks jest poza zakresem.
     */
    E get(int index);

    /**
     * Ustawia nowy element na określonym indeksie, zastępując istniejący.
     *
     * @param index indeks, na którym ma zostać ustawiony nowy element.
     * @param element element do ustawienia.
     * @return poprzedni element na podanym indeksie.
     * @throws UnsupportedOperationException jeśli operacja ustawiania nie jest obsługiwana.
     * @throws ClassCastException jeśli typ elementu nie jest zgodny z typem listy.
     * @throws NullPointerException jeśli lista nie pozwala na null, a element jest null.
     * @throws IllegalArgumentException jeśli element narusza specyficzne ograniczenia listy.
     * @throws IndexOutOfBoundsException jeśli indeks jest poza zakresem.
     */
    E set(int index, E element);

    /**
     * Zwraca indeks pierwszego wystąpienia danego elementu w liście.
     *
     * @param o element, którego indeks należy znaleźć.
     * @return indeks pierwszego wystąpienia elementu lub -1, jeśli element nie istnieje.
     * @throws ClassCastException jeśli typ elementu nie jest zgodny z typem listy.
     * @throws NullPointerException jeśli lista nie pozwala na null, a element jest null.
     */
    int indexOf(Object o);

    /**
     * Zwraca indeks ostatniego wystąpienia danego elementu w liście.
     *
     * @param o element, którego indeks należy znaleźć.
     * @return indeks ostatniego wystąpienia elementu lub -1, jeśli element nie istnieje.
     * @throws ClassCastException jeśli typ elementu nie jest zgodny z typem listy.
     * @throws NullPointerException jeśli lista nie pozwala na null, a element jest null.
     */
    int lastIndexOf(Object o);

    /**
     * Zwraca listę zawierającą elementy między podanymi indeksami.
     *
     * @param fromIndex początkowy indeks (włącznie).
     * @param toIndex końcowy indeks (wyłącznie).
     * @return podlista zawierająca elementy w podanym zakresie.
     * @throws IndexOutOfBoundsException jeśli indeksy są poza zakresem.
     * @throws IllegalArgumentException jeśli `fromIndex` jest większe lub równe `toIndex`.
     */
    List<E> subList(int fromIndex, int toIndex);

    /**
     * Iteracja po liście.
     */
    ListIterator<E> listIterator();

    /**
     * Iteracja od podanego indeksu.
     */
    ListIterator<E> listIterator(int index);

    /**
     * Zamienia wszystkie elementy w liście zgodnie z podanym operatorem.
     *
     * @param operator operator transformacji elementów.
     * @throws NullPointerException jeśli operator jest null.
     */
    default void replaceAll(UnaryOperator<E> operator) {
        ListIterator<E> li = this.listIterator();
        while (li.hasNext()) {
            li.set(operator.apply(li.next()));
        }
    }

    /**
     * Sortuje listę według podanego komparatora.
     *
     * @param c komparator do sortowania elementów.
     * @throws NullPointerException jeśli komparator jest null.
     */
    default void sort(Comparator<? super E> c) {
        Object[] a = this.toArray();
        Arrays.sort(a, (Comparator) c);
        ListIterator<E> i = this.listIterator();
        for (Object o : a) {
            i.next();
            i.set((E) o);
        }
    }

    /**
     * Tworzy niemodyfikowalną listę z podanych elementów (JDK 10).
     *
     * @param elements elementy do dodania do listy.
     * @return niemodyfikowalna lista.
     */
    static <E> List<E> of(E... elements) {
        return ImmutableCollections.listFromArray(elements); //klasa `ImmutableCollections` nie jest publicznie dostępna ze względu na domyślny (pakietowy) modyfikator dostępu.
    }

    /**
     * Tworzy kopię istniejącej listy (JDK 10).
     *
     * @param coll kolekcja do skopiowania.
     * @return niemodyfikowalna lista będąca kopią.
     */
    static <E> List<E> copyOf(Collection<? extends E> coll) {
        return ImmutableCollections.listFromArray(coll.toArray()); //klasa `ImmutableCollections` nie jest publicznie dostępna ze względu na domyślny (pakietowy) modyfikator dostępu.
    }
}
```

> **Uwaga!** Interfejsy takie jak `Iterable`, `Stream`, `UnaryOperator` i inne związane z przetwarzaniem kolekcji zostaną szczegółowo omówione w kolejnych dokumentach.

#### **Kluczowe metody można podzielić na kilka grup::**

1. **Operacje podstawowe:**
    - `add(int index, E element)` – Dodaje element na określonym indeksie. Elementy znajdujące się za wskazanym indeksem zostają przesunięte w prawo.
    - `remove(int index)` – Usuwa element na podanym indeksie i przesuwa pozostałe elementy w lewo.
    - `get(int index)` – Pobiera element znajdujący się na podanym indeksie.
    - `set(int index, E element)` – Zamienia element na danym indeksie nową wartością.

2. **Operacje masowe:**
    - `replaceAll(UnaryOperator<E> operator)` – Zamienia każdy element w liście zgodnie z podanym operatorem.
    - `sort(Comparator<? super E> c)` – Sortuje listę według podanego komparatora.

3. **Tworzenie niemodyfikowalnych list:**
    - `of(E... elements)` – Tworzy niemodyfikowalną listę z podanych elementów.
    - `copyOf(Collection<? extends E> coll)` – Tworzy niemodyfikowalną kopię istniejącej kolekcji.

4. **Operacje na kolejności elementów (JDK 21, interfejs `SequencedCollection`):**
    - **Dodawanie elementów:**
        - `addFirst(E e)` – Dodaje element na początku listy.
        - `addLast(E e)` – Dodaje element na końcu listy.
    - **Usuwanie elementów:**
        - `removeFirst()` – Usuwa pierwszy element w liście i zwraca jego wartość.
        - `removeLast()` – Usuwa ostatni element w liście i zwraca jego wartość.
    - **Pobieranie elementów:**
        - `getFirst()` – Zwraca pierwszy element w liście.
        - `getLast()` – Zwraca ostatni element w liście.
    - **Odwracanie kolejności:**
        - `reversed()` – Zwraca widok listy z elementami w odwróconej kolejności.

---

### **4. Typowe błędy i wyjątki w pracy z List**

- **`IndexOutOfBoundsException`** – występuje, gdy operacja odnosi się do indeksu spoza zakresu listy.
- **`UnsupportedOperationException`** – próba modyfikacji niemodyfikowalnej listy lub operacja niedostępna w implementacji.
- **`NullPointerException`** – występuje w przypadku operacji na null, jeśli lista nie obsługuje wartości null.

---

### **5. Przykłady użycia interfejsu List**

#### Tworzenie listy i podstawowe operacje:
```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.set(1, "C");
System.out.println(list); // [A, C]
```

#### Iteracja z użyciem `ListIterator`:
```java
List<Integer> numbers = new LinkedList<>(List.of(1, 2, 3, 4));
ListIterator<Integer> iterator = numbers.listIterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

#### Tworzenie niemodyfikowalnej listy:
```java
List<String> immutableList = List.of("A", "B", "C");
System.out.println(immutableList); // [A, B, C]
```

#### Sortowanie listy:
```java
List<String> names = new ArrayList<>(List.of("Bob", "Alice", "Charlie"));
names.sort(Comparator.naturalOrder());
System.out.println(names); // [Alice, Bob, Charlie]
```

---

### **6. Implementacje interfejsu List**

Główne implementacje interfejsu `List` i ich charakterystyka:

- **`ArrayList`**:
    - Implementacja oparta na dynamicznej tablicy.
    - Oferuje szybki dostęp do elementów przez indeks, ale operacje wstawiania i usuwania w środku są kosztowne.
    - Optymalna do przechowywania dużych zbiorów, gdzie operacje losowego dostępu dominują.

- **`LinkedList`**:
    - Implementacja oparta na liście powiązanej.
    - Efektywna w operacjach dodawania i usuwania na końcach, ale wolniejszy dostęp przez indeks.
    - Sprawdzi się w scenariuszach, gdzie dominują operacje dodawania/usuwania na początku lub końcu listy.

- **`Vector`**:
    - Starsza, synchronizowana implementacja listy, obecnie rzadko używana.
    - Zaletą jest wbudowana synchronizacja, co może być przydatne w wielowątkowych aplikacjach, ale w większości przypadków lepiej użyć nowocześniejszych rozwiązań.

- **`CopyOnWriteArrayList`**:
    - Implementacja wielowątkowa, kopiuje całą listę przy każdej modyfikacji, co czyni ją bezpieczną dla współbieżnych odczytów.
    - Idealna w aplikacjach wielowątkowych, gdzie modyfikacje są rzadkie, a odczyty częste.

---

### **7. Operacje na indeksach**

Interfejs `List` pozwala na zaawansowane operacje na elementach za pomocą ich indeksów:

- **`add(int index, E element)`**:  
  Dodaje element na określonym indeksie. Elementy za wskazanym indeksem zostają przesunięte w prawo.  
  Przykład:
  ```java
  List<String> list = new ArrayList<>();
  list.add("A");
  list.add(0, "B"); // ["B", "A"]
  ```

- **`remove(int index)`**:  
  Usuwa element na podanym indeksie i przesuwa pozostałe elementy w lewo.  
  Przykład:
  ```java
  List<String> list = new ArrayList<>(List.of("A", "B", "C"));
  list.remove(1); // ["A", "C"]
  ```

- **`set(int index, E element)`**:  
  Podmienia istniejący element na określonym indeksie.  
  Przykład:
  ```java
  List<String> list = new ArrayList<>(List.of("A", "B", "C"));
  list.set(1, "D"); // ["A", "D", "C"]
  ```

- **`get(int index)`**:  
  Pobiera element z podanego indeksu.  
  Przykład:
  ```java
  List<String> list = new ArrayList<>(List.of("A", "B", "C"));
  String element = list.get(2); // "C"
  ```

---

### **8. Iteracja po List**

#### Iteracja z użyciem `enhanced for`:
```java
List<String> list = List.of("A", "B", "C");
for (String element : list) {
    System.out.println(element);
}
```

#### Iteracja z użyciem `ListIterator`:
```java
List<Integer> numbers = new LinkedList<>(List.of(1, 2, 3, 4));
ListIterator<Integer> iterator = numbers.listIterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

#### Iteracja z użyciem strumienia (`Stream API`):
```java
List<String> list = List.of("A", "B", "C");
list.stream()
    .filter(s -> !s.equals("B"))
    .forEach(System.out::println); // A, C
```

---

### **9. Podlisty i widoki**

Metoda `subList(int fromIndex, int toIndex)` pozwala na tworzenie widoków na fragmenty listy. 
Ważne jest, że podlista nie jest oddzielną listą, ale widokiem – zmiany w podliście wpływają na oryginalną listę.

Przykład:
```java
List<String> list = new ArrayList<>(List.of("A", "B", "C", "D"));
List<String> sublist = list.subList(1, 3); // ["B", "C"]
sublist.set(0, "X");
System.out.println(list); // ["A", "X", "C", "D"]
```

---

### **10. Niemodyfikowalne i wielowątkowe listy**

#### Tworzenie niemodyfikowalnych list
Od JDK 10, można tworzyć niemodyfikowalne listy za pomocą metod fabrycznych:
- **`List.of`**: Tworzy listę z podanych elementów.
- **`List.copyOf`**: Tworzy niemodyfikowalną listę na podstawie istniejącej kolekcji.

Przykłady:
```java
List<String> immutableList = List.of("A", "B", "C");
System.out.println(immutableList); // [A, B, C]

List<String> sourceList = new ArrayList<>(List.of("X", "Y", "Z"));
List<String> copiedList = List.copyOf(sourceList);
System.out.println(copiedList); // [X, Y, Z]
```

> **Uwaga!** Próba dodania elementu na niemodyfikowalnej listy zakończy się wyjątkiem typu `UnsupportedOperationException`

#### Wielowątkowe implementacje list
- **`CopyOnWriteArrayList`**:  
  Idealna w aplikacjach wielowątkowych, gdzie modyfikacje są rzadkie, a odczyty częste. Ze względu na synchronizację nie
  jest zalecana dla operacji jednowątkowych.

  Przykład:
  ```java
  CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>(List.of("A", "B", "C"));
  list.add("D");
  System.out.println(list); // [A, B, C, D]
  ```

---

### **11. Przykłady zaawansowanego użycia interfejsu List**

#### Sortowanie listy
```java
List<String> names = new ArrayList<>(List.of("Bob", "Alice", "Charlie"));
names.sort(Comparator.naturalOrder());
System.out.println(names); // [Alice, Bob, Charlie]
```

#### Kopiowanie i modyfikacja
```java
List<String> source = List.of("A", "B", "C");
List<String> modifiableCopy = new ArrayList<>(source);
modifiableCopy.add("D");
System.out.println(modifiableCopy); // [A, B, C, D]
```

#### Tworzenie podlisty i synchronizacja
```java
List<Integer> numbers = new ArrayList<>(List.of(1, 2, 3, 4, 5));
List<Integer> sublist = numbers.subList(1, 4); // [2, 3, 4]
sublist.set(0, 10);
System.out.println(numbers); // [1, 10, 3, 4, 5]
```

---

### **Pytania i odpowiedzi**

#### **1. Jakie są kluczowe cechy interfejsu List?**

- Uporządkowana sekwencja elementów, w której kolejność jest zgodna z kolejnością dodania.
- Możliwość przechowywania duplikatów.
- Dostęp do elementów za pomocą indeksów.


#### **2. Jakie wyjątki można spotkać podczas pracy z interfejsem List i w jakich sytuacjach występują?**

Podczas pracy z `List` najczęściej występują następujące wyjątki:

- **`IndexOutOfBoundsException`**: Występuje, gdy odwołujemy się do indeksu spoza zakresu listy.  
  **Przykład:**
  ```java
  List<String> list = List.of("A", "B");
  list.get(2); // IndexOutOfBoundsException, ponieważ lista ma tylko dwa elementy (indeksy 0 i 1)
  ```

- **`UnsupportedOperationException`**: Pojawia się, gdy próbujemy wykonać operację, która nie jest obsługiwana przez konkretną implementację listy, np. modyfikowanie niemutowalnej listy.  
  **Przykład:**
  ```java
  List<String> list = List.of("A", "B");
  list.add("C"); // UnsupportedOperationException
  ```

- **`NullPointerException`**: Może wystąpić, gdy próbujemy dodać wartość `null` do listy, która tego nie obsługuje, np. list utworzonych za pomocą `List.of(...)`.  
  **Przykład:**
  ```java
  List<String> list = List.of("A", null); // NullPointerException
  ```

#### **3. Jakie metody tworzą niemutowalne listy i w jakim celu są używane?**

Do tworzenia niemodyfikowalnych list służą:

- **`List.of(E... elements)`**: Tworzy listę, która jest niemodyfikowalna i nie pozwala na przechowywanie wartości `null`.  
  **Przykład:**
  ```java
  List<String> immutableList = List.of("A", "B", "C");
  ```

- **`List.copyOf(Collection<? extends E> coll)`**: Tworzy niemodyfikowalną kopię istniejącej kolekcji.  
  **Przykład:**
  ```java
  List<String> source = List.of("X", "Y");
  List<String> copiedList = List.copyOf(source);
  ```

Niemodyfikowalne listy zapewniają większe bezpieczeństwo w aplikacjach, ponieważ ich zawartość nie może zostać zmieniona. 
Są szczególnie przydatne w środowiskach wielowątkowych i do przechowywania stałych danych.

Pamiętaj, że próba dodania elementu do niemodyfikowalnej listy zakończy się wyrzuceniem wyjątku `UnsupportedOperationException`

#### **4. Co zwraca metoda `indexOf` w przypadku, gdy element nie zostanie znaleziony? Czym różni się od `lastIndexOf`?**

- Metoda **`indexOf(Object o)`** zwraca:
    - Indeks pierwszego wystąpienia elementu w liście.
    - `-1`, jeśli element nie zostanie znaleziony.

- Metoda **`lastIndexOf(Object o)`** działa podobnie, ale zwraca indeks ostatniego wystąpienia elementu w liście.

#### **5. Czy kolekcja utworzona za pomocą `subList` wpływa na główną kolekcję?**

Tak, kolekcja utworzona metodą `subList` jest widokiem oryginalnej listy. Zmiany dokonane na podliście odzwierciedlają
się w głównej liście i odwrotnie.

**Przykład:**
```java
List<String> list = new ArrayList<>(List.of("A", "B", "C", "D"));
List<String> sublist = list.subList(1, 3); // ["B", "C"]
sublist.set(0, "X"); // Zmienia element w podliście
System.out.println(list); // ["A", "X", "C", "D"] (zmiana widoczna w głównej liście)
```

#### **6. Jakie zmiany wprowadzono w interfejsie List w JDK 21?**

W JDK 21 wprowadzono metody wynikające z rozszerzenia interfejsu `SequencedCollection`:

- **Dodawanie elementów**: `addFirst`, `addLast`
- **Usuwanie elementów**: `removeFirst`, `removeLast`
- **Pobieranie elementów**: `getFirst`, `getLast`
- **Odwrócenie kolejności**: `reversed`

#### **7. Jakie implementacje interfejsu List nie pozwalają na przechowywanie wartości `null`?**

Niektóre implementacje i metody w `List` nie obsługują wartości `null`:

- **Niemodyfikowalne listy (od JDK 9)**:  
  Listy utworzone za pomocą `List.of(...)` lub `List.copyOf(...)` nie pozwalają na dodanie wartości `null`.  
  **Przykład:**
  ```java
  List<String> list = List.of("A", null); // NullPointerException
  ```

- **Guava ImmutableList**:  
  Listy niemodyfikowalne utworzone za pomocą biblioteki Guava również nie obsługują `null`.

- **Specjalne struktury, np. `ArrayDeque`**:  
  `ArrayDeque` nie pozwala na przechowywanie `null`.
  **Przykład:**
  ```java
    ArrayDeque arrayDeque = new ArrayDeque();
    arrayDeque.add(null); // NullPointerException
  ```


#### **8. Jaka jest różnica między `Vector` a `CopyOnWriteArrayList`?**

Obie klasy, `Vector` i `CopyOnWriteArrayList`, obsługują bezpieczne dla wątków (thread-safe) listy, ale są
zaprojektowane z myślą o różnych scenariuszach i działają w zupełnie inny sposób. 

| Cecha                     | Vector                                        | CopyOnWriteArrayList            |
|---------------------------|-----------------------------------------------|---------------------------------|
| **Synchronizacja**        | Blokady na poziomie metod                     | Kopiowanie przy modyfikacji     |
| **Wydajność odczytów**    | Średnia                                       | Bardzo wysoka                   |
| **Wydajność modyfikacji** | Wysoka w przypadku małej konkurencji          | Niska przy dużych listach       |
| **Obsługa iteracji**      | Może rzucić `ConcurrentModificationException` | Iteracja na kopii, brak wyjątku |
| **Wersja wprowadzenia**   | JDK 1.0                                       | JDK 1.5                         |
| **Przeznaczenie**         | Ogólna synchronizacja                         | Odczyty dominujące nad zmianami |

- Używaj **`Vector`**, jeśli potrzebujesz wbudowanej synchronizacji dla listy i pracujesz w środowisku, gdzie
  modyfikacje są częste, ale wymagana jest pełna synchronizacja.
- Używaj **`CopyOnWriteArrayList`**, jeśli Twoja aplikacja wymaga dużej liczby odczytów i rzadkich modyfikacji w
  środowiskach wielowątkowych. Jest to bardziej nowoczesne i wydajne rozwiązanie w takich przypadkach.
