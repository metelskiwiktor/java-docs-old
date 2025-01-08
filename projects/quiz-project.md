# Wytyczne Projektu Aplikacji Quizowej

Aplikacja umożliwia tworzenie zestawów quizów, generowanie testów oraz ich wykonywanie przez użytkowników. Backend aplikacji jest odpowiedzialny za zarządzanie danymi quizów i testów, przetwarzanie odpowiedzi użytkowników oraz odpowiednią walidację i obsługę błędów.

## Endpointy API

### 1. Tworzenie Quizu

**Endpoint:** `POST /quiz/create`

**Opis:** Umożliwia tworzenie nowego quizu z zestawem pytań i odpowiedzi.

**Ciało żądania (JSON):**
```json
{
  "title": "string",
  "questions": [
    {
      "questionText": "string",
      "answers": ["string"],
      "correctAnswers": ["string"]
    }
  ]
}
```

**Walidacja danych wejściowych:**

- **title**:
    - Typ: `string`
    - Minimalna długość: **5 znaków**

- **questions**:
    - Typ: `array`
    - Minimalna liczba pytań: **1**

- **Każdy element w `questions`**:
    - **questionText**:
        - Typ: `string`
        - Minimalna długość: **5 znaków**
        - Pytania nie mogą się powtarzać w ramach jednego quizu
    - **answers**:
        - Typ: `array` of `string`
        - Minimalna liczba odpowiedzi: **2**
        - Każda odpowiedź:
            - Minimalna długość: **1 znak**
        - Odpowiedzi nie mogą się duplikować
    - **correctAnswers**:
        - Typ: `array` of `string`
        - Minimalna liczba poprawnych odpowiedzi: **1**
        - Każda odpowiedź:
            - Minimalna długość: **1 znak**
        - Poprawne odpowiedzi nie mogą się duplikować
        - Liczba poprawnych odpowiedzi nie może przekraczać liczby możliwych odpowiedzi

**Przykład odpowiedzi:**

- **Sukces (201 Created):**
  ```json
  {
    "quizId": "uuid" // ID nowo utworzonego quizu
  }
  ```

- **Błąd (400 Bad Request):**
  ```json
  {
    "error": "Opis błędu"
  }
  ```

---

### 2. Pobranie Listy Quizów

**Endpoint:** `GET /quiz/list`

**Opis:** Zwraca listę dostępnych quizów.

**Odpowiedź (JSON):**
```json
[
  {
    "id": "uuid",
    "title": "string",
    "questionsCount": number
  }
]
```

**Parametry odpowiedzi:**

- **id**: UUID quizu jako `string`
- **title**: nazwa quizu
- **questionsCount**: liczba pytań w quizie

---

### 3. Generowanie Testu

**Endpoint:** `POST /quiz/{quizId}/generate`

**Opis:** Generuje nowy test na podstawie istniejącego quizu.

**Parametry ścieżki:**

- **quizId**: UUID quizu

**Ciało żądania (JSON):**
```json
{
  "maxAttempts": number,
  "deadline": "ISO8601 string"
}
```

**Walidacja danych wejściowych:**

- **quizId**:
    - Musi istnieć w bazie danych
- **maxAttempts**:
    - Minimalna wartość: **1**
    - Maksymalna wartość: **2147483647**
- **deadline**:
    - Data w formacie ISO 8601
    - Musi być późniejsza niż aktualny czas

**Odpowiedź:**

- **Sukces (201 Created):**
  ```json
  {
    "testId": "uuid",
    "questions": [
      {
        "questionText": "string",
        "answers": ["string"]
      }
    ],
    "deadline": "ISO8601 string",
    "maxAttempts": number
  }
  ```

- **Błąd (400 Bad Request):**
  ```json
  {
    "error": "Opis błędu"
  }
  ```

---

### 4. Dostarczenie Rozwiązania Testu

**Endpoint:** `POST /quiz/test/{testId}/submit`

**Opis:** Umożliwia przesłanie odpowiedzi na pytania testu.

**Parametry ścieżki:**

- **testId**: UUID testu

**Ciało żądania (JSON):**
```json
{
  "answers": [
    {
      "questionText": "string",
      "selectedAnswers": ["string"]
    }
  ]
}
```

