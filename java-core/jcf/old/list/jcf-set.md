# Java Collections Framework: Interfejs `Set`

## Spis treści
1. [Wprowadzenie](#wprowadzenie)
2. [Ogólny opis interfejsu `Set`](#ogólny-opis-interfejsu-set)
3. [Główne cechy i właściwości](#główne-cechy-i-właściwości)
4. [Główne metody interfejsu `Set`](#główne-metody-interfejsu-set)
5. [Porównanie z list](#porównanie-z-list)
    - [`Set` vs `List`](#set-vs-list)
6. [Przykłady użycia](#przykłady-użycia)
7. [Najlepsze praktyki](#najlepsze-praktyki)
8. [Pułapki i często popełniane błędy](#pułapki-i-często-popełniane-błędy)
9. [Pytania rekrutacyjne](#pytania-rekrutacyjne)
    - [Pytania podstawowe](#pytania-podstawowe)
    - [Pytania zaawansowane](#pytania-zaawansowane)

---

## Wprowadzenie

Interfejs `Set` jest częścią **Java Collections Framework** i reprezentuje zbiór unikalnych elementów. W przeciwieństwie do `List`, `Set` nie pozwala na przechowywanie duplikatów, co czyni go idealnym wyborem w sytuacjach, gdy unikalność elementów jest kluczowa.

---

## Ogólny opis interfejsu `Set`

- **Pakiet:** `java.util`
- **Dziedziczenie:**

  ```
  java.lang.Object
     ↳ java.util.Collection<E>
         ↳ java.util.Set<E>
  ```

- **Interfejsy pokrewne:**
    - `SortedSet<E>`: Rozszerza `Set`, zapewniając posortowany zbiór elementów.
    - `NavigableSet<E>`: Rozszerza `SortedSet`, dodając metody nawigacji po zbiorze.

**Główne cechy:**

- **Unikalność elementów:** `Set` nie pozwala na przechowywanie duplikatów.
- **Brak gwarancji kolejności:** W zależności od implementacji, `Set` może nie zachowywać kolejności elementów.
- **Operacje matematyczne na zbiorach:** Ułatwia implementację operacji takich jak suma, przecięcie, różnica.

---

## Główne cechy i właściwości

1. **Brak duplikatów:**

    - Gdy próbujesz dodać element, który już istnieje w `Set`, operacja dodawania zostanie zignorowana lub nadpisana (w zależności od implementacji), ale nie spowoduje błędu.

2. **Kolejność elementów:**

    - **`HashSet`:** Nie gwarantuje żadnej kolejności.
    - **`LinkedHashSet`:** Zachowuje kolejność wstawiania elementów.
    - **`TreeSet`:** Przechowuje elementy w naturalnym porządku lub zgodnie z dostarczonym komparatorem.

3. **Implementacje:**

    - Interfejs `Set` jest implementowany przez kilka klas w JCF, takich jak `HashSet`, `LinkedHashSet`, `TreeSet`.

4. **Operacje na zbiorach:**

    - `Set` umożliwia łatwe wykonywanie operacji matematycznych na zbiorach, takich jak:

        - **Suma:** Dodawanie wszystkich elementów z jednego zbioru do drugiego.
        - **Przecięcie:** Zachowanie tylko tych elementów, które są wspólne dla obu zbiorów.
        - **Różnica:** Usunięcie ze zbioru wszystkich elementów, które występują w innym zbiorze.

---

## Główne metody interfejsu `Set`

Interfejs `Set` dziedziczy wszystkie metody z interfejsu `Collection`, ale nie dodaje żadnych nowych metod. Ważne jest jednak zrozumienie, jak te metody zachowują się w kontekście `Set`.

1. **`boolean add(E e)`**

    - Dodaje element `e` do zbioru.
    - Zwraca `true` tylko wtedy, gdy element nie był wcześniej obecny w zbiorze.

2. **`boolean remove(Object o)`**

    - Usuwa element `o` ze zbioru.
    - Zwraca `true`, jeśli element był obecny i został usunięty.

3. **`boolean contains(Object o)`**

    - Sprawdza, czy element `o` jest obecny w zbiorze.
    - Zwraca `true`, jeśli element jest obecny.

4. **`int size()`**

    - Zwraca liczbę elementów w zbiorze.

5. **`void clear()`**

    - Usuwa wszystkie elementy ze zbioru.

6. **`Iterator<E> iterator()`**

    - Zwraca iterator do przeglądania elementów w zbiorze.

**Operacje zbiorowe:**

- **`boolean addAll(Collection<? extends E> c)`**

    - Dodaje wszystkie elementy z kolekcji `c` do zbioru.
    - Ignoruje duplikaty.

- **`boolean retainAll(Collection<?> c)`**

    - Zachowuje w zbiorze tylko te elementy, które są obecne w kolekcji `c`.

- **`boolean removeAll(Collection<?> c)`**

    - Usuwa ze zbioru wszystkie elementy, które są obecne w kolekcji `c`.

---

## Porównanie z `List`

### `Set` vs `List`

|                | **`Set`**                               | **`List`**                                  |
|----------------|-----------------------------------------|---------------------------------------------|
| **Duplikaty**  | Nie pozwala na duplikaty                | Pozwala na duplikaty                        |
| **Kolejność**  | Zależy od implementacji, często brak    | Zachowuje kolejność wstawiania              |
| **Dostęp**     | Brak dostępu przez indeks               | Dostęp przez indeks (`get(int index)`)      |
| **Zastosowania** | Gdy ważna jest unikalność elementów     | Gdy ważna jest kolejność i dostęp przez indeks |

---

## Przykłady użycia

### Przykład 1: Podstawowe operacje na `Set`

```java
Set<String> fruits = new HashSet<>();

// Dodawanie elementów
fruits.add("Apple");
fruits.add("Banana");
fruits.add("Orange");
fruits.add("Apple"); // Ignorowane, już istnieje

// Rozmiar zbioru
System.out.println("Size: " + fruits.size()); // Size: 3

// Iteracja po elementach
for (String fruit : fruits) {
    System.out.println(fruit);
}

// Sprawdzanie obecności elementu
if (fruits.contains("Banana")) {
    System.out.println("We have bananas!");
}

// Usuwanie elementu
fruits.remove("Orange");
```

### Przykład 2: Operacje na zbiorach

```java
Set<Integer> setA = new HashSet<>(Arrays.asList(1, 2, 3, 4));
Set<Integer> setB = new HashSet<>(Arrays.asList(3, 4, 5, 6));

// Suma zbiorów (unii)
Set<Integer> union = new HashSet<>(setA);
union.addAll(setB); // [1, 2, 3, 4, 5, 6]

// Przecięcie zbiorów (intersection)
Set<Integer> intersection = new HashSet<>(setA);
intersection.retainAll(setB); // [3, 4]

// Różnica zbiorów (difference)
Set<Integer> difference = new HashSet<>(setA);
difference.removeAll(setB); // [1, 2]
```

---

## Najlepsze praktyki

- **Wybór odpowiedniej implementacji:**

    - **`HashSet`:** Gdy nie zależy Ci na kolejności elementów i potrzebujesz szybkich operacji dodawania, usuwania i sprawdzania obecności (O(1)).
    - **`LinkedHashSet`:** Gdy chcesz zachować kolejność wstawiania elementów.
    - **`TreeSet`:** Gdy potrzebujesz przechowywać elementy w posortowanej kolejności.

- **Unikanie duplikatów:**

    - Używaj `Set` do automatycznego usuwania duplikatów z kolekcji.

- **Przekształcanie `List` na `Set`:**

    - Jeśli masz listę z potencjalnymi duplikatami i chcesz je usunąć:

      ```java
      List<String> namesWithDuplicates = Arrays.asList("Anna", "Bob", "Anna", "Charlie");
      Set<String> uniqueNames = new HashSet<>(namesWithDuplicates);
      ```

- **Implementacja metod `equals()` i `hashCode()`:**

    - Dla poprawnego działania `Set`, szczególnie `HashSet`, ważne jest, aby elementy miały poprawnie zaimplementowane metody `equals()` i `hashCode()`.

---

## Pułapki i często popełniane błędy

1. **Brak implementacji `equals()` i `hashCode()`:**

    - Jeśli przechowujesz obiekty własnych klas w `Set`, a nie zaimplementujesz metod `equals()` i `hashCode()`, możesz napotkać problemy z identyfikacją duplikatów.

2. **Zakładanie kolejności elementów:**

    - `HashSet` nie gwarantuje żadnej kolejności. Jeśli potrzebujesz zachować kolejność, użyj `LinkedHashSet` lub `TreeSet`.

3. **Modyfikacja elementów w `Set`:**

    - Jeśli zmienisz stan obiektu w `Set` w taki sposób, że wpływa to na wynik metody `hashCode()`, może to spowodować nieprzewidywalne zachowanie.

4. **Próba dodania elementu `null` do `TreeSet`:**

    - `TreeSet` nie pozwala na przechowywanie wartości `null`, ponieważ nie można porównać `null` z innymi elementami.

---

## Pytania rekrutacyjne

### Pytania podstawowe

1. **Co to jest interfejs `Set` i jakie są jego główne cechy?**
    - `Set` to interfejs w Java Collections Framework reprezentujący zbiór unikalnych elementów. Główne cechy to brak duplikatów i, w zależności od implementacji, brak gwarancji kolejności elementów.

2. **Jakie są główne implementacje interfejsu `Set` w Javie?**
    - `HashSet`, `LinkedHashSet`, `TreeSet`.

3. **Czy `Set` pozwala na przechowywanie duplikatów?**
    - Nie, `Set` nie pozwala na duplikaty. Jeśli próbujesz dodać istniejący element, operacja dodawania jest ignorowana.

4. **Jakie metody są dostępne w interfejsie `Set`?**
    - `Set` dziedziczy metody z interfejsu `Collection`, takie jak `add()`, `remove()`, `contains()`, `size()`, `iterator()`, ale nie dodaje żadnych nowych metod.

5. **Jakie są różnice między `Set` a `List`?**
    - `Set` nie pozwala na duplikaty i zazwyczaj nie gwarantuje kolejności elementów, podczas gdy `List` pozwala na duplikaty i zachowuje kolejność wstawiania elementów.

### Pytania zaawansowane

1. **Jak działa mechanizm identyfikacji duplikatów w `HashSet`?**
    - `HashSet` używa metod `hashCode()` i `equals()` do sprawdzania, czy element już istnieje w zbiorze. Jeśli dwa obiekty mają ten sam `hashCode()` i są równe według metody `equals()`, uznawane są za duplikaty.

2. **Co się stanie, jeśli zmienisz stan obiektu będącego elementem `Set` w taki sposób, że zmieni się jego `hashCode()`?**
    - Może to spowodować, że obiekt stanie się niedostępny w zbiorze, ponieważ `HashSet` nie będzie w stanie prawidłowo zlokalizować go w wewnętrznej strukturze danych.

3. **Jakie są różnice między `HashSet`, `LinkedHashSet` i `TreeSet`?**
    - **`HashSet`:** Nie gwarantuje kolejności elementów, szybkie operacje (O(1)).
    - **`LinkedHashSet`:** Zachowuje kolejność wstawiania elementów.
    - **`TreeSet`:** Przechowuje elementy w posortowanej kolejności, operacje są wolniejsze (O(log n)).

4. **Czy można przechowywać `null` w implementacjach `Set`?**
    - **`HashSet` i `LinkedHashSet`:** Pozwalają na przechowywanie jednego `null`.
    - **`TreeSet`:** Nie pozwala na przechowywanie `null`, ponieważ wymaga porównywania elementów.

5. **Jak zaimplementować operacje matematyczne na zbiorach w Javie?**
    - Używając metod takich jak `addAll()` (suma), `retainAll()` (przecięcie), `removeAll()` (różnica).
