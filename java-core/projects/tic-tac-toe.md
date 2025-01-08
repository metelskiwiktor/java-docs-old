**Wymagania projektu "Kółko i krzyżyk"**

---

**1. Opis ogólny**

Celem projektu jest stworzenie gry "Kółko i krzyżyk" w języku Java. Aplikacja powinna umożliwiać rozgrywkę w dwóch trybach interfejsu użytkownika:

- **Interfejs konsolowy**: Gra z wykorzystaniem konsoli tekstowej.
- **Interfejs graficzny (JavaFX)**: Gra z wykorzystaniem biblioteki JavaFX.

Gracz powinien mieć możliwość wyboru trybu gry:

- **Gra przeciwko innemu graczowi**: Dwóch graczy grających na zmianę.
- **Gra przeciwko botowi**: Gracz przeciwko komputerowi (botowi).

---

**2. Wymagania funkcjonalne**

**2.1. Tryby gry**

- **Gra dwóch graczy**:
    - Gracze na zmianę wykonują ruchy, wprowadzając swoje symbole ("X" lub "O") na planszę.
    - Aplikacja informuje o kolejności ruchów i wynikach gry.

- **Gra przeciwko botowi**:
    - Gracz wykonuje ruch pierwszy lub drugi, w zależności od wyboru.
    - Bot wykonuje ruchy automatycznie zgodnie z zaprogramowaną logiką.

**2.2. Interfejs użytkownika**

- **Konsolowy**:
    - Wyświetlanie planszy gry w formacie tekstowym.
    - Pobieranie ruchów od użytkownika poprzez wprowadzenie współrzędnych.

- **Graficzny (JavaFX)**:
    - Wizualna reprezentacja planszy 3x3.
    - Obsługa zdarzeń myszki do wykonywania ruchów.
    - Interaktywne komunikaty dla użytkownika (np. informacja o wygranej, remisie).

**2.3. Funkcjonalności gry**

- **Inicjalizacja gry**:
    - Możliwość wyboru trybu gry i interfejsu przy uruchomieniu aplikacji.

- **Przebieg gry**:
    - Aktualizacja planszy po każdym ruchu.
    - Sprawdzanie warunków wygranej lub remisu po każdym ruchu.
    - Informowanie graczy o aktualnym stanie gry.

- **Zakończenie gry**:
    - Ogłoszenie wyniku (wygrana jednego z graczy lub remis).
    - Możliwość rozpoczęcia nowej gry bez konieczności zamykania aplikacji.

---

**3. Proces implementacyjny**

Projekt powinien być realizowany w trzech głównych fazach:

**Faza 1: Implementacja logiki gry z interfejsem konsolowym**

- **Cel**: Stworzenie podstawowej wersji gry, w której dwóch graczy gra na zmianę, korzystając z konsoli.
- **Zadania**:
    - Zaimplementowanie logiki gry:
        - Reprezentacja planszy (np. tablica dwuwymiarowa).
        - Mechanizmy sprawdzania poprawności ruchów.
        - Algorytmy sprawdzania warunków wygranej lub remisu.
    - Utworzenie interfejsu konsolowego:
        - Wyświetlanie aktualnego stanu planszy w konsoli.
        - Pobieranie ruchów od graczy poprzez wprowadzenie współrzędnych (np. wiersz i kolumna).
    - Testowanie funkcjonalności poprzez symulację rozgrywki między dwoma graczami.

**Faza 2: Dodanie interfejsu graficznego (JavaFX)**

- **Cel**: Umożliwienie gry z wykorzystaniem interfejsu graficznego, poprawiając doświadczenie użytkownika.
- **Zadania**:
    - Stworzenie interfejsu graficznego przy użyciu JavaFX:
        - Wizualna reprezentacja planszy za pomocą przycisków lub pól.
        - Obsługa zdarzeń myszki do rejestrowania ruchów gracza.
        - Wyświetlanie komunikatów (np. informacja o wygranej, remisie).
    - Integracja interfejsu graficznego z istniejącą logiką gry.
    - Dodanie możliwości wyboru interfejsu (konsolowy lub graficzny) przy starcie aplikacji.

