### **1. Wprowadzenie do klasy ArrayList**

`ArrayList` to jedna z najczęściej używanych klas w Java Collection Framework. Służy do przechowywania dynamicznych tablic, które mogą automatycznie zmieniać swój rozmiar. `ArrayList` została wprowadzona w **Java 2 (JDK 1.2)**.

#### **Hierarchia dziedziczenia:**

```plaintext
java.lang.Object
  ↳ java.util.AbstractCollection<E>
       ↳ java.util.AbstractList<E>
            ↳ java.util.ArrayList<E>
```

- **`AbstractList`**: Klasa bazowa, która dostarcza częściową implementację interfejsu `List`. Pozwala zaimplementować listy poprzez nadpisywanie tylko kluczowych metod, takich jak `get(int index)` i `size()`.

- **`RandomAccess`**: Znacznikowy interfejs (marker interface), który wskazuje, że implementacja listy obsługuje szybki, stały czas dostępu (`O(1)`) do elementów na podstawie indeksu. `ArrayList` implementuje ten interfejs, w odróżnieniu od np. `LinkedList`.

---

### **2. Kluczowe cechy `ArrayList`**

- **Dynamiczna tablica**: Rozmiar `ArrayList` jest automatycznie zwiększany, gdy brakuje miejsca na nowe elementy.
- **Losowy dostęp (`RandomAccess`)**: Umożliwia szybki dostęp do elementów za pomocą indeksów (`O(1)`).
- **Obsługa `null`**: `ArrayList` pozwala na przechowywanie wartości `null` jako elementów.
- **Niesynchronizowana**: `ArrayList` nie jest bezpieczna w środowisku wielowątkowym, ale może być używana z mechanizmem synchronizacji (`Collections.synchronizedList`).

---

### **3. Metody w `ArrayList`**

Metody `ArrayList` można podzielić na trzy kategorie w zależności od tego, z jakiego interfejsu pochodzą lub jakie zostały dodane w samej klasie `ArrayList`.

#### **Metody pochodzące z `Collection`**
- **`add(E e)`**: Dodaje element na końcu listy.
- **`remove(Object o)`**: Usuwa pierwsze wystąpienie podanego obiektu.
- **`contains(Object o)`**: Sprawdza, czy lista zawiera podany element.
- **`isEmpty()`**: Sprawdza, czy lista jest pusta.
- **`size()`**: Zwraca liczbę elementów w liście.
- **`clear()`**: Usuwa wszystkie elementy z listy.

#### **Metody pochodzące z `List`**
- **`add(int index, E element)`**: Dodaje element na określonym indeksie.
- **`get(int index)`**: Zwraca element na podanym indeksie.
- **`set(int index, E element)`**: Zastępuje element na podanym indeksie nową wartością.
- **`remove(int index)`**: Usuwa element na określonym indeksie.
- **`indexOf(Object o)`**: Zwraca indeks pierwszego wystąpienia podanego elementu.
- **`lastIndexOf(Object o)`**: Zwraca indeks ostatniego wystąpienia elementu.
- **`subList(int fromIndex, int toIndex)`**: Zwraca widok na fragment listy.

#### **Metody `ArrayList`**
- **`ensureCapacity(int minCapacity)`**: Ręcznie zwiększa pojemność wewnętrznej tablicy.
- **`trimToSize()`**: Zmniejsza pojemność tablicy wewnętrznej do bieżącej liczby elementów.

---

### 4. Struktura klasy

`ArrayList` opiera się na wewnętrznej tablicy obiektów, która dynamicznie się rozszerza.

**Pseudokod kluczowych metod**:

```java
public class ArrayList {
  Object[] elementData = new Object[10];
  int size;

  void add(Object e) {
    if (size == elementData.length)
      resize();
    elementData[size++] = e;
  }

  Object get(int index) {
    // Sprawdź zakres
    return elementData[index];
  }

  void resize() {
    int newCapacity = elementData.length * 2;
    Object[] newArray = new Object[newCapacity];
    // Kopiowanie elementów
    for (int i = 0; i < size; i++)
      newArray[i] = elementData[i];
    elementData = newArray;
  }
}
```

### **5. Mechanizm działania `ArrayList`**

#### **Dynamiczna zmiana rozmiaru**
`ArrayList` opiera się na dynamicznej tablicy. Gdy liczba elementów przekroczy bieżącą pojemność, tablica zostaje rozszerzona. Proces ten wygląda następująco:
1. Wewnętrzna tablica jest kopiowana do nowej tablicy o większym rozmiarze.
2. Rozmiar nowej tablicy jest zwiększany o 50% w stosunku do bieżącego rozmiaru.
3. Elementy są przepisywane do nowej tablicy.

#### **Złożoność operacji**
- **Dodawanie elementów (`add`)**:
    - Gdy jest wystarczająco dużo miejsca w tablicy: `O(1)`.
    - Gdy konieczne jest zwiększenie rozmiaru tablicy: `O(n)` (z powodu kopiowania elementów).

- **Dostęp przez indeks (`get`)**:
    - Stały czas dostępu: `O(1)`.

- **Usuwanie elementów (`remove`)**:
    - Operacja wymaga przesunięcia elementów w lewo, co ma złożoność `O(n)`.

---

