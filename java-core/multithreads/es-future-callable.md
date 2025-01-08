**ExecutorService, Callable i Future - Teoria i Praktyka**

---

### Wprowadzenie

Programowanie współbieżne w Javie umożliwia efektywne wykorzystanie mocy obliczeniowej współczesnych procesorów wielordzeniowych. W tym artykule skupimy się na trzech kluczowych elementach tego paradygmatu:

- **ExecutorService**: Mechanizm zarządzania pulami wątków.
- **Callable**: Interfejs umożliwiający tworzenie zadań zwracających wynik.
- **Future**: Interfejs reprezentujący wynik asynchronicznego obliczenia.

Poznasz teorię stojącą za tymi narzędziami, zobaczysz praktyczne przykłady ich zastosowania oraz będziesz mieć okazję rozwiązać zadania, które pomogą utrwalić zdobytą wiedzę.

---

## 1. ExecutorService

### 1.1. Co to jest ExecutorService?

**ExecutorService** to interfejs z pakietu `java.util.concurrent`, który reprezentuje mechanizm wykonywania zadań asynchronicznych za pomocą puli wątków. Zamiast tworzyć i zarządzać wątkami bezpośrednio, możesz zlecić zadania ExecutorService, który zajmie się ich wykonaniem.

### 1.2. Dlaczego warto używać ExecutorService?

- **Efektywność**: Zarządzanie pulą wątków pozwala na ponowne wykorzystanie istniejących wątków, co zmniejsza narzut związany z ich tworzeniem i niszczeniem.
- **Skalowalność**: Możesz łatwo dostosować liczbę wątków w puli do potrzeb aplikacji.
- **Prostota**: Uproszczone zarządzanie wątkami i zadaniami.

### 1.3. Tworzenie ExecutorService

Możesz utworzyć ExecutorService za pomocą klasy `Executors`, która dostarcza kilka metod fabrykujących:

- `newFixedThreadPool(int nThreads)`: Tworzy pulę z określoną liczbą wątków.
- `newCachedThreadPool()`: Tworzy pulę, która tworzy nowe wątki w razie potrzeby, ale ponownie wykorzystuje wątki, gdy są dostępne.
- `newSingleThreadExecutor()`: Tworzy pulę z jednym wątkiem.

**Przykład:**

```java
ExecutorService executor = Executors.newFixedThreadPool(5);
```

### 1.4. Wykonywanie zadań

ExecutorService pozwala na zlecanie zadań za pomocą metod:

- `execute(Runnable command)`: Wykonuje zadanie bez zwracania wyniku.
- `submit(Callable<T> task)`: Wykonuje zadanie i zwraca obiekt `Future` reprezentujący wynik.

**Przykład:**

```java
executor.execute(new RunnableTask());
executor.submit(new CallableTask());
```

### 1.5. Zamknięcie ExecutorService

Po zakończeniu pracy z ExecutorService należy go zamknąć:

- `shutdown()`: Inicjuje uporządkowane zamknięcie, nie przyjmuje nowych zadań, ale wykonuje już złożone.
- `shutdownNow()`: Próbuje zatrzymać wszystkie aktywne zadania i zwraca listę zadań oczekujących.

**Przykład:**

```java
executor.shutdown();
```

### 1.6. Przykład użycia ExecutorService

