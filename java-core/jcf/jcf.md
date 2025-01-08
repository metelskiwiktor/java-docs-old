### **Wprowadzenie do Java Collection Framework**

Java Collection Framework (JCF) to zestaw klas i interfejsów, które ułatwiają pracę z grupami obiektów w języku Java.
Wprowadzenie JCF w Javie 2 (wersja 1.2) zrewolucjonizowało sposób obsługi struktur danych, zastępując starsze, mniej
wydajne i niespójne mechanizmy, takie jak `Vector` czy `Hashtable`. Dzięki JCF programiści zyskali spójne, wydajne i
łatwe w użyciu narzędzia do pracy z kolekcjami.

---

### **Co było przed JCF i po co powstał?**

Przed wprowadzeniem JCF, programiści napotykali następujące trudności:

1. **Brak spójnego API**  
   Starsze klasy, takie jak `Vector` czy `Hashtable`, oraz tablice, miały swoje własne, niespójne metody do zarządzania
   danymi. Prowadziło to do złożonego i mało czytelnego kodu.

2. **Problemy z wydajnością**  
   W klasach takich jak `Vector` synchronizacja była wbudowana na poziomie metod, co wprowadzało dodatkowe koszty w
   środowiskach jednowątkowych, gdzie synchronizacja nie była potrzebna.

3. **Brak zaawansowanych struktur danych**  
   Starsze mechanizmy nie oferowały takich struktur jak drzewa, kolejki priorytetowe czy dynamiczne zbiory, co wymuszało
   ręczne ich implementowanie.

4. **Brak typów generycznych (dopiero w Java 5)**  
   Przed wprowadzeniem generyków w Javie 5, kolekcje przechowywały obiekty typu `Object`. Wymagało to rzutowania podczas
   pobierania elementów, co prowadziło do potencjalnych błędów w czasie wykonywania. Generyki, wprowadzone kilka lat po
   JCF, dodatkowo poprawiły jego bezpieczeństwo typów.

**Dlaczego powstało JCF?**

Wprowadzenie Java Collection Framework miało na celu:

- **Standaryzację API** – dostarczenie spójnego zestawu interfejsów i klas do obsługi struktur danych.
- **Zwiększenie wydajności** – wprowadzenie bardziej wydajnych implementacji (np. `ArrayList`, `HashMap`) i opcjonalnej
  synchronizacji (np. `Collections.synchronizedList()`).
- **Ułatwienie rozszerzalności** – dzięki abstrakcyjnym interfejsom, takim jak `Collection`, `List`, `Set` i `Map`,
  programiści mogą tworzyć własne implementacje.
- **Wsparcie dla zaawansowanych struktur danych** – JCF wprowadził dynamiczne struktury, takie jak `TreeMap`,
  `LinkedList` i `PriorityQueue`, które były wcześniej niedostępne w standardowej bibliotece Javy.

---

### **Kluczowe interfejsy w JCF**

Hierarchia JCF jest zorganizowana wokół głównych interfejsów:

- **Iterable**: Najbardziej podstawowy interfejs w JCF, który umożliwia iterację po elementach kolekcji za pomocą pętli
  `for-each`. Wszystkie kolekcje w JCF (oprócz `Map` i interfejsów po niej dziedziczących) implementują ten interfejs.
- **Collection**: Podstawa większości kolekcji. Zawiera ogólne operacje, takie jak dodawanie i usuwanie elementów.
- **List**: Kolekcja uporządkowana, pozwalająca na duplikaty i dostęp do elementów według indeksu. Przykłady:
  `ArrayList`, `LinkedList`.
- **Set**: Kolekcja unikalnych elementów, bez duplikatów. Nie jest zarządzana indeksami oraz nie gwarantuje kolejności.
  Przykłady: `HashSet`, `TreeSet`.
- **Queue**: Kolejka, która umożliwia przetwarzanie elementów w kolejności FIFO. Przykłady: `PriorityQueue`.
  - **Deque**: Rozszerza interfejs `Queue` i obsługuje operacje wstawiania i usuwania elementów z obu końców (FIFO lub
    LIFO). Przykłady: `ArrayDeque`, `LinkedList`.
- **Map**: Struktura mapująca klucze na wartości. Przykłady: `HashMap`, `TreeMap`.

---

### **Hierarchia JCF**

Hierarchia JCF pokazuje, jak różne interfejsy są ze sobą powiązane. Główna hierarchia wygląda następująco:

```
           Iterable
              |
          Collection
        /     |    \
      List   Set   Queue
                      \
                     Deque     
```

Obok głównej hierarchii `Collection`, istnieje osobna hierarchia dla `Map`:

```
           Map
            |
        SortedMap
```

---

### **Podsumowanie implementacji interfejsów JCF**

W Java Collection Framework różne interfejsy mają swoje specyficzne implementacje, które są zoptymalizowane pod kątem
różnych scenariuszy. Poniżej znajduje się tabelaryczne zestawienie głównych interfejsów oraz ich typowych implementacji:

| **Interface** | **Hash Table** | **Resizable Array** | **Balanced Tree** | **Linked List** | **Hash Table + Linked List** |
|---------------|----------------|---------------------|-------------------|-----------------|------------------------------|
| **Set**       | `HashSet`      |                     | `TreeSet`         |                 | `LinkedHashSet`              |
| **List**      |                | `ArrayList`         |                   | `LinkedList`    |                              |
| **Deque**     |                | `ArrayDeque`        |                   | `LinkedList`    |                              |
| **Map**       | `HashMap`      |                     | `TreeMap`         |                 | `LinkedHashMap`              |

