# Dodanie obsługi operacji pośrednich i terminalnych w `Stream` oraz short-circuiting

## 1. Wprowadzenie do operacji na `Stream` w Javie

Strumienie (`Stream`) w Javie to potężne narzędzie do przetwarzania kolekcji danych w sposób funkcyjny, umożliwiając wykonywanie różnych operacji na danych w sposób deklaratywny. Strumień w Javie można sobie wyobrazić jako ciąg danych, który przemieszcza się przez różne operacje. Wszystkie te operacje możemy podzielić na dwa główne typy:
- **Operacje pośrednie** (intermediate operations)
- **Operacje terminalne** (terminal operations)

Strumienie mają również pewną szczególną cechę, jaką jest **leniwe wywoływanie** (`lazy evaluation`), co oznacza, że operacje pośrednie nie są wykonywane natychmiast po ich zadeklarowaniu, a dopiero wtedy, gdy zostanie wywołana operacja terminalna.

## 2. Operacje pośrednie (intermediate operations)

Operacje pośrednie w strumieniach to takie operacje, które zwracają nowy strumień jako wynik. Mogą być łańcuchowane, ponieważ wynik jednej operacji pośredniej jest strumieniem, który może zostać przekazany do kolejnej operacji.

Przykłady operacji pośrednich to:
- `filter(Predicate<? super T> predicate)` – filtruje elementy, które spełniają dany warunek.
- `map(Function<? super T, ? extends R> mapper)` – przekształca elementy strumienia na inny typ.
- `distinct()` – usuwa duplikaty z strumienia.
- `sorted()` lub `sorted(Comparator<? super T> comparator)` – sortuje elementy strumienia.
- `limit(long maxSize)` – ogranicza strumień do określonej liczby elementów.
- `skip(long n)` – pomija określoną liczbę pierwszych elementów strumienia.

### Leniwe wywoływanie operacji pośrednich

Jedną z najważniejszych cech operacji pośrednich jest to, że są one **leniwe**. Oznacza to, że deklarowanie tych operacji na strumieniu nie powoduje ich natychmiastowego wykonania. Strumień jest "konfigurowany" do momentu, aż zostanie wywołana operacja terminalna. Operacje pośrednie są "odkładane" i będą wykonane dopiero, gdy strumień zostanie zakończony.

## 3. Operacje terminalne (terminal operations)

Operacje terminalne w strumieniach to operacje, które kończą strumień i zwracają konkretny wynik (czy to wartość, kolekcję, czy też efekt uboczny, jak wydrukowanie elementów). Po wykonaniu operacji terminalnej strumień jest "wykorzystany" i nie można na nim wykonywać dalszych operacji.

Przykłady operacji terminalnych to:
- `forEach(Consumer<? super T> action)` – wykonuje akcję dla każdego elementu strumienia.
- `collect(Collector<? super T, A, R> collector)` – zbiera elementy strumienia do określonej kolekcji.
- `reduce(BinaryOperator<T> accumulator)` – agreguje elementy strumienia do jednej wartości.
- `findFirst()` lub `findAny()` – zwraca pierwszy element strumienia.
- `count()` – zwraca liczbę elementów w strumieniu.
- `anyMatch()`, `allMatch()`, `noneMatch()` – sprawdzają, czy elementy strumienia spełniają określone warunki.

Operacje terminalne kończą strumień, dlatego też dopiero one uruchamiają wykonanie operacji pośrednich. Bez operacji terminalnej żadna z operacji pośrednich nie zostanie wykonana.

## 4. Short-circuiting w strumieniach

**Short-circuiting** to mechanizm w strumieniach, który pozwala na wcześniejsze zakończenie przetwarzania danych, zanim cały strumień zostanie przetworzony. Działa to w taki sposób, że operacje terminalne, które mogą dać wynik przed przetworzeniem całego strumienia, zakończą strumień szybciej.

Operacje short-circuiting są zazwyczaj związane z operacjami, które mogą dać odpowiedź bez konieczności przetwarzania wszystkich elementów, np.:
- `findFirst()` – kończy przetwarzanie po znalezieniu pierwszego elementu.
- `anyMatch(Predicate<? super T> predicate)` – kończy przetwarzanie, gdy znajdzie pierwszy element spełniający warunek.
- `limit(long maxSize)` – ogranicza przetwarzanie do określonej liczby elementów.

Short-circuiting jest bardzo wydajnym mechanizmem, ponieważ pozwala na przerwanie przetwarzania strumienia, co może znacznie oszczędzić zasoby w przypadku dużych kolekcji danych.

