## CompletableFuture w Javie - Teoria i Praktyka

### Wstęp do CompletableFuture

`CompletableFuture` to klasa wprowadzona w Javie 8 w ramach pakietu `java.util.concurrent`, która umożliwia asynchroniczne wykonywanie zadań. Jest to zaawansowane narzędzie pozwalające na pracę z programowaniem współbieżnym i asynchronicznym, które ułatwia realizację wieloetapowych procesów, takich jak pobieranie danych, przetwarzanie wyników oraz obsługę błędów, bez blokowania głównego wątku.

W odróżnieniu od bardziej podstawowych mechanizmów takich jak `Future`, `CompletableFuture` oferuje dodatkowe możliwości, takie jak:
- Zastosowanie pełnej asynchroniczności, dzięki metodom takim jak `supplyAsync` i `runAsync`.
- Obsługę łańcuchów zadań (ang. chaining) poprzez metody transformacyjne.
- Łatwą obsługę wyjątków w przepływie asynchronicznym.
- Możliwość synchronizowania wyników z wielu zadań.

### Podstawowe klasy i metody

1. **`CompletableFuture`** - główna klasa odpowiedzialna za asynchroniczne wykonanie zadania i uzyskanie wyniku. Wartość może być ustawiona w sposób asynchroniczny, a także wykorzystywana w przepływach danych.

2. **Kluczowe metody**:
    - **`supplyAsync()`** - uruchamia asynchroniczne zadanie zwracające wynik.
    - **`runAsync()`** - uruchamia asynchroniczne zadanie bez zwracania wyniku.
    - **`thenApply()`** - pozwala na transformację wyniku po zakończeniu zadania.
    - **`thenAccept()`** - wykonuje akcję po zakończeniu zadania bez zwracania wyniku.
    - **`thenCombine()`** - łączy wyniki dwóch asynchronicznych zadań.
    - **`exceptionally()`** - umożliwia obsługę wyjątków pojawiających się w trakcie wykonywania zadania.

### Teoria wykorzystania CompletableFuture

`CompletableFuture` umożliwia wykonywanie zadań w sposób nieblokujący. Dzięki temu możemy uruchomić operację w tle, a następnie kontynuować dalsze działania w aplikacji, zamiast czekać na zakończenie operacji. Jest to szczególnie przydatne, gdy aplikacja wykonuje operacje wejścia/wyjścia (np. pobieranie danych z API) lub intensywne obliczenia, które mogą trwać dłuższy czas. `CompletableFuture` umożliwia także łączenie wielu zadań, tworzenie przepływów danych i przetwarzanie rezultatów w sposób sekwencyjny lub równoległy.

---

## Praktyczne przykłady

Poniżej przedstawiamy dwa przykłady: jeden pokazujący prostą operację asynchroniczną i transformację wyników, a drugi łączący wyniki kilku zadań asynchronicznych.

### Przykład 1: Pobieranie danych asynchronicznie

W tym przykładzie wykonamy operację pobierania danych asynchronicznie oraz transformację wyniku po jego zakończeniu.

#### Kod sekwencyjny

Zacznijmy od prostego kodu, który symuluje pobieranie danych w sposób sekwencyjny, bez `CompletableFuture`.

```java
public class SequentialExample {
    public static void main(String[] args) {
        String data = fetchData();
        System.out.println("Pobrane dane: " + data);
    }

    public static String fetchData() {
        try {
            Thread.sleep(2000); // symulacja opóźnienia
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        return "Dane z serwera";
    }
}
```

W tej wersji program oczekuje na zakończenie `fetchData()` przez 2 sekundy, co blokuje główny wątek.

#### Wersja z użyciem CompletableFuture

Teraz przedstawimy asynchroniczne pobieranie danych, w którym `CompletableFuture` wykona zadanie w tle i pozwoli na dalsze działania w aplikacji.

```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureExample {
    public static void main(String[] args) {
        CompletableFuture<String> futureData = CompletableFuture.supplyAsync(() -> fetchData());

        futureData
            .thenApply(data -> "Przetworzone: " + data)
            .thenAccept(System.out::println)
            .exceptionally(e -> {
                System.out.println("Błąd: " + e.getMessage());
                return null;
            });

        System.out.println("Operacja asynchroniczna rozpoczęta.");
        try {
            Thread.sleep(3000); // Czekamy na zakończenie wątku roboczego
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }

    public static String fetchData() {
        try {
            Thread.sleep(2000); // symulacja opóźnienia
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        return "Dane z serwera";
    }
}
```

W tej wersji kodu:
- **`supplyAsync()`** uruchamia zadanie asynchroniczne, które zwraca wynik (dane).
- **`thenApply()`** wykonuje transformację wyniku, dodając tekst "Przetworzone: ".
- **`thenAccept()`** odbiera przetworzony wynik i wyświetla go na konsoli.
- **`exceptionally()`** obsługuje ewentualne błędy podczas pobierania danych.

