# Własna implementacja Stream w Javie - część 2

## 1. allMatch

**Opis:**  
Metoda `allMatch` zwraca `true`, jeśli wszystkie elementy strumienia spełniają warunek określony przez `Predicate<T>`.

**Sygnatura metody:**
```java
public boolean allMatch(Predicate<T> predicate)
```

**Przykład użycia:**
```java
List<Integer> numbers = Arrays.asList(2, 4, 6, 8);
MyStream<Integer> stream = new MyStream<>(numbers);
boolean allEven = stream.allMatch(n -> n % 2 == 0);
System.out.println(allEven);  // Wyjście: true
```

---

## 2. anyMatch

**Opis:**  
Metoda `anyMatch` zwraca `true`, jeśli przynajmniej jeden element strumienia spełnia warunek określony przez `Predicate<T>`.

**Sygnatura metody:**
```java
public boolean anyMatch(Predicate<T> predicate)
```

**Przykład użycia:**
```java
List<Integer> numbers = Arrays.asList(1, 3, 4, 5);
MyStream<Integer> stream = new MyStream<>(numbers);
boolean hasEven = stream.anyMatch(n -> n % 2 == 0);
System.out.println(hasEven);  // Wyjście: true
```

---

## 3. Złożona operacja `allMatch`, `anyMatch`

**Opis:**  
Używając metod `allMatch` i `anyMatch`, sprawdź, czy wszystkie liczby są parzyste oraz czy przynajmniej jedna liczba jest większa niż 10.

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
public boolean noneMatch(Predicate<T> predicate)
```

**Przykład użycia:**
```java
List<Integer> numbers = Arrays.asList(1, 3, 5, 7);
MyStream<Integer> stream = new MyStream<>(numbers);
boolean noneEven = stream.noneMatch(n -> n % 2 == 0);
System.out.println(noneEven);  // Wyjście: true
```

---

## 5. reduce

**Opis:**  
Metoda `reduce` agreguje elementy strumienia na podstawie operatora binarnego. Przekazywana wartość `identity` jest wartością początkową.

**Sygnatura metody:**
```java
public T reduce(T identity, BinaryOperator<T> accumulator)
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

**Przykład użycia:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
MyStream<Integer> stream = new MyStream<>(numbers);
int sum = stream.reduce(0, Integer::sum);
System.out.println(sum);  // Wyjście: 10
```

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
public Optional<T> min(Comparator<T> comparator)
```

**Przykład użycia:**
```java
List<Integer> numbers = Arrays.asList(4, 2, 6, 1);
MyStream<Integer> stream = new MyStream<>(numbers);
Optional<Integer> minValue = stream.min(Integer::compareTo);
minValue.ifPresent(System.out::println);  // Wyjście: 1
```

---

## 8. max

**Opis:**  
Metoda `max` zwraca maksymalny element w strumieniu według podanego komparatora.

**Sygnatura metody:**
```java
public Optional<T> max(Comparator<T> comparator)
```

**Przykład użycia:**
```java
List<Integer> numbers = Arrays.asList(4, 2, 6, 1);
MyStream<Integer> stream = new MyStream<>(numbers);
Optional<Integer> maxValue = stream.max(Integer::compareTo);
maxValue.ifPresent(System.out::println);  // Wyjście: 6
```

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
public MyStream<T> peek(Consumer<T> action)
```

**Przykład użycia:**
```java
List<String> names = Arrays.asList("Anna", "Bartek", "Cecylia");
MyStream<String> stream = new MyStream<>(names);
stream.peek(System.out::println).toList();
// Wyjście:
// Anna
// Bartek
// Cecylia
```

---

## 11. flatMap

**Opis:**  
Metoda `flatMap` w Javie służy do "spłaszczenia" złożonych struktur danych, takich jak lista list. W jej działaniu
najpierw przekształca każdy element strumienia na inny strumień, a następnie łączy wszystkie te strumienie w jeden
strumień. To oznacza, że zamiast uzyskać strumień strumieni, otrzymujemy jeden płaski strumień, co ułatwia operacje na
danych.

#### Wyjaśnienie krok po kroku:
1. **Przekształcenie (mapowanie)**: Dla każdego elementu oryginalnego strumienia wykonujemy operację, która zwraca strumień lub strukturę danych. Dla przykładu: jeśli mamy listę list, każdą podlistę możemy przekształcić w osobny strumień.

2. **Spłaszczenie**: Zamiast zwracać strumień strumieni (czyli zagnieżdżoną strukturę), `flatMap` spłaszcza je, tworząc pojedynczy strumień zawierający wszystkie elementy z każdej z podstruktur.


#### Porównanie z `map`:
Gdybyśmy użyli metody `map`, wynikiem byłaby lista strumieni, czyli bardziej złożona struktura, np. lista list. `flatMap` natomiast "rozplątuje" te struktury, tworząc płaski, jednowymiarowy strumień.

**Sygnatura metody:**
```java
public <R> MyStream<R> flatMap(Function<T, MyStream<R>> mapper)
```

**Przykład użycia:**
```java
List<List<Integer>> numbers = Arrays.asList(Arrays.asList(1, 2), Arrays.asList(3, 4));
MyStream<List<Integer>> stream = new MyStream<>(numbers);
MyStream<Integer> flattened = stream.flatMap(list -> new MyStream<>(list));
List<Integer> result = flattened.toList();
System.out.println(result);  // Wyjście: [1, 2, 3, 4]
```

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
public String join(CharSequence delimiter)
```

**Przykład użycia:**
```java
List<String> names = Arrays.asList("Anna", "Bartek", "Cecylia");
MyStream<String> stream = new MyStream<>(names);
String result = stream.join(", ");
System.out.println(result);  // Wyjście: "Anna, Bartek, Cecylia"
```

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
