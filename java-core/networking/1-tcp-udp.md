# Podstawy Sieci w Javie: TCP/IP oraz UDP

## Podstawy sieci komputerowych

Sieć komputerowa to zbiór połączonych ze sobą urządzeń umożliwiających wymianę danych. Komunikacja w sieci opiera się na stosie protokołów, gdzie każdy poziom odpowiada za określone funkcje. W modelu OSI mamy siedem warstw, ale w praktyce często używa się prostszego modelu TCP/IP z czterema warstwami: dostęp do sieci, internetowa, transportowa i aplikacji.

### Protokoły komunikacyjne

- **TCP/IP (Transmission Control Protocol/Internet Protocol)**: Jest to zestaw protokołów, które są podstawą funkcjonowania Internetu. Zapewniają niezawodne, połączeniowe przesyłanie danych między urządzeniami.
- **UDP (User Datagram Protocol)**: Jest to prostszy protokół transportowy, który oferuje komunikację bezpołączeniową. Nie gwarantuje dostarczenia pakietów ani ich kolejności, ale jest szybszy i bardziej efektywny w pewnych zastosowaniach.

## Protokół TCP/IP

### Charakterystyka TCP

TCP jest protokołem połączeniowym, co oznacza, że przed rozpoczęciem transmisji danych między klientem a serwerem musi zostać ustanowione połączenie. TCP zapewnia:

- **Niezawodność**: Gwarantuje dostarczenie wszystkich danych w odpowiedniej kolejności.
- **Kontrola przepływu**: Reguluje ilość danych przesyłanych między nadawcą a odbiorcą.
- **Kontrola błędów**: Wykrywa i retransmituje utracone lub uszkodzone segmenty danych.

### Zastosowania TCP

- **Przesyłanie plików**: FTP (File Transfer Protocol)
- **Aplikacje webowe**: HTTP/HTTPS
- **Poczta elektroniczna**: SMTP, POP3, IMAP

### Implementacja TCP w Javie

Java dostarcza klasy `Socket` i `ServerSocket` do implementacji komunikacji klient-serwer z użyciem TCP.

#### Klasa `ServerSocket`

Służy do tworzenia serwera, który nasłuchuje na określonym porcie i akceptuje połączenia od klientów.

- **Tworzenie serwera**:
  ```java
  ServerSocket serverSocket = new ServerSocket(int port);
  ```
- **Akceptowanie połączeń**:
  ```java
  Socket clientSocket = serverSocket.accept();
  ```

#### Klasa `Socket`

Służy do tworzenia klienta, który łączy się z serwerem na określonym adresie IP i porcie.

- **Tworzenie klienta**:
  ```java
  Socket socket = new Socket(String host, int port);
  ```

### Przykład komunikacji TCP: Prosty klient-serwer

**Serwer TCP**:

```java
import java.io.*;
import java.net.*;

public class TcpServer {
    public static void main(String[] args) {
        try {
            // Tworzenie gniazda serwera na porcie 8080
            ServerSocket serverSocket = new ServerSocket(8080);
            System.out.println("Serwer nasłuchuje na porcie 8080...");

            // Akceptowanie połączenia od klienta
            Socket clientSocket = serverSocket.accept();
            System.out.println("Połączenie z klientem nawiązane.");

            // Strumienie do komunikacji z klientem
            BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
            PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);

            // Odczytanie wiadomości od klienta
            String message = in.readLine();
            System.out.println("Otrzymano od klienta: " + message);

            // Wysłanie odpowiedzi do klienta
            out.println("Serwer otrzymał twoją wiadomość: " + message);

            // Zamknięcie połączeń
            clientSocket.close();
            serverSocket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**Klient TCP**:

```java
import java.io.*;
import java.net.*;

public class TcpClient {
    public static void main(String[] args) {
        try {
            // Połączenie z serwerem na localhost i porcie 8080
            Socket socket = new Socket("localhost", 8080);

            // Strumienie do komunikacji z serwerem
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));

            // Wysłanie wiadomości do serwera
            out.println("Witaj, serwerze!");

