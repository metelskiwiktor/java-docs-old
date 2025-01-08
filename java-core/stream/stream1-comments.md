# Własna implementacja Stream w Javie - część 1

## 1. Wstęp do Stream w Javie

Stream w Javie pozwala na przetwarzanie danych w sposób funkcyjny, nie modyfikując źródłowej kolekcji. Obsługuje operacje takie jak filtrowanie (`filter`), przekształcanie (`map`), redukcja (`reduce`), a także wykonywanie operacji na elementach (`forEach`). W tym artykule zaimplementujemy własną uproszczoną wersję `Stream`, aby lepiej zrozumieć, jak działają te mechanizmy.

---

## 2. Struktura klasy `MyStream`

Zaczynamy od struktury klasy `MyStream`. Dodajemy metodę `toList()`, która pozwoli łatwo przekształcić strumień na listę do testowania.

```java
public class MyStream<T> {

    private final List<T> elements;

    public MyStream(List<T> elements) {
        this.elements = elements;
    }

    public List<T> toList() {
        return new ArrayList<>(elements);
    }

    // Tutaj będą implementowane kolejne metody
}
```

---

## 3. Implementacja metody `filter`

Metoda `filter` przetwarza strumień, pozostawiając tylko elementy, które spełniają warunek określony przez `Predicate<T>`.

**Sygnatura metody:**

```java
public MyStream<T> filter(Predicate<T> predicate) {
    // Utwórz nową listę do przechowywania przefiltrowanych elementów
    // Dla każdego elementu w 'elements':
        // Jeśli element spełnia warunek 'predicate.test(element)':
            // Dodaj element do nowej listy
    // Zwróć nowy MyStream z przefiltrowanymi elementami
}
```

**Interfejs `Predicate`:**

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

### Zadania do wykonania:

**Zadanie 1:**

Przefiltruj listę liczb całkowitych, pozostawiając tylko liczby parzyste.

**Dane wejściowe:**

- Lista: `[1, 2, 3, 4, 5, 6]`

**Dane wyjściowe:**

- `[2, 4, 6]`

---

**Zadanie 2:**

Przefiltruj listę słów, pozostawiając tylko te, które mają więcej niż 3 znaki.

**Dane wejściowe:**

- Lista: `["kot", "pies", "słoń", "żyrafa"]`

**Dane wyjściowe:**

- `["pies", "słoń", "żyrafa"]`

---

## 4. Implementacja metody `map`

Metoda `map` przekształca elementy strumienia za pomocą funkcji określonej przez `Function<T, R>`.

**Sygnatura metody:**

```java
public <R> MyStream<R> map(Function<T, R> mapper) {
    // Utwórz nową listę do przechowywania przekształconych elementów
    // Dla każdego elementu w 'elements':
        // Zastosuj funkcję 'mapper.apply(element)' i dodaj wynik do nowej listy
    // Zwróć nowy MyStream z przekształconymi elementami
}
```

**Interfejs `Function`:**

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

### Zadania do wykonania:

**Zadanie 1:**

Przekształć listę liczb całkowitych, mnożąc każdą przez 2.

**Dane wejściowe:**

- Lista: `[1, 2, 3, 4]`

**Dane wyjściowe:**

- `[2, 4, 6, 8]`

---

**Zadanie 2:**

Przekształć listę imion, zamieniając je na wielkie litery.

**Dane wejściowe:**

- Lista: `["Anna", "Bartek", "Cecylia"]`

**Dane wyjściowe:**

- `["ANNA", "BARTEK", "CECYLIA"]`

---

## 5. Implementacja metody `forEach`

Metoda `forEach` wykonuje podaną akcję dla każdego elementu strumienia.

**Sygnatura metody:**

```java
public void forEach(Consumer<T> action) {
    // Dla każdego elementu w 'elements':
        // Wykonaj 'action.accept(element)'
}
```

**Interfejs `Consumer`:**

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

### Zadania do wykonania:

**Zadanie 1:**

Wypisz wszystkie elementy listy liczb całkowitych.

**Dane wejściowe:**

