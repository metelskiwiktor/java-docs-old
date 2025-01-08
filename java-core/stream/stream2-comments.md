# Własna implementacja Stream w Javie - część 2

## 1. allMatch

**Opis:**  
Metoda `allMatch` zwraca `true`, jeśli wszystkie elementy strumienia spełniają warunek określony przez `Predicate<T>`.

**Sygnatura metody:**
```java
public boolean allMatch(Predicate<T> predicate) {
    // Przechodzimy przez każdy element w 'elements'
        // Jeśli jakikolwiek element nie spełnia warunku 'predicate.test(element)'
            // Zwróć false
    // Jeśli wszystkie elementy spełniają warunek, zwróć true
}
```

**Zadania do wykonania:**

1. **Sprawdź, czy wszystkie liczby w liście są parzyste.**

    - **Dane wejściowe:**
        - Lista: `[2, 4, 6, 8]`
    - **Oczekiwane wyjście:**
        - Wynik `allMatch`: `true`

2. **Sprawdź, czy wszystkie imiona w liście zaczynają się na literę 'A'.**

    - **Dane wejściowe:**
        - Lista: `["Anna", "Andrzej", "Agnieszka", "Barbara"]`
    - **Oczekiwane wyjście:**
        - Wynik `allMatch`: `false`

---

## 2. anyMatch

**Opis:**  
Metoda `anyMatch` zwraca `true`, jeśli przynajmniej jeden element strumienia spełnia warunek określony przez `Predicate<T>`.

**Sygnatura metody:**
```java
public boolean anyMatch(Predicate<T> predicate) {
    // Przechodzimy przez każdy element w 'elements'
        // Jeśli element spełnia warunek 'predicate.test(element)'
            // Zwróć true
    // Jeśli żaden element nie spełnia warunku, zwróć false
}
```

**Zadania do wykonania:**

1. **Sprawdź, czy lista zawiera przynajmniej jedną liczbę większą niż 10.**

    - **Dane wejściowe:**
        - Lista: `[1, 3, 7, 12]`
    - **Oczekiwane wyjście:**
        - Wynik `anyMatch`: `true`

2. **Sprawdź, czy lista imion zawiera imię zaczynające się na literę 'B'.**

    - **Dane wejściowe:**
        - Lista: `["Anna", "Bartek", "Cecylia", "Darek"]`
    - **Oczekiwane wyjście:**
        - Wynik `anyMatch`: `true`

---

## 3. Złożona operacja `allMatch`, `anyMatch`

**Opis:**  
Używając metody `allMatch` sprawdź, czy wszystkie liczby są parzyste. <br>
Używając metody `anyMatch` sprawdź, czy przynajmniej jedna liczba jest większa niż 10.

**Dane wejściowe:**
- Lista: `[2, 4, 6, 9, 12]`

**Oczekiwane wyjście:**
- Wynik `allMatch`: `false`
- Wynik `anyMatch`: `true`

---

## 4. noneMatch

**Opis:**  
Metoda `noneMatch` zwraca `true`, jeśli żaden element strumienia nie spełnia warunku określonego przez `Predicate<T>`.

**Sygnatura metody:**
```java
public boolean noneMatch(Predicate<T> predicate) {
    // Przechodzimy przez każdy element w 'elements'
        // Jeśli element spełnia warunek 'predicate.test(element)'
            // Zwróć false
    // Jeśli żaden element nie spełnia warunku, zwróć true
}
```

**Zadania do wykonania:**

1. **Sprawdź, czy żaden element w liście nie jest ujemny.**

    - **Dane wejściowe:**
        - Lista: `[1, 3, 5, 7]`
    - **Oczekiwane wyjście:**
        - Wynik `noneMatch`: `true`

2. **Sprawdź, czy w liście nie ma liczby 0.**

    - **Dane wejściowe:**
        - Lista: `[0, 1, 2, 3]`
    - **Oczekiwane wyjście:**
        - Wynik `noneMatch`: `false`

---

## 5. reduce

**Opis:**  
Metoda `reduce` agreguje elementy strumienia na podstawie operatora binarnego. Przekazywana wartość `identity` jest wartością początkową.

**Sygnatura metody:**
```java
public T reduce(T identity, BinaryOperator<T> accumulator) {
    // Ustaw zmienną wynikową na wartość 'identity'
    // Przechodzimy przez każdy element w 'elements'
        // Zastosuj 'accumulator' na bieżącym wyniku i elemencie
        // Zaktualizuj wynik
    // Zwróć ostateczny wynik
}
```

**Interfejs `BinaryOperator` rozszerzający `BiFunction`:**
```java
@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T,T,T> {
    
}

@FunctionalInterface
public interface BiFunction<T, U, R> {
    R apply(T t, U u);
}
```

**Zadania do wykonania:**

1. **Zsumuj wszystkie liczby w liście.**

    - **Dane wejściowe:**
        - Lista: `[1, 2, 3, 4]`
    - **Oczekiwane wyjście:**
        - Suma: `10`

