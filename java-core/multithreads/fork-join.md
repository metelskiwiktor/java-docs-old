## Framework Fork/Join w Javie - Teoria i Praktyka

### Wstęp do frameworku Fork/Join

Fork/Join to framework wprowadzony w Javie 7, który umożliwia efektywne przetwarzanie równoległe. Głównym celem tego narzędzia jest umożliwienie rozwiązywania problemów poprzez ich podział na mniejsze podzadania (fork), które są wykonywane równocześnie, a następnie łączenie wyników (join). Jest to przydatne szczególnie w kontekście zadań wymagających intensywnych obliczeń, gdzie standardowe podejście sekwencyjne byłoby mniej wydajne.

Framework Fork/Join opiera się na **paradygmacie Divide and Conquer** (dziel i zwyciężaj), w którym zadanie dzielone jest na mniejsze fragmenty aż do osiągnięcia pewnego poziomu szczegółowości. Gdy zadanie jest wystarczająco małe, jest wykonywane bez dalszego dzielenia. Wyniki wszystkich podzadań są następnie łączone, co pozwala uzyskać wynik dla pierwotnego problemu.

### Podstawowe klasy

1. **ForkJoinPool** - specjalna pula wątków, która zarządza wykonywaniem zadań w Fork/Join. Pool zarządza przydzielaniem zasobów dynamicznie, co pozwala optymalizować wykorzystanie dostępnych rdzeni procesora.

2. **RecursiveTask** - klasa służąca do reprezentowania zadań, które zwracają wyniki. Jest stosowana, gdy zadanie wymaga zwrócenia wyniku, np. sumy wartości. Metody używane w tej klasie:
    - **fork()** - uruchamia nowe podzadanie równolegle.
    - **join()** - czeka na wynik podzadania i łączy go z wynikiem innego podzadania.
    - **compute()** - podstawowa metoda, która definiuje logikę zadania. Sprawdza ona, czy zadanie jest wystarczająco małe do wykonania bez podziału, a jeśli nie, to dzieli zadanie na mniejsze części.

3. **RecursiveAction** - klasa służąca do reprezentowania zadań, które nie zwracają wyniku, ale mają efekt uboczny, np. aktualizują dane. Używana jest podobnie jak `RecursiveTask`, lecz nie zwraca wartości.

### Teoria wykorzystania frameworku Fork/Join

Framework Fork/Join najlepiej działa w przypadku zadań, które można efektywnie rozdzielić na mniejsze fragmenty i które mogą być wykonywane równocześnie. Jest to szczególnie przydatne przy dużych zbiorach danych, np. przy algorytmach sortowania, obliczeniach numerycznych lub zliczaniu wystąpień wartości.

Aby wykorzystać Fork/Join, należy stworzyć zadanie przez rozszerzenie klasy `RecursiveTask` lub `RecursiveAction`. Następnie zadanie to jest dzielone w miarę potrzeby, a wyniki są zbierane po zakończeniu przetwarzania. ForkJoinPool optymalizuje przydzielanie zasobów automatycznie, co pozwala uniknąć przeciążenia systemu.

---

## Praktyczne przykłady

W poniższych przykładach przedstawimy najpierw rozwiązanie w tradycyjnym podejściu sekwencyjnym, a następnie wersję z użyciem frameworku Fork/Join. Dzięki temu łatwiej będzie zauważyć, jak wprowadzenie `RecursiveTask` wpływa na strukturę kodu.

### Przykład 1: Obliczanie sumy elementów tablicy

#### Podejście sekwencyjne

W tradycyjnym podejściu sumowanie elementów tablicy wykonuje się w jednej pętli. Oto przykładowy kod:

```java
public class SequentialSumExample {
    public static void main(String[] args) {
        int[] array = new int[1_000_000];
        for (int i = 0; i < array.length; i++) {
            array[i] = i + 1;
        }

        long sum = 0;
        for (int value : array) {
            sum += value;
        }
        System.out.println("Suma elementów tablicy: " + sum);
    }
}
```