#### **Ręczna kontrola pojemności**

Metody **`ensureCapacity`** i **`trimToSize`**, a także konstruktor z parametrem `capacity`, umożliwiają ręczną kontrolę pojemności wewnętrznej tablicy w `ArrayList`.

##### **Konstruktor z początkową pojemnością**
Konstruktor `ArrayList(int initialCapacity)` pozwala na utworzenie listy z początkową pojemnością określoną przez użytkownika.

- Jeśli z góry wiemy, że lista będzie przechowywać dużą liczbę elementów, użycie tego konstruktora może zminimalizować liczbę operacji rozszerzania tablicy.
- Domyślna pojemność wynosi 10, jeśli konstruktor bezargumentowy zostanie użyty.

**Przykład:**
```java
ArrayList<String> list = new ArrayList<>(100); // Tworzy listę z początkową pojemnością 100
list.add("A");
System.out.println(list.size()); // Rozmiar logiczny: 1
System.out.println(list.capacity()); // Rozmiar fizyczny (pojemność): 100 (możesz dodać 99 elementów bez rozszerzania)
```

---

##### **Metody kontrolujące pojemność**

1. **`ensureCapacity`**  
   Zwiększa pojemność wewnętrznej tablicy do co najmniej `minCapacity`. Przydatna, gdy przewidujemy dodanie wielu elementów naraz, co pozwala uniknąć wielokrotnego kopiowania tablicy.

   **Przykład:**
   ```java
   ArrayList<String> list = new ArrayList<>();
   list.ensureCapacity(50); // Rezerwuje miejsce na 50 elementów
   ```

2. **`trimToSize`**  
   Zmniejsza pojemność tablicy do bieżącej liczby elementów (`size`). Pozwala zaoszczędzić pamięć, jeśli lista nie będzie dalej powiększana.

   **Przykład:**
   ```java
   ArrayList<String> list = new ArrayList<>(100);
   list.add("A");
   list.trimToSize(); // Zmniejsza rozmiar tablicy do 1 (liczba elementów w liście)
   ```

---

### **6. Zalety i wady `ArrayList`**

#### **Zalety**
- **Elastyczność**: Automatyczna zmiana rozmiaru tablicy.
- **Wysoka wydajność dla odczytów**: Operacje `get` są bardzo szybkie.

#### **Wady**
- **Kosztowne operacje wstawiania i usuwania**: Operacje dodawania/usuwania w środku listy są kosztowne z powodu konieczności przesuwania elementów.
- **Brak synchronizacji**: `ArrayList` nie jest bezpieczna dla wątków bez dodatkowych mechanizmów synchronizacji.

---

### **7. Przykłady użycia**

#### **Dodawanie i usuwanie elementów**
```java
ArrayList<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add(1, "C"); // Dodaje "C" na indeksie 1
list.remove("A"); // Usuwa "A"
System.out.println(list); // [C, B]
```

#### **Iteracja za pomocą iteratora**
```java
ArrayList<String> list = new ArrayList<>(List.of("A", "B", "C"));

Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

#### **Ręczne zarządzanie pojemnością**
```java
ArrayList<String> list = new ArrayList<>(2);
list.add("A");
list.add("B");
list.ensureCapacity(10); // Rezerwuje miejsce na 10 elementów
list.trimToSize(); // Zmniejsza rozmiar tablicy do 2
```

---

## 8. Wydajność

| Operacja             | Bez rozszerzania | Z rozszerzaniem |
|----------------------|------------------|-----------------|
| Dodawanie na końcu   | O(1)             | O(n)            |
| Dodawanie w środku   | O(n)             | O(n)            |
| Usuwanie             | O(n)             | niedotyczy      |
| Dostęp przez indeks  | O(1)             | niedotyczy      |

**Wyjaśnienie**:

- **Dodawanie na końcu**: Szybkie, chyba że następuje rozszerzenie tablicy.
- **Dodawanie/usuwanie w środku**: Wymaga przesuwania elementów.
- **Dostęp przez indeks**: Bezpośredni dostęp do elementu.

---

### **9. Pytania i odpowiedzi**

#### **1. Co się stanie, gdy tablica `ArrayList` przekroczy swoją pojemność?**
Gdy liczba elementów przekroczy pojemność tablicy, `ArrayList` zwiększa jej rozmiar o 50%, a następnie kopiuje wszystkie elementy do nowej tablicy.

#### **2. Jakie operacje są kosztowne w `ArrayList`?**
Operacje dodawania/usuwania w środku listy są kosztowne (`O(n)`), ponieważ wymagają przesunięcia elementów.

#### **3. Jak synchronizować `ArrayList`?**
`ArrayList` można opakować w synchronizowaną wersję za pomocą `Collections.synchronizedList`.

**Przykład:**
```java
List<String> synchronizedList = Collections.synchronizedList(new ArrayList<>());
```

#### **4. Czy `ArrayList` obsługuje `null`?**
Tak, `ArrayList` pozwala na przechowywanie wartości `null`.

#### **5. Czym różni się `ArrayList` od `LinkedList`?**
`ArrayList` zapewnia szybki dostęp do elementów przez indeks, natomiast `LinkedList` jest bardziej wydajny przy dodawaniu/usuwaniu elementów na początku lub końcu listy.
