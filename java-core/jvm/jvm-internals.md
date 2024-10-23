### Wprowadzenie do wewnętrznych mechanizmów JVM (Java Virtual Machine)

Java Virtual Machine (JVM) jest kluczowym komponentem platformy Java, odpowiedzialnym za uruchamianie programów
napisanych w tym języku. JVM tłumaczy kod bajtowy (bytecode) na instrukcje natywne dla maszyny, zarządza pamięcią,
wielowątkowością i optymalizuje wykonywanie aplikacji.

---

### 1. Architektura JVM

JVM można podzielić na kilka kluczowych komponentów, które pełnią określone funkcje:

- **Class Loader**: Odpowiada za ładowanie klas Java do pamięci na żądanie.
- **Memory Areas (Obszary pamięci)**: JVM dzieli pamięć na różne sekcje, takie jak stos (stack), sterta (heap) i
  metaspace, które pełnią różne funkcje związane z przechowywaniem danych i zarządzaniem pamięcią.
- **Execution Engine (Silnik wykonawczy)**: Interpretuje lub kompiluje bytecode do natywnego kodu maszynowego.
- **Garbage Collector (GC)**: Zarządza pamięcią, automatycznie zwalniając zasoby zajmowane przez obiekty, które nie są
  już używane.

Każdy z tych komponentów odgrywa istotną rolę w działaniu aplikacji Java, umożliwiając uruchamianie programów na różnych
platformach sprzętowych bez konieczności rekompilacji kodu źródłowego.

---

### 2. Class Loader (Ładowanie klas)

**Class Loader** jest mechanizmem JVM odpowiedzialnym za ładowanie plików `.class` (które zawierają bytecode) do
pamięci. Class Loader jest niezwykle istotnym elementem, ponieważ umożliwia dynamiczne ładowanie klas na żądanie, co
jest kluczowe w nowoczesnych aplikacjach, takich jak serwery aplikacji, które mogą wczytywać i aktualizować komponenty w
czasie rzeczywistym.

JVM korzysta z hierarchicznego modelu ładowania klas, który składa się z kilku poziomów:

- **Bootstrap Class Loader**: Ładuje podstawowe klasy Java, takie jak `java.lang.Object`, które są niezbędne do
  funkcjonowania JVM.
- **Extension Class Loader**: Ładuje rozszerzenia platformy Java, znajdujące się w katalogu `ext`.
- **Application Class Loader**: Odpowiada za ładowanie klas aplikacji zdefiniowanych na ścieżce klas użytkownika (
  classpath).

Ładowanie klas odbywa się w trzech etapach:

1. **Ładowanie (Loading)**: JVM lokalizuje i ładuje plik `.class` do pamięci.
2. **Łączenie (Linking)**: JVM sprawdza poprawność klasy, przygotowuje statyczne zasoby i wykonuje rozwiązywanie
   symboli.
3. **Inicjalizacja (Initialization)**: Inicjalizowane są statyczne pola i bloki statyczne.

Dzięki takiej strukturze, Class Loader umożliwia elastyczne zarządzanie klasami w czasie działania aplikacji, co jest
szczególnie przydatne w frameworkach takich jak Spring czy Hibernate.

---

### 3. Zarządzanie pamięcią (Memory Management)

JVM automatycznie zarządza pamięcią, dzieląc ją na różne obszary, co pozwala na efektywne zarządzanie zasobami podczas
działania aplikacji. Każdy obszar pamięci ma specyficzne zadanie:

- **Heap (Sterta)**: Główna przestrzeń pamięci, w której JVM alokuje obiekty dynamiczne. Heap jest podzielony na młodszą
  generację (Young Generation) i starszą generację (Old Generation), co pomaga w optymalizacji pracy garbage collectora.
- **Stack (Stos)**: Każdy wątek w JVM ma swój własny stos, który służy do przechowywania zmiennych lokalnych, wywołań
  metod i wskaźników na bieżące ramki stosu.
- **Metaspace**: Przechowuje metadane klas, takie jak nazwy metod, typy zmiennych oraz inne informacje potrzebne JVM do
  zarządzania klasami.

Kluczowym elementem zarządzania pamięcią w JVM jest automatyczne zwalnianie nieużywanych zasobów przez **Garbage
Collector**, co zapobiega problemom takim jak wycieki pamięci.

---

### 4. Garbage Collection (GC)

Garbage Collection (GC) w JVM to mechanizm odpowiedzialny za automatyczne zarządzanie pamięcią. JVM używa generacyjnego
modelu garbage collection, co oznacza, że pamięć jest podzielona na generacje, a obiekty są przenoszone między
generacjami w zależności od tego, jak długo są używane:

- **Young Generation**: Przechowuje nowe obiekty. Większość obiektów w tej generacji szybko „umiera” i jest usuwana.
- **Old Generation**: Zawiera obiekty, które przetrwały kilka cykli garbage collection w młodszej generacji.
- **Metaspace**: Przechowuje metadane klas i informacje o typach obiektów.

