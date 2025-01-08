# **Multithreading w Javie: Podstawy i Praktyczne Przykłady**

---

## **Spis treści**

1. [Proces vs Wątek](#proces-vs-wątek)
2. [Klasa `Thread` a Interfejs `Runnable`](#klasa-thread-a-interfejs-runnable)
3. [Sposoby Tworzenia Wątku przez `Thread` i `Runnable`](#sposoby-tworzenia-wątku-przez-thread-i-runnable)
4. [Przykład Nieprzemyślanej Pracy Wątków i Nadpisywania Pól Klasy](#przykład-nieprzemyślanej-pracy-wątków-i-nadpisywania-pól-klasy)
5. [Sposoby na Synchronizację Wątków](#sposoby-na-synchronizację-wątków)
6. [Przykład Producenta i Konsumenta (Bez `ExecutorService`, Tylko Czyste Wątki)](#przykład-producenta-i-konsumenta-bez-executorservice-tylko-czyste-wątki)

---

## **1. Proces vs Wątek**

### **Proces**

- **Definicja**: Proces to samodzielnie działający program uruchomiony w systemie operacyjnym, posiadający własną przestrzeń adresową w pamięci.
- **Cechy**:
    - Izolacja: Procesy są od siebie izolowane; błąd w jednym procesie nie wpływa bezpośrednio na inne.
    - Zasoby: Każdy proces ma przydzielone własne zasoby systemowe, takie jak pamięć, uchwyty do plików itp.
    - Przełączanie kontekstu: Przełączanie między procesami jest kosztowne, ponieważ wymaga zmiany kontekstu całego procesu.

### **Wątek**

- **Definicja**: Wątek (ang. *thread*) to lekka jednostka wykonawcza w ramach procesu. Wiele wątków może działać równocześnie w jednym procesie, dzieląc tę samą przestrzeń adresową.
- **Cechy**:
    - Współdzielenie zasobów: Wątki w ramach jednego procesu mają dostęp do wspólnej pamięci i zasobów.
    - Wydajność: Przełączanie między wątkami jest szybsze niż między procesami.
    - Komunikacja: Wątki mogą łatwo komunikować się i wymieniać dane, ale wymaga to synchronizacji.

### **Porównanie**

| Cecha               | Proces                           | Wątek                               |
|---------------------|----------------------------------|-------------------------------------|
| Przestrzeń adresowa | Własna, odizolowana              | Współdzielona z innymi wątkami      |
| Przełączanie        | Kosztowne (zmiana kontekstu)     | Szybkie                             |
| Izolacja            | Wysoka (błąd nie wpływa na inne) | Niska (błąd może wpłynąć na proces) |
| Komunikacja         | Trudna (IPC)                     | Łatwa (wspólna pamięć)              |

---

## **2. Klasa `Thread` a Interfejs `Runnable`**

### **Klasa `Thread`**

- **Opis**: `Thread` to klasa w Javie reprezentująca wątek wykonawczy.
- **Dziedziczenie**: Możesz utworzyć własny wątek, dziedzicząc po klasie `Thread` i nadpisując metodę `run()`.
- **Zalety**:
    - Prosta implementacja dla prostych zadań.
- **Wady**:
    - Brak możliwości dziedziczenia z innej klasy (Java nie obsługuje wielokrotnego dziedziczenia).

### **Interfejs `Runnable`**

- **Opis**: `Runnable` to interfejs funkcyjny z jedną metodą `run()`.
- **Implementacja**: Tworzysz klasę implementującą `Runnable` i przekazujesz jej instancję do konstruktora `Thread`.
- **Zalety**:
    - Elastyczność: Możesz dziedziczyć z innej klasy.
    - Oddzielenie logiki wątku od jego wykonania.
- **Wady**:
    - Wymaga dodatkowego kroku utworzenia obiektu `Thread`.

### **Porównanie**

- **Dziedziczenie**:
    - `Thread`: Musisz dziedziczyć po `Thread`.
    - `Runnable`: Możesz implementować `Runnable` i dziedziczyć z innej klasy.
- **Organizacja kodu**:
    - `Thread`: Logika wątku jest związana z klasą wątku.
    - `Runnable`: Logika jest oddzielona od mechanizmu wykonawczego.

---

## **3. Sposoby Tworzenia Wątku przez `Thread` i `Runnable`**

### **Tworzenie wątku przez dziedziczenie po `Thread`**

**Przykład:**

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Wątek działa: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start(); // Uruchamia nowy wątek
    }
}
```

**Wyjaśnienie:**

- Tworzymy klasę `MyThread`, która dziedziczy po `Thread`.
- Nadpisujemy metodę `run()`, w której umieszczamy kod do wykonania.
- W `main` tworzymy instancję `MyThread` i wywołujemy `start()`.

### **Tworzenie wątku przez implementację `Runnable`**

**Przykład:**

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Wątek działa: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable());
        thread.start(); // Uruchamia nowy wątek
    }
}
```

**Wyjaśnienie:**

- Tworzymy klasę `MyRunnable`, która implementuje `Runnable`.
- Implementujemy metodę `run()`.
- W `main` tworzymy instancję `Thread`, przekazując do konstruktora `MyRunnable`.
- Wywołujemy `start()` na obiekcie `Thread`.

### **Użycie wyrażenia lambda z `Runnable` (Java 8+)**

**Przykład:**

```java
public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            System.out.println("Wątek działa: " + Thread.currentThread().getName());
        });
        thread.start();
    }
}
```

**Wyjaśnienie:**

- Zamiast tworzyć osobną klasę, używamy wyrażenia lambda.
- Uproszczenie kodu dla prostych zadań.

---

## **4. Przykład Nieprzemyślanej Pracy Wątków i Nadpisywania Pól Klasy**

### **Problem: Warunki Wyścigu (Race Conditions)**

Kiedy wiele wątków jednocześnie modyfikuje wspólny zasób bez odpowiedniej synchronizacji, może dojść do niespójności danych.

### **Przykład Bez Synchronizacji**

```java
public class Counter {
    private int count = 0;

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class CounterThread extends Thread {
    private Counter counter;

    public CounterThread(Counter counter) {
        this.counter = counter;
    }

    @Override
    public void run() {
        for(int i = 0; i < 100000; i++) {
            counter.increment();
        }
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        Thread t1 = new CounterThread(counter);
        Thread t2 = new CounterThread(counter);

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Rzeczywisty wynik: " + counter.getCount());
    }
}
```

**Wyjaśnienie:**

- Dwa wątki (`t1` i `t2`) modyfikują ten sam obiekt `Counter`.
- Każdy z nich powinien zwiększyć `count` o 1000, więc oczekiwany wynik to 2000.
- Jednak bez synchronizacji wynik może być mniejszy, np. 1875, ze względu na nakładanie się operacji `increment()`.

### **Dlaczego tak się dzieje?**

- Metoda `increment()` nie jest atomowa. Składa się z trzech operacji:
    1. Odczyt wartości `count`.
    2. Zwiększenie wartości o 1.
    3. Zapis z powrotem do `count`.
- Jeśli dwa wątki wykonują te operacje jednocześnie, mogą nadpisać wartość `count`, tracąc inkrementacje.

---

## **5. Sposoby na Synchronizację Wątków**

### **1. Słowo kluczowe `synchronized`**

Synchronizuje dostęp do metody lub bloku kodu, zapewniając, że tylko jeden wątek naraz może go wykonywać na danym obiekcie.

**Synchronizacja metody:**

```java
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }
}
```

**Synchronizacja bloku kodu:**

```java
public class Counter {
    private int count = 0;

    public void increment() {
        synchronized(this) {
            count++;
        }
    }
}
```

### **2. Blokady (`Lock` i `ReentrantLock`)**

Zapewniają bardziej zaawansowane mechanizmy synchronizacji niż `synchronized`.

**Przykład:**

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Counter {
    private int count = 0;
    private Lock lock = new ReentrantLock();

    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }
}
```

**Zalety:**

- Możliwość próby zdobycia blokady (`tryLock()`).
- Blokady mogą być bardziej elastyczne.

### **3. Zmienne atomowe (`AtomicInteger`, `AtomicLong`, itp.)**

Zapewniają operacje atomowe na typach prymitywnych bez potrzeby używania `synchronized`.

**Przykład:**

```java
import java.util.concurrent.atomic.AtomicInteger;

public class Counter {
    private AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();
    }

    public int getCount() {
        return count.get();
    }
}
```

### **4. Klasy z pakietu `java.util.concurrent`**

- **Blokujące kolejki** (`BlockingQueue`)
- **Semafory** (`Semaphore`)
- **Barierki** (`CyclicBarrier`, `CountDownLatch`)

Te klasy umożliwiają synchronizację i komunikację między wątkami w bardziej zaawansowany sposób.

---

## **6. Przykład Producenta i Konsumenta (Bez `ExecutorService`)**

### **Opis Problemu**

- **Producent**: Wytwarza dane i umieszcza je w buforze.
- **Konsument**: Pobiera dane z bufora i je przetwarza.
- **Cel**: Synchronizacja dostępu do wspólnego bufora, aby uniknąć konfliktów.

### **Implementacja**

**Klasa Bufora**

```java
public class Buffer {
    private int data;
    private boolean isEmpty = true;

    public synchronized void produce(int value) throws InterruptedException {
        while (!isEmpty) {
            wait(); // Czeka, aż bufor będzie pusty
        }
        data = value;
        isEmpty = false;
        System.out.println("Wyprodukowano: " + data);
        notify(); // Powiadamia konsumenta
    }

    public synchronized int consume() throws InterruptedException {
        while (isEmpty) {
            wait(); // Czeka, aż bufor będzie pełny
        }
        int value = data;
        isEmpty = true;
        System.out.println("Skonsumowano: " + value);
        notify(); // Powiadamia producenta
        return value;
    }
}
```

**Klasa Producenta**

```java
public class Producer extends Thread {
    private Buffer buffer;

    public Producer(Buffer buffer) {
        this.buffer = buffer;
    }

    @Override
    public void run() {
        int value = 0;
        try {
            while (true) {
                buffer.produce(value++);
                Thread.sleep(500); // Symulacja czasu produkcji
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

**Klasa Konsumenta**

```java
public class Consumer extends Thread {
    private Buffer buffer;

    public Consumer(Buffer buffer) {
        this.buffer = buffer;
    }

    @Override
    public void run() {
        try {
            while (true) {
                buffer.consume();
                Thread.sleep(1000); // Symulacja czasu konsumpcji
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

**Główna Klasa**

```java
public class Main {
    public static void main(String[] args) {
        Buffer buffer = new Buffer();

        Producer producer = new Producer(buffer);
        Consumer consumer = new Consumer(buffer);

        producer.start();
        consumer.start();
    }
}
```

### **Wyjaśnienie**

- **Bufor**: Synchronizuje metody `produce()` i `consume()`, używając `wait()` i `notify()`.
- **Producent**: Wytwarza nowe dane i umieszcza je w buforze.
- **Konsument**: Pobiera dane z bufora i je przetwarza.
- **Synchronizacja**:
    - Jeśli bufor jest pełny, producent czeka.
    - Jeśli bufor jest pusty, konsument czeka.
    - `wait()` powoduje, że wątek czeka, aż zostanie powiadomiony przez `notify()`.
    - `notify()` budzi jeden z wątków czekających na monitorze obiektu.

### **Uwagi**

- **Unikanie zakleszczeń**: Używamy `while` zamiast `if` przy sprawdzaniu warunków w `produce()` i `consume()`, aby uniknąć spurious wakeups.
- **Kolejki blokujące**: W praktycznych zastosowaniach można użyć `BlockingQueue`, która upraszcza implementację.

---

## **Podsumowanie**

W tym artykule omówiliśmy kluczowe aspekty wielowątkowości w Javie:

1. **Proces vs Wątek**: Zrozumieliśmy różnice między procesami a wątkami oraz ich wpływ na wykonywanie programów.
2. **Klasa `Thread` a Interfejs `Runnable`**: Przeanalizowaliśmy dwa główne podejścia do tworzenia wątków w Javie, ich zalety i wady.
3. **Sposoby Tworzenia Wątków**: Zaprezentowaliśmy praktyczne przykłady tworzenia wątków przez dziedziczenie i implementację.
4. **Problemy z Brakiem Synchronizacji**: Zobaczyliśmy, jak nieprzemyślana praca wątków może prowadzić do niespójności danych.
5. **Synchronizacja Wątków**: Omówiliśmy różne metody synchronizacji, takie jak `synchronized`, `Lock`, czy zmienne atomowe.
6. **Przykład Producenta i Konsumenta**: Przedstawiliśmy klasyczny problem synchronizacji wątków i jego rozwiązanie bez użycia zaawansowanych frameworków.

