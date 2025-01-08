**Projekt: System Analizy Danych Finansowych z Wykorzystaniem Java Stream API i Architektury Hexagonalnej**

---

### **Cel projektu:**

Stworzenie aplikacji Java do analizy danych finansowych z wykorzystaniem Java Stream API oraz obsługą wyjątków, z zastosowaniem architektury hexagonalnej. Aplikacja powinna umożliwiać wczytywanie danych z pliku CSV lub bezpośrednio ze stringa. Backend powinien być odseparowany od widoku, aby mógł być używany jako biblioteka. Komunikacja z użytkownikiem odbywa się przez konsolę.

---

### **Wymagania funkcjonalne:**

1. **Wczytywanie danych finansowych:**
    - Aplikacja powinna umożliwiać wczytywanie danych transakcji finansowych z pliku CSV lub ze stringa.
    - Struktura CSV powinna być zdefiniowana i opisana.

2. **Struktura danych:**
    - **Transakcja**:
        - **Data** (format `YYYY-MM-DD`)
        - **Kwota** (liczba zmiennoprzecinkowa)
        - **Kategoria** (np. FOOD, TRANSPORT, ENTERTAINMENT)
        - **Opis** (tekst)

3. **Przetwarzanie danych z użyciem Stream API:**
    - **Filtrowanie** transakcji według:
        - Zakresu dat
        - Minimalnej/maksymalnej kwoty
        - Kategorii
    - **Sortowanie** transakcji według daty lub kwoty.
    - **Agregacja** danych:
        - Suma wydatków w danej kategorii.
        - Średnia wartość transakcji.
    - **Grupowanie** transakcji według kategorii lub miesiąca.

4. **Generowanie raportu:**
    - Raport tekstowy zawierający podsumowanie analiz.
    - Raport powinien być zwracany jako string lub zapisywany do pliku.
    - Przykładowy raport powinien być przedstawiony.

5. **Interakcja z użytkownikiem:**
    - Aplikacja powinna komunikować się z użytkownikiem przez konsolę.
    - Użytkownik może wybierać kryteria filtrowania i przetwarzania danych.
    - Obsługa komunikatów błędów i wyjątków powinna być czytelna dla użytkownika.

---

### **Wymagania niefunkcjonalne:**

1. **Architektura hexagonalna:**
    - Rozdzielenie warstw aplikacji na:
        - **Domenę** (logika biznesowa)
        - **Porty i adaptery** (wejście/wyjście)
        - **Interfejs użytkownika** (konsola)
    - Umożliwienie łatwej zamiany źródła danych (plik CSV, string) bez modyfikacji logiki biznesowej.

2. **Obsługa wyjątków:**
    - Implementacja obsługi wyjątków takich jak:
        - Błędny format daty lub kwoty.
        - Brak wymaganych danych w CSV.
        - Problemy z dostępem do pliku.
    - Wyświetlanie czytelnych komunikatów błędów dla użytkownika.

3. **Testowalność:**
    - Logika biznesowa powinna być testowalna niezależnie od warstwy interfejsu.
    - Przygotowanie testów jednostkowych dla kluczowych metod.

4. **Czystość kodu i dokumentacja:**
    - Stosowanie konwencji Java.
    - Komentarze i JavaDoc dla publicznych metod i klas.
    - Czytelna struktura pakietów i klas.

---

### **Struktura CSV i przykładowe dane:**

**Struktura CSV:**

```
date,amount,category,description
```

**Przykładowe dane CSV:**

```
date,amount,category,description
2023-01-01,150.00,FOOD,Zakupy spożywcze
2023-01-02,200.00,TRANSPORT,Bilet miesięczny
2023-01-05,50.00,ENTERTAINMENT,Kino
2023-01-10,300.00,FOOD,Restauracja
2023-01-12,120.00,UTILITIES,Rachunek za prąd
```

---

### **Architektura aplikacji:**

1. **Domena (logika biznesowa):**
    - **Model:**
        - `Transaction` - klasa reprezentująca transakcję.
        - `Category` - enum z kategoriami.
    - **Serwisy:**
        - `TransactionService` - zawiera metody do przetwarzania transakcji.

2. **Porty i adaptery:**
    - **Porty:**
        - `TransactionRepository` - interfejs dla źródeł danych.
    - **Adaptery:**
        - `CsvTransactionRepository` - wczytuje dane z pliku CSV.
        - `StringTransactionRepository` - wczytuje dane ze stringa.

