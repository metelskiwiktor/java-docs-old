### 1. Cykl życia wątku (Thread Life Cycle)

Wątek w Javie przechodzi przez kilka stanów w trakcie swojego życia. Zrozumienie tych stanów jest kluczowe do
zrozumienia mechanizmu wielowątkowości. Wątek może znajdować się w jednym z następujących stanów:

#### 1.1. **NEW (Nowy)**

Jest to stan, w którym wątek został utworzony, ale jeszcze nie został uruchomiony. Oznacza to, że instancja klasy
`Thread` została zainicjalizowana, ale metoda `start()` nie została jeszcze wywołana.

Przykład:

```java
Thread thread = new Thread(() -> {
    // zadanie wątku
});
```

W tym momencie wątek jest w stanie **NEW**. Nie wykonuje żadnych operacji.

#### 1.2. **RUNNABLE (Uruchomiony)**

Po wywołaniu metody `start()`, wątek przechodzi do stanu **RUNNABLE**, co oznacza, że jest gotowy do wykonania. Wątek w
tym stanie może zostać wybrany przez JVM do przydzielenia mu czasu procesora, aby wykonywać swoje zadanie. Stan *
*RUNNABLE** w Javie obejmuje zarówno aktywne wykonywanie instrukcji, jak i oczekiwanie na dostęp do zasobów procesora.

Przykład:

```java
thread.start(); // wątek przechodzi do stanu RUNNABLE
```

Wątek jest teraz gotowy do wykonania i czeka, aż JVM przydzieli mu czas procesora.

#### 1.3. **BLOCKED (Zablokowany)**

Wątek w stanie **BLOCKED** czeka na uzyskanie zamka (lock) do zasobu współdzielonego. Taka sytuacja ma miejsce, gdy
wątek próbuje uzyskać dostęp do sekcji krytycznej, która została zablokowana przez inny wątek.

Przykład:

```java
public synchronized void criticalSection() {
    // wątek czeka, aż ten zasób stanie się dostępny
}
```

Wątek, który chce uzyskać dostęp do metody zsynchronizowanej, ale nie ma do niej zamka, zostaje zablokowany i przechodzi
do stanu **BLOCKED**.

#### 1.4. **WAITING (Oczekujący)**

Wątek przechodzi do stanu **WAITING**, gdy oczekuje na sygnał od innego wątku, aby mógł kontynuować działanie. Ten stan
występuje, gdy wątek wywołuje metody takie jak `Object.wait()` czy `Thread.join()`. Wątek w stanie **WAITING** nie
konsumuje czasu procesora, ponieważ czeka na specyficzne zdarzenie.

Przykład:

```java
synchronized (lock){
        lock.wait(); // wątek przechodzi do stanu WAITING
}
```

Wątek oczekuje na powiadomienie, które przyjdzie, np. przez wywołanie `lock.notify()`.

#### 1.5. **TIMED_WAITING (Oczekujący z czasem)**

Stan **TIMED_WAITING** różni się od stanu **WAITING** tym, że wątek czeka na określony czas, po którym zostanie
wznowiony. Wątek może wejść w ten stan, jeśli wywołane zostaną metody takie jak `Thread.sleep(long millis)` lub
`Object.wait(long timeout)`.

Przykład:

```java
Thread.sleep(1000); // wątek przechodzi do stanu TIMED_WAITING na 1 sekundę
```

W tym przypadku wątek będzie oczekiwał przez określony czas, a po jego upływie wznowi działanie.

#### 1.6. **TERMINATED (Zakończony)**

Stan **TERMINATED** oznacza, że wątek zakończył swoje działanie. Może się to stać po normalnym zakończeniu metody
`run()` albo w wyniku nieobsłużonego wyjątku. Wątek w tym stanie nie może być ponownie uruchomiony.

Przykład:

```java
thread.run(); // Po zakończeniu wątek przechodzi do stanu TERMINATED
```

---

### 2. Mechanizm zarządzania wątkami

JVM zarządza wątkami poprzez przydzielanie im zasobów procesora zgodnie z mechanizmem planowania (thread scheduling).
Wątki mogą wykonywać swoje zadania w trybie **współbieżnym** lub **równoległym**, w zależności od liczby dostępnych
rdzeni procesora.

