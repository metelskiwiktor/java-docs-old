### Wprowadzenie do Node

#### Węzeł (Node) to podstawowy element w strukturach danych takich jak listy, stosy, drzewa czy grafy.
Węzeł zwykle przechowuje dane i wskaźnik (referencję) do następnego węzła (lub węzłów, w przypadku bardziej złożonych struktur).

---

### 1. **Wprowadzenie do relacji między obiektami**

#### Zadanie 1: "Łańcuch znajomych"
- **Opis**: Stwórz klasę `Person`, która ma dwa pola:
    - `name` (String): imię znajomego.
    - `friend` (referencja do innej osoby `Person`).

- **Cel**: Zbuduj łańcuch znajomych, np.:
   ```
   Alice -> Bob -> Charlie
   ```

- **Zadanie**:
    1. Utwórz obiekty `Person` dla osób Alice, Bob i Charlie.
    2. Połącz ich w łańcuch za pomocą pola `friend`.
    3. Napisz metodę `void printFriends(Person person)`, która wypisze wszystkie imiona w łańcuchu znajomych, zaczynając od podanej osoby, używając pętli `while`.

---

### 2. **Proste operacje na relacjach**

#### Zadanie 2: "Dodawanie znajomego"
- **Opis**: Rozbuduj klasę `Person`, aby miała metodę `addFriend(Person newFriend)`, która dodaje nowego znajomego na końcu łańcucha.

- **Przykład**:
   ```
   Alice -> Bob -> Charlie
   Alice.addFriend(Dave)
   Wynik: Alice -> Bob -> Charlie -> Dave
   ```

- **Zadanie**:
    1. Zaimplementuj metodę `addFriend`.
    2. Metoda powinna przejść przez cały łańcuch znajomych, aż znajdzie ostatnią osobę, i ustawić jej pole `friend` na `newFriend`.
    3. Przetestuj dodawanie nowych znajomych, dodając kilku do istniejącego łańcucha.

#### Zadanie 3: "Wyszukiwanie znajomego"
- **Opis**: Dodaj metodę `Person findFriend(String name)`, która przeszuka łańcuch znajomych i zwróci obiekt osoby o podanym imieniu lub `null`, jeśli taka osoba nie istnieje.

- **Przykład**:
   ```
   Alice -> Bob -> Charlie
   Alice.findFriend("Bob") -> Obiekt Bob
   Alice.findFriend("Dave") -> null
   ```

- **Zadanie**:
    1. Zaimplementuj metodę `findFriend`, która przechodzi przez łańcuch znajomych, używając pętli `while`, i porównuje imiona (`name`).
    2. Przetestuj wyszukiwanie znajomych w różnych scenariuszach.


#### Zadanie 4: "Usuwanie znajomego"
- **Opis**: Dodaj metodę `void removeFriend(String name)`, która usuwa znajomego o podanym imieniu.

- **Przykład**:
   ```
   Alice -> Bob -> Charlie
   Alice.removeFriend("Bob")
   Wynik: Alice -> Charlie
   ```

- **Podpowiedź do implementacji**:
    - Użyj metody `findFriend(String name)` jako pierwszego kroku.
    - Usuń znajomego, zmieniając referencję `friend`, aby pomijała usuwaną osobę:
    - Pamiętaj o obsłużeniu sytuacji, gdy znajomy do usunięcia jest pierwszy w łańcuchu.

---

### 4. **Tworzenie własnych struktur na bazie node'ów**

#### Zadanie 5: "Stos na node'ach"
- **Opis**: Stos (ang. Stack) to struktura danych, która działa na zasadzie **Last In, First Out** (LIFO). Oznacza to, że element dodany jako ostatni jest usuwany jako pierwszy.

- **Zadanie**:
    1. Dodaj metody:
        - `push(int value)` – dodaje element na szczyt stosu.
        - `pop()` – usuwa i zwraca element ze szczytu stosu.
        - `peek()` – zwraca element na szczycie stosu bez jego usuwania.
    2. Przetestuj działanie stosu, dodając kilka elementów, a następnie usuwając je.
    3. Sprawdź, co znajduje się na szczycie stosu, używając metody `peek`.
