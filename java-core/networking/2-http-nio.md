# Komunikacja sieciowa w Javie: HTTP, NIO oraz Wielowątkowość

## 1. Komunikacja HTTP w Javie

### 1.1. Protokół HTTP i jego rola w aplikacjach sieciowych

**HTTP (Hypertext Transfer Protocol)** to protokół warstwy aplikacji używany do przesyłania dokumentów hipertekstowych w sieci WWW. Jest podstawą komunikacji między klientami (np. przeglądarkami internetowymi) a serwerami WWW. HTTP jest protokołem bezstanowym, co oznacza, że każde żądanie jest niezależne i nie zawiera informacji o poprzednich żądaniach.

#### Cechy protokołu HTTP:

- **Metody żądań**: GET, POST, PUT, DELETE, HEAD, OPTIONS, PATCH.
- **Kody odpowiedzi**: Informują o statusie żądania (np. 200 OK, 404 Not Found, 500 Internal Server Error).
- **Nagłówki**: Przenoszą dodatkowe informacje o żądaniu lub odpowiedzi (np. typ treści, autoryzacja).

### 1.2. Klasy `HttpURLConnection` i `HttpsURLConnection`

Java dostarcza wbudowane klasy do obsługi komunikacji HTTP:

- **`HttpURLConnection`**: Umożliwia nawiązywanie połączeń HTTP.
- **`HttpsURLConnection`**: Rozszerza `HttpURLConnection` o obsługę protokołu HTTPS (HTTP Secure).

#### Wysyłanie żądań HTTP typu GET i POST

##### Żądanie GET:

- Służy do pobierania danych z serwera.
- Parametry są przekazywane w URL.

##### Żądanie POST:

- Służy do wysyłania danych do serwera (np. formularzy).
- Dane są przekazywane w ciele żądania.

#### Przykład wysyłania żądania GET:

```java
import java.io.*;
import java.net.*;

public class HttpGetExample {
    public static void main(String[] args) {
        try {
            // Tworzenie obiektu URL
            URL url = new URL("https://api.github.com/");
            // Otwarcie połączenia
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            // Ustawienie metody żądania
            connection.setRequestMethod("GET");

            // Odczytanie kodu odpowiedzi
            int responseCode = connection.getResponseCode();
            System.out.println("Kod odpowiedzi: " + responseCode);

            // Odczytanie treści odpowiedzi
            BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String inputLine;
            StringBuilder content = new StringBuilder();

            while ((inputLine = in.readLine()) != null) {
                content.append(inputLine);
            }

            // Zamknięcie strumienia
            in.close();

            // Wyświetlenie treści odpowiedzi
            System.out.println("Treść odpowiedzi: " + content.toString());

            // Zamknięcie połączenia
            connection.disconnect();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Obsługa odpowiedzi z serwera

- **Kody odpowiedzi**: Metoda `getResponseCode()` zwraca kod odpowiedzi HTTP.
- **Nagłówki**: Można je odczytać za pomocą metod `getHeaderField()` i `getHeaderFields()`.
- **Treść odpowiedzi**: Odczytywana ze strumienia `InputStream`.

### 1.3. Wysyłanie żądania POST

```java
import java.io.*;
import java.net.*;

