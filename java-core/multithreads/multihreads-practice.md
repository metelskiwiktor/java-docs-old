### Wielowątkowość w Javie – Wprowadzenie

Wielowątkowość (multithreading) to mechanizm, który pozwala aplikacjom na jednoczesne wykonywanie wielu wątków, co
umożliwia realizację zadań współbieżnie, a w niektórych przypadkach nawet równolegle. Programowanie wielowątkowe jest
szczególnie istotne w aplikacjach, które muszą obsługiwać dużą liczbę operacji wejścia-wyjścia (I/O), takich jak serwery
aplikacji, usługi przetwarzania danych w czasie rzeczywistym czy systemy oparte na zdarzeniach. Java zapewnia solidne
wsparcie dla wielowątkowości i współbieżności poprzez klasę `Thread`, interfejs `Runnable`, oraz narzędzia takie jak
`ExecutorService`.

---

### 1. Wątki w Javie

Wątki (threads) w Javie to jednostki wykonawcze, które mogą być uruchamiane równolegle. JVM tworzy wątek główny (main
thread), który wykonuje kod w metodzie `main()`. Możemy jednak tworzyć dodatkowe wątki, aby równolegle wykonywać inne
zadania.

#### 1.1. Tworzenie wątków

W Javie możemy utworzyć nowy wątek na dwa sposoby:

- **Poprzez rozszerzenie klasy `Thread`:**

```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Wątek: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start(); // Uruchomienie wątku
    }
}
```

- **Poprzez implementację interfejsu `Runnable`:**

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Wątek: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable());
        thread.start(); // Uruchomienie wątku
    }
}
```

Metoda `run()` zawiera kod, który będzie wykonywany przez dany wątek. Aby uruchomić wątek, wywołujemy metodę `start()`,
która w tle tworzy nowy wątek systemowy i uruchamia metodę `run()`.

#### 1.2. Metody zarządzania wątkami

Java oferuje wiele metod do zarządzania wątkami:

- **`Thread.sleep(long millis)`**: Wstrzymuje działanie wątku na określony czas.
- **`Thread.yield()`**: Sugeruje, aby wątek zrezygnował z procesora i pozwolił innym wątkom kontynuować.
- **`Thread.join()`**: Czeka, aż dany wątek zakończy swoje działanie przed kontynuowaniem bieżącego wątku.

---

### 2. Synchronizacja

W środowisku wielowątkowym, wątki mogą próbować jednocześnie uzyskać dostęp do tych samych zasobów, co może prowadzić do
problemów takich jak wyścigi (race conditions). Synchronizacja pozwala na kontrolę dostępu do współdzielonych zasobów.

#### 2.1. Bloki synchronizowane

Blok synchronizowany (`synchronized`) zapewnia, że tylko jeden wątek naraz może uzyskać dostęp do danej sekcji kodu.

```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Końcowa wartość licznika: " + counter.getCount());
    }
}
```

W tym przykładzie metoda `increment()` jest zsynchronizowana, co oznacza, że tylko jeden wątek może ją jednocześnie
wykonywać. Dzięki temu unikamy wyścigu o dostęp do zmiennej `count`.

#### 2.2. `volatile`

Zmienna oznaczona jako `volatile` gwarantuje, że jej wartość będzie od razu widoczna dla innych wątków. Zwykłe zmienne
mogą być buforowane w lokalnych pamięciach procesora, co oznacza, że ich zmiany mogą nie być natychmiast widoczne dla
innych wątków.

```java
class Flag {
    private volatile boolean running = true;

    public void stop() {
        running = false;
    }

    public boolean isRunning() {
        return running;
    }
}
```

Zmienna `running` jest `volatile`, co zapewnia, że jej zmiana dokonana w jednym wątku będzie natychmiast widoczna w
innych wątkach.

---

### 3. Zarządzanie wątkami z ExecutorService

Zamiast tworzyć i zarządzać wątkami bezpośrednio, możemy użyć klasy `ExecutorService`, która upraszcza zarządzanie pulą
wątków i zadaniami współbieżnymi.

#### 3.1. Tworzenie puli wątków

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);

        for (int i = 0; i < 5; i++) {
            executor.submit(() -> {
                System.out.println("Wątek: " + Thread.currentThread().getName());
            });
        }

        executor.shutdown(); // Zakończ pracę executor
    }
}
```

W tym przykładzie utworzono pulę trzech wątków (`newFixedThreadPool(3)`), które będą obsługiwały zadania wysyłane za
pomocą metody `submit()`.

#### 3.2. Użycie Future i Callable

Jeśli potrzebujemy zwrócić wynik z wątku, możemy użyć interfejsu `Callable` oraz klasy `Future`.

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executor = Executors.newFixedThreadPool(3);

        Callable<Integer> task = () -> {
            int sum = 0;
            for (int i = 1; i <= 10; i++) {
                sum += i;
            }
            return sum;
        };

        Future<Integer> future = executor.submit(task);
        System.out.println("Suma: " + future.get());

        executor.shutdown();
    }
}
```

Interfejs `Callable` pozwala na zwrócenie wartości, a `Future` umożliwia pobranie wyniku, gdy wątek zakończy swoje działanie.

---

### 4. Nowoczesne podejścia do współbieżności w Javie

Od Javy 5 dodano wiele narzędzi wspomagających programowanie współbieżne, które ułatwiają zarządzanie wątkami i poprawiają czytelność kodu:

- **`ConcurrentHashMap`**: Specjalna wersja mapy, która wspiera współbieżny dostęp bez potrzeby używania synchronizacji.
- **`CountDownLatch`**: Narzędzie, które pozwala jednemu lub więcej wątkom czekać, aż inne wątki zakończą określone zadanie.
- **`CyclicBarrier`**: Umożliwia synchronizację wątków, które muszą spotkać się w jednym punkcie, zanim kontynuują wykonywanie dalszych działań.
- **`Semaphore`**: Kontroluje dostęp do zasobów, pozwalając na jednoczesny dostęp tylko określonej liczbie wątków.

Przykład użycia `CountDownLatch`:

```java
import java.util.concurrent.CountDownLatch;

public class Main {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch latch = new CountDownLatch(3);

        Runnable task = () -> {
            System.out.println("Wątek: " + Thread.currentThread().getName());
            latch.countDown();
        };

        new Thread(task).start();
        new Thread(task).start();
        new Thread(task).start();

        latch.await(); // Czeka, aż wszystkie wątki zakończą swoje zadanie
        System.out.println("Wszystkie wąt

ki zakończone.");
    }
}
```

---

### 5. Współbieżność w Javie 8 i nowszych

Od Javy 8 wprowadzono wyrażenia lambda i strumienie, co dodatkowo ułatwia programowanie współbieżne. Możemy np. równolegle przetwarzać dane w strumieniach:

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        Arrays.stream(numbers)
            .parallel()
            .forEach(number -> System.out.println("Wątek: " + Thread.currentThread().getName() + " - " + number));
    }
}
```

Równoległe strumienie (`parallelStream`) umożliwiają rozdzielenie zadań na wiele wątków i ich jednoczesne przetwarzanie.