3. **Interfejs użytkownika:**
    - `ConsoleUI` - klasa obsługująca interakcję z użytkownikiem.

---

### **Główne metody i ich opis:**

1. **TransactionService:**

    - `List<Transaction> filterTransactions(Predicate<Transaction> predicate)`
        - Filtrowanie transakcji według podanego predykatu.
    - `double calculateTotalAmount(List<Transaction> transactions)`
        - Oblicza sumę kwot dla podanej listy transakcji.
    - `Map<Category, Double> calculateTotalAmountByCategory(List<Transaction> transactions)`
        - Oblicza sumy kwot dla każdej kategorii.
    - `List<Transaction> sortTransactions(Comparator<Transaction> comparator)`
        - Sortuje transakcje według podanego komparatora.

2. **ReportGenerator:**

    - `String generateReport(List<Transaction> transactions)`
        - Generuje raport tekstowy na podstawie listy transakcji.

3. **TransactionRepository:**

    - `List<Transaction> getAllTransactions() throws DataAccessException`
        - Zwraca wszystkie transakcje ze źródła danych.

---

### **Przykładowy raport:**

```
=== RAPORT TRANSAKCJI ===

Suma wszystkich transakcji: 820.00

Suma według kategorii:
- FOOD: 450.00
- TRANSPORT: 200.00
- ENTERTAINMENT: 50.00
- UTILITIES: 120.00

Średnia wartość transakcji: 164.00

Największa transakcja:
- Data: 2023-01-10
- Kwota: 300.00
- Kategoria: FOOD
- Opis: Restauracja

Najmniejsza transakcja:
- Data: 2023-01-05
- Kwota: 50.00
- Kategoria: ENTERTAINMENT
- Opis: Kino
```

---

### **Obsługa błędów i komunikaty:**

1. **Rodzaje błędów:**
    - **Błędny format danych:**
        - Nieprawidłowy format daty (`DateTimeParseException`).
        - Nieprawidłowy format kwoty (`NumberFormatException`).
        - Nieznana kategoria (`IllegalArgumentException`).
    - **Brak danych:**
        - Puste pola w CSV.
    - **Błędy wejścia/wyjścia:**
        - Problemy z odczytem pliku (`IOException`).
    - **Błędy dostępu do danych:**
        - `DataAccessException` - własny wyjątek dla problemów z dostępem do danych.

2. **Przykładowe komunikaty błędów:**
    - "Błąd odczytu pliku: nie znaleziono pliku 'transactions.csv'."
    - "Nieprawidłowy format daty w wierszu 3: '2023/01/05'."
    - "Nieznana kategoria 'FOODD' w wierszu 5."
    - "Pole 'kwota' jest wymagane w wierszu 4."

3. **Obsługa wyjątków:**
    - Wszystkie wyjątki powinny być łapane i przekazywane jako czytelne komunikaty dla użytkownika.
    - Logika biznesowa powinna rzucać własne wyjątki, które będą obsługiwane w warstwie interfejsu.

---

### **Komunikacja z klientem przez konsolę:**

- Użytkownik uruchamia aplikację i jest proszony o wybór źródła danych:
    - "Wybierz źródło danych: 1 - Plik CSV, 2 - Wprowadź dane ręcznie."
- W przypadku wyboru pliku CSV:
    - Użytkownik podaje ścieżkę do pliku.
- W przypadku wprowadzenia danych ręcznie:
    - Użytkownik wprowadza dane w formacie CSV jako string.
- Następnie użytkownik może wybrać operacje:
    - "Wybierz operację: 1 - Wyświetl wszystkie transakcje, 2 - Filtrowanie, 3 - Generuj raport."
- W przypadku filtrowania:
    - Użytkownik jest proszony o podanie kryteriów (np. minimalna kwota, kategoria).
- Po wykonaniu operacji wyniki są wyświetlane w konsoli.

---

### **Przykładowa interakcja z użytkownikiem:**

```
Wybierz źródło danych:
1 - Plik CSV
2 - Wprowadź dane ręcznie
> 1

Podaj ścieżkę do pliku CSV:
> transactions.csv

Dane zostały wczytane pomyślnie.

Wybierz operację:
1 - Wyświetl wszystkie transakcje
2 - Filtrowanie
3 - Generuj raport
> 3

Raport został wygenerowany:
=== RAPORT TRANSAKCJI ===
...

Czy chcesz wykonać inną operację? (T/N)
> N

```
