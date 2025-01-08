# Rozwiązanie: Zadanie Refaktoryzacyjne: Konwersja na `Optional`

> [!WARNING]
> **Uwaga!** Zapoznaj się z kodem dopiero po własnoręcznym rozwiązaniu [optional-refaktor1.md](..%2Fjava-core%2Fcwiczenia%2Foptional-refaktor1.md)

```java
import java.util.Optional;
import java.util.regex.Pattern;
import java.util.Set;


public class ProductValidator {

    private static final Pattern PRODUCT_CODE_PATTERN = Pattern.compile("^[A-Za-z0-9]{5,10}$");
    private static final Pattern PRICE_PATTERN = Pattern.compile("^[0-9]+(\\.[0-9]{1,2})?$");
    private static final Set<String> VALID_CATEGORIES = Set.of("Electronics", "Books", "Clothing", "Home", "Toys");

    public boolean validateProductAttributes(String productCode, String price, String productName) {
        boolean isProductCodeValid = Optional.ofNullable(productCode)
                .filter(code -> !code.trim().isEmpty())
                .filter(code -> PRODUCT_CODE_PATTERN.matcher(code).matches())
                .isPresent();

        boolean isPriceValid = Optional.ofNullable(price)
                .filter(p -> !p.trim().isEmpty())
                .filter(p -> PRICE_PATTERN.matcher(p).matches())
                .map(Double::parseDouble)
                .filter(parsedPrice -> parsedPrice > 0)
                .isPresent();

        boolean isProductNameValid = Optional.ofNullable(productName)
                .filter(name -> !name.trim().isEmpty())
                .filter(name -> Character.isUpperCase(name.charAt(0)))
                .isPresent();

        return isProductCodeValid && isPriceValid && isProductNameValid;
    }

    public boolean validateProductOptionalAttributes(String description, String category) {
        boolean isDescriptionValid = Optional.ofNullable(description)
                .map(String::trim)
                .map(desc -> desc.isEmpty() || desc.length() >= 15)
                .orElse(true);

        boolean isCategoryValid = Optional.ofNullable(category)
                .map(String::trim)
                .map(cat -> !cat.isEmpty() && VALID_CATEGORIES.contains(cat))
                .orElse(true);

        return isDescriptionValid && isCategoryValid;
    }
}

```
# Rozwiązanie: Refaktoryzacja: LibraryService - Kod o Niskiej Jakości

> [!WARNING]
> **Uwaga!** Zapoznaj się z kodem dopiero po własnoręcznym rozwiązaniu [optional-refaktor2.md](..%2Fjava-core%2Fcwiczenia%2Foptional-refaktor2.md)

```java
public class LibraryService {
    private final List<Book> books;
    private final List<Author> authors;

    public LibraryService(List<Book> books, List<Author> authors) {
        this.books = books;
        this.authors = authors;
    }

    public Book borrowBook(String title, String authorName) {
        return findBookByTitleAndAuthor(title, authorName)
                .filter(Book::available)
                .map(book -> updateBookAvailability(book, false))
                .orElseThrow(() -> new IllegalStateException("Book is not available for borrowing."));
    }

    public Book releaseBook(Long bookId) {
        return findBookById(bookId)
                .filter(book -> !book.available())
                .map(book -> updateBookAvailability(book, true))
                .orElseThrow(() -> new IllegalStateException("Book is not available for release."));
    }

    private Optional<Book> findBookByTitleAndAuthor(String title, String authorName) {
        return findAuthorByName(authorName)
                .flatMap(author -> author.books().stream()
                        .filter(book -> book.title().equalsIgnoreCase(title))
                        .findFirst());
    }

    private Optional<Book> findBookById(Long bookId) {
        return books.stream()
                .filter(book -> book.id().equals(bookId))
                .findFirst();
    }


    private Optional<Author> findAuthorByName(String authorName) {
        return authors.stream()
                .filter(author -> author.name().equalsIgnoreCase(authorName))
                .findFirst();
    }

    private Book updateBookAvailability(Book book, boolean available) {
        Book updatedBook = new Book(book.id(), book.title(), book.author(), available);
        books.remove(book);
        books.add(updatedBook);
        return updatedBook;
    }
}
```