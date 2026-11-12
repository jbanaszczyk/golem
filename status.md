Proponuję jeden `golem_state` opisujący aktualny stan Jira issue z perspektywy Project Golem.

Nie powinien być drugim polem aktualizowanym niezależnie od GitHub. Orchestrator wyznacza go deterministycznie na podstawie Memory Banku, PR i metadata, a następnie udostępnia w indeksach i raportach.

## 1. Kanoniczne statusy

| `golem_state`          | Znaczenie                                             | Zdarzenie ustanawiające stan                 |
|------------------------|-------------------------------------------------------|----------------------------------------------|
| `not_started`          | Golem nie podjął zadania                              | Issue istnieje w metadata, brak pracy Golema |
| `triaged`              | Zakończono L1                                         | L1 zakończył klasyfikację                    |
| `clarification_needed` | L2 lub L3 wymaga odpowiedzi                           | Agent przygotował pytania                    |
| `ready_for_l3`         | Analiza zakończona, możliwa implementacja             | L2 zakończył pracę                           |
| `in_review`            | Implementacja oczekuje na decyzję developera          | L3 przygotował kod i PR                      |
| `parking`              | Trwa przygotowanie lub review dokumentacji parkowania | `golem:park` albo decyzja AI                 |
| `parked`               | Dokumentacja odłożonego zadania została opublikowana  | Merge dokumentacyjnego PR                    |
| `merged`               | Implementacja rzeczywiście trafiła do `develop`       | Merge implementacyjnego PR                   |

`clarification_needed` jest jednym stanem. Informacja, czy pytanie powstało podczas L2, czy L3, należy do historii wykonania i Memory Banku.

`in_review` obejmuje również sytuację, gdy developer zgłosił uwagi. Samo Request changes nie oznacza jeszcze uruchomienia Golema.

## 2. State machine

`not_started`

L1

`triaged`

L2

`clarification_needed`

Feedback → L2/L3

`ready_for_l3`

L3

`in_review`

Developer: merge lub continue

`merged`

Z każdego niezakończonego etapu: decyzja o parkowaniu

`parking`

`parked`

Powroty wynikające z `golem:continue`:

- `clarification_needed` → `ready_for_l3`, `in_review` albo ponownie `clarification_needed`, zależnie od wyniku L2/L3.
- `in_review` → `in_review`, `clarification_needed` albo `ready_for_l3`.
- `parked` → ponowna ocena L2/L3, wyłącznie po manualnym wznowieniu.

## 3. Jednoznaczne źródła prawdy

| Fakt                     | Źródło                                           |
|--------------------------|--------------------------------------------------|
| Zakończono L1/L2/L3      | Wynik agenta zapisany przez orchestrator         |
| Potrzebne doprecyzowanie | Wynik L2/L3 oraz Memory Bank                     |
| Oczekuje na review       | Otwarty PR z `golem:ready_for_review`            |
| Polecenie kontynuacji    | PR label `golem:continue`                        |
| Polecenie parkowania     | PR label `golem:park`                            |
| Zaparkowano              | Memory Bank `parked` i merge dokumentacyjnego PR |
| Zmergowano implementację | GitHub `merged_at` implementacyjnego PR          |

Reguła nadrzędna: zdarzenie GitHub merge implementacji ma pierwszeństwo przed wcześniejszymi wynikami AI.

W szczególności merge dokumentacyjnego PR nie ustanawia stanu `merged`.

## 4. Memory Bank a `golem_state`

W Memory Banku zapisywałbym wynik ostatniego etapu AI, np.:

```
golem_result: implemented
```

Natomiast kanoniczny `golem_state` będzie:

- `in_review`, dopóki PR pozostaje otwarty.
- `merged`, gdy developer faktycznie wykona merge.
- `parking`, jeżeli developer zleci parkowanie.

W ten sposób nie trzeba przedwcześnie wpisywać `merged` do Memory Banku, ani później poprawiać go dodatkowym commitem.

Stan `parked` pozostaje zapisany w Memory Banku, ponieważ jest trwałym wynikiem odłożenia zadania.

## 5. Zasada implementacyjna

Jeden task, jeden kanoniczny `golem_state`, wyliczany przez orchestrator.

GitHub labels są poleceniami albo sygnałami dla developera. Memory Bank przechowuje wynik analizy. Historia wykonań zawiera zdarzenia L1/L2/L3. GitHub potwierdza integrację.

Indeks i raport mogą zawierać zmaterializowany `golem_state`, odświeżany przy uruchomieniu Golema lub reindeksacji.

Dzięki temu mamy jeden słownik statusów do selekcji, raportowania i metryk, a `merged` zawsze oznacza faktycznie wykonaną integrację, nie deklarację agenta.