- Lista: `[1, 2, 3, 4, 5]`

**Dane wyjściowe:**

- Wyświetlone na ekranie: `1 2 3 4 5`

---

**Zadanie 2:**

Wypisz wszystkie imiona z listy, poprzedzając je tekstem "Imię: ".

**Dane wejściowe:**

- Lista: `["Ewa", "Franek", "Gosia"]`

**Dane wyjściowe:**

- Wyświetlone na ekranie:
    - `Imię: Ewa`
    - `Imię: Franek`
    - `Imię: Gosia`

---

## 6. Implementacja metody `count`

Metoda `count` zwraca liczbę elementów w strumieniu.

**Sygnatura metody:**

```java
public long count() {
    // Zwróć rozmiar listy 'elements'
}
```

### Zadania do wykonania:

**Zadanie 1:**

Policz liczbę elementów w liście słów.

**Dane wejściowe:**

- Lista: `["jabłko", "banan", "pomarańcza"]`

**Dane wyjściowe:**

- `3`

---

**Zadanie 2:**

Policz liczbę liczb większych niż 10 w liście.

**Dane wejściowe:**

- Lista: `[5, 12, 17, 9, 21]`

**Dane wyjściowe:**

- `3`

---

## 7. Implementacja metody `distinct`

Metoda `distinct` zwraca nowy strumień z unikalnymi elementami.

**Sygnatura metody:**

```java
public MyStream<T> distinct() {
    // Utwórz nową listę do przechowywania unikalnych elementów
    // Dla każdego elementu w 'elements':
        // Jeśli element nie jest jeszcze w nowej liście:
            // Dodaj element do nowej listy
    // Zwróć nowy MyStream z unikalnymi elementami
}
```

### Zadania do wykonania:

**Zadanie 1:**

Usuń duplikaty z listy liczb całkowitych.

**Dane wejściowe:**

- Lista: `[1, 2, 2, 3, 3, 3, 4, 4, 4, 4]`

**Dane wyjściowe:**

- `[1, 2, 3, 4]`

---

**Zadanie 2:**

Usuń duplikaty z listy słów.

**Dane wejściowe:**

- Lista: `["kot", "pies", "kot", "ryba", "pies"]`

**Dane wyjściowe:**

- `["kot", "pies", "ryba"]`

---

## 8. Implementacja metody `limit`

Metoda `limit` ogranicza strumień do podanej liczby elementów.

**Sygnatura metody:**

```java
public MyStream<T> limit(int maxSize) {
    // Sprawdź, czy 'maxSize' jest większe od 0
    // Utwórz nową listę do przechowywania ograniczonych elementów
    // Iteruj od indeksu 0 do 'maxSize' lub do końca listy 'elements':
        // Dodaj element o danym indeksie do nowej listy
    // Zwróć nowy MyStream z ograniczoną liczbą elementów
}
```

### Zadania do wykonania:

**Zadanie 1:**

Weź pierwsze 3 elementy z listy liczb.

**Dane wejściowe:**

- Lista: `[10, 20, 30, 40, 50]`

**Dane wyjściowe:**

- `[10, 20, 30]`

---

**Zadanie 2:**

Weź pierwsze 2 imiona z listy.

**Dane wejściowe:**

- Lista: `["Ola", "Piotr", "Rafał", "Sylwia"]`

**Dane wyjściowe:**

- `["Ola", "Piotr"]`

---

## 9. Implementacja metody `skip`

Metoda `skip` pomija podaną liczbę elementów w strumieniu.

**Sygnatura metody:**

```java
public MyStream<T> skip(int n) {
    // Sprawdź, czy 'n' jest mniejsze niż rozmiar listy 'elements'
    // Utwórz nową listę do przechowywania wynikowych elementów
    // Iteruj od indeksu 'n' do końca listy 'elements':
        // Dodaj element o danym indeksie do nowej listy
    // Zwróć nowy MyStream z pominiętymi elementami
}
```

### Zadania do wykonania:

**Zadanie 1:**

Pomiń pierwsze 2 elementy listy liczb i wypisz pozostałe.