---

### **Krótka charakterystyka implementacji**

1. **Hash Table**
    - Nazwa pochodzi od użycia funkcji haszującej (`hash`), która przekształca klucze w indeksy, oraz struktury
      tablicy (`table`) do przechowywania danych.
    - Wykorzystywana w `HashSet` i `HashMap`, zapewnia szybkie operacje wyszukiwania i wstawiania (średnio O(1)).

2. **Resizable Array**
    - Dynamicznie zmieniająca rozmiar tablica (`resizable` oznacza możliwość powiększania), podstawowa struktura w
      `ArrayList` i `ArrayDeque`.
    - Zapewnia szybki dostęp do elementów przez indeks (O(1)).

3. **Balanced Tree**
    - Struktura drzewa binarnego, które automatycznie utrzymuje równowagę (`balanced`) dla optymalnej wydajności
      operacji.
    - Wykorzystywana w `TreeSet` i `TreeMap`, gwarantuje uporządkowanie elementów i złożoność O(log n).

4. **Linked List**
    - Sekwencja elementów powiązanych referencjami (`linked`), co pozwala na wydajne dodawanie i usuwanie elementów na
      końcach.
    - Używana w `LinkedList` jako implementacja zarówno `List`, jak i `Deque`.

5. **Hash Table + Linked List**
    - Łączy tablicę haszującą dla szybkiego dostępu (`Hash Table`) z listą powiązaną dla zachowania kolejności
      wstawiania (`Linked List`).
    - Wykorzystywana w `LinkedHashSet` i `LinkedHashMap`.

--- 

### **Kolekcje niesparametryzowane a generyczne**

Przed wprowadzeniem typów generycznych programiści używali niesparametryzowanych kolekcji (bazujących na `Object`), co
prowadziło do potencjalnych błędów
w czasie wykonywania. Dzięki generykom można określić typ elementów kolekcji podczas deklaracji, co zwiększa
bezpieczeństwo typów i ułatwia czytelność kodu.

**Przykład niesparametryzowanej kolekcji:**
```java
List list = new ArrayList();
list.add("tekst");
list.add(123); // Brak błędu w czasie kompilacji
String str = (String) list.get(1); // Spowoduje ClassCastException
```

**Przykład generycznej kolekcji:**

```java
List<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
names.add(123); // Kompilator zgłosi błąd
```

---

### **Jak JCF ewoluowało z czasem?**

Java Collection Framework (JCF) był stale rozwijany, aby sprostać rosnącym wymaganiom programistów oraz zmieniającym się
trendom w tworzeniu oprogramowania. Oto kluczowe etapy jego ewolucji:

1. **Java 2 (1.2)** – Wprowadzenie JCF:
    - Pojawienie się podstawowych interfejsów (`Collection`, `List`, `Set`, `Map`, `Queue`).
    - Implementacje takie jak `ArrayList`, `LinkedList`, `HashSet`, `HashMap` oraz `TreeMap`.
    - Klasa `Collections` z podstawowymi operacjami na kolekcjach.

2. **Java 5 (1.5)** – Typy generyczne i usprawnienia:
    - Wprowadzenie generyków, które zwiększyły bezpieczeństwo typów i zmniejszyły konieczność rzutowania.
    - `ConcurrentHashMap` – bezpieczna dla wątków implementacja mapy.
    - `PriorityQueue` – kolejka priorytetowa do przetwarzania elementów według ustalonego porządku.
    - For-each (pętla iteracyjna), która upraszcza iterację po kolekcjach implementujących `Iterable`.

3. **Java 8** – Funkcyjność i strumienie:
    - Wprowadzenie **Stream API**, umożliwiającego przetwarzanie danych w sposób deklaratywny.
    - Metody domyślne w interfejsach (np. `forEach`, `removeIf`, `spliterator`).
    - `Map` otrzymał nowe funkcje, takie jak `compute`, `merge`, `forEach`.

4. **Java 9+** – Kolekcje niemodyfikowalne i usprawnienia:
    - Metody fabryczne do tworzenia kolekcji niemodyfikowalnych (`List.of`, `Set.of`, `Map.of`).
    - Ulepszenia w `Stream` API, takie jak metody `takeWhile` i `dropWhile`.
    - `Map` z obsługą `Optional` (`getOrDefault`).

5. **Java 20+** – Nowoczesne optymalizacje:
    - Zoptymalizowano `HashMap` i `HashSet` pod kątem mniejszych zbiorów, zmniejszając narzut pamięciowy.
    - `ConcurrentHashMap` otrzymał nowe metody wspierające równoległe przetwarzanie danych, takie jak `reduce`.
    - `List.copyOf` i `Map.copyOf` zostały ulepszone, aby szybciej tworzyć niemodyfikowalne kopie kolekcji.

---

### **Dodatkowe zasoby**

- Oficjalna dokumentacja:
  - [Oracle Collections Framework Overview](https://docs.oracle.com/javase/8/docs/technotes/guides/collections/overview.html)
  - [Oracle Collections Framework Api Reference](https://docs.oracle.com/javase/8/docs/technotes/guides/collections/reference.html)
  - [Oracle Collections Framework Tutorials](https://docs.oracle.com/javase/tutorial/collections/interfaces/index.html)