2. **Połącz wszystkie napisy w liście w jeden ciąg znaków.**

    - **Dane wejściowe:**
        - Lista: `["a", "b", "c", "d"]`
    - **Oczekiwane wyjście:**
        - Wynik: `"abcd"`

---

## 6. Złożona operacja `reduce`, `noneMatch`

**Opis:**  
Użyj metod `reduce` i `noneMatch`, aby zsumować liczby większe niż 10 i sprawdzić, czy żaden element nie jest mniejszy niż 5.

**Dane wejściowe:**
- Lista: `[12, 15, 8, 22]`

**Oczekiwane wyjście:**
- Suma: `49`
- Wynik `noneMatch`: `true`

---

## 7. min

**Opis:**  
Metoda `min` zwraca minimalny element w strumieniu według podanego komparatora.

**Sygnatura metody:**
```java
public Optional<T> min(Comparator<T> comparator) {
    // Jeśli 'elements' jest puste, zwróć Optional.empty()
    // Ustaw zmienną 'minValue' na pierwszy element w 'elements'
    // Przechodzimy przez pozostałe elementy w 'elements'
        // Jeśli 'comparator.compare(element, minValue)' jest mniejsze niż 0
            // Zaktualizuj 'minValue' na bieżący element
    // Zwróć Optional.of(minValue)
}
```

**Zadania do wykonania:**

1. **Znajdź minimalną liczbę w liście.**

    - **Dane wejściowe:**
        - Lista: `[4, 2, 6, 1]`
    - **Oczekiwane wyjście:**
        - Minimalna wartość: `1`

2. **Znajdź słowo o najkrótszej długości w liście.**

    - **Dane wejściowe:**
        - Lista: `["Anna", "Bartek", "Celina", "Dom"]`
    - **Oczekiwane wyjście:**
        - Najkrótsze słowo: `"Dom"`

---

## 8. max

**Opis:**  
Metoda `max` zwraca maksymalny element w strumieniu według podanego komparatora.

**Sygnatura metody:**
```java
public Optional<T> max(Comparator<T> comparator) {
    // Jeśli 'elements' jest puste, zwróć Optional.empty()
    // Ustaw zmienną 'maxValue' na pierwszy element w 'elements'
    // Przechodzimy przez pozostałe elementy w 'elements'
        // Jeśli 'comparator.compare(element, maxValue)' jest większe niż 0
            // Zaktualizuj 'maxValue' na bieżący element
    // Zwróć Optional.of(maxValue)
}
```

**Zadania do wykonania:**

1. **Znajdź największą liczbę w liście.**

    - **Dane wejściowe:**
        - Lista: `[4, 2, 6, 1]`
    - **Oczekiwane wyjście:**
        - Maksymalna wartość: `6`

2. **Znajdź słowo o najdłuższej długości w liście.**

    - **Dane wejściowe:**
        - Lista: `["Anna", "Bartek", "Celina", "Dominika"]`
    - **Oczekiwane wyjście:**
        - Najdłuższe słowo: `"Dominika"`

---

## 9. Złożona operacja `min`, `max`, `reduce`

**Opis:**  
Użyj metod `min`, `max` oraz `reduce`, aby znaleźć minimalną, maksymalną oraz zsumowaną wartość z listy liczb.

**Dane wejściowe:**
- Lista: `[3, 7, 2, 9, 1]`

**Oczekiwane wyjście:**
- Minimalna: `1`
- Maksymalna: `9`
- Suma: `22`

---

## 10. peek

**Opis:**  
Metoda `peek` pozwala na wykonanie akcji dla każdego elementu w strumieniu, bez zmiany elementów strumienia.

**Sygnatura metody:**
```java
public MyStream<T> peek(Consumer<T> action) {
    // Przechodzimy przez każdy element w 'elements'
        // Wykonaj 'action.accept(element)' na elemencie
    // Zwróć this lub nowy MyStream z oryginalnymi elementami
}
```

**Zadania do wykonania:**

1. **Wydrukuj każdy element listy podczas przetwarzania strumienia.**

    - **Dane wejściowe:**
        - Lista: `["A", "B", "C"]`
    - **Oczekiwane wyjście:**
        - Drukowane elementy: `A`, `B`, `C`

2. **Zwiększ wartość każdej liczby w liście o 1 i wydrukuj nową wartość (ale nie zmieniaj oryginalnej listy).**

    - **Dane wejściowe:**
        - Lista: `[1, 2, 3]`
    - **Oczekiwane wyjście:**
        - Drukowane elementy: `2`, `3`, `4`

---

## 11. flatMap

**Opis:**  
Metoda `flatMap` służy do przekształcania i spłaszczania złożonych struktur danych. Przekształca każdy element strumienia na inny strumień, a następnie łączy wszystkie te strumienie w jeden.

**Sygnatura metody:**
```java
public <R> MyStream<R> flatMap(Function<T, MyStream<R>> mapper) {
    // Utwórz nową listę do przechowywania wyników
    // Przechodzimy przez każdy element w 'elements'
        // Zastosuj 'mapper' do elementu, otrzymując MyStream<R>
        // Dodaj wszystkie elementy z otrzymanego MyStream<R> do nowej listy
    // Zwróć nowy MyStream<R> z połączonymi wynikami
}
```

