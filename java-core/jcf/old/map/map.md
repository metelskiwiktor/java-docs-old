# Interfejsy `Map` i `SortedMap` w Java Collections Framework

## 1. Wstęp

### Znaczenie kolekcji map

Mapy są kluczowym elementem kolekcji w Javie, umożliwiając przechowywanie par klucz-wartość. Pozwalają na szybki dostęp do danych na podstawie unikalnych kluczy, co jest niezbędne w wielu aplikacjach, takich jak bazy danych, cache czy implementacje słowników.

---

## 2. Interfejs `Map`

### Definicja i podstawowe cechy

`Map` to interfejs w Java Collections Framework, który reprezentuje strukturę danych przechowującą pary klucz-wartość. Klucze są unikalne, co oznacza, że w mapie nie może istnieć więcej niż jeden element o tym samym kluczu. Wartości mogą się powtarzać.

**Kluczowe właściwości:**

- **Unikalność kluczy:** Każdy klucz w mapie jest unikalny.
- **Asocjacja klucz-wartość:** Mapy przechowują dane w formie par klucz-wartość.
- **Brak gwarancji kolejności:** Standardowy `Map` nie gwarantuje kolejności przechowywania elementów.

### Struktura interfejsu `Map`

Interfejs `Map` jest podstawowym interfejsem map w Java Collections Framework. Poniżej przedstawiono uproszczoną definicję interfejsu `Map`:

```java
public interface Map<K, V> {

    interface Entry<K, V> {
        K getKey();
        V getValue();
        V setValue(V value);
        boolean equals(Object o);
        int hashCode();
    }

    // Podstawowe metody Map
    int size();
    boolean isEmpty();
    boolean containsKey(Object key);
    boolean containsValue(Object value);
    V get(Object key);
    V put(K key, V value);
    V remove(Object key);
    void putAll(Map<? extends K, ? extends V> m);
    void clear();

    // Widoki na mapę
    Set<K> keySet();
    Collection<V> values();
    Set<Map.Entry<K, V>> entrySet();

    // Domyślne metody (Java 8 i nowsze)
    default V getOrDefault(Object key, V defaultValue) { ... }
    default void forEach(BiConsumer<? super K, ? super V> action) { ... }
    default void replaceAll(BiFunction<? super K, ? super V, ? extends V> function) { ... }
    default V putIfAbsent(K key, V value) { ... }
    default boolean remove(Object key, Object value) { ... }
    default boolean replace(K key, V oldValue, V newValue) { ... }
    default V replace(K key, V value) { ... }
    default V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction) { ... }
    default V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction) { ... }
    default V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction) { ... }
    default V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction) { ... }
}
```

### Rola interfejsu `Entry`

Interfejs `Map.Entry` jest wewnętrznym interfejsem `Map`, który reprezentuje pojedynczą parę klucz-wartość w mapie. Umożliwia dostęp do klucza i wartości oraz modyfikację wartości.

**Kluczowe metody interfejsu `Entry`:**

- `K getKey()`: Zwraca klucz wpisu.
- `V getValue()`: Zwraca wartość wpisu.
- `V setValue(V value)`: Ustawia nową wartość dla wpisu.
- `boolean equals(Object o)`: Porównuje wpis z innym obiektem.
- `int hashCode()`: Zwraca kod haszujący wpisu.

**Rola i zastosowanie:**

Interfejs `Entry` jest używany głównie podczas iteracji po zestawie wpisów mapy uzyskanym za pomocą metody `entrySet()`. Pozwala to na dostęp i modyfikację zarówno kluczy, jak i wartości.

**Przykład użycia `Map.Entry`:**

