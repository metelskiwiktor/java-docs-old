
### Cel lekcji

- **Zrozumienie działania i zastosowania ExecutorService, Callable i Future.**
- **Praktyczne zastosowanie tych narzędzi w programowaniu współbieżnym.**

### Plan lekcji

1. **Wprowadzenie do programowania współbieżnego (5 minut)**
    - Znaczenie i zastosowania.
2. **ExecutorService (20 minut)**
    - Teoria i praktyka.
    - Zadania praktyczne.
3. **Callable i Future (25 minut)**
    - Teoria i różnice między Runnable a Callable.
    - Zadania praktyczne.
4. **Podsumowanie i pytania (5 minut)**

---

### Szczegółowe instrukcje

#### 1. Wprowadzenie do programowania współbieżnego

- **Rozpocznij od pytania:** *"Dlaczego programowanie współbieżne jest ważne w dzisiejszych aplikacjach?"*
- **Omów znaczenie:**
    - Wykorzystanie wielordzeniowych procesorów.
    - Poprawa wydajności aplikacji.
    - Reakcja na zdarzenia asynchroniczne.

#### 2. ExecutorService

##### Teoria

- **Wyjaśnij, czym jest ExecutorService:**
    - Mechanizm zarządzania pulą wątków.
    - Ułatwia wykonywanie zadań asynchronicznych.
- **Omów korzyści:**
    - Efektywne zarządzanie zasobami.
    - Prostsze zarządzanie życiem wątków.
- **Przedstaw różne implementacje:**
    - FixedThreadPool, CachedThreadPool, ScheduledThreadPool.

##### Praktyka

- **Przeanalizuj przykładowy kod (z artykułu dla ucznia).**
- **Zwróć uwagę na:**
    - Tworzenie ExecutorService.
    - Zlecanie zadań.
    - Zamknięcie ExecutorService.

##### Zadania

###### Zadanie 1:

- **Polecenie:** *"Utwórz program z pulą 4 wątków wykonujący 8 zadań, każde wypisujące swój numer i nazwę wątku."*
- **Kroki:**
    - Użyj pętli do zlecenia zadań.
    - W zadaniu wypisz informacje.

**Rozwiązanie:**

```java
ExecutorService executor = Executors.newFixedThreadPool(4);

for (int i = 1; i <= 8; i++) {
    int taskNumber = i;
    Runnable task = () -> {
        System.out.println("Zadanie " + taskNumber + " wykonywane przez " + Thread.currentThread().getName());
    };
    executor.execute(task);
}

executor.shutdown();
```

###### Zadanie 2:

- **Polecenie:** *"Wykorzystaj ScheduledExecutorService do wykonywania zadania co 2 sekundy, 5 razy."*
- **Kroki:**
    - Użyj `scheduleAtFixedRate`.
    - Ustal mechanizm zatrzymania po 5 wykonaniach.

**Rozwiązanie:**

```java
ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);

Runnable task = new Runnable() {
    int count = 0;

    @Override
    public void run() {
        count++;
        System.out.println("Wykonanie " + count + " przez " + Thread.currentThread().getName());
        if (count >= 5) {
            scheduler.shutdown();
        }
    }
};

scheduler.scheduleAtFixedRate(task, 0, 2, TimeUnit.SECONDS);
```

#### 3. Callable i Future

##### Teoria

- **Zapytaj ucznia:** *"Czy wiesz, czym różni się Runnable od Callable?"*
- **Wyjaśnij różnice (patrz tabela w artykule dla ucznia).**
- **Omów interfejs Future:**
    - Monitorowanie stanu zadania.
    - Pobieranie wyników.
    - Anulowanie zadań.

##### Praktyka

- **Przeanalizuj przykładowy kod z Callable i Future.**
- **Zwróć uwagę na:**
    - Tworzenie zadania Callable.
    - Zlecanie zadania za pomocą submit().
    - Pobieranie wyniku za pomocą get().