**Zadania do wykonania:**

1. **Spłaszcz listę list liczb do pojedynczej listy liczb.**

    - **Dane wejściowe:**
        - Lista: `[[1, 2], [3, 4], [5, 6]]`
    - **Oczekiwane wyjście:**
        - Wynik: `[1, 2, 3, 4, 5, 6]`

2. **Z listy zdań utwórz listę wszystkich słów.**

    - **Dane wejściowe:**
        - Lista: `["Hello World", "Java Streams"]`
    - **Oczekiwane wyjście:**
        - Wynik: `["Hello", "World", "Java", "Streams"]`

---

## 12. Złożona operacja `flatMap`, `peek`

**Opis:**  
Użyj `flatMap` i `peek`, aby spłaszczyć listę list liczb i wydrukować każdy element przed jego dodaniem do wyniku.

**Dane wejściowe:**
- Lista: `[[1, 2], [3, 4], [5, 6]]`

**Oczekiwane wyjście:**
- Drukowane elementy: `1, 2, 3, 4, 5, 6`
- Wynik: `[1, 2, 3, 4, 5, 6]`

---

## 13. join

**Opis:**  
Metoda `join` łączy elementy strumienia w jeden ciąg znaków, oddzielając elementy podanym separatorem.

**Sygnatura metody:**
```java
public String join(CharSequence delimiter) {
    // Jeśli 'elements' jest puste, zwróć pusty string ""
    // Utwórz StringBuilder
    // Przechodzimy przez każdy element w 'elements'
        // Dodaj reprezentację stringową elementu do StringBuilder
        // Jeśli to nie jest ostatni element, dodaj 'delimiter' do StringBuilder
    // Zwróć StringBuilder jako string
}
```

**Zadania do wykonania:**

1. **Połącz imiona w liście w jeden ciąg znaków, oddzielając je przecinkami.**

    - **Dane wejściowe:**
        - Lista: `["Anna", "Bartek", "Cecylia"]`
    - **Oczekiwane wyjście:**
        - Wynik: `"Anna, Bartek, Cecylia"`

2. **Połącz liczby w liście w jeden ciąg znaków, oddzielając je myślnikiem.**

    - **Dane wejściowe:**
        - Lista: `[1, 2, 3, 4, 5]`
    - **Oczekiwane wyjście:**
        - Wynik: `"1-2-3-4-5"`

---

## 14. Zadania złożone

Po zaimplementowaniu wszystkich metod, spróbuj rozwiązać te bardziej złożone zadania.

### 1. Sprawdzenie dopasowania

Sprawdź, czy wszystkie liczby w liście są dodatnie i czy żadna liczba nie jest większa niż 100.

**Dane wejściowe:**
- Lista: `[12, 50, 70, 33]`

**Oczekiwane wyjście:**
- Wynik `allMatch`: `true`
- Wynik `noneMatch`: `true`

**Podpowiedź:**  
Użyj metod `allMatch` i `noneMatch` z odpowiednimi predykatami.

---

### 2. Operacje na liście nazw

Przekształć listę nazw, które zaczynają się na literę "A", do formy wielkich liter, połącz je w jeden ciąg i wyświetl.

**Dane wejściowe:**
- Lista: `["Anna", "Agnieszka", "Bartek", "Cecylia", "Adam"]`

**Oczekiwane wyjście:**
- Wynik: `"ANNA, AGNIESZKA, ADAM"`

**Podpowiedź:**  
Skorzystaj z `filter`, `map` i `join`, aby przekształcić i połączyć wyniki.

---

### 3. Sumowanie i sprawdzanie

Zsumuj wszystkie liczby w liście, a następnie sprawdź, czy wynik jest mniejszy niż 50.

**Dane wejściowe:**
- Lista: `[10, 15, 20]`

**Oczekiwane wyjście:**
- Suma: `45`
- Wynik `noneMatch`: `false`

**Podpowiedź:**  
Użyj `reduce` do sumowania i `noneMatch`, aby sprawdzić, czy wynik spełnia warunek.

---

### 4. Minimalna i maksymalna wartość

Znajdź minimalną i maksymalną wartość w liście liczb.

**Dane wejściowe:**
- Lista: `[3, 7, 9, 2, 8]`

**Oczekiwane wyjście:**
- Minimalna: `2`
- Maksymalna: `9`

**Podpowiedź:**  
Wykorzystaj metody `min` i `max` z odpowiednimi komparatorami.

---

### 5. Spłaszczenie i wydrukowanie elementów

Spłaszcz listę list liczb, a następnie wydrukuj każdy element i zsumuj wszystkie liczby.

**Dane wejściowe:**
- Lista: `[[1, 2], [3, 4], [5, 6]]`

**Oczekiwane wyjście:**
- Drukowane elementy: `1, 2, 3, 4, 5, 6`
- Suma: `21`

**Podpowiedź:**  
Użyj `flatMap`, `peek` do wydrukowania elementów, a następnie `reduce` do obliczenia sumy.