#### Wersja z użyciem Fork/Join

Teraz zobaczymy, jak można rozwiązać ten problem z użyciem Fork/Join. Zadanie zostanie podzielone na mniejsze fragmenty, a każda część tablicy będzie przetwarzana równocześnie.

```java
import java.util.concurrent.RecursiveTask;
import java.util.concurrent.ForkJoinPool;

class SumTask extends RecursiveTask<Long> {
    private static final int THRESHOLD = 10_000;
    private final int[] array;
    private final int start;
    private final int end;

    public SumTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Long compute() {
        if (end - start <= THRESHOLD) {
            long sum = 0;
            for (int i = start; i < end; i++) {
                sum += array[i];
            }
            return sum;
        } else {
            int middle = (start + end) / 2;
            SumTask leftTask = new SumTask(array, start, middle);
            SumTask rightTask = new SumTask(array, middle, end);

            leftTask.fork();
            long rightResult = rightTask.compute();
            long leftResult = leftTask.join();

            return leftResult + rightResult;
        }
    }
}

public class ForkJoinExample {
    public static void main(String[] args) {
        int[] array = new int[1_000_000];
        for (int i = 0; i < array.length; i++) {
            array[i] = i + 1;
        }

        ForkJoinPool pool = new ForkJoinPool();
        SumTask task = new SumTask(array, 0, array.length);

        long result = pool.invoke(task);
        System.out.println("Suma elementów tablicy: " + result);
    }
}
```

W tej wersji kodu zadanie `SumTask` dzieli tablicę na mniejsze fragmenty aż do momentu, gdy fragment jest mniejszy niż ustalony próg (`THRESHOLD`). Każdy fragment jest sumowany osobno, a wyniki są następnie łączone.

### Przykład 2: Liczenie częstotliwości występowania słów w tekście

#### Podejście sekwencyjne

Tradycyjne podejście polega na podzieleniu tekstu na słowa i zliczaniu ich wystąpień przy użyciu jednej pętli:

```java
import java.util.Map;
import java.util.HashMap;
import java.util.List;
import java.util.Arrays;

public class SequentialWordCountExample {
    public static void main(String[] args) {
       String text = """
               To jest przykład tekstu który posłuży do przetwarzania równoległego.
               Tekst zawiera powtarzające się słowa i chcemy policzyć ich częstotliwość.
               """;

        String[] words = text.toLowerCase().split("\\W+");
        Map<String, Integer> wordCount = new HashMap<>();
        
        for (String word : words) {
            wordCount.put(word, wordCount.getOrDefault(word, 0) + 1);
        }
        System.out.println("Częstotliwość występowania słów: " + wordCount);
    }
}
```

#### Wersja z użyciem Fork/Join

Poniżej prezentujemy, jak zliczanie słów można zrównoleglić za pomocą Fork/Join. Zadanie `WordCountTask` dzieli listę słów na mniejsze fragmenty, zlicza wystąpienia słów w każdym fragmencie, a wyniki są łączone w całość.

