## L1 — Topic Classification

### Cel

Przeanalizuj opis zadania i sklasyfikuj je tematycznie.

### Zadania

1. Ustal, jakich funkcjonalności, komponentów i zagadnień technicznych dotyczy issue.
2. Przypisz odpowiednie `ai_labels`. Możesz tworzyć nowe tagi, bez preferowania istniejących.
3. Zidentyfikuj podobne lub potencjalnie powiązane issues na podstawie indeksów.
4. Oceń, czy opis pozwala zrozumieć intencję zadania.
5. Wskaż potencjalne duplikaty oraz istotne zależności.

### Zasady

- Nie modyfikuj `labels` pochodzących z Jira.
- Nie zgaduj znaczenia niejasnych opisów.
- Nie zakładaj, że każde issue musi otrzymać `ai_labels`.
- Nie modyfikuj Memory Banków innych issues.

### Wynik

- `ai_labels`
- Powiązane issues, wraz z krótkim uzasadnieniem
- Krótkie podsumowanie rozpoznanego tematu
- Ocena: `understood`, `unclear` lub `potential_duplicate`

## L2 — Context Refinement

### Cel

Zrozum zadanie w kontekście istniejącego systemu i opracuj rozwiązanie nadające się do implementacji.

### Zadania

1. Przeanalizuj opis issue, Memory Bank oraz załączniki.
2. Zidentyfikuj powiązane moduły, klasy, interfejsy i testy.
3. Sprawdź podobne implementacje i wcześniejsze decyzje projektowe.
4. Ustal rzeczywisty cel zadania i oczekiwane zachowanie.
5. Oddziel potwierdzone fakty od założeń.
6. Zaproponuj rozwiązanie zgodne z istniejącą architekturą.
7. Zidentyfikuj pytania wymagające decyzji developera.
8. Oceń, czy dostępny kontekst wystarcza do rozpoczęcia implementacji.

### Zasady

- Opieraj ustalenia na kodzie, testach i dokumentacji.
- Podawaj konkretne referencje do istotnych elementów kodu.
- Respektuj wcześniej zapisane decyzje projektowe.
- Rozstrzygaj samodzielnie kwestie wynikające jednoznacznie z istniejącej implementacji.
- Dziel pytania na blokujące i nieblokujące.
- Nie wymagaj formalnych acceptance criteria, jeśli oczekiwane zachowanie można wiarygodnie ustalić.
- Nie implementuj rozwiązania na tym etapie.

### Wynik

- **Context** — cel i zakres zadania
- **AI Findings** — potwierdzone ustalenia
- **Assumptions** — przyjęte założenia
- **Proposed Solution** — projekt implementacji
- **Open Questions** — pytania z rozróżnieniem blocking/non-blocking
- **Affected Components** — przewidywany zakres zmian
- **Result:** `ready_for_implementation` lub `clarification_needed`

## L3 — Implementation Prototyping

### Cel

Zaimplementuj rozwiązanie na podstawie przeprowadzonej analizy i zapisanych decyzji projektowych.

### Zadania

1. Zweryfikuj Proposed Solution względem aktualnego kodu.
2. Zaimplementuj wymagane zmiany.
3. Dodaj lub dostosuj testy.
4. Uruchom odpowiednie testy lokalne oraz build.
5. Przeanalizuj wykryte błędy i popraw implementację.
6. Uwzględnij wcześniejszy feedback z code review i CI, jeżeli istnieje.
7. Zidentyfikuj istotne odstępstwa od Proposed Solution.
8. Przygotuj podsumowanie implementacji i wyników testów.

### Zasady

- Przestrzegaj istniejących konwencji i architektury projektu.
- Zachowuj zgodność z zatwierdzonymi decyzjami.
- Dopuszczalne są lokalne decyzje implementacyjne niewymagające udziału developera.
- Jeśli odkryjesz niejednoznaczność wpływającą na zachowanie systemu lub kontrakt API, zgłoś pytanie i wskaż, co blokuje dalszą pracę.
- Nie zmieniaj wymagań tylko po to, żeby uzyskać pozytywny wynik testów.
- Zachowaj użyteczny wynik również w przypadku niepełnej implementacji.

### Wynik

- **Implementation Summary** — zrealizowane zmiany
- **Tests** — dodane testy i wyniki uruchomień
- **Deviations** — odstępstwa od planu wraz z uzasadnieniem
- **Open Questions** — zagadnienia nierozstrzygnięte
- **Implementation Notes** — informacje istotne dla dalszej pracy
- **Result:** `implemented`, `clarification_needed` lub `failed`

Warto zachować jedną granicę odpowiedzialności: L1 ustala temat, L2 ustala rozwiązanie, L3 realizuje rozwiązanie. Każdy etap może odkryć problem wymagający ponownej analizy, ale nie powinien bez potrzeby powtarzać pracy
poprzednika.