##### Zadania

###### Zadanie 1:

- **Polecenie:** *"Napisz program obliczający silnię liczby podanej przez użytkownika za pomocą Callable i Future."*
- **Kroki:**
    - Poproś ucznia o implementację.
    - Pomóż w obsłudze wyjątków.

**Rozwiązanie:**

```java
Scanner scanner = new Scanner(System.in);
System.out.print("Podaj liczbę: ");
int number = scanner.nextInt();

Callable<Long> factorialTask = () -> {
    long result = 1;
    for (int i = 2; i <= number; i++) {
        result *= i;
    }
    return result;
};

ExecutorService executor = Executors.newSingleThreadExecutor();
Future<Long> future = executor.submit(factorialTask);

try {
    Long factorial = future.get();
    System.out.println("Silnia liczby " + number + " wynosi: " + factorial);
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
} finally {
    executor.shutdown();
}
```

###### Zadanie 2:

- **Polecenie:** *"Utwórz program uruchamiający 3 zadania Callable, każde zwracające losową liczbę po losowym czasie. Wyświetl wyniki po zakończeniu wszystkich zadań."*
- **Kroki:**
    - Użyj `ThreadLocalRandom` do generacji.
    - Skorzystaj z pętli do uruchomienia i pobrania wyników.

**Rozwiązanie:**

```java
ExecutorService executor = Executors.newFixedThreadPool(3);
List<Future<Integer>> futures = new ArrayList<>();

for (int i = 1; i <= 3; i++) {
    Callable<Integer> task = () -> {
        int delay = ThreadLocalRandom.current().nextInt(1, 4);
        Thread.sleep(delay * 1000);
        int result = ThreadLocalRandom.current().nextInt(1, 101);
        System.out.println("Zadanie zakończone przez " + Thread.currentThread().getName());
        return result;
    };
    futures.add(executor.submit(task));
}

for (Future<Integer> future : futures) {
    try {
        Integer result = future.get();
        System.out.println("Wynik: " + result);
    } catch (InterruptedException | ExecutionException e) {
        e.printStackTrace();
    }
}

executor.shutdown();
```

##### Dodatkowe Zadanie (Anulowanie zadań)

- **Polecenie:** *"Zmodyfikuj poprzednie zadanie, aby anulować zadania, które nie zakończą się w ciągu 2 sekund."*
- **Kroki:**
    - Użyj `get(2, TimeUnit.SECONDS)`.
    - Obsłuż `TimeoutException`.

**Rozwiązanie:**

```java
for (Future<Integer> future : futures) {
    try {
        Integer result = future.get(2, TimeUnit.SECONDS);
        System.out.println("Wynik: " + result);
    } catch (TimeoutException e) {
        System.out.println("Zadanie przekroczyło limit czasu. Anulowanie...");
        future.cancel(true);
    } catch (InterruptedException | ExecutionException e) {
        e.printStackTrace();
    }
}
```

#### 4. Podsumowanie i pytania

- **Podkreśl kluczowe punkty:**
    - **ExecutorService** ułatwia zarządzanie wątkami.
    - **Callable** i **Future** pozwalają na asynchroniczne wykonywanie zadań z wynikami.
    - **Anulowanie zadań** jest ważne w aplikacjach wymagających ograniczeń czasowych.

---

### Proponowane zadanie domowe

- **Zadanie 1:**

  Napisz program, który uruchamia 10 zadań `Callable`, każde generujące losową liczbę po losowym czasie. Użyj `ExecutorService` i `invokeAll()` do uruchomienia wszystkich zadań i pobrania wyników.

- **Zadanie 2:**

  Zaimplementuj aplikację, która pobiera zawartość kilku stron internetowych równolegle, korzystając z `Callable` i `ExecutorService`. Wyświetl łączną liczbę znaków pobranych ze wszystkich stron.