**Kod:**

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorServiceExample {
    public static void main(String[] args) {
        // Tworzenie puli wątków z 3 wątkami
        ExecutorService executor = Executors.newFixedThreadPool(3);

        // Definiowanie zadań
        Runnable task1 = () -> System.out.println("Zadanie 1 wykonywane przez " + Thread.currentThread().getName());
        Runnable task2 = () -> System.out.println("Zadanie 2 wykonywane przez " + Thread.currentThread().getName());
        Runnable task3 = () -> System.out.println("Zadanie 3 wykonywane przez " + Thread.currentThread().getName());

        // Zlecanie zadań
        executor.execute(task1);
        executor.execute(task2);
        executor.execute(task3);

        // Zamknięcie ExecutorService
        executor.shutdown();
    }
}
```

**Wyjście (przykładowe):**

```
Zadanie 1 wykonywane przez pool-1-thread-1
Zadanie 2 wykonywane przez pool-1-thread-2
Zadanie 3 wykonywane przez pool-1-thread-3
```

### 1.7. Zadania praktyczne

#### **Zadanie 1:**

Utwórz program, który korzysta z `ExecutorService` z pulą 4 wątków do wykonania 8 zadań. Każde zadanie powinno wypisać swój numer oraz nazwę wątku, który je wykonuje.

**Podpowiedź:**

- Użyj pętli do zlecenia 8 zadań.
- Wykorzystaj `Runnable` jako zadanie.

#### **Zadanie 2:**

Zaimplementuj program, który wykorzystuje `ScheduledExecutorService` do wykonywania zadania co 2 sekundy, 5 razy.

**Podpowiedź:**

- Użyj `ScheduledExecutorService` i metody `scheduleAtFixedRate`.
- Skonfiguruj zadanie do anulowania po 5 wykonaniach.

---

## 2. Callable i Future

### 2.1. Interfejs Callable

**Callable** to interfejs reprezentujący zadanie, które zwraca wynik i może rzucać wyjątki. Jest to podobne do interfejsu `Runnable`, ale z możliwością zwracania wartości.

**Metoda:**

```java
V call() throws Exception;
```

### 2.2. Interfejs Future

**Future** to interfejs reprezentujący wynik asynchronicznego obliczenia. Pozwala na:

- Sprawdzenie, czy zadanie zostało ukończone (`isDone()`).
- Anulowanie zadania (`cancel()`).
- Pobranie wyniku (`get()`), co może zablokować wątek, jeśli zadanie nie jest jeszcze ukończone.

### 2.3. Różnice między Runnable a Callable

| Cecha            | Runnable              | Callable          |
|------------------|-----------------------|-------------------|
| Metoda           | `void run()`          | `V call()`        |
| Zwraca wartość   | Nie                   | Tak               |
| Rzuca wyjątki    | Tylko niekontrolowane | Tak, kontrolowane |
| Używany z Future | Nie                   | Tak               |

### 2.4. Przykład użycia Callable i Future

**Kod:**

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class CallableFutureExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        // Zadanie Callable
        Callable<Integer> task = () -> {
            // Symulacja długotrwałego obliczenia
            Thread.sleep(2000);
            return 42;
        };

        // Zlecanie zadania i otrzymanie Future
        Future<Integer> future = executor.submit(task);

        // Wykonywanie innych operacji...

        try {
            // Pobieranie wyniku
            Integer result = future.get(); // Blokuje, jeśli zadanie nie jest ukończone
            System.out.println("Wynik: " + result);
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        } finally {
            executor.shutdown();
        }
    }
}
```

**Wyjście:**

```
Wynik: 42
```

### 2.5. Zadania praktyczne

#### **Zadanie 1:**

Napisz program, który korzysta z `Callable` do obliczenia silni liczby podanej przez użytkownika. Użyj `Future`, aby pobrać wynik i wyświetlić go.

**Podpowiedź:**

- Poproś użytkownika o podanie liczby.
- Zaimplementuj `Callable<Integer>` do obliczenia silni.
- Obsłuż wyjątki.

#### **Zadanie 2:**

Utwórz program, który uruchamia 3 zadania `Callable`, każde zwracające losową liczbę po losowym czasie (np. od 1 do 3 sekund). Skorzystaj z `ExecutorService` z pulą 3 wątków. Wyświetl wyniki po zakończeniu wszystkich zadań.

**Podpowiedź:**

- Użyj `ThreadLocalRandom` do generowania losowych liczb i opóźnień.
- Przechowuj `Future` dla każdego zadania i użyj `get()` do pobrania wyników.

---

## 3. Głębsze zrozumienie Future

### 3.1. Metody interfejsu Future

- **`boolean cancel(boolean mayInterruptIfRunning)`**: Próbuje anulować zadanie.
- **`boolean isCancelled()`**: Sprawdza, czy zadanie zostało anulowane.
- **`boolean isDone()`**: Sprawdza, czy zadanie zostało ukończone.
- **`V get()`**: Pobiera wynik, blokując wątek, jeśli zadanie nie jest ukończone.
- **`V get(long timeout, TimeUnit unit)`**: Pobiera wynik, czekając maksymalnie określony czas.

### 3.2. Przykład z anulowaniem zadania

**Kod:**

```java
Future<Integer> future = executor.submit(task);

try {
    Integer result = future.get(1, TimeUnit.SECONDS);
    System.out.println("Wynik: " + result);
} catch (TimeoutException e) {
    System.out.println("Zadanie trwa zbyt długo. Anulowanie...");
    future.cancel(true);
}
```

### 3.3. Zadania praktyczne

#### **Zadanie 1:**

Zmodyfikuj **Zadanie 2** z poprzedniej sekcji tak, aby anulowało zadanie, jeśli nie zakończy się w ciągu 2 sekund.

#### **Zadanie 2:**

Napisz program, który uruchamia listę zadań `Callable` (np. 10 zadań), każde zwracające swoje ID po losowym czasie. Skorzystaj z `ExecutorService` i metody `invokeAll()`, aby uruchomić wszystkie zadania i pobrać wyniki.

**Podpowiedź:**

- `invokeAll()` zwraca listę `Future`.
- Zadania będą wykonywane równolegle.

---

## 4. Podsumowanie

- **ExecutorService** ułatwia zarządzanie pulami wątków i wykonywanie zadań asynchronicznych.
- **Callable** umożliwia tworzenie zadań zwracających wartość i rzucających wyjątki.
- **Future** pozwala na monitorowanie stanu zadań i pobieranie wyników asynchronicznych obliczeń.
- Różnica między `Runnable` a `Callable` jest kluczowa przy wyborze odpowiedniego interfejsu do zadania.