```java
Map<String, Integer> scores = new HashMap<>();

scores.put("Alice", 90);
scores.put("Bob", 85);
scores.put("Charlie", 95);

for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

### Główne metody interfejsu `Map`

Interfejs `Map` definiuje zestaw metod, które muszą być zaimplementowane przez klasy go implementujące:

- **Operacje podstawowe:**
    - `V put(K key, V value)`: Wstawia parę klucz-wartość do mapy.
    - `V get(Object key)`: Zwraca wartość związaną z danym kluczem.
    - `V remove(Object key)`: Usuwa parę klucz-wartość na podstawie klucza.
    - `boolean containsKey(Object key)`: Sprawdza, czy mapa zawiera dany klucz.
    - `boolean containsValue(Object value)`: Sprawdza, czy mapa zawiera daną wartość.
    - `int size()`: Zwraca liczbę par klucz-wartość w mapie.
    - `boolean isEmpty()`: Sprawdza, czy mapa jest pusta.
    - `void clear()`: Usuwa wszystkie pary klucz-wartość z mapy.

- **Widoki na mapę:**
    - `Set<K> keySet()`: Zwraca zestaw wszystkich kluczy w mapie.
    - `Collection<V> values()`: Zwraca kolekcję wszystkich wartości w mapie.
    - `Set<Map.Entry<K, V>> entrySet()`: Zwraca zestaw wpisów (par klucz-wartość) w mapie.

- **Domyślne metody (Java 8 i nowsze):**
    - `default V getOrDefault(Object key, V defaultValue)`: Zwraca wartość dla klucza lub domyślną, jeśli klucz nie istnieje.
    - `default void forEach(BiConsumer<? super K, ? super V> action)`: Wykonuje akcję dla każdego wpisu w mapie.
    - `default void replaceAll(BiFunction<? super K, ? super V, ? extends V> function)`: Zastępuje wszystkie wartości na podstawie funkcji.
    - `default V putIfAbsent(K key, V value)`: Wstawia wartość tylko wtedy, gdy klucz nie jest obecny.
    - `default boolean remove(Object key, Object value)`: Usuwa parę tylko wtedy, gdy klucz jest mapowany na daną wartość.
    - `default boolean replace(K key, V oldValue, V newValue)`: Zastępuje wartość tylko wtedy, gdy jest równa starej wartości.
    - `default V replace(K key, V value)`: Zastępuje wartość dla danego klucza.
    - `default V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)`: Oblicza wartość i wstawia ją, jeśli klucz nie jest obecny.
    - `default V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)`: Oblicza nową wartość na podstawie istniejącej.
    - `default V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)`: Oblicza wartość dla klucza, niezależnie od tego, czy klucz jest obecny.
    - `default V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)`: Łączy wartości dla danego klucza.

### Implementacje interfejsu `Map`

- **`HashMap`**: Implementacja oparta na tablicy haszującej. Nie gwarantuje kolejności przechowywania elementów. Oferuje szybki dostęp do danych dzięki użyciu funkcji hashującej.

- **`LinkedHashMap`**: Rozszerza `HashMap`, utrzymując kolejność wstawiania elementów lub kolejność dostępu, jeśli jest skonfigurowana w trybie dostępowym.

- **`TreeMap`**: Implementuje interfejs `SortedMap`, przechowując klucze w naturalnym porządku lub według dostarczonego komparatora.

### Przykłady użycia `Map`

```java
Map<String, Integer> studentScores = new HashMap<>();

// Dodawanie elementów
studentScores.put("Jan Kowalski", 85);
studentScores.put("Anna Nowak", 92);
studentScores.put("Piotr Wiśniewski", 78);

// Dostęp do wartości
int score = studentScores.get("Anna Nowak"); // 92

// Iteracja po kluczach
for (String student : studentScores.keySet()) {
    System.out.println(student);
}

// Iteracja po wartościach
for (Integer s : studentScores.values()) {
    System.out.println(s);
}