## 5. Przykład: Leniwe wywoływanie i short-circuiting

Poniżej przykład pokazujący działanie operacji pośrednich, terminalnych oraz short-circuiting w Javie:

```java
import java.util.Arrays;
import java.util.List;

public class StreamExample {

    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // Leniwe wywoływanie operacji pośrednich
        numbers.stream()
            .filter(n -> {
                System.out.println("Filtering: " + n);
                return n % 2 == 0;
            })  // Operacja pośrednia (leniwa)
            .map(n -> {
                System.out.println("Mapping: " + n);
                return n * 2;
            })  // Operacja pośrednia (leniwa)
            .limit(3)  // Short-circuiting
            .forEach(n -> System.out.println("Final result: " + n));  // Operacja terminalna (uruchamia strumień)
    }
}
```

### Wyjaśnienie przykładu:

1. **Leniwe wywoływanie**: Operacje `filter` i `map` są operacjami pośrednimi. Zauważ, że żadne z tych operacji nie zostaną wykonane, dopóki nie zostanie wywołana operacja terminalna `forEach`.

2. **Short-circuiting**: Gdy tylko `limit(3)` ograniczy liczbę elementów do trzech, strumień zakończy przetwarzanie wcześniej, zamiast przetwarzać wszystkie 10 elementów.

3. **Kolejność wykonania**: Dla każdego elementu:
    - Najpierw element przechodzi przez `filter`.
    - Następnie, jeśli spełnia warunek filtru, przechodzi przez `map`.
    - Po przetworzeniu trzech elementów, strumień kończy się dzięki `limit(3)`.

Wyjście programu będzie wyglądać tak:

```
Filtering: 1
Filtering: 2
Mapping: 2
Final result: 4
Filtering: 3
Filtering: 4
Mapping: 4
Final result: 8
Filtering: 5
Filtering: 6
Mapping: 6
Final result: 12
```

Zauważ, że operacje pośrednie `filter` i `map` są wywoływane leniwie, dopiero w momencie, gdy operacja terminalna (`forEach`) uruchamia przetwarzanie strumienia. Dodatkowo, strumień kończy się szybciej dzięki `limit(3)`, co jest przykładem **short-circuitingu**.

---

## 6. Podsumowanie

- **Operacje pośrednie** są leniwe i pozwalają na zbudowanie "konfiguracji" strumienia, która nie zostanie wykonana, dopóki nie wywołamy operacji terminalnej.
- **Operacje terminalne** uruchamiają przetwarzanie strumienia i zwracają wynik lub wykonują efekt uboczny.
- **Short-circuiting** pozwala na zakończenie przetwarzania strumienia wcześniej, co może poprawić wydajność przetwarzania danych.

---

# Implementacja mechanizmów

Aby zaimplementować mechanizm obsługi operacji pośrednich, terminalnych oraz short-circuiting, należy zmienić sposób przechowywania i przetwarzania operacji w strumieniu. Zamiast od razu przetwarzać elementy podczas deklaracji operacji pośrednich (jak to jest obecnie w klasie `MyStream`), można zastosować **leniwe wywoływanie** poprzez przechowywanie operacji do momentu, aż zostanie wywołana operacja terminalna.

### Kluczowe kroki:

1. **Przechowywanie operacji**: Operacje pośrednie (takie jak `filter`, `map`, itd.) będą jedynie rejestrowane, a nie wykonywane od razu. Dopiero operacje terminalne uruchomią wszystkie wcześniej zadeklarowane operacje.
2. **Short-circuiting**: Operacje terminalne, takie jak `findFirst()` czy `limit()`, mogą zakończyć przetwarzanie strumienia wcześniej, gdy spełnione są pewne warunki.

Oto przykładowa implementacja, która obsługuje te mechanizmy:

### Implementacja mechanizmu leniwego wywoływania i short-circuiting