**Walidacja danych wejściowych:**

- **testId**:
    - Musi być aktywny i nieprzekroczony termin
- **answers**:
    - Każde `questionText` musi odpowiadać pytaniu z testu
    - `selectedAnswers` muszą być zawarte w możliwych odpowiedziach
    - Brak duplikacji w `questionText` i `selectedAnswers`

**Odpowiedź:**

- **Sukces (200 OK):**
  ```json
  {
    "quizTitle": "string",
    "correctAnswersPercentage": "string", // np. "80%"
    "questions": [
      {
        "questionText": "string",
        "userAnswers": ["string"],
        "possibleAnswers": ["string"],
        "correctAnswers": ["string"]
      }
    ],
    "completedAt": "ISO8601 string"
  }
  ```

- **Błąd (400 Bad Request):**
  ```json
  {
    "error": "Opis błędu"
  }
  ```

---

### 5. Lista Testów dla Quizu

**Endpoint:** `GET /quiz/{quizId}/tests`

**Opis:** Zwraca listę testów wygenerowanych dla danego quizu.

**Parametry ścieżki:**

- **quizId**: UUID quizu

**Odpowiedź (JSON):**
```json
[
  {
    "testId": "uuid",
    "deadline": "ISO8601 string",
    "maxAttempts": number,
    "attemptsMade": number,
    "status": "string" // np. "Aktywny", "Zakończony"
  }
]
```

---

### 6. Lista Wykonań Testu

**Endpoint:** `GET /quiz/test/{testId}/attempts`

**Opis:** Zwraca historię podejść do danego testu.

**Parametry ścieżki:**

- **testId**: UUID testu

**Odpowiedź (JSON):**
```json
[
  {
    "attemptNumber": number,
    "completedAt": "ISO8601 string",
    "scorePercentage": "string" // np. "75%"
  }
]
```

---

### 7. Szczegóły Wykonania Testu

**Endpoint:** `GET /quiz/test/{testId}/attempt/{attemptNumber}`

**Opis:** Zwraca szczegółowe informacje o konkretnym podejściu do testu.

**Parametry ścieżki:**

- **testId**: UUID testu
- **attemptNumber**: numer podejścia

**Odpowiedź (JSON):**
```json
{
  "quizTitle": "string",
  "scorePercentage": "string",
  "completedAt": "ISO8601 string",
  "questions": [
    {
      "questionText": "string",
      "userAnswers": ["string"],
      "correctAnswers": ["string"],
      "possibleAnswers": ["string"]
    }
  ]
}
```

---

## Działanie Backend

### Walidacja i Obsługa Błędów

- **Walidacja danych wejściowych:**
    - Wszystkie pola są sprawdzane pod kątem poprawności typu i wartości.
    - Unikalność pytań i odpowiedzi w ramach quizu.
- **Obsługa odpowiedzi i błędów:**
    - Zwracanie odpowiednich kodów HTTP (np. 200, 201, 400, 404).
    - Szczegółowe komunikaty błędów dla ułatwienia debugowania.

### Zarządzanie Testami i Limitami

- **Śledzenie liczby prób:**
    - Backend monitoruje, ile razy użytkownik podjął próbę rozwiązania testu.
- **Sprawdzanie terminów:**
    - Testy po przekroczeniu `deadline` są automatycznie oznaczane jako nieważne.
- **Zapisywanie wyników:**
    - Wszystkie wyniki są przechowywane dla późniejszego wglądu.

## Scenariusze Użycia

1. **Tworzenie nowego quizu:**
    - Użytkownik przesyła żądanie `POST /quiz/create` z danymi quizu.
    - Backend tworzy quiz i zwraca `quizId`.

2. **Generowanie testu:**
    - Użytkownik przesyła żądanie `POST /quiz/{quizId}/generate`.
    - Backend tworzy test i zwraca `testId` wraz z pytaniami.

3. **Wykonywanie testu:**
    - Użytkownik przesyła odpowiedzi na `POST /quiz/test/{testId}/submit`.
    - Backend ocenia odpowiedzi i zwraca wynik oraz szczegółowy raport.

4. **Przeglądanie wyników:**
    - Użytkownik może pobrać historię swoich prób i szczegóły podejść poprzez odpowiednie endpointy `GET`.