### Przykład 2: Łączenie wyników z wielu zadań asynchronicznych

W tym przykładzie uruchomimy dwa asynchroniczne zadania równocześnie, a następnie połączymy ich wyniki w jedno.

#### Kod sekwencyjny

Kod sekwencyjny wykonuje dwa zadania jedno po drugim, co zajmuje więcej czasu.

```java
public class SequentialCombineExample {
    public static void main(String[] args) {
        String result1 = fetchData("Zadanie 1");
        String result2 = fetchData("Zadanie 2");
        System.out.println("Wynik łączony: " + result1 + " i " + result2);
    }

    public static String fetchData(String taskName) {
        try {
            Thread.sleep(2000); // symulacja opóźnienia
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        return "Dane z " + taskName;
    }
}
```

#### Wersja z użyciem CompletableFuture

Zobaczmy, jak `CompletableFuture` pozwala na wykonanie obu zadań równocześnie, a następnie ich połączenie.

```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureCombineExample {
    public static void main(String[] args) {
        CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> fetchData("Zadanie 1"));
        CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> fetchData("Zadanie 2"));

        future1
            .thenCombine(future2, (result1, result2) -> result1 + " i " + result2)
            .thenAccept(System.out::println)
            .exceptionally(e -> {
                System.out.println("Błąd: " + e.getMessage());
                return null;
            });

        System.out.println("Operacje asynchroniczne rozpoczęte.");
        try {
            Thread.sleep(3000); // Czekamy na zakończenie wątków roboczych
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }

    public static String fetchData(String taskName) {
        try {
            Thread.sleep(2000); // symulacja opóźnienia
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        return "Dane z " + taskName;
    }
}
```

W tej wersji:
- **`thenCombine()`** łączy wyniki `future1` i `future2`, gdy oba zadania zakończą się sukcesem.
- **`thenAccept()`** wyświetla wynik na konsoli, gdy połączone zadanie jest gotowe.

Dzięki równoczesnemu wykonaniu obu zadań za pomocą `CompletableFuture`, czas oczekiwania jest skrócony do czasu najdłuższego z tych zadań, a nie sumy ich czasów.

---

### Zalety i wady stosowania CompletableFuture

#### Zalety

1. **Równoległe wykonywanie zadań** - `CompletableFuture` pozwala na jednoczesne uruchamianie wielu zadań, co skraca czas wykonania operacji.
2. **Efektywna obsługa złożonych przepływów** - umożliwia tworzenie wieloetapowych przepływów zadań z obsługą wyjątków i transformacją wyników.
3. **Wspiera programowanie reaktywne** - pozwala na asynchroniczne przetwarzanie danych, co jest idealne do zastosowań opartych na modelu reaktywnym, jak interakcje z API.

#### Wady

1. **Złożoność kodu** - skomplikowane przepływy asynchroniczne mogą być trudne w debugowaniu i testowaniu.
2. **Potrzeba ręcznej obsługi wyjątków** - w przypadku złożonych łańcuchów zadań wymaga się pełnej obsługi błędów, co może skomplikować kod.
3. **Zarządzanie zasobami** - intensywne wykorzystanie `CompletableFuture` może prowadzić do zbyt dużego obciążenia systemu wątków, co wymaga ostrożnego zarządzania.

---

### Zastosowania `CompletableFuture`

`CompletableFuture` jest często stosowany w miejscach, gdzie ważne jest równoległe i asynchroniczne przetwarzanie zadań:

Oto cztery rzeczywiste przykłady bibliotek w Javie, które wykorzystują `CompletableFuture`, wraz z przykładowym kodem dla każdej z nich:

#### 1. **AsyncHttpClient**

AsyncHttpClient to biblioteka do wykonywania asynchronicznych zapytań HTTP. Dzięki `CompletableFuture` pozwala na wykonanie żądania HTTP w tle i obsługę odpowiedzi po zakończeniu przetwarzania.

##### Kod przykładowy

```java
import org.asynchttpclient.AsyncHttpClient;
import org.asynchttpclient.Dsl;
import java.util.concurrent.CompletableFuture;

public class AsyncHttpClientExample {
    public static void main(String[] args) {
        AsyncHttpClient client = Dsl.asyncHttpClient();

        CompletableFuture<String> futureResponse = client
                .prepareGet("https://jsonplaceholder.typicode.com/todos/1")
                .execute()
                .toCompletableFuture()
                .thenApply(response -> response.getResponseBody());

        futureResponse.thenAccept(System.out::println)
                      .exceptionally(e -> {
                          System.out.println("Błąd: " + e.getMessage());
                          return null;
                      });

        futureResponse.thenRun(client::close);
    }
}
```