```java
import java.util.*;
import java.util.function.*;
import java.util.Optional;

public class MyStream<T> {

    private List<T> elements;
    private List<Function<T, T>> intermediateOperations;  // Lista operacji pośrednich
    private int limit = Integer.MAX_VALUE;  // Limit do obsługi short-circuiting

    public MyStream(List<T> elements) {
        this.elements = elements;
        this.intermediateOperations = new ArrayList<>();  // Inicjalizacja operacji pośrednich
    }

    // Leniwe dodanie operacji filter
    public MyStream<T> filter(Predicate<T> predicate) {
        intermediateOperations.add(element -> predicate.test(element) ? element : null);
        return this;
    }

    // Leniwe dodanie operacji map
    public <R> MyStream<R> map(Function<? super T, ? extends R> mapper) {
        MyStream<R> newStream = new MyStream<>(new ArrayList<>());
        newStream.intermediateOperations.addAll((List<Function<R, R>>) (List<?>) this.intermediateOperations);
        newStream.intermediateOperations.add(element -> mapper.apply((T) element)); // typowanie T na R
        return newStream;
    }

    // Leniwe dodanie operacji limit (short-circuiting)
    public MyStream<T> limit(int maxSize) {
        this.limit = maxSize;  // Zapisz limit jako ograniczenie
        return this;
    }

    // Przetwarzanie operacji pośrednich
    private <R> List<R> processElements() {
        List<R> processed = new ArrayList<>();
        int count = 0;  // Do kontrolowania limitu
        for (T element : elements) {
            T processedElement = element;
            for (Function<T, T> operation : intermediateOperations) {
                processedElement = operation.apply(processedElement);
                if (processedElement == null) {
                    break;  // Element został przefiltrowany, pomiń go
                }
            }
            if (processedElement != null) {
                processed.add((R) processedElement);
                count++;
                if (count >= limit) {  // Przerwij, gdy osiągnięto limit (short-circuiting)
                    break;
                }
            }
        }
        return processed;
    }

    // Operacja terminalna forEach
    public void forEach(Consumer<? super T> action) {
        List<T> processed = processElements();
        for (T element : processed) {
            action.accept(element);
        }
    }

    // Operacja terminalna count
    public long count() {
        return processElements().size();
    }

    // Operacja terminalna findFirst (short-circuiting)
    public Optional<T> findFirst() {
        for (T element : elements) {
            T processedElement = element;
            for (Function<T, T> operation : intermediateOperations) {
                processedElement = operation.apply(processedElement);
                if (processedElement == null) {
                    break;  // Element odfiltrowany
                }
            }
            if (processedElement != null) {
                return Optional.of(processedElement);
            }
        }
        return Optional.empty();
    }

    // Metoda distinct (leniwa)
    public MyStream<T> distinct() {
        intermediateOperations.add(new HashSet<>()::add);
        return this;
    }
}
```

### Wyjaśnienie:
1. **Operacje pośrednie (`filter`, `map`)**: Zamiast natychmiastowego przetwarzania, każda operacja jest rejestrowana w liście `intermediateOperations`. Każda operacja pośrednia jest przechowywana jako funkcja, która zostanie później wywołana w momencie przetwarzania.

2. **Operacje terminalne (`forEach`, `count`, `findFirst`)**: Kiedy wywoływana jest operacja terminalna, jak np. `forEach`, wszystkie zarejestrowane operacje pośrednie są wykonywane w kolejności dla każdego elementu z listy `elements`. Operacja `findFirst` przerywa przetwarzanie po znalezieniu pierwszego elementu, co jest przykładem **short-circuitingu**.

3. **Przykład short-circuiting**: Operacja `findFirst` przerywa przetwarzanie po znalezieniu pierwszego elementu, który spełnia warunki. Podobnie `limit(int)` ogranicza liczbę przetwarzanych elementów.

### Przykład użycia:

```java
import java.util.Arrays;

public class MyStreamExample {

    public static void main(String[] args) {
        MyStream<Integer> stream = new MyStream<>(Arrays.asList(1, 2, 3, 4, 5, 6));

        // Przykład leniwego wywoływania operacji pośrednich i short-circuitingu
        stream.filter(n -> n % 2 == 0)   // Operacja pośrednia (leniwa)
              .map(n -> n * 2)           // Operacja pośrednia (leniwa)
              .limit(2)                  // Short-circuiting
              .forEach(n -> System.out.println("Result: " + n));  // Operacja terminalna

        // Wywołanie operacji terminalnej findFirst z short-circuiting
        stream.filter(n -> n > 2).findFirst().ifPresent(n -> System.out.println("First element: " + n));
    }
}
```

### Wyjaśnienie działania:

1. **Leniwe przetwarzanie**: Zauważ, że `filter` i `map` są przetwarzane tylko wtedy, gdy wywoływana jest operacja terminalna (`forEach` lub `findFirst`). Dzięki temu operacje pośrednie są leniwie wykonywane.

2. **Short-circuiting**: Funkcja `limit(2)` zatrzymuje przetwarzanie po pierwszych dwóch elementach, a `findFirst` kończy przetwarzanie po znalezieniu pierwszego elementu, który spełnia warunki.
