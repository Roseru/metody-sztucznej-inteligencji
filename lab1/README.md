# Laboratorium 1

- [Sprawozdanie](sprawozdanie.md)
- [Oddany wynik](results/exercise-summary.json)
- [Zapisy sesji](sesje/)
- [Kod, dane i testy](project/)

## Odtworzenie wyników

Dołączony test na danych syntetycznych można uruchomić od razu.
Odtworzenie raportów ćwiczeniowych wymaga dodatkowo plików
`project/data/exercise.csv` i `project/data/exercise-original.csv`, których
obecnie nie ma w repozytorium.

Wymagany jest Python 3.10 lub nowszy. Projekt korzysta wyłącznie z biblioteki
standardowej. Nie wymaga instalacji pakietów ani GPU.
Polecenia uruchamiaj z katalogu `lab1/project/`.

Windows:

```powershell
py -3 describe_data.py --data data/exercise-original.csv --out results/reproduced-before.json
py -3 describe_data.py --data data/exercise.csv --out results/reproduced-final.json
py -3 -m unittest discover -s tests -v
```

Linux i macOS:

```bash
python3 describe_data.py --data data/exercise-original.csv --out results/reproduced-before.json
python3 describe_data.py --data data/exercise.csv --out results/reproduced-final.json
python3 -m unittest discover -s tests -v
```

| Sprawdzenie | Wiadomości | ham | spam |
| --- | ---: | ---: | ---: |
| Przed zmianą etykiety | 8 | 5 | 3 |
| Po zmianie etykiety | 8 | 4 | 4 |
| Osobne dane gotowego testu | 3 | 2 | 1 |

Test powinien zakończyć się `Ran 1 test`, `OK` i kodem zakończenia `0`.
Sprawdza osobny syntetyczny CSV; raport ćwiczeniowy odczytaj niezależnie.

`data/exercise-original.csv` powinien być niezmienioną kopią CSV z oryginalnej paczki.
`data/exercise.csv` zawiera jedną zmianę: `exercise-001`, `ham` → `spam`.

Nowe wykonania zapisują wyniki pod innymi nazwami i pozostawiają oddany raport
bez zmian. Przy porównywaniu sprawdzaj liczebności oraz `input_sha256`.
Ścieżki bezwzględne i pola `runtime` zależą od komputera i interpretera.
Historycznych wiadomości nie wysyłaj i nie otwieraj ich linków.