W tym przykładzie:
- Wysyłamy żądanie GET do API JSONPlaceholder.
- `thenApply` przekształca wynik na treść odpowiedzi.
- `thenAccept` wypisuje wynik na konsolę, a `exceptionally` obsługuje ewentualne błędy.

---

#### 2. **JDA (Java Discord API)**

JDA to biblioteka do tworzenia botów dla Discorda, która intensywnie korzysta z `CompletableFuture` do obsługi operacji takich jak wysyłanie wiadomości i pobieranie danych o użytkownikach.

##### Kod przykładowy

```java
import net.dv8tion.jda.api.JDABuilder;
import net.dv8tion.jda.api.entities.MessageChannel;
import net.dv8tion.jda.api.events.message.MessageReceivedEvent;
import net.dv8tion.jda.api.hooks.ListenerAdapter;

import javax.security.auth.login.LoginException;
import java.util.concurrent.CompletableFuture;

public class JDADiscordBot extends ListenerAdapter {
    public static void main(String[] args) throws LoginException {
        JDABuilder.createDefault("BOT_TOKEN")
                .addEventListeners(new JDADiscordBot())
                .build();
    }

    @Override
    public void onMessageReceived(MessageReceivedEvent event) {
        MessageChannel channel = event.getChannel();
        channel.sendMessage("Hello, this is a CompletableFuture example!")
                .submit()  // Zwraca CompletableFuture
                .thenRun(() -> System.out.println("Wiadomość wysłana!"))
                .exceptionally(e -> {
                    System.out.println("Błąd podczas wysyłania wiadomości: " + e.getMessage());
                    return null;
                });
    }
}
```

W przykładzie:
- `submit()` w JDA zwraca `CompletableFuture`, które można wykorzystać do obsługi wiadomości asynchronicznie.
- `thenRun` wykonuje akcję po wysłaniu wiadomości, a `exceptionally` obsługuje błędy.

---

#### 3. **Spring WebFlux**

Spring WebFlux to moduł Spring, który pozwala na tworzenie reaktywnych aplikacji webowych. W WebFlux można korzystać z `CompletableFuture` do obsługi asynchronicznych zapytań do zewnętrznych API.

##### Kod przykładowy

```java
import org.springframework.web.reactive.function.client.WebClient;
import java.util.concurrent.CompletableFuture;

public class SpringWebFluxExample {
    private final WebClient webClient = WebClient.create("https://jsonplaceholder.typicode.com");

    public CompletableFuture<String> getTodo() {
        return webClient.get()
                .uri("/todos/1")
                .retrieve()
                .bodyToMono(String.class)
                .toFuture();
    }

    public static void main(String[] args) {
        SpringWebFluxExample example = new SpringWebFluxExample();
        CompletableFuture<String> futureResponse = example.getTodo();

        futureResponse.thenAccept(System.out::println)
                      .exceptionally(e -> {
                          System.out.println("Błąd: " + e.getMessage());
                          return null;
                      });
    }
}
```

W powyższym przykładzie:
- `bodyToMono` zwraca `Mono`, które przekształcamy na `CompletableFuture` za pomocą `toFuture()`.
- `thenAccept` wypisuje wynik na konsolę, a `exceptionally` obsługuje błędy.

---

#### 4. **Hibernate Reactive**

Hibernate Reactive to wersja Hibernate przystosowana do obsługi zapytań asynchronicznych do baz danych. Można z niej korzystać z `CompletableFuture` do asynchronicznego pobierania i przetwarzania danych.

##### Kod przykładowy

```java
import org.hibernate.reactive.mutiny.Mutiny;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import java.util.concurrent.CompletableFuture;

public class HibernateReactiveExample {
    public static void main(String[] args) {
        Mutiny.SessionFactory sessionFactory = ...; // Konfiguracja SessionFactory

        sessionFactory.withTransaction((session, tx) -> session.createQuery("from MyEntity", MyEntity.class)
                .getResultList()
                .thenAccept(entities -> entities.forEach(entity -> System.out.println("Pobrano: " + entity)))).subscribeAsCompletionStage()
                .exceptionally(e -> {
                    System.out.println("Błąd: " + e.getMessage());
                    return null;
                });
    }

    @Entity
    public static class MyEntity {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        private String name;

        // Gettery i settery
    }
}
```

W tym przykładzie:
- `withTransaction` obsługuje transakcję asynchronicznie.
- `thenAccept` wypisuje pobrane dane, a `exceptionally` obsługuje błędy.

---

### Podsumowanie

`CompletableFuture` to narzędzie o szerokim zastosowaniu, które znalazło miejsce w wielu popularnych bibliotekach Javy, służąc do asynchronicznego i równoległego przetwarzania. Dzięki możliwościom, jakie oferuje, jest niezastąpione wszędzie tam, gdzie wymagana jest szybka i nieblokująca obsługa zadań – od przetwarzania HTTP, poprzez dostęp do baz danych, aż po współpracę z mikroserwisami i API.