```java
import java.util.concurrent.RecursiveTask;
import java.util.concurrent.ForkJoinPool;
import java.util.Map;
import java.util.HashMap;
import java.util.List;
import java.util.Arrays;

class WordCountTask extends RecursiveTask<Map<String, Integer>> {
    private static final int THRESHOLD = 100;
    private final List<String> words;

    public WordCountTask(List<String> words) {
        this.words = words;
    }

    @Override
    protected Map<String, Integer> compute() {
        if (words.size() <= THRESHOLD) {
            return countWords(words);
        } else {
            int middle = words.size() / 2;
            WordCountTask leftTask = new WordCountTask(words.subList(0, middle));
            WordCountTask rightTask = new WordCountTask(words.subList(middle, words.size()));

            leftTask.fork();
            Map<String, Integer> rightResult = rightTask.compute();
            Map<String, Integer> leftResult = leftTask.join();

            return mergeResults(leftResult, rightResult);
        }
    }

   private Map<String, Integer> countWords(List<String> words) {
      Map<String, Integer> wordCount = new HashMap<>();
      for (String word : words) {
         wordCount.put(word, wordCount.getOrDefault(word, 0) + 1);
      }
      return wordCount;
   }

    private Map<String, Integer> mergeResults(Map<String, Integer> leftResult, Map<String, Integer> rightResult) {
        Map<String, Integer> merged = new HashMap<>(leftResult);
        for (Map.Entry<String, Integer> entry : rightResult.entrySet()) {
            merged.put(entry.getKey(), merged.getOrDefault(entry.getKey(), 0) + entry.getValue());
        }
        return merged;
    }
}

public class ForkJoinWordCountExample {
    public static void main(String[] args) {
       String text = """
               To jest przykład tekstu który posłuży do przetwarzania równoległego.
               Tekst zawiera powtarzające się słowa i chcemy policzyć ich częstotliwość.
               """;

        List<String> words = Arrays.asList(text.toLowerCase().split("\\W+"));
        ForkJoinPool pool = new ForkJoinPool();
        WordCountTask task = new WordCountTask(words);

        Map<String, Integer> wordCounts = pool.invoke(task);
        System.out.println("Częstotliwość występowania słów: " + wordCounts);
    }
}
```

W wersji z Fork/Join:
- `WordCountTask` dzieli listę słów na mniejsze fragmenty, zlicza wystąpienia słów, a następnie scala wyniki.
- Dzięki równoległemu przetwarzaniu Fork/Join zapewnia lepszą wydajność przy dużych zbiorach danych.

---

### Przykład 3: Generowanie fraktalu Mandelbrota

Fraktal Mandelbrota jest generowany przez iteracyjne sprawdzanie, czy liczby zespolone dla danych współrzędnych punktu należą do zbioru Mandelbrota. Jest to zadanie o dużym obciążeniu obliczeniowym, które łatwo podzielić na mniejsze fragmenty (np. generowanie fraktalu dla różnych pasów obrazu).

#### Wersja sekwencyjna

Wersja sekwencyjna generuje obraz 800x800 pikseli, gdzie dla każdego piksela przeprowadzamy obliczenia iteracyjne.

```java
import java.awt.image.BufferedImage;
import javax.imageio.ImageIO;
import java.io.File;
import java.io.IOException;

public class MandelbrotSequential {
    public static void main(String[] args) throws IOException {
        int width = 800;
        int height = 800;
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        int maxIter = 1000;

        for (int y = 0; y < height; y++) {
            for (int x = 0; x < width; x++) {
                double zx = 0;
                double zy = 0;
                double cX = (x - 400) / 200.0;
                double cY = (y - 400) / 200.0;
                int iter = maxIter;
                while (zx * zx + zy * zy < 4 && iter > 0) {
                    double tmp = zx * zx - zy * zy + cX;
                    zy = 2.0 * zx * zy + cY;
                    zx = tmp;
                    iter--;
                }
                int color = iter | (iter << 8);
                image.setRGB(x, y, color);
            }
        }

        ImageIO.write(image, "png", new File("mandelbrot_sequential.png"));
        System.out.println("Obraz fraktalu Mandelbrota wygenerowany sekwencyjnie.");
    }
}
```

#### Wersja z Fork/Join

Poniżej przedstawiamy wersję z użyciem Fork/Join, w której zadanie `MandelbrotTask` dzieli obraz na mniejsze pasy poziome, które są przetwarzane równocześnie.