// Iteracja po wpisach
for (Map.Entry<String, Integer> entry : studentScores.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

**Zastosowania praktyczne:**

- Przechowywanie danych konfiguracyjnych.
- Implementacja cache.
- Mapowanie identyfikatorów na obiekty.

---

## 3. Interfejs `SortedMap`

### Definicja i rozszerzenia względem `Map`

`SortedMap` to interfejs, który rozszerza `Map`, gwarantując, że klucze są uporządkowane. Kolejność może być naturalna (np. dla typów liczbowych lub `String`) lub zdefiniowana przez dostarczony komparator.

Poniżej przedstawiono uproszczoną definicję interfejsu `SortedMap`:

```java
public interface SortedMap<K, V> extends Map<K, V> {

    Comparator<? super K> comparator();

    SortedMap<K, V> subMap(K fromKey, K toKey);

    SortedMap<K, V> headMap(K toKey);

    SortedMap<K, V> tailMap(K fromKey);

    K firstKey();

    K lastKey();

    Set<K> keySet();

    Collection<V> values();

    Set<Map.Entry<K, V>> entrySet();
}
```

**Główne różnice względem `Map`:**

- **Uporządkowanie kluczy:** `SortedMap` zapewnia, że klucze są przechowywane w określonym porządku.
- **Dodatkowe metody:** Dostarcza metody umożliwiające operacje na zakresach kluczy.
- **Wymagania dotyczące kluczy:** Klucze muszą być wzajemnie porównywalne.

### Rola interfejsu `SortedMap`

Interfejs `SortedMap` rozszerza `Map` i definiuje dodatkowe metody dla map, w których klucze są przechowywane w określonym porządku. Umożliwia to:

- Iterację po kluczach w uporządkowany sposób.
- Wykonywanie operacji na zakresach kluczy.
- Dostęp do pierwszego i ostatniego klucza.

### Główne metody interfejsu `SortedMap`

- **Porządkowanie i porównywanie:**
    - `Comparator<? super K> comparator()`: Zwraca komparator używany do sortowania kluczy lub `null`, jeśli używany jest naturalny porządek.

- **Operacje na zakresach kluczy:**
    - `SortedMap<K, V> subMap(K fromKey, K toKey)`: Zwraca widok części mapy zawierający klucze od `fromKey` (włącznie) do `toKey` (wyłącznie).
    - `SortedMap<K, V> headMap(K toKey)`: Zwraca widok mapy z kluczami mniejszymi niż `toKey`.
    - `SortedMap<K, V> tailMap(K fromKey)`: Zwraca widok mapy z kluczami większymi lub równymi `fromKey`.

- **Dostęp do ekstremów:**
    - `K firstKey()`: Zwraca pierwszy (najmniejszy) klucz.
    - `K lastKey()`: Zwraca ostatni (największy) klucz.

### Implementacje interfejsu `SortedMap`

- **`TreeMap`**: Najczęściej używana implementacja `SortedMap`. Wykorzystuje drzewo czerwono-czarne, zapewniając logarytmiczną złożoność podstawowych operacji.

### Przykłady użycia `SortedMap`

```java
SortedMap<String, Integer> sortedScores = new TreeMap<>();

// Dodawanie elementów
sortedScores.put("Jan Kowalski", 85);
sortedScores.put("Anna Nowak", 92);
sortedScores.put("Piotr Wiśniewski", 78);

// Iteracja w porządku alfabetycznym kluczy
for (Map.Entry<String, Integer> entry : sortedScores.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// Użycie subMap
SortedMap<String, Integer> subMap = sortedScores.subMap("Anna Nowak", "Piotr Wiśniewski");
for (Map.Entry<String, Integer> entry : subMap.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

**Praktyczne zastosowania:**

- Implementacje słowników.
- Aplikacje wymagające danych posortowanych, np. raporty, rankingi.
- Operacje na zakresach danych.

---

## 4. Porównanie `Map` i `SortedMap`

### Kiedy używać `Map`, a kiedy `SortedMap`

- **`Map`:** Gdy kolejność kluczy nie ma znaczenia, a priorytetem jest szybki dostęp do danych. `HashMap` oferuje szybkie operacje dzięki użyciu funkcji hashującej.

- **`SortedMap`:** Gdy ważne jest utrzymanie kluczy w określonym porządku. Ułatwia to iterację w kolejności oraz operacje na zakresach kluczy.

### Wydajność i zachowanie

#### `HashMap`

- **Złożoność operacji:**
    - Średnio: O(1) dla operacji `put`, `get`, `remove`.
    - W najgorszym przypadku: Może wzrosnąć do O(n) (przed Java 8) lub O(log n) (od Java 8), w przypadku wielu kolizji hashów.

- **Kolizje hashów:**
    - **Co to jest kolizja?** Gdy dwa różne klucze mają ten sam wynik funkcji `hashCode()`.
    - **Wpływ na wydajność:** Kolizje mogą powodować degradację wydajności operacji.
    - **Mechanizmy obsługi kolizji:**
        - **Przed Java 8:** Kolizje były przechowywane w listach powiązanych.
        - **Od Java 8:** Po przekroczeniu pewnego progu, struktura zmienia się na drzewo czerwono-czarne.

#### `TreeMap`

- **Złożoność operacji:**
    - O(log n) dla operacji `put`, `get`, `remove`.

- **Uporządkowanie kluczy:**
    - Klucze są przechowywane w określonym porządku, co umożliwia efektywne operacje na zakresach kluczy.

**Różnice w zachowaniu:**

- **Modyfikacje kolekcji:** W `SortedMap`, modyfikacja klucza może wpłynąć na jego pozycję w mapie.
- **Null klucze i wartości:**
    - `HashMap` pozwala na jeden `null` klucz i dowolną liczbę `null` wartości.
    - `TreeMap` nie pozwala na `null` jako klucz, chyba że dostarczony komparator to obsługuje.

---

## 5. Praktyczne przykłady i przypadki użycia

### Kod źródłowy

**Przykład użycia `Map`:**

```java
Map<Integer, String> productCatalog = new HashMap<>();

productCatalog.put(1001, "Laptop");
productCatalog.put(1002, "Smartphone");
productCatalog.put(1003, "Tablet");

// Dostęp do produktu
String product = productCatalog.get(1002); // "Smartphone"
```

**Przykład użycia `SortedMap`:**

```java
SortedMap<LocalDate, String> eventSchedule = new TreeMap<>();

eventSchedule.put(LocalDate.of(2023, 9, 20), "Spotkanie programistów");
eventSchedule.put(LocalDate.of(2023, 10, 5), "Konferencja Java");
eventSchedule.put(LocalDate.of(2023, 11, 12), "Warsztaty Spring");

// Iteracja po wydarzeniach w porządku chronologicznym
for (Map.Entry<LocalDate, String> event : eventSchedule.entrySet()) {
    System.out.println(event.getKey() + ": " + event.getValue());
}
```

### Analiza przypadków użycia

**Przypadek 1: Implementacja cache z limitem czasu**

- **Problem:** Potrzeba przechowywania obiektów z możliwością ich wygaszenia po określonym czasie.
- **Rozwiązanie:** Użycie `SortedMap` z czasami wygaszenia jako kluczami pozwala na łatwe identyfikowanie i usuwanie przeterminowanych obiektów.

**Przypadek 2: System rezerwacji**

- **Problem:** Zarządzanie rezerwacjami według identyfikatorów i dat.
- **Rozwiązanie:** Użycie `Map` do szybkiego dostępu do rezerwacji po identyfikatorze oraz `SortedMap` do przeglądania rezerwacji w kolejności chronologicznej.

---

## 6. Najlepsze praktyki

### Optymalny wybór implementacji

- **Analiza wymagań:** Zastanów się, czy potrzebujesz uporządkowania kluczy.
- **Wydajność:** Jeśli priorytetem jest szybkość wstawiania i wyszukiwania, a kolejność nie ma znaczenia, wybierz `HashMap`.
- **Kolejność wstawiania:** Jeśli chcesz zachować kolejność wstawiania, użyj `LinkedHashMap`.

### Unikanie typowych błędów

- **Mutowalne klucze:** Unikaj używania obiektów, które mogą się zmieniać, jako kluczy.
- **Null klucze i wartości:** Bądź świadomy, które implementacje pozwalają na `null` jako klucz lub wartość.
- **Równoważność kluczy:** Zapewnij poprawną implementację metod `equals()` i `hashCode()` dla obiektów używanych jako klucze.

### Zarządzanie wielowątkowością

- **Bezpieczeństwo wątkowe:** Standardowe implementacje `Map` nie są bezpieczne w środowiskach wielowątkowych.
- **Synchronizacja:** Użyj `Collections.synchronizedMap()` lub implementacji z `java.util.concurrent`, takich jak `ConcurrentHashMap`, jeśli potrzebujesz bezpiecznego dostępu z wielu wątków.

---

## 7. Podsumowanie

- `Map` to interfejs przechowujący pary klucz-wartość z unikalnymi kluczami.
- `SortedMap` rozszerza `Map`, zapewniając uporządkowanie kluczy.
- Struktura interfejsów obejmuje definicje metod i rolę wewnętrznego interfejsu `Entry`.
- Wybór implementacji zależy od wymagań dotyczących kolejności i wydajności.
- Złożoność operacji w `HashMap` jest średnio O(1), ale może ulec degradacji w przypadku kolizji hashów.
- Najlepsze praktyki obejmują świadomy wybór struktury danych, unikanie typowych błędów i zarządzanie wielowątkowością.

---

## 8. Pytania kontrolne z odpowiedziami

### Pytania podstawowe

1. **Co to jest interfejs `Map` i jakie są jego podstawowe cechy?**

   Interfejs `Map` reprezentuje strukturę danych przechowującą pary klucz-wartość. Podstawowe cechy:

    - **Unikalność kluczy:** Każdy klucz jest unikalny.
    - **Asocjacja klucz-wartość:** Umożliwia przechowywanie i dostęp do wartości na podstawie klucza.
    - **Brak gwarancji kolejności:** Standardowe implementacje `Map` nie gwarantują kolejności elementów.

2. **Jakie są główne różnice między `Map` a `SortedMap`?**

    - **Uporządkowanie kluczy:**
        - `Map` nie zapewnia kolejności kluczy.
        - `SortedMap` utrzymuje klucze w określonym porządku (naturalnym lub zdefiniowanym przez komparator).
    - **Dodatkowe metody:**
        - `SortedMap` oferuje metody do operacji na zakresach kluczy (`subMap()`, `headMap()`, `tailMap()`).
    - **Wymagania dotyczące kluczy:**
        - W `SortedMap` klucze muszą być wzajemnie porównywalne.

3. **Jakie są kluczowe metody interfejsu `Map`? Wymień i krótko opisz.**

    - `put(K key, V value)`: Wstawia parę klucz-wartość.
    - `get(Object key)`: Pobiera wartość na podstawie klucza.
    - `remove(Object key)`: Usuwa parę klucz-wartość.
    - `containsKey(Object key)`: Sprawdza obecność klucza.
    - `containsValue(Object value)`: Sprawdza obecność wartości.
    - `keySet()`: Zwraca zestaw kluczy.
    - `values()`: Zwraca kolekcję wartości.
    - `entrySet()`: Zwraca zestaw wpisów (klucz-wartość).

4. **Co to jest naturalny porządek kluczy i jak jest określany?**

   Naturalny porządek to domyślny sposób sortowania obiektów, określany przez implementację interfejsu `Comparable` i metodę `compareTo()`. Klasy implementujące `Comparable` definiują, jak ich obiekty są porównywane i sortowane.

5. **W jakich scenariuszach warto użyć `SortedMap` zamiast zwykłego `Map`?**

    - Gdy potrzebne jest utrzymanie kluczy w określonym porządku.
    - Przy iteracji po mapie w uporządkowany sposób.
    - Podczas operacji na zakresach kluczy.
    - W aplikacjach wymagających danych posortowanych, takich jak kalendarze czy rankingi.

6. **Jaka jest rola interfejsu `Map.Entry` i jak go używać?**

   `Map.Entry` reprezentuje pojedynczą parę klucz-wartość w mapie. Używany jest głównie podczas iteracji po mapie za pomocą metody `entrySet()`, umożliwiając dostęp do kluczy i wartości oraz ich modyfikację.

7. **Jakie są domyślne metody w interfejsie `Map` i do czego służą?**

   Domyślne metody, wprowadzone w Java 8, takie jak `getOrDefault()`, `forEach()`, `replaceAll()`, `putIfAbsent()`, `compute()`, `merge()`, ułatwiają operacje na mapie, umożliwiając np. bezpieczne modyfikacje, obliczenia wartości czy łączenie wpisów.

### Pytania zaawansowane

1. **Jakie są implikacje użycia `TreeMap` jako implementacji `SortedMap` pod względem wydajności operacji?**

    - **Złożoność operacji:** Operacje `put`, `get`, `remove` mają złożoność O(log n).
    - **Koszt utrzymania porządku:** Dodatkowy narzut wydajnościowy związany z utrzymaniem struktury drzewa.
    - **Korzyści:** Umożliwia efektywne operacje na uporządkowanych kluczach i zakresach.

2. **W jaki sposób `SortedMap` zapewnia uporządkowanie kluczy i jakie są tego konsekwencje dla porównywania kluczy?**

    - **Uporządkowanie:** Klucze są sortowane według naturalnego porządku lub dostarczonego komparatora.
    - **Porównywanie kluczy:** Klucze muszą być wzajemnie porównywalne; jeśli nie są, zostanie rzucony `ClassCastException`.
    - **Konsekwencje:**
        - Wszystkie klucze muszą być tego samego typu lub implementować `Comparable`.
        - Należy zapewnić spójność metod `equals()`, `hashCode()` i `compareTo()`.

3. **Jakie są potencjalne problemy z używaniem mutowalnych obiektów jako kluczy w mapach i jak ich unikać?**

    - **Problemy:**
        - Zmiana stanu klucza po dodaniu do mapy może uniemożliwić jego odnalezienie.
        - Może prowadzić do nieprzewidywalnego zachowania mapy.
    - **Unikanie:**
        - Używaj niemutowalnych obiektów jako kluczy (np. `String`, `Integer`).
        - Nie modyfikuj kluczy po dodaniu do mapy.

4. **Jak wpływają kolizje hashów na wydajność `HashMap` i jakie mechanizmy są stosowane do ich obsługi?**

    - **Wpływ na wydajność:**
        - Kolizje mogą powodować degradację złożoności operacji z O(1) do O(n) (przed Java 8) lub O(log n) (od Java 8).
    - **Mechanizmy obsługi kolizji:**
        - **Listy powiązane:** Wcześniej kolizje były przechowywane w listach, co zwiększało złożoność.
        - **Drzewa czerwono-czarne:** Od Java 8, po przekroczeniu pewnego progu, struktura zmienia się na drzewo, redukując złożoność do O(log n).

5. **Wyjaśnij różnicę między metodami `subMap()`, `headMap()` i `tailMap()` w interfejsie `SortedMap` i podaj przykłady ich zastosowania.**

    - **`subMap(K fromKey, K toKey)`:**
        - Zwraca widok mapy od `fromKey` (włącznie) do `toKey` (wyłącznie).
        - **Przykład:** Pobranie wpisów od "A" do "M".
    - **`headMap(K toKey)`:**
        - Zwraca widok mapy z kluczami mniejszymi niż `toKey`.
        - **Przykład:** Wszystkie klucze mniejsze niż "G".
    - **`tailMap(K fromKey)`:**
        - Zwraca widok mapy z kluczami większymi lub równymi `fromKey`.
        - **Przykład:** Wszystkie klucze od "K" w górę.

6. **Dlaczego `TreeMap` nie pozwala na `null` jako klucz i jakie są tego konsekwencje?**

    - **Powód:** `TreeMap` używa metod porównujących klucze (`compareTo()` lub `compare()`), które nie są zdefiniowane dla `null`.
    - **Konsekwencje:** Próba użycia `null` jako klucza w `TreeMap` spowoduje `NullPointerException`. Należy unikać `null` jako klucza lub użyć `HashMap` jeśli to konieczne.

7. **Jak bezpiecznie używać implementacji `Map` w środowiskach wielowątkowych? Wymień i opisz dostępne strategie.**

    - **Synchronizacja:**
        - Użycie `Collections.synchronizedMap()` do stworzenia synchronizowanej mapy.
        - Należy pamiętać o synchronizacji podczas iteracji.
    - **Kolekcje z `java.util.concurrent`:**
        - Użycie `ConcurrentHashMap`, która jest zaprojektowana do użytku w środowiskach wielowątkowych.
        - Zapewnia wysoką wydajność i bezpieczeństwo wątkowe bez konieczności ręcznej synchronizacji.
    - **Blokady:**
        - Stosowanie mechanizmów blokujących, takich jak `ReentrantLock`, do kontrolowania dostępu.