**Dane wejściowe:**

- Lista: `[5, 10, 15, 20, 25]`

**Dane wyjściowe:**

- `[15, 20, 25]`

---

**Zadanie 2:**

Pomiń pierwsze 3 imiona z listy.

**Dane wejściowe:**

- Lista: `["Tomek", "Ula", "Wojtek", "Xenia", "Yvette"]`

**Dane wyjściowe:**

- `["Xenia", "Yvette"]`

---

## 10. Implementacja metody `sorted`

Metoda `sorted` sortuje elementy strumienia na podstawie podanego komparatora.

**Sygnatura metody:**

```java
public MyStream<T> sorted(Comparator<T> comparator) {
    // Użyj metody 'sort' na liście 'elements' z podanym komparatorem
    // Zwróć nowy MyStream z posortowanymi elementami
}
```

**Interfejs `Comparator`:**

```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```

### Zadania do wykonania:

**Zadanie 1:**

Posortuj listę liczb w kolejności rosnącej.

**Dane wejściowe:**

- Lista: `[30, 10, 20, 50, 40]`

**Dane wyjściowe:**

- `[10, 20, 30, 40, 50]`

---

**Zadanie 2:**

Posortuj listę imion w kolejności alfabetycznej.

**Dane wejściowe:**

- Lista: `["Zenon", "Andrzej", "Marek", "Beata"]`

**Dane wyjściowe:**

- `["Andrzej", "Beata", "Marek", "Zenon"]`

---

## 11. Implementacja metody `findFirst`

Metoda `findFirst` zwraca pierwszy element strumienia.

**Sygnatura metody:**

```java
public Optional<T> findFirst() {
    // Jeśli lista 'elements' nie jest pusta:
        // Zwróć Optional z pierwszym elementem
    // W przeciwnym razie:
        // Zwróć pusty Optional
}
```

### Zadania do wykonania:

**Zadanie 1:**

Znajdź pierwszy element w liście liczb większych niż 10.

**Dane wejściowe:**

- Lista: `[5, 8, 12, 15, 22]`

**Dane wyjściowe:**

- `12`

---

**Zadanie 2:**

Znajdź pierwszy element w pustej liście.

**Dane wejściowe:**

- Lista: `[]`

**Dane wyjściowe:**

- `Optional.empty`

---

## 12. Zadania złożone

Po zaimplementowaniu wszystkich metod, spróbuj rozwiązać te bardziej złożone zadania.

---

### Zadanie 1:

Przefiltruj listę liczb, pozostawiając tylko te większe niż 10, przekształć je, dodając do nich 5, a następnie znajdź pierwszy taki element.

**Dane wejściowe:**

- Lista: `[5, 8, 12, 15, 22]`

**Dane wyjściowe:**

- `17`

---

### Zadanie 2:

Policz unikalne liczby większe niż 5, ograniczając wynik do pierwszych 3 elementów.

**Dane wejściowe:**

- Lista: `[1, 6, 7, 8, 6, 9, 10]`

**Dane wyjściowe:**

- Liczba elementów: `3`

---

### Zadanie 3:

Posortuj listę nazw, pomijając pierwsze 3 elementy, a następnie znajdź pierwszy taki element.

**Dane wejściowe:**

- Lista: `["Daniel", "Anna", "Bartek", "Cecylia", "Ela"]`

**Dane wyjściowe:**

- `"Daniel"`

---

### Zadanie 4:

Policz liczbę elementów, które są większe niż 50 po przekształceniu ich o wartość +20.

**Dane wejściowe:**

- Lista: `[10, 30, 40, 60, 80]`

**Dane wyjściowe:**

- Liczba elementów: `3`

---

### Zadanie 5:

Przekształć listę nazw, które zaczynają się na literę "A", do formy wielkich liter, a następnie wyświetl ich liczbę.

**Dane wejściowe:**

- Lista: `["Anna", "Agnieszka", "Bartek", "Cecylia", "Adam"]`

**Dane wyjściowe:**

- Liczba elementów: `3`

