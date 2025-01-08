### **1. Wprowadzenie do interfejsów `Iterable` i `Iterator`**

Interfejsy `Iterable` i `Iterator` są kluczowymi elementami w Java Collection Framework, wprowadzonymi, aby umożliwić łatwą iterację po elementach kolekcji.
- **`Iterable`**: Reprezentuje obiekt, który dostarcza `Iterator` do iteracji po swoich elementach.
- **`Iterator`**: Jest obiektem, który pozwala przechodzić przez elementy kolekcji jeden po drugim.

---

### **2. Interfejs `Iterable`**

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

### **3. Interfejs `Iterator`**

```java
public interface Iterator<E> {

    /**
     * Sprawdza, czy w kolekcji są jeszcze elementy do przetworzenia.
     *
     * @return true, jeśli iterator ma kolejny element, false w przeciwnym razie.
     */
    boolean hasNext();

    /**
     * Zwraca następny element w iterowanej kolekcji.
     *
     * @return następny element w iterowanej kolekcji.
     * @throws NoSuchElementException jeśli nie ma kolejnych elementów.
     */
    E next();

    /**
     * Usuwa ostatni element zwrócony przez iterator.
     *
     * @throws UnsupportedOperationException jeśli operacja usuwania nie jest obsługiwana.
     * @throws IllegalStateException jeśli `next()` nie został jeszcze wywołany lub `remove()` został już wywołany po ostatnim `next()`.
     */
    default void remove() {
        throw new UnsupportedOperationException("remove");
    }

    /**
     * Przetwarza pozostałe elementy w kolekcji za pomocą podanej akcji.
     *
     * @param action akcja do wykonania na każdym pozostałym elemencie.
     * @throws NullPointerException jeśli akcja jest null.
     * @since 1.8
     */
    default void forEachRemaining(Consumer<? super E> action) {
        Objects.requireNonNull(action);
        while (hasNext()) {
            action.accept(next());
        }
    }
}
```

---

### **4. Opis metod interfejsu `Iterator`**

- **`boolean hasNext()`**:  
  Sprawdza, czy w kolekcji znajdują się kolejne elementy do iteracji. Zwraca `true`, jeśli są, lub `false`, jeśli iterator dotarł do końca.

- **`E next()`**:  
  Zwraca następny element w iteracji. Jeśli nie ma więcej elementów, rzuca wyjątek `NoSuchElementException`.

- **`void remove()`**:  
  Usuwa ostatni element zwrócony przez metodę `next()`. Rzuca wyjątek `UnsupportedOperationException`, jeśli operacja usuwania nie jest obsługiwana przez iterator.

- **`void forEachRemaining(Consumer<? super E> action)`** (od JDK 8):  
  Przetwarza wszystkie pozostałe elementy w kolekcji za pomocą podanej akcji (wyrażenia lambda lub referencji do metody).

---

### **5. Przykłady użycia metod `Iterator`**

#### **`hasNext()` i `next()`**
```java
List<String> list = List.of("A", "B", "C");
Iterator<String> iterator = list.iterator();

while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

#### **`remove()`**
```java
List<String> list = new ArrayList<>(List.of("A", "B", "C"));
Iterator<String> iterator = list.iterator();

while (iterator.hasNext()) {
    String element = iterator.next();
    if (element.equals("B")) {
        iterator.remove(); // Usuwa "B"
    }
}
System.out.println(list); // [A, C]
```

#### **`forEachRemaining()`**
```java
List<String> list = List.of("A", "B", "C");
Iterator<String> iterator = list.iterator();
iterator.next();
iterator.forEachRemaining(System.out::println); // Wypisze: B, C
```

---

### **6. Implementacja własnego `Iterable` i `Iterator`**

#### **Prosta kolekcja: `PseudoArrayList`**
Poniżej implementujemy prostą kolekcję działającą jak lista oraz odpowiedni `Iterator` z obsługą `modCount`.

```java
import java.util.ConcurrentModificationException;
import java.util.Iterator;
import java.util.NoSuchElementException;

public class PseudoArrayList<E> implements Iterable<E> {
  private Object[] elements;
  private int size = 0;
  private int modCount = 0; // Licznik modyfikacji

  public PseudoArrayList(int capacity) {
    elements = new Object[capacity];
  }

  public void add(E element) {
    if (size == elements.length) {
      throw new IllegalStateException("List is full");
    }
    elements[size++] = element;
    modCount++; // Zwiększenie licznika modyfikacji
  }

  public E get(int index) {
    if (index < 0 || index >= size) {
      throw new IndexOutOfBoundsException("Index out of bounds");
    }
    return (E) elements[index];
  }

  public int size() {
    return size;
  }

  @Override
  public Iterator<E> iterator() {
    return new PseudoArrayListIterator();
  }

  private class PseudoArrayListIterator implements Iterator<E> {
    private int currentIndex = 0;
    private final int expectedModCount = modCount; // Zapamiętanie bieżącego stanu `modCount`

    @Override
    public boolean hasNext() {
      return currentIndex < size;
    }

    @Override
    public E next() {
      // Sprawdzamy, czy kolekcja została zmodyfikowana
      if (expectedModCount != modCount) {
        throw new ConcurrentModificationException("List was modified during iteration");
      }
      if (!hasNext()) {
        throw new NoSuchElementException("No more elements");
      }
      return (E) elements[currentIndex++];
    }