Rodzaje garbage collectorów:

- **Serial GC**: Działa w jednym wątku i jest stosowany w prostszych aplikacjach.
- **Parallel GC**: Używa wielu wątków do przeprowadzania garbage collection, co zwiększa wydajność w aplikacjach
  wielowątkowych.
- **G1 Garbage Collector**: Minimalizuje przestoje (stop-the-world events) i optymalizuje przenoszenie obiektów między
  generacjami.
- **ZGC i Shenandoah**: Nowoczesne garbage collectory o niskim czasie zatrzymania, dedykowane do aplikacji wymagających
  niskiej latencji.

Dzięki generacyjnemu podejściu, GC może efektywnie zarządzać pamięcią, minimalizując opóźnienia w działaniu aplikacji.

---

### 5. Interpreter JVM

**Interpreter** w JVM to komponent odpowiedzialny za tłumaczenie bytecode na natywne instrukcje maszynowe w trakcie
wykonywania aplikacji, linia po linii. Interpreter jest kluczowym mechanizmem w początkowej fazie działania aplikacji,
ponieważ pozwala na szybkie uruchomienie programu bez potrzeby wcześniejszej kompilacji.

- Interpreter działa natychmiast po uruchomieniu programu, tłumacząc bytecode na bieżąco.
- Każda instrukcja jest tłumaczona i wykonywana bezpośrednio, co powoduje, że aplikacja może działać wolniej w dłuższym
  czasie, gdy te same instrukcje są powtarzane.

Interpreter zapewnia elastyczność, ale z czasem staje się mniej wydajny w porównaniu do natywnego kodu maszynowego.

---

### 6. Kompilacja Just-in-Time (JIT)

**JIT (Just-in-Time compiler)** to mechanizm JVM, który dynamicznie kompiluje często używane fragmenty bytecode do
natywnego kodu maszynowego. Gdy JVM zauważy, że pewne metody lub bloki kodu są często wykonywane, kompiluje je do
natywnego kodu, co znacząco zwiększa wydajność.

Główne cechy JIT:

- JIT analizuje kod w czasie rzeczywistym, identyfikując gorące metody (hot methods), które są często wywoływane.
- Po skompilowaniu, kod natywny jest wykonywany bez potrzeby ponownego tłumaczenia, co skraca czas wykonania.
- JVM korzysta z dwóch poziomów kompilacji JIT:
  - **C1 Compiler**: Kompiluje kod szybko, zapewniając podstawowe optymalizacje.
  - **C2 Compiler**: Przeprowadza zaawansowane optymalizacje, co jest idealne dla długoterminowych aplikacji.

---

### 7. Różnice między Interpreterem a JIT

Podstawowa różnica między interpreterem a JIT polega na tym, jak i kiedy bytecode jest tłumaczony na kod natywny:

| **Interpreter**                                                                 | **JIT**                                                                                                   |
|---------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Tłumaczy każdą instrukcję bytecode na bieżąco, co jest wolniejsze.              | Kompiluje często używany bytecode do natywnego kodu maszynowego, co jest szybsze w dłuższej perspektywie. |
| Szybki start aplikacji, ponieważ nie wymaga kompilacji.                         | Aplikacja może uruchomić się wolniej, ale zyskuje na wydajności w trakcie działania.                      |
| Idealny do krótkotrwałych aplikacji lub kodu, który nie jest często wykonywany. | Idealny do długotrwałych aplikacji z dużą ilością powtarzalnych operacji.                                 |

---

### 8. Bytecode

Java bytecode to pośredni kod generowany przez kompilator języka Java. Jest niezależny od platformy sprzętowej, co
pozwala uruchamiać aplikacje Java na różnych systemach operacyjnych bez potrzeby rekompilacji. JVM interpretuje lub
kompiluje ten bytecode, aby uruchomić programy na różnych platformach.

Instrukcje bytecode są przenośne i mogą być wykonywane przez dowolną implementację JVM. Narzędzia takie jak `javap`
pozwalają na analizowanie bytecode, co może pomóc w optymalizacji i debugowaniu.

---

### 9. JVM Opcje Konfiguracyjne

JVM oferuje wiele opcji konfiguracyjnych, które pozwalają na dostosowanie jej działania do wymagań aplikacji:

- **Opcje zarządzania pamięcią**:
    - `-Xmx`: Określa maksymalną ilość pamięci heap, którą JVM może używać.
    - `-Xms`: Określa początkową ilość pamięci heap, co może przyspieszyć uruchamianie aplikacji.
- **Opcje garbage collection**:
    - `-XX:+UseG1GC`: Włącza nowoczesny Garbage Collector G1.
    - `-XX:+PrintGCDetails`: Wyświetla szczegółowe informacje o działaniu GC, co jest przydatne w optymalizacji wydajności.

Optymalne ustawienie tych parametrów może znacząco wpłynąć na wydajność aplikacji, szczególnie w kontekście serwerów aplikacyjnych.