public class HttpPostExample {
    public static void main(String[] args) {
        try {
            URL url = new URL("https://reqres.in/api/users");
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setRequestMethod("POST");
            // Ustawienie nagłówków
            connection.setRequestProperty("Content-Type", "application/json; utf-8");
            connection.setDoOutput(true);

            // Dane do wysłania
            String jsonInputString = "{\"name\": \"Jan\", \"job\": \"Programista\"}";

            // Wysłanie danych
            try(OutputStream os = connection.getOutputStream()) {
                byte[] input = jsonInputString.getBytes("utf-8");
                os.write(input, 0, input.length);           
            }

            // Odczytanie odpowiedzi
            int responseCode = connection.getResponseCode();
            System.out.println("Kod odpowiedzi: " + responseCode);

            BufferedReader br = new BufferedReader(new InputStreamReader(
                connection.getInputStream(), "utf-8"
            ));
            StringBuilder response = new StringBuilder();
            String responseLine;

            while ((responseLine = br.readLine()) != null) {
                response.append(responseLine.trim());
            }

            System.out.println("Treść odpowiedzi: " + response.toString());

            connection.disconnect();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 2. Non-blocking I/O (NIO)

### 2.1. Wprowadzenie do `java.nio`

`java.nio` (New Input/Output) to pakiet wprowadzony w Javie 1.4, który dostarcza nowe mechanizmy wejścia/wyjścia, bardziej wydajne niż tradycyjne strumienie. NIO wprowadza:

- **Kanały (Channels)**: Podobne do strumieni, ale mogą być dwukierunkowe.
- **Bufory (Buffers)**: Przechowują dane podczas operacji I/O.
- **Selektory (Selectors)**: Umożliwiają obsługę wielu kanałów za pomocą jednego wątku.

### 2.2. Blokujące vs. nieblokujące I/O

- **Blokujące I/O**: Operacje wejścia/wyjścia blokują wątek do momentu zakończenia operacji.
- **Nieblokujące I/O**: Operacje zwracają natychmiast, a aplikacja jest informowana o gotowości danych.

### 2.3. Klasy `SocketChannel` i `ServerSocketChannel`

#### `SocketChannel`

- Reprezentuje połączenie do zdalnego hosta.
- Może działać w trybie blokującym lub nieblokującym.

#### `ServerSocketChannel`

- Służy do nasłuchiwania połączeń przychodzących.
- Może akceptować połączenia i tworzyć `SocketChannel`.

### 2.4. Tworzenie serwera i klienta za pomocą NIO

#### Serwer NIO:

```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;

public class NioServer {
    public static void main(String[] args) {
        try {
            // Tworzenie kanału serwera
            ServerSocketChannel serverChannel = ServerSocketChannel.open();
            serverChannel.bind(new InetSocketAddress(5000));
            System.out.println("Serwer NIO nasłuchuje na porcie 5000...");

            while (true) {
                // Akceptowanie połączenia (blokujące)
                SocketChannel clientChannel = serverChannel.accept();

                // Odczytanie danych od klienta
                ByteBuffer buffer = ByteBuffer.allocate(256);
                clientChannel.read(buffer);
                String message = new String(buffer.array()).trim();
                System.out.println("Otrzymano: " + message);

                // Wysłanie odpowiedzi
                String response = "Serwer NIO otrzymał twoją wiadomość.";
                buffer = ByteBuffer.wrap(response.getBytes());
                clientChannel.write(buffer);

                // Zamknięcie połączenia
                clientChannel.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Klient NIO:

```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SocketChannel;

public class NioClient {
    public static void main(String[] args) {
        try {
            // Połączenie z serwerem
            SocketChannel clientChannel = SocketChannel.open(new InetSocketAddress("localhost", 5000));

            // Wysłanie wiadomości
            String message = "Wiadomość od klienta NIO";
            ByteBuffer buffer = ByteBuffer.wrap(message.getBytes());
            clientChannel.write(buffer);

            // Odczytanie odpowiedzi
            buffer.clear();
            clientChannel.read(buffer);
            String response = new String(buffer.array()).trim();
            System.out.println("Odpowiedź serwera: " + response);

            // Zamknięcie połączenia
            clientChannel.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 2.5. Korzyści z użycia NIO

- **Wydajność**: Obsługa wielu połączeń za pomocą jednego wątku.
- **Skalowalność**: Lepsze wykorzystanie zasobów przy dużej liczbie połączeń.
- **Elastyczność**: Możliwość przełączania między trybem blokującym i nieblokującym.

---

## 3. Wielowątkowość w serwerach sieciowych

### 3.1. Dlaczego wielowątkowość jest potrzebna?

W serwerach sieciowych konieczne jest obsługiwanie wielu klientów jednocześnie. Bez wielowątkowości serwer mógłby obsłużyć tylko jedno połączenie naraz, co jest nieefektywne.

### 3.2. Implementacja serwera wielowątkowego

#### Klasa `Thread` i `Runnable`

- **`Thread`**: Reprezentuje niezależny wątek wykonania.
- **`Runnable`**: Interfejs z metodą `run()`, którą należy zdefiniować.

#### Użycie `ExecutorService`

- Dostarcza zarządzanie pulą wątków.
- Ułatwia zarządzanie wieloma zadaniami bez ręcznego tworzenia i zarządzania wątkami.

### 3.3. Przykład serwera wielowątkowego

```java
import java.io.*;
import java.net.*;
import java.util.concurrent.*;

public class MultiThreadedServer {
    public static void main(String[] args) {
        try {
            // Tworzenie gniazda serwera
            ServerSocket serverSocket = new ServerSocket(6000);
            System.out.println("Serwer wielowątkowy nasłuchuje na porcie 6000...");

            // Tworzenie puli wątków
            ExecutorService executorService = Executors.newFixedThreadPool(10);

            while (true) {
                // Akceptowanie połączenia
                Socket clientSocket = serverSocket.accept();
                System.out.println("Nowe połączenie od " + clientSocket.getInetAddress());

                // Przekazanie obsługi klienta do wątku
                executorService.submit(new ClientHandler(clientSocket));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// Klasa obsługująca klienta
class ClientHandler implements Runnable {
    private Socket clientSocket;

    public ClientHandler(Socket socket) {
        this.clientSocket = socket;
    }

    public void run() {
        try {
            // Strumienie komunikacji
            BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
            PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);

            // Odczytanie wiadomości
            String message = in.readLine();
            System.out.println("Otrzymano od klienta: " + message);

            // Wysłanie odpowiedzi
            out.println("Serwer wielowątkowy otrzymał twoją wiadomość.");

            // Zamknięcie połączenia
            clientSocket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 3.4. Analiza kodu

- **Serwer**:
    - Nasłuchuje na porcie 6000.
    - Używa `ExecutorService` do zarządzania wątkami.
    - Dla każdego połączenia tworzy nowy obiekt `ClientHandler`.
- **`ClientHandler`**:
    - Implementuje `Runnable`.
    - Obsługuje komunikację z pojedynczym klientem.

---

## Podsumowanie

- **Komunikacji HTTP**: Wykorzystanie klas `HttpURLConnection` do wysyłania żądań i odbierania odpowiedzi.
- **Non-blocking I/O (NIO)**: Nowe podejście do wejścia/wyjścia, pozwalające na bardziej wydajne i skalowalne aplikacje.
- **Wielowątkowości**: Implementacja serwerów zdolnych obsłużyć wiele połączeń jednocześnie.

---

## Pytania i odpowiedzi

### Pytania podstawowe:

1. **Co to jest protokół HTTP i jakie są jego główne zastosowania?**

   HTTP (Hypertext Transfer Protocol) to protokół warstwy aplikacji używany głównie do przesyłania dokumentów hipertekstowych w sieci WWW. Jest podstawą komunikacji między klientami (np. przeglądarkami internetowymi) a serwerami WWW. Umożliwia przesyłanie stron internetowych, obrazów, plików i innych zasobów.

2. **Jakie klasy w Javie służą do komunikacji HTTP i jakie są ich podstawowe funkcje?**

   Klasy `HttpURLConnection` i `HttpsURLConnection` służą do komunikacji HTTP w Javie. `HttpURLConnection` umożliwia nawiązywanie połączeń HTTP, wysyłanie żądań (GET, POST, itp.) oraz odbieranie odpowiedzi. `HttpsURLConnection` rozszerza tę funkcjonalność o obsługę protokołu HTTPS.

3. **Czym różni się metoda żądania GET od POST w protokole HTTP?**

   Metoda GET służy do pobierania danych z serwera, a parametry są przekazywane w URL. Metoda POST służy do wysyłania danych do serwera (np. formularzy), a dane są przekazywane w ciele żądania. GET jest używany dla operacji idempotentnych, a POST dla operacji, które modyfikują stan na serwerze.

4. **Co to jest NIO w Javie i jakie problemy rozwiązuje?**

   NIO (New Input/Output) to pakiet w Javie wprowadzony w wersji 1.4, który dostarcza nowe mechanizmy wejścia/wyjścia. Rozwiązuje problemy związane z wydajnością i skalowalnością tradycyjnych strumieni I/O, umożliwiając nieblokujące operacje I/O, efektywne zarządzanie wieloma połączeniami oraz lepsze wykorzystanie zasobów systemowych.

5. **Jakie są główne różnice między blokującym a nieblokującym I/O?**

   W blokującym I/O operacje wejścia/wyjścia blokują wątek do momentu zakończenia operacji, co może prowadzić do nieefektywnego wykorzystania zasobów. W nieblokującym I/O operacje zwracają natychmiast, a aplikacja jest informowana o gotowości danych, co pozwala na obsługę wielu połączeń w jednym wątku i zwiększa wydajność.

### Pytania zaawansowane:

6. **Jak działa selektor (`Selector`) w NIO i jakie korzyści przynosi jego użycie?**

   `Selector` umożliwia monitorowanie wielu kanałów (np. `SocketChannel`) pod kątem gotowości do operacji (np. odczyt, zapis) za pomocą jednego wątku. Pozwala to na efektywne zarządzanie wieloma połączeniami jednocześnie bez konieczności tworzenia wielu wątków, co zwiększa skalowalność aplikacji.

7. **W jaki sposób można zaimplementować serwer NIO obsługujący tysiące jednoczesnych połączeń?**

   Można użyć `ServerSocketChannel` w trybie nieblokującym oraz `Selector` do monitorowania wielu kanałów. Aplikacja rejestruje kanały w selektorze i w pętli obsługuje zdarzenia (np. gotowość do odczytu), co pozwala na obsługę wielu połączeń w jednym lub kilku wątkach.

8. **Dlaczego wielowątkowość jest ważna w serwerach sieciowych i jakie są potencjalne wyzwania z nią związane?**

   Wielowątkowość pozwala na równoczesną obsługę wielu klientów, zwiększając responsywność i wydajność serwera. Wyzwania to synchronizacja danych współdzielonych między wątkami, unikanie warunków wyścigu, zakleszczeń oraz zarządzanie zasobami w celu uniknięcia przeciążenia systemu.

9. **Jakie są różnice między użyciem puli wątków (`ExecutorService`) a tworzeniem nowych wątków dla każdego połączenia?**

   Użycie puli wątków pozwala na ponowne wykorzystanie wątków i kontrolę nad ich liczbą, co jest bardziej wydajne i skalowalne. Tworzenie nowego wątku dla każdego połączenia może prowadzić do nadmiernego zużycia zasobów i degradacji wydajności przy dużej liczbie połączeń.

10. **Jak można zabezpieczyć komunikację HTTP w Javie i jakie mechanizmy są dostępne?**

    Komunikację HTTP można zabezpieczyć, używając protokołu HTTPS, który szyfruje dane za pomocą TLS/SSL. W Javie można użyć klasy `HttpsURLConnection`, która obsługuje bezpieczne połączenia. Dodatkowo można zarządzać certyfikatami SSL, skonfigurować zaufane magazyny kluczy oraz implementować uwierzytelnianie i autoryzację na poziomie aplikacji.