#### 2.1. Współbieżność (Concurrency) a równoległość (Parallelism)

- **Współbieżność (Concurrency)**: Oznacza wykonywanie wielu zadań w sposób naprzemienny. Wątki mogą dzielić czas
  procesora, przełączając się między sobą. W ten sposób wygląda to, jakby wykonywały się równocześnie, chociaż w
  rzeczywistości procesor obsługuje je w krótkich sekwencjach.

- **Równoległość (Parallelism)**: Oznacza jednoczesne wykonywanie wielu zadań. Jest możliwa wtedy, gdy system posiada
  więcej niż jeden rdzeń procesora, dzięki czemu kilka wątków może rzeczywiście wykonywać się równocześnie.

JVM może dynamicznie przełączać wątki między stanami **RUNNABLE** i **WAITING**, aby optymalnie zarządzać dostępem do
zasobów procesora.

#### 2.2. Priorytety wątków

W Javie każdy wątek ma przypisany priorytet, który informuje system operacyjny, jak ważny jest dany wątek w stosunku do
innych wątków. Priorytety są wartościami liczbowymi od 1 (najniższy) do 10 (najwyższy). Domyślnie każdy wątek ma
priorytet `5`.

```java
Thread thread = new Thread();
thread.setPriority(Thread.MAX_PRIORITY); // ustawienie maksymalnego priorytetu
```

JVM nie gwarantuje, że wątki o wyższym priorytecie zawsze będą miały pierwszeństwo. Zachowanie może się różnić w
zależności od implementacji planowania wątków przez system operacyjny.

---

### 3. Stany wątków a synchronizacja

W środowisku wielowątkowym, wątki mogą współbieżnie uzyskiwać dostęp do tych samych zasobów, co może prowadzić do
problemów takich jak wyścigi danych (race conditions). Aby temu zapobiec, wątki muszą być odpowiednio zsynchronizowane.

- **Zablokowanie zasobu**: Gdy jeden wątek uzyska zamek na zasób, inne wątki próbujące uzyskać ten zamek zostają
  zablokowane i przechodzą do stanu **BLOCKED**.
- **Oczekiwanie na powiadomienie**: Wątki mogą oczekiwać na sygnał od innego wątku, co powoduje, że przechodzą do stanu
  **WAITING** lub **TIMED_WAITING**.

---

### 4. Synchronizacja a bezpieczeństwo wątków

Ważnym aspektem wielowątkowości jest bezpieczeństwo wątkowe (thread-safety), czyli zapewnienie, że kod może być
poprawnie wykonywany współbieżnie przez wiele wątków. Mechanizmy synchronizacji, takie jak `synchronized`, `Lock`,
`volatile` i zmienne atomowe, zapewniają, że wątki nie będą interferować ze sobą podczas pracy na współdzielonych
zasobach.

- **Synchronized**: Używane do zablokowania sekcji krytycznych kodu, co pozwala tylko jednemu wątkowi naraz uzyskać
  dostęp do danej sekcji.
- **Lock**: Bardziej zaawansowany mechanizm blokady, pozwalający na większą kontrolę nad odblokowywaniem zasobów.
- **Volatile**: Gwarantuje, że wartość zmiennej będzie zawsze aktualna dla wszystkich wątków.
- **Zmienna atomowa** (`AtomicInteger`, `AtomicBoolean`): Oferuje operacje atomowe, które są niepodzielne i nie wymagają
  dodatkowej synchronizacji.

### 5. Problemy wynikające z korzystania z wątków

Wykorzystanie wielowątkowości w Javie może prowadzić do wydajniejszych aplikacji, ale wprowadza również szereg
potencjalnych problemów, które są charakterystyczne dla programowania współbieżnego. Problemy te wynikają głównie z
tego, że wątki dzielą wspólną przestrzeń pamięci, a brak odpowiedniej synchronizacji może prowadzić do błędów trudnych
do wykrycia. Omówimy teraz najczęściej występujące problemy, takie jak **race conditions**, **deadlock**, **livelock**
oraz **starvation**.