**Faza 3: Dodanie bota do gry w obu interfejsach**

- **Cel**: Umożliwienie gry przeciwko komputerowi (botowi) zarówno w konsoli, jak i w interfejsie graficznym.
- **Zadania**:
    - Zaimplementowanie klasy `BotPlayer`, która będzie implementować interfejs `Player`:
        - Zaprogramowanie logiki bota (np. losowe wybieranie wolnych pól lub prosta strategia).
    - Modyfikacja mechanizmu inicjalizacji gry, aby umożliwić wybór gry przeciwko botowi.
    - Aktualizacja interfejsów użytkownika:
        - W konsoli: wyświetlanie ruchów bota i komunikatów dla gracza.
        - W interfejsie graficznym: automatyczne aktualizowanie planszy po ruchu bota.
    - Testowanie gry przeciwko botowi w obu interfejsach.

---

**4. Końcowe wymagania niefunkcjonalne**

- **Język programowania**: Java.
- **Biblioteka graficzna**: JavaFX dla interfejsu graficznego.
- **Czytelność i jakość kodu**:
    - Stosowanie konwencji nazewniczych Java.
    - Dodawanie komentarzy wyjaśniających działanie kluczowych fragmentów kodu.
    - Modularność i podział na logiczne komponenty.
- **Rozszerzalność**:
    - Projektowanie kodu w sposób umożliwiający łatwe dodawanie nowych funkcjonalności (np. różne poziomy trudności bota).
- **Testowanie**:
    - Przeprowadzanie testów jednostkowych dla kluczowych metod i klas logiki aplikacji.

---

**5. Proces rozszerzania kodu**

Aby ułatwić przyszłe modyfikacje i rozszerzenia aplikacji, należy:

- **Stosować zasady programowania obiektowego**:
    - Wykorzystać klasy i obiekty do reprezentacji elementów gry (np. `Game`, `Board`, `Player`).
- **Wydzielić warstwy aplikacji**:
    - **Logika gry**: Klasy odpowiedzialne za mechanikę gry, niezależne od interfejsu użytkownika.
    - **Interfejs użytkownika**: Klasy odpowiedzialne za interakcję z użytkownikiem (konsolowy i graficzny).
- **Zastosować interfejsy i dziedziczenie**:
    - Utworzyć interfejs `Player` z metodą `makeMove()`, który będzie implementowany przez `HumanPlayer` i `BotPlayer`.
- **Sugerowane klasy i ich odpowiedzialności**:
    - **Klasa `Game`**:
        - Zarządza przebiegiem gry, inicjalizuje graczy i planszę.
        - Kontroluje kolejność ruchów i sprawdzanie stanu gry.
    - **Klasa `Board`**:
        - Przechowuje stan planszy.
        - Udostępnia metody do modyfikacji i wyświetlania planszy.
    - **Interfejs `Player`**:
        - Definiuje metody wymagane dla gracza (np. `makeMove()`).
    - **Klasa `HumanPlayer`** (implementuje `Player`):
        - Odpowiada za pobieranie ruchów od użytkownika.
        - Współpracuje z wybranym interfejsem użytkownika.
    - **Klasa `BotPlayer`** (implementuje `Player`):
        - Generuje ruchy na podstawie zaprogramowanej logiki.
    - **Klasa `ConsoleUI`**:
        - Obsługuje interakcje w trybie konsolowym.
        - Wyświetla planszę i komunikaty tekstowe.
        - Pobiera dane od użytkownika.
    - **Klasa `GUI`**:
        - Obsługuje interakcje w trybie graficznym przy użyciu JavaFX.
        - Rysuje planszę i reaguje na zdarzenia użytkownika.
        - Aktualizuje interfejs po każdym ruchu.
