# Java Collections Framework: Implementacje interfejsu `Set`

## Spis treści

1. [Wprowadzenie](#wprowadzenie)
2. [Implementacje interfejsu `Set`](#implementacje-interfejsu-set)
    - [HashSet](#hashset)
    - [LinkedHashSet](#linkedhashset)
    - [TreeSet](#treeset)
    - [EnumSet](#enumset)
3. [Porównanie implementacji](#porównanie-implementacji)
4. [Najlepsze praktyki](#najlepsze-praktyki)
5. [Przykłady kodu](#przykłady-kodu)
6. [Pułapki i często popełniane błędy](#pułapki-i-często-popełniane-błędy)
7. [Pytania rekrutacyjne](#pytania-rekrutacyjne)
    - [Pytania podstawowe](#pytania-podstawowe)
    - [Pytania zaawansowane](#pytania-zaawansowane)

---

## Wprowadzenie

Interfejs `Set` w **Java Collections Framework** reprezentuje zbiór unikalnych elementów. Istnieje kilka implementacji tego interfejsu, z których każda ma swoje unikalne cechy i zastosowania. W tym dokumencie omówimy główne implementacje `Set`, takie jak `HashSet`, `LinkedHashSet`, `TreeSet` i `EnumSet`.

---

## Implementacje interfejsu `Set`

### HashSet

#### Opis

- **Pakiet:** `java.util`
- `HashSet` jest najczęściej używaną implementacją interfejsu `Set`.
- Bazuje na tablicy hashującej (`HashMap` wewnętrznie).

**Główne cechy:**

- **Unikalność elementów:** Zapewnia przechowywanie unikalnych elementów.
- **Brak gwarancji kolejności:** Nie zachowuje kolejności wstawiania ani żadnej innej kolejności.
- **Szybkie operacje:** Większość operacji, takich jak `add()`, `remove()`, `contains()`, działa w czasie O(1).

#### Struktura wewnętrzna

- Wewnętrznie `HashSet` używa instancji `HashMap` do przechowywania elementów.
- Każdy element dodawany do `HashSet` jest przechowywany jako klucz w wewnętrznym `HashMap`, a wartością jest stały obiekt `PRESENT`. <br>
W `HashMap` wartość nie jest potrzebna, ponieważ wszystkie operacje (`add()`, `contains()`, `remove()`) bazują wyłącznie na kluczach. <br>
 Użycie stałej `PRESENT` upraszcza implementację `HashSet`, pozwalając na maksymalne wykorzystanie mechanizmów `HashMap`.
- **Hashing:**
    - Przy dodawaniu elementu `HashSet` oblicza wartość hash (`hashCode()`) elementu, aby określić, gdzie w tablicy hashującej umieścić element.
    - Elementy z tym samym wynikiem funkcji `hashCode()` są umieszczane w tej samej "kubełkowej" liście (bucket).
- **Krytyczne znaczenie metod `hashCode()` i `equals()`:**
    - Poprawna implementacja tych metod w przechowywanych obiektach jest kluczowa dla poprawnego działania `HashSet`.

#### Wydajność

| Operacja             | Złożoność czasowa bez kolizji | Złożoność czasowa z kolizjami |
|----------------------|-------------------------------|-------------------------------|
| `add(E e)`           | O(1)                          | O(n)                          |
| `remove(Object o)`   | O(1)                          | O(n)                          |
| `contains(Object o)` | O(1)                          | O(n)                          |
| `size()`             | O(1)                          | O(1)                          |

**Uwagi:**

- W najgorszym przypadku (dużo kolizji hashów) złożoność może wzrosnąć do O(n), gdzie n to liczba elementów w danym kubełku.
- Wydajność zależy od jakości implementacji `hashCode()` i równomiernego rozkładu elementów.

**Zastosowania:**

- Gdy potrzebujesz szybkiego dostępu i nie zależy Ci na kolejności elementów.
- Idealny do przechowywania dużych zbiorów danych, gdzie operacje wyszukiwania są częste.
- Usuwanie duplikatów z kolekcji.

---

### LinkedHashSet

#### Opis

- **Pakiet:** `java.util`
- Rozszerza `HashSet` i implementuje interfejs `Set`.
- Zachowuje kolejność wstawiania elementów.

**Główne cechy:**

- **Unikalność elementów:** Tak jak `HashSet`, nie pozwala na duplikaty.
- **Zachowanie kolejności:** Elementy są przechowywane w kolejności ich wstawiania.
- **Szybkie operacje:** Operacje takie jak `add()`, `remove()`, `contains()` działają w czasie O(1), choć nieco wolniej niż w `HashSet` z powodu dodatkowego narzutu na utrzymanie kolejności.

#### Struktura wewnętrzna

- Wewnętrznie `LinkedHashSet` używa `LinkedHashMap` do przechowywania elementów.
- `LinkedHashMap` jest rozszerzeniem `HashMap`, które dodatkowo utrzymuje podwójnie powiązaną listę elementów, aby zachować kolejność wstawiania.
- Każdy węzeł w mapie ma odniesienia do poprzedniego i następnego elementu.

#### Wydajność

| Operacja             | Złożoność czasowa bez kolizji | Złożoność czasowa z kolizjami |
|----------------------|-------------------------------|-------------------------------|
| `add(E e)`           | O(1)                          | O(n)                          |
| `remove(Object o)`   | O(1)                          | O(n)                          |
| `contains(Object o)` | O(1)                          | O(n)                          |
| `size()`             | O(1)                          | O(1)                          |

**Uwagi:**

- Nieco wolniejszy od `HashSet` z powodu dodatkowej struktury danych do utrzymania kolejności.
- Wydajność zależy od rozkładu `hashCode()` oraz od liczby elementów w kolekcji.

**Zastosowania:**

- Gdy potrzebujesz zachować kolejność wstawiania elementów.
- Przydatny w sytuacjach, gdzie kolejność elementów ma znaczenie, np. do tworzenia listy unikalnych elementów w kolejności ich pojawienia się.

---

### TreeSet

#### Opis

- **Pakiet:** `java.util`
- Implementuje interfejsy `NavigableSet`, `SortedSet` i `Set`.
- Przechowuje elementy w posortowanej kolejności.

**Główne cechy:**

- **Unikalność elementów:** Nie pozwala na duplikaty.
- **Posortowana kolejność:** Elementy są przechowywane w naturalnym porządku lub zgodnie z dostarczonym komparatorem.
- **Operacje wydajne dla posortowanych danych:** Dostarcza metody takie jak `first()`, `last()`, `headSet()`, `tailSet()`.

#### Struktura wewnętrzna

- Wewnętrznie `TreeSet` używa `TreeMap` do przechowywania elementów.
- `TreeMap` jest implementacją **drzewa czerwono-czarnego**, które jest samobalansującą się strukturą danych.
- Drzewo czerwono-czarne gwarantuje, że wysokość drzewa jest O(log n), co zapewnia wydajne operacje wyszukiwania, dodawania i usuwania.

#### Wydajność

| Operacja             | Złożoność czasowa |
|----------------------|-------------------|
| `add(E e)`           | O(log n)          |
| `remove(Object o)`   | O(log n)          |
| `contains(Object o)` | O(log n)          |
| `size()`             | O(1)              |

**Uwagi:**

- Wolniejszy niż `HashSet` i `LinkedHashSet` dla operacji podstawowych, ale oferuje dodatkowe operacje na posortowanych danych.
- Idealny dla aplikacji wymagających operacji na posortowanych zbiorach.
- W `TreeSet` nie dochodzi do kolizji hashów, ponieważ implementacja nie używa mechanizmu hashowania do przechowywania elementów.

**Zastosowania:**

- Gdy potrzebujesz przechowywać elementy w posortowanej kolejności.
- Idealny

do implementacji zbiorów posortowanych, np. w algorytmach wymagających szybkiego dostępu do minimalnego lub maksymalnego elementu.
- Przydatny, gdy potrzebujesz operacji zakresowych, takich jak podzbiory (`subSet()`).

---

### EnumSet

#### Opis

- **Pakiet:** `java.util`
- Specjalizowana implementacja `Set` dla typów wyliczeniowych (`enum`).
- Nie może być instancjonowany bezpośrednio; używa się metod fabrykujących.

**Główne cechy:**

- **Wysoce wydajna:** Bardzo szybkie operacje, ponieważ jest wewnętrznie reprezentowany jako bitowa mapa.
- **Ograniczony do typów `enum`:** Może przechowywać tylko wartości jednego typu wyliczeniowego.
- **Nie pozwala na `null`:** Próba dodania `null` spowoduje `NullPointerException`.

#### Struktura wewnętrzna

- Wewnętrznie reprezentowany jako tablica bitów (bitset), gdzie każdy bit odpowiada jednej stałej `enum`.
- Ponieważ liczba stałych w `enum` jest znana i zazwyczaj niewielka, `EnumSet` może efektywnie przechowywać elementy i wykonywać operacje bitowe.

#### Wydajność

| Operacja             | Złożoność czasowa | 
|----------------------|-------------------|
| `add(E e)`           | O(1)              |
| `remove(Object o)`   | O(1)              |
| `contains(Object o)` | O(1)              |
| `size()`             | O(1)              |

**Uwagi:**

- Operacje są niezwykle szybkie i efektywne pamięciowo.
- Wydajność jest lepsza niż w przypadku innych implementacji `Set` ze względu na specjalizację dla `enum`.
- W `EnumSet` nie dochodzi do kolizji hashów, ponieważ implementacja nie używa mechanizmu hashowania do przechowywania elementów.

**Zastosowania:**

- Gdy potrzebujesz wydajnego zbioru dla typów wyliczeniowych.
- Idealny do przechowywania zbiorów flag lub stanów.
- Przydatny w sytuacjach, gdy operacje na zbiorach typów `enum` są częste i wymagają wysokiej wydajności.

---

## Porównanie implementacji

| **Cecha**                          | **HashSet**              | **LinkedHashSet**                | **TreeSet**                        | **EnumSet**            |
|------------------------------------|--------------------------|----------------------------------|------------------------------------|------------------------|
| **Unikalność**                     | Tak                      | Tak                              | Tak                                | Tak                    |
| **Kolejność**                      | Brak                     | Wstawiania                       | Posortowana                        | Wstawiania (enum)      |
| **Pozwala na `null`**              | Tak (jeden `null`)       | Tak (jeden `null`)               | Nie                                | Nie                    |
| **Struktura wewnętrzna**           | `HashMap`                | `LinkedHashMap`                  | `TreeMap` (drzewo czerwono-czarne) | Tablica bitów (bitset) |
| **Zastosowania**                   | Ogólne, szybkie operacje | Zachowanie kolejności wstawiania | Posortowane zbiory                 | Zbiory `enum`          |

---

## Najlepsze praktyki

- **Wybór odpowiedniej implementacji:**

    - **`HashSet`:** Używaj, gdy potrzebujesz szybkich operacji i nie zależy Ci na kolejności elementów.
    - **`LinkedHashSet`:** Wybierz, gdy chcesz zachować kolejność wstawiania.
    - **`TreeSet`:** Użyj, gdy potrzebujesz przechowywać elementy w posortowanej kolejności.
    - **`EnumSet`:** Idealny dla zbiorów typu `enum`.

- **Implementacja `equals()` i `hashCode()`:**

    - Dla poprawnego działania `HashSet` i `LinkedHashSet`, upewnij się, że elementy mają poprawnie zaimplementowane metody `equals()` i `hashCode()`.

- **Unikanie `null` w `TreeSet` i `EnumSet`:**

    - Te implementacje nie pozwalają na przechowywanie `null`. Unikaj dodawania `null` do tych zbiorów.

- **Świadomość wydajności:**

    - Pamiętaj, że `TreeSet` ma wolniejsze operacje podstawowe niż `HashSet` z powodu utrzymania posortowanej struktury.

---

## Przykłady kodu

### Przykład 1: Użycie `HashSet`

```java
Set<String> hashSet = new HashSet<>();
hashSet.add("Apple");
hashSet.add("Banana");
hashSet.add("Orange");
hashSet.add("Apple"); // Ignorowane, już istnieje

System.out.println(hashSet); // Kolejność nie jest gwarantowana
```

**Przykładowy wynik:**

```
[Banana, Orange, Apple]
```

### Przykład 2: Użycie `LinkedHashSet`

```java
Set<String> linkedHashSet = new LinkedHashSet<>();
linkedHashSet.add("Apple");
linkedHashSet.add("Orange");
linkedHashSet.add("Banana");

System.out.println(linkedHashSet); // Zachowuje kolejność wstawiania
```

**Przykładowy wynik:**

```
[Apple, Orange, Banana]
```

### Przykład 3: Użycie `TreeSet`

```java
Set<String> treeSet = new TreeSet<>();
treeSet.add("Banana");
treeSet.add("Apple");
treeSet.add("Orange");

System.out.println(treeSet); // Elementy w kolejności alfabetycznej
```

**Przykładowy wynik:**

```
[Apple, Banana, Orange]
```

### Przykład 4: Użycie `EnumSet`

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

Set<Day> weekend = EnumSet.of(Day.SATURDAY, Day.SUNDAY);
System.out.println(weekend);
```

**Przykładowy wynik:**

```
[SATURDAY, SUNDAY]
```

---

## Pułapki i często popełniane błędy

1. **Brak implementacji `equals()` i `hashCode()`:**

    - Jeśli przechowujesz obiekty własnych klas w `HashSet` lub `LinkedHashSet` i nie zaimplementujesz `equals()` oraz `hashCode()`, mogą pojawić się problemy z identyfikacją duplikatów.
    - **Rozwiązanie:** Zawsze nadpisuj `equals()` i `hashCode()` w swoich klasach, jeśli planujesz używać ich instancji w zbiorach.

2. **Dodawanie `null` do `TreeSet` lub `EnumSet`:**

    - Próba dodania `null` spowoduje `NullPointerException`, ponieważ te implementacje wymagają porównywania elementów.
    - **Rozwiązanie:** Unikaj dodawania `null` do tych zbiorów.

3. **Zakładanie kolejności w `HashSet`:**

    - `HashSet` nie gwarantuje żadnej kolejności. Jeśli kolejność jest istotna, użyj `LinkedHashSet` lub `TreeSet`.
    - **Rozwiązanie:** Wybierz odpowiednią implementację `Set` w zależności od potrzeb dotyczących kolejności elementów.

4. **Zmiana stanu obiektów w `Set`:**

    - Jeśli zmienisz stan obiektu przechowywanego w `HashSet` lub `LinkedHashSet` w taki sposób, że zmienia to wynik `hashCode()` lub `equals()`, może to spowodować nieprzewidywalne zachowanie.
    - **Rozwiązanie:** Unikaj modyfikowania obiektów po dodaniu ich do zbioru lub upewnij się, że zmiany nie wpływają na `equals()` i `hashCode()`.

---

## Pytania rekrutacyjne

### Pytania podstawowe

1. **Czym jest `HashSet` i jakie są jego główne cechy?**

    - `HashSet` to implementacja interfejsu `Set` oparta na tablicy hashującej (`HashMap` wewnętrznie). Główne cechy to brak duplikatów, brak gwarancji kolejności elementów i szybkie operacje dodawania, usuwania oraz sprawdzania obecności (średnio O(1)).

2. **Jak `LinkedHashSet` różni się od `HashSet`?**

   - `LinkedHashSet` zachowuje kolejność wstawiania elementów, podczas gdy `HashSet` nie gwarantuje żadnej kolejności. Wewnętrznie `LinkedHashSet` używa `LinkedHashMap`, która utrzymuje podwójnie powiązaną listę elementów.

3. **Co to jest `TreeSet` i kiedy powinno się go używać?**

    - `TreeSet` to implementacja `Set`, która przechowuje elementy w posortowanej kolejności (naturalnej lub według dostarczonego komparatora). Powinno się go używać, gdy potrzebujesz posortowanego zbioru elementów i operacji takich jak wyszukiwanie zakresowe.

4. **Czy `HashSet` pozwala na przechowywanie wartości `null`?**

    - Tak, `HashSet` pozwala na przechowywanie jednej wartości `null`.

5. **Jakie są zalety użycia `EnumSet`?**

    - `EnumSet` jest wysoce wydajną implementacją `Set` dla typów wyliczeniowych. Jest bardzo szybki, zużywa mało pamięci i oferuje operacje w czasie stałym lub bliskim stałemu.

### Pytania zaawansowane

1. **Jak działa `HashSet` wewnętrznie i jak wpływa to na wydajność?**

    - `HashSet` używa wewnętrznie `HashMap` do przechowywania elementów jako kluczy z wartością dummy. Operacje są szybkie (O(1)), ale wydajność zależy od dobrej funkcji `hashCode()` i równomiernego rozkładu elementów w kubełkach.

2. **Jak `TreeSet` określa kolejność elementów i co się stanie, jeśli dodasz elementy nieporównywalne?**

    - `TreeSet` określa kolejność elementów na podstawie ich naturalnego porządku (implementacja `Comparable`) lub dostarczonego komparatora. Jeśli dodasz elementy, które nie są porównywalne i nie dostarczysz komparatora, zostanie rzucony `ClassCastException`.

3. **Dlaczego `EnumSet` jest bardziej wydajny niż inne implementacje `Set` dla typów wyliczeniowych?**

    - `EnumSet` jest specjalnie zoptymalizowany dla typów wyliczeniowych. Wewnętrznie reprezentuje zbiór jako bitową mapę, gdzie każdy bit odpowiada jednej stałej `enum`, co czyni operacje niezwykle szybkie i efektywne pamięciowo.

4. **Co się stanie, jeśli zmienisz stan obiektu będącego elementem `HashSet` w taki sposób, że zmieni się jego `hashCode()`?**

    - Może to spowodować, że obiekt stanie się niedostępny w zbiorze, ponieważ `HashSet` nie będzie w stanie prawidłowo zlokalizować go w wewnętrznej strukturze danych. Może to prowadzić do nieprzewidywalnego zachowania, w tym do duplikatów lub problemów z usuwaniem elementów.

5. **Jakie są różnice między `HashSet`, `LinkedHashSet` a `TreeSet` pod względem wydajności operacji?**

    - **`HashSet`:** Szybkie operacje (O(1)) dla `add()`, `remove()`, `contains()`, brak gwarancji kolejności.
    - **`LinkedHashSet`:** Podobna wydajność do `HashSet`, z dodatkowym narzutem na utrzymanie kolejności wstawiania.
    - **`TreeSet`:** Wolniejsze operacje (O(log n)) ze względu na utrzymanie posortowanej struktury drzewa czerwono-czarnego, ale oferuje dodatkowe funkcjonalności związane z sortowaniem.

---