#### 5.1. Race Condition (Wyścig danych)

Race condition to sytuacja, w której dwa lub więcej wątków współbieżnie uzyskują dostęp do współdzielonego zasobu, a co
najmniej jeden z tych wątków zmienia jego stan. Problem polega na tym, że wynik operacji zależy od kolejności
wykonywania wątków, co może prowadzić do nieprzewidywalnych wyników.

Przykład wyścigu danych może wystąpić wtedy, gdy dwa wątki jednocześnie modyfikują wspólną zmienną bez synchronizacji.
Bez odpowiedniego mechanizmu kontroli dostępu może dojść do sytuacji, w której jeden wątek odczytuje wartość zmiennej
zanim drugi wątek zdąży ją zaktualizować, co prowadzi do nieprawidłowych wyników.

Rozwiązaniem tego problemu jest użycie mechanizmów synchronizacji, takich jak bloki `synchronized` lub zmienne atomowe,
które zapewniają, że tylko jeden wątek naraz może uzyskać dostęp do zasobu współdzielonego.

#### 5.2. Deadlock (Zakleszczenie)

Deadlock to sytuacja, w której dwa lub więcej wątków blokuje się wzajemnie, oczekując na zasoby, które są już trzymane
przez inne wątki. Wątki czekają na siebie nawzajem, a ich stan nigdy się nie zmienia, co prowadzi do zakleszczenia
systemu.

Deadlock występuje, gdy wątki uzyskują dostęp do zasobów w niewłaściwej kolejności. Na przykład, jeśli wątek A zablokuje
zasób X i próbuje uzyskać dostęp do zasobu Y, podczas gdy wątek B zablokował zasób Y i próbuje uzyskać dostęp do zasobu
X, oba wątki będą czekać w nieskończoność na zwolnienie zasobów przez drugi wątek.

Aby uniknąć deadlocka, można stosować kilka strategii, takich jak:

- Unikanie cykli wzajemnych zależności zasobów (hierarchia blokad).
- Wprowadzenie limitów czasowych na oczekiwanie na zablokowane zasoby.
- Wykorzystanie mechanizmów prób uzyskania blokady, takich jak `tryLock`, które nie blokują wątku w nieskończoność.

#### 5.3. Livelock

Livelock to sytuacja, w której wątki pozostają aktywne i próbują reagować na blokady, ale nigdy nie osiągają postępu. W
przeciwieństwie do deadlocka, wątki w stanie livelock nie są całkowicie zablokowane, ale wciąż zmieniają swój stan,
starając się rozwiązać konflikt. Jednak te ciągłe zmiany stanu nie prowadzą do zakończenia działania wątków.

Przykład livelocka może wystąpić wtedy, gdy wątki próbują ustąpić sobie nawzajem, np. dwa wątki, które w odpowiedzi na
blokadę ciągle zmieniają swoje zachowanie, ale nigdy nie uzyskują dostępu do zasobu.

Rozwiązaniem problemu livelocka może być wprowadzenie mechanizmu zwłoki (backoff), który wstrzymuje działanie wątków na
losowy czas, aby umożliwić innemu wątkowi wykonanie operacji.

#### 5.4. Starvation (Głodzenie)

Starvation występuje, gdy jeden z wątków nigdy nie otrzymuje dostępu do zasobu, ponieważ inne wątki stale uzyskują do
niego dostęp z wyższym priorytetem. Wątek o niższym priorytecie może być nieustannie blokowany przez inne wątki, przez
co nigdy nie dostaje szansy na wykonanie swojej pracy.

Starvation może wystąpić w systemach, w których wątki mają różne priorytety. W takim przypadku wątki o niższym
priorytecie mogą być ignorowane przez harmonogram wątków, ponieważ wątki o wyższym priorytecie są stale wybierane do
wykonywania.

Aby uniknąć starvation, można stosować techniki takie jak:

- Priorytety dynamiczne, które zwiększają priorytet wątków, które czekają zbyt długo.
- Wprowadzenie zasady sprawiedliwości (fairness), która gwarantuje, że każdy wątek dostanie zasoby po odpowiednim czasie
  oczekiwania.