    @Override
    public void remove() {
      throw new UnsupportedOperationException("Remove not supported");
    }
  }
}
```

#### **Przykład użycia `PseudoArrayList`**
```java
PseudoArrayList<String> list = new PseudoArrayList<>(5);
list.add("A");
list.add("B");
list.add("C");

for (String item : list) {
    System.out.println(item);
}
```

---

### **7. Wyjaśnienie `modCount`**

- **Czym jest `modCount`?**  
  `modCount` to licznik modyfikacji w kolekcji, który zwiększa się za każdym razem, gdy kolekcja zostanie zmodyfikowana (np. przez dodanie, usunięcie lub inne zmiany strukturalne). Mechanizm ten jest wbudowany w wiele implementacji kolekcji w Java Collection Framework, takich jak `ArrayList` czy `HashMap`.

- **Dlaczego jest potrzebny?**  
  `modCount` chroni przed nieoczekiwanym zachowaniem iteratora w sytuacji, gdy kolekcja zostanie zmodyfikowana podczas iteracji. Iterator porównuje stan `modCount` zapisany w momencie jego utworzenia (`expectedModCount`) z bieżącym stanem kolekcji. Jeśli wartości te się różnią, iterator wykrywa modyfikację i rzuca wyjątek **`ConcurrentModificationException`**.

- **Czy wszystkie `Iterator` korzystają z `modCount`?**  
  Nie wszystkie implementacje `Iterator` w Javie wykorzystują `modCount` do wykrywania modyfikacji.
  - **Iterator oparty na `modCount`**: Większość standardowych implementacji, takich jak `ArrayList`, `HashMap`, i inne kolekcje z JCF (Java Collection Framework), używa `modCount` jako mechanizmu detekcji zmian.
  - **Inne implementacje iteratora**: Istnieją iteratorzy, którzy nie korzystają z `modCount`, np. `CopyOnWriteArrayList` używa kopii danych i nie rzuca `ConcurrentModificationException` w przypadku modyfikacji, ponieważ iteracja odbywa się na statycznej kopii kolekcji.

---

- **Jaki wyjątek jest rzucany?**
  Jeśli iterator korzystający z `modCount` wykryje zmianę w kolekcji, rzuca **`ConcurrentModificationException`**, co wskazuje, że iteracja została przerwana przez nielegalną modyfikację kolekcji.

- **Przykład z `modCount`:**
  ```java
  List<String> list = new ArrayList<>(List.of("A", "B", "C"));
  Iterator<String> iterator = list.iterator();

  list.add("D"); // Modyfikacja kolekcji po utworzeniu iteratora

  while (iterator.hasNext()) {
      System.out.println(iterator.next()); // Rzuci ConcurrentModificationException
  }
  ```

- **Przykład iteratora, który nie rzuca `ConcurrentModificationException`:**
  ```java
  List<String> list = new CopyOnWriteArrayList<>(List.of("A", "B", "C"));
  Iterator<String> iterator = list.iterator();

  list.add("D"); // Modyfikacja kolekcji po utworzeniu iteratora

  while (iterator.hasNext()) {
      System.out.println(iterator.next()); // A,B,C // Brak wyjątku, iterator działa na kopii
  }
  System.out.println(list); // ["A", "B", "C", "D"]
  ```

---

### **8. Pytania i odpowiedzi**

#### **1. Co się stanie, jeśli podczas iteracji kolekcja zostanie zmodyfikowana?**
- Jeśli `Iterator` wykryje zmianę kolekcji (np. przez `modCount`), rzuci wyjątek `ConcurrentModificationException`.

#### **2. Jak działa `forEachRemaining`?**
- Przetwarza wszystkie pozostałe elementy w kolekcji za pomocą podanej akcji, np. wyrażenia lambda. Jest wygodne do przetwarzania elementów w sposób deklaratywny.

#### **3. Jakie są różnice między `Iterator` a `for-each`?**
- `Iterator` pozwala na bardziej szczegółowe sterowanie iteracją (np. usuwanie elementów za pomocą `remove`), podczas gdy pętla `for-each` automatycznie korzysta z iteratora.

#### **4. Dlaczego metoda `remove` w `Iterator` może rzucić `UnsupportedOperationException`?**
- Nie wszystkie implementacje kolekcji wspierają usuwanie elementów za pomocą iteratora, np. kolekcje niemodyfikowalne.

#### **5. Jak działa licznik `modCount`?**
- Jest to mechanizm bezpieczeństwa, który wykrywa nielegalne modyfikacje kolekcji podczas iteracji i rzuca wyjątek.

#### **6. Jak zaimplementować własny `Iterable`?**
- Klasa musi zaimplementować interfejs `Iterable` i dostarczyć metodę `iterator()`, która zwraca obiekt `Iterator`.

#### **7. Co to jest iterator typu fail-fast i fail-safe?**
- **Fail-fast:** Wykrywa zmiany w kolekcji podczas iteracji i rzuca `ConcurrentModificationException`.
- **Fail-safe:** Pozwala na modyfikacje kolekcji podczas iteracji bez rzucania wyjątku (np. iterator na `CopyOnWriteArrayList`).
