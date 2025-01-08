stream2

# zlozone operacje
```java
import java.util.Arrays;
import java.util.List;

public class MyStreamOperations {

    public static void main(String[] args) {

        // 3. Złożona operacja `allMatch`, `anyMatch`
        List<Integer> numbers1 = Arrays.asList(2, 4, 6, 9, 12);
        MyStream<Integer> stream1 = new MyStream<>(numbers1);
        
        boolean allEven = stream1.allMatch(n -> n % 2 == 0);
        boolean anyGreaterThanTen = stream1.anyMatch(n -> n > 10);

        System.out.println("All even: " + allEven);  // false
        System.out.println("Any greater than 10: " + anyGreaterThanTen);  // true

        // 6. Złożona operacja `reduce`, `noneMatch`
        List<Integer> numbers2 = Arrays.asList(12, 15, 8, 22);
        MyStream<Integer> stream2 = new MyStream<>(numbers2);

        int sumGreaterThanTen = stream2.filter(n -> n > 10).reduce(0, Integer::sum);
        boolean noneLessThanFive = stream2.noneMatch(n -> n < 5);

        System.out.println("Sum of numbers > 10: " + sumGreaterThanTen);  // 49
        System.out.println("None less than 5: " + noneLessThanFive);  // false

        // 9. Złożona operacja `min`, `max`, `reduce`
        List<Integer> numbers3 = Arrays.asList(3, 7, 2, 9, 1);
        MyStream<Integer> stream3 = new MyStream<>(numbers3);

        int sum = stream3.reduce(0, Integer::sum);
        stream3.min(Integer::compareTo).ifPresent(min -> System.out.println("Min: " + min));  // Min: 1
        stream3.max(Integer::compareTo).ifPresent(max -> System.out.println("Max: " + max));  // Max: 9
        System.out.println("Sum: " + sum);  // Sum: 22

        // 12. Złożona operacja `flatMap`, `peek`
        List<List<Integer>> nestedNumbers = Arrays.asList(Arrays.asList(1, 2), Arrays.asList(3, 4), Arrays.asList(5, 6));
        MyStream<List<Integer>> stream4 = new MyStream<>(nestedNumbers);

        MyStream<Integer> flattenedStream = stream4.flatMap(MyStream::new);
        flattenedStream.peek(System.out::println).toList();  // 1 2 3 4 5 6
    }
}

```

# zadania do wykonania
```java
import java.util.Arrays;
import java.util.List;

public class MyStreamTasks {

    public static void main(String[] args) {

        // Zadanie 1: Sprawdzenie dopasowania
        List<Integer> task1Numbers = Arrays.asList(12, 50, 70, 33);
        MyStream<Integer> stream1 = new MyStream<>(task1Numbers);

        boolean allPositive = stream1.allMatch(n -> n > 0);
        boolean noneGreaterThan100 = stream1.noneMatch(n -> n > 100);

        System.out.println("All positive: " + allPositive);  // true
        System.out.println("None greater than 100: " + noneGreaterThan100);  // true

        // Zadanie 2: Operacje na liście nazw
        List<String> task2Names = Arrays.asList("Anna", "Agnieszka", "Bartek", "Cecylia", "Adam");
        MyStream<String> stream2 = new MyStream<>(task2Names);

        String joinedNames = stream2.filter(name -> name.startsWith("A"))
                                    .map(String::toUpperCase)
                                    .join(", ");
        System.out.println(joinedNames);  // "ANNA, AGNIESZKA, ADAM"

        // Zadanie 3: Sumowanie i sprawdzanie
        List<Integer> task3Numbers = Arrays.asList(10, 15, 20);
        MyStream<Integer> stream3 = new MyStream<>(task3Numbers);

        int sum = stream3.reduce(0, Integer::sum);
        boolean sumLessThan50 = stream3.noneMatch(n -> sum < 50);

        System.out.println("Sum: " + sum);  // 45
        System.out.println("Sum less than 50: " + sumLessThan50);  // false

        // Zadanie 4: Minimalna i maksymalna wartość
        List<Integer> task4Numbers = Arrays.asList(3, 7, 9, 2, 8);
        MyStream<Integer> stream4 = new MyStream<>(task4Numbers);

        stream4.min(Integer::compareTo).ifPresent(min -> System.out.println("Min: " + min));  // 2
        stream4.max(Integer::compareTo).ifPresent(max -> System.out.println("Max: " + max));  // 9

        // Zadanie 5: Spłaszczenie i wydrukowanie elementów
        List<List<Integer>> task5NestedNumbers = Arrays.asList(Arrays.asList(1, 2), Arrays.asList(3, 4), Arrays.asList(5, 6));
        MyStream<List<Integer>> stream5 = new MyStream<>(task5NestedNumbers);

        MyStream<Integer> flattenedStream = stream5.flatMap(MyStream::new);
        flattenedStream.peek(System.out::println).reduce(0, Integer::sum);  // Wyświetli elementy i zsumuje
    }
}
```