```java
import java.util.*;
import java.util.function.*;
import java.util.Optional;

public class MyStream<T> {

    private List<T> elements;

    public MyStream(List<T> elements) {
        this.elements = elements;
    }

    public List<T> toList() {
        return new ArrayList<>(elements);
    }

    // Zadanie 1: filter
    public MyStream<T> filter(Predicate<T> predicate) {
        List<T> filteredElements = new ArrayList<>();
        for (T element : elements) {
            if (predicate.test(element)) {
                filteredElements.add(element);
            }
        }
        return new MyStream<>(filteredElements);
    }

    // Zadanie 2: map
    public <R> MyStream<R> map(Function<T, R> mapper) {
        List<R> mappedElements = new ArrayList<>();
        for (T element : elements) {
            mappedElements.add(mapper.apply(element));
        }
        return new MyStream<>(mappedElements);
    }

    // Zadanie 3: forEach
    public void forEach(Consumer<T> action) {
        for (T element : elements) {
            action.accept(element);
        }
    }

    // Zadanie 5: count
    public long count() {
        return elements.size();
    }

    // Zadanie 6: distinct (własna implementacja)
    public MyStream<T> distinct() {
        List<T> distinctElements = new ArrayList<>();
        for (T element : elements) {
            boolean isDuplicate = false;
            for (T distinctElement : distinctElements) {
                if (distinctElement.equals(element)) {
                    isDuplicate = true;
                    break;
                }
            }
            if (!isDuplicate) {
                distinctElements.add(element);
            }
        }
        return new MyStream<>(distinctElements);
    }

    // Zadanie 7: limit
    public MyStream<T> limit(int maxSize) {
        List<T> limitedElements = new ArrayList<>();
        for (int i = 0; i < maxSize && i < elements.size(); i++) {
            limitedElements.add(elements.get(i));
        }
        return new MyStream<>(limitedElements);
    }

    // Zadanie 9: skip
    public MyStream<T> skip(int n) {
        List<T> skippedElements = new ArrayList<>();
        for (int i = n; i < elements.size(); i++) {
            skippedElements.add(elements.get(i));
        }
        return new MyStream<>(skippedElements);
    }

    // Zadanie 10: sorted (własna implementacja)
    public MyStream<T> sorted(Comparator<T> comparator) {
        List<T> sortedElements = new ArrayList<>(elements);
        for (int i = 0; i < sortedElements.size() - 1; i++) {
            for (int j = 0; j < sortedElements.size() - i - 1; j++) {
                if (comparator.compare(sortedElements.get(j), sortedElements.get(j + 1)) > 0) {
                    // Swap elements
                    T temp = sortedElements.get(j);
                    sortedElements.set(j, sortedElements.get(j + 1));
                    sortedElements.set(j + 1, temp);
                }
            }
        }
        return new MyStream<>(sortedElements);
    }

    // Zadanie 12: findFirst
    public Optional<T> findFirst() {
        if (elements.isEmpty()) {
            return Optional.empty();
        }
        return Optional.of(elements.get(0));
    }

    // Nowe metody z dokumentu:

    // allMatch
    public boolean allMatch(Predicate<T> predicate) {
        for (T element : elements) {
            if (!predicate.test(element)) {
                return false;
            }
        }
        return true;
    }

    // anyMatch
    public boolean anyMatch(Predicate<T> predicate) {
        for (T element : elements) {
            if (predicate.test(element)) {
                return true;
            }
        }
        return false;
    }

    // noneMatch
    public boolean noneMatch(Predicate<T> predicate) {
        for (T element : elements) {
            if (predicate.test(element)) {
                return false;
            }
        }
        return true;
    }

    // reduce
    public T reduce(T identity, BinaryOperator<T> accumulator) {
        T result = identity;
        for (T element : elements) {
            result = accumulator.apply(result, element);
        }
        return result;
    }

    // min
    public Optional<T> min(Comparator<T> comparator) {
        if (elements.isEmpty()) {
            return Optional.empty();
        }
        T minElement = elements.get(0);
        for (T element : elements) {
            if (comparator.compare(element, minElement) < 0) {
                minElement = element;
            }
        }
        return Optional.of(minElement);
    }

    // max
    public Optional<T> max(Comparator<T> comparator) {
        if (elements.isEmpty()) {
            return Optional.empty();
        }
        T maxElement = elements.get(0);
        for (T element : elements) {
            if (comparator.compare(element, maxElement) > 0) {
                maxElement = element;
            }
        }
        return Optional.of(maxElement);
    }

    // peek
    public MyStream<T> peek(Consumer<T> action) {
        for (T element : elements) {
            action.accept(element);
        }
        return this;
    }

    // flatMap
    public <R> MyStream<R> flatMap(Function<T, MyStream<R>> mapper) {
        List<R> flatMappedElements = new ArrayList<>();
        for (T element : elements) {
            MyStream<R> mappedStream = mapper.apply(element);
            flatMappedElements.addAll(mappedStream.toList());
        }
        return new MyStream<>(flatMappedElements);
    }

    // join
    public String join(CharSequence delimiter) {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < elements.size(); i++) {
            result.append(elements.get(i));
            if (i < elements.size() - 1) {
                result.append(delimiter);
            }
        }
        return result.toString();
    }
}
```