            // Odczytanie odpowiedzi od serwera
            String response = in.readLine();
            System.out.println("Odpowiedź serwera: " + response);

            // Zamknięcie połączenia
            socket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### Analiza kodu

- **Serwer**:
    - Inicjalizuje `ServerSocket` na porcie 8080.
    - Nasłuchuje połączeń za pomocą `accept()`.
    - Po nawiązaniu połączenia, komunikuje się z klientem za pomocą strumieni.
    - Odbiera wiadomość i wysyła odpowiedź.
- **Klient**:
    - Łączy się z serwerem na `localhost` i porcie 8080.
    - Wysyła wiadomość do serwera.
    - Odbiera odpowiedź.

## Protokół UDP

### Charakterystyka UDP

UDP jest protokołem bezpołączeniowym, co oznacza, że nie ma potrzeby ustanawiania połączenia przed wysłaniem danych. Główne cechy UDP:

- **Brak niezawodności**: Nie gwarantuje dostarczenia ani kolejności pakietów.
- **Niski narzut**: Mniejsza ilość danych kontrolnych w pakietach.
- **Szybkość**: Lepsza wydajność w zastosowaniach wymagających szybkiej transmisji.

### Zastosowania UDP

- **Streaming audio/wideo**
- **Gry online**
- **Protokoły czasu rzeczywistego**: VoIP

### Implementacja UDP w Javie

Java dostarcza klasy `DatagramSocket` i `DatagramPacket` do implementacji komunikacji z użyciem UDP.

#### Klasa `DatagramSocket`

Służy do wysyłania i odbierania pakietów UDP.

- **Tworzenie gniazda**:
    - **Serwer**: `DatagramSocket serverSocket = new DatagramSocket(int port);`
    - **Klient**: `DatagramSocket clientSocket = new DatagramSocket();`

#### Klasa `DatagramPacket`

Reprezentuje pakiet danych wysyłany lub odbierany przez `DatagramSocket`.

- **Tworzenie pakietu do wysłania**:
  ```java
  DatagramPacket sendPacket = new DatagramPacket(byte[] buf, int length, InetAddress address, int port);
  ```
- **Tworzenie pakietu do odbioru**:
  ```java
  DatagramPacket receivePacket = new DatagramPacket(byte[] buf, int length);
  ```

### Przykład komunikacji UDP: Prosty klient-serwer

**Serwer UDP**:

```java
import java.net.*;

public class UdpServer {
    public static void main(String[] args) {
        try {
            // Tworzenie gniazda serwera na porcie 9876
            DatagramSocket serverSocket = new DatagramSocket(9876);
            byte[] receiveData = new byte[1024];

            System.out.println("Serwer UDP nasłuchuje na porcie 9876...");

            while (true) {
                // Tworzenie pakietu do odbioru
                DatagramPacket receivePacket = new DatagramPacket(receiveData, receiveData.length);

                // Odbieranie pakietu
                serverSocket.receive(receivePacket);

                // Konwersja danych na tekst
                String message = new String(receivePacket.getData(), 0, receivePacket.getLength());
                System.out.println("Otrzymano od klienta: " + message);

                // Opcjonalnie: Można wysłać odpowiedź do klienta
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**Klient UDP**:

```java
import java.net.*;

public class UdpClient {
    public static void main(String[] args) {
        try {
            // Tworzenie gniazda klienta
            DatagramSocket clientSocket = new DatagramSocket();

            // Adres IP serwera
            InetAddress IPAddress = InetAddress.getByName("localhost");

            // Dane do wysłania
            String sentence = "Witaj, serwerze UDP!";
            byte[] sendData = sentence.getBytes();

            // Tworzenie pakietu do wysłania
            DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, IPAddress, 9876);

            // Wysłanie pakietu
            clientSocket.send(sendPacket);

            // Zamknięcie gniazda
            clientSocket.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Analiza kodu

- **Serwer**:
    - Inicjalizuje `DatagramSocket` na porcie 9876.
    - W pętli nieskończonej odbiera pakiety od klientów.
    - Wyświetla otrzymane wiadomości.
- **Klient**:
    - Tworzy `DatagramSocket` bez określonego portu (system przydzieli automatycznie).
    - Wysyła pakiet z wiadomością do serwera na `localhost` i porcie 9876.

## Porównanie TCP i UDP

| Cechy               | TCP                                                 | UDP                                        |
|---------------------|-----------------------------------------------------|--------------------------------------------|
| **Typ połączenia**  | Połączeniowy (wymaga ustanowienia połączenia)       | Bezpołączeniowy                            |
| **Niezawodność**    | Gwarantuje dostarczenie danych i ich kolejność      | Nie gwarantuje dostarczenia ani kolejności |
| **Kontrola błędów** | Tak                                                 | Minimalna                                  |
| **Prędkość**        | Wolniejszy ze względu na dodatkowe mechanizmy       | Szybszy, mniejszy narzut                   |
| **Zastosowania**    | HTTP, FTP, SMTP, aplikacje wymagające niezawodności | Streaming, VoIP, gry online                |

## Klasa InetAddress

`InetAddress` jest klasą reprezentującą adres IP. Umożliwia uzyskanie informacji o adresach IP i nazwach hostów.

### Użycie klasy InetAddress

- **Uzyskanie adresu IP na podstawie nazwy hosta**:
  ```java
  InetAddress address = InetAddress.getByName("www.example.com");
  System.out.println("Adres IP: " + address.getHostAddress());
  ```
- **Uzyskanie lokalnego adresu IP**:
  ```java
  InetAddress localAddress = InetAddress.getLocalHost();
  System.out.println("Lokalny adres IP: " + localAddress.getHostAddress());
  ```

### **Pytania podstawowe:**

1. **Co to jest protokół TCP/IP i jakie są jego główne cechy?**

   TCP/IP to zestaw protokołów komunikacyjnych stosowanych w sieci internetowej. Główne cechy TCP to: połączeniowość, niezawodność, kontrola przepływu i błędów.

2. **Jakie klasy w Javie służą do implementacji komunikacji klient-serwer z wykorzystaniem TCP?**

   Klasy `Socket` (klient) i `ServerSocket` (serwer).

3. **Jak zaimplementować prostego klienta UDP w Javie?**

   Należy użyć klas `DatagramSocket` i `DatagramPacket` do wysyłania i odbierania danych.

4. **Czym różni się protokół TCP od UDP?**

   TCP jest połączeniowy i niezawodny, a UDP jest bezpołączeniowy i szybszy, lecz mniej niezawodny.

5. **Do czego służy klasa `InetAddress` i jak można jej użyć?**

   `InetAddress` reprezentuje adres IP. Służy do uzyskania adresów IP hostów oraz lokalnych adresów IP.

---

### **Pytania zaawansowane:**

6. **Jak działa trójfazowy proces ustanawiania połączenia w TCP (three-way handshake)?**

   Proces obejmuje wysłanie segmentu SYN przez klienta, odpowiedź SYN-ACK od serwera oraz potwierdzenie ACK od klienta.

7. **Jakie są różnice w implementacji serwera między TCP a UDP w Javie?**

   Serwer TCP używa `ServerSocket` i `Socket` do ustanawiania połączeń, natomiast serwer UDP używa `DatagramSocket` do wysyłania i odbierania pakietów bez połączenia.

8. **Dlaczego UDP jest szybszy od TCP i jakie są konsekwencje tej szybkości?**

   UDP jest szybszy, ponieważ nie wymaga połączenia i retransmisji, co może skutkować utratą lub nieuporządkowaniem pakietów.

9. **Jak można zaimplementować wielowątkowy serwer TCP w Javie?**

   Można użyć pętli nasłuchującej w `ServerSocket`, a każdy wątek klienta obsługiwany jest przez nowy obiekt `Runnable` lub klasę `Thread`.

10. **Jakie są mechanizmy kontroli błędów w protokole TCP?**

    Mechanizmy kontroli błędów to sumy kontrolne, potwierdzenia (ACK), retransmisje oraz zarządzanie oknem.