```java
import java.awt.image.BufferedImage;
import java.util.concurrent.RecursiveAction;
import java.util.concurrent.ForkJoinPool;
import javax.imageio.ImageIO;
import java.io.File;
import java.io.IOException;

class MandelbrotTask extends RecursiveAction {
    private static final int THRESHOLD = 50;
    private final BufferedImage image;
    private final int startY, endY, width, height, maxIter;

    public MandelbrotTask(BufferedImage image, int startY, int endY, int width, int height, int maxIter) {
        this.image = image;
        this.startY = startY;
        this.endY = endY;
        this.width = width;
        this.height = height;
        this.maxIter = maxIter;
    }

    @Override
    protected void compute() {
        if (endY - startY <= THRESHOLD) {
            for (int y = startY; y < endY; y++) {
                for (int x = 0; x < width; x++) {
                    double zx = 0;
                    double zy = 0;
                    double cX = (x - 400) / 200.0;
                    double cY = (y - 400) / 200.0;
                    int iter = maxIter;
                    while (zx * zx + zy * zy < 4 && iter > 0) {
                        double tmp = zx * zx - zy * zy + cX;
                        zy = 2.0 * zx * zy + cY;
                        zx = tmp;
                        iter--;
                    }
                    int color = iter | (iter << 8);
                    image.setRGB(x, y, color);
                }
            }
        } else {
            int midY = (startY + endY) / 2;
            MandelbrotTask topTask = new MandelbrotTask(image, startY, midY, width, height, maxIter);
            MandelbrotTask bottomTask = new MandelbrotTask(image, midY, endY, width, height, maxIter);
            invokeAll(topTask, bottomTask);
        }
    }
}

public class MandelbrotForkJoin {
    public static void main(String[] args) throws IOException {
        int width = 800;
        int height = 800;
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        int maxIter = 1000;

        ForkJoinPool pool = new ForkJoinPool();
        MandelbrotTask task = new MandelbrotTask(image, 0, height, width, height, maxIter);

        pool.invoke(task);
        ImageIO.write(image, "png", new File("mandelbrot_forkjoin.png"));
        System.out.println("Obraz fraktalu Mandelbrota wygenerowany przy użyciu Fork/Join.");
    }
}
```

---

### Zalety i wady stosowania Fork/Join

#### Zalety

1. **Efektywne wykorzystanie procesorów wielordzeniowych** - dzięki równoległemu przetwarzaniu Fork/Join przyspiesza wykonywanie zadań, które można podzielić na niezależne fragmenty.
2. **Automatyczne zarządzanie wątkami** - ForkJoinPool samodzielnie przydziela wątki i równoważy obciążenie, co pozwala uniknąć ręcznego zarządzania.
3. **Przeznaczenie dla obciążających obliczeń** - idealnie sprawdza się w algorytmach intensywnie korzystających z procesora, takich jak generowanie fraktali, przetwarzanie grafów czy symulacje numeryczne.

#### Wady

1. **Skomplikowana implementacja dla prostych zadań** - Fork/Join może wprowadzać zbędną złożoność, gdy zadania są proste lub wymagają dostępu do zasobów współdzielonych.
2. **Złożoność w debugowaniu** - równoległe przetwarzanie i dzielenie zadań może utrudnić śledzenie błędów oraz zarządzanie stanem.
3. **Koszty komunikacji między wątkami** - dla zadań o niskiej złożoności komunikacja między wątkami i zarządzanie pamięcią podręczną może zniwelować korzyści wynikające z równoległości.

---

### Podsumowanie

Framework Fork/Join umożliwia efektywne równoległe przetwarzanie zadań w Javie, szczególnie tych, które można podzielić na mniejsze podzadania. Użycie `RecursiveTask` i `RecursiveAction` pozwala na zdefiniowanie logiki podziału i przetwarzania równoległego.

**Kluczowe punkty:**
- `ForkJoinPool` optymalizuje przydzielanie zasobów, co pozwala na efektywne zarządzanie wątkami.
- Fork/Join idealnie nadaje się do zadań, które można przetwarzać równocześnie, co pozwala na pełne wykorzystanie współczesnych procesorów wielordzeniowych.