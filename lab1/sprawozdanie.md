[Repozytorium na GitHubie](https://github.com/Roseru/metody-sztucznej-inteligencji)

# Laboratorium 1. Podstawy pracy z agentem

**Aplikacja:** Codex, lokalny komputer z Windows.  
**Model:** GPT-6.1 Sol. **Poziom rozumowania:** Extra High.  
**Data wykonania:** 2026-11-05.

[Końcowy raport JSON](results/exercise-summary.json) · [Pełne zapisy sesji](sesje/)

Eksporty sesji zachowują wszystkie zdarzenia; kopie do publikacji mają
zanonimizowane dane osobowe i instytucjonalne oraz ścieżki. Te same aliasy
ścieżek zastosowano w raporcie JSON. Zakres zmian opisuje
[informacja o anonimizacji](sesje/README.md).

## 1. Zlecony wynik, granice i odbiór

Zleciłem zliczenie wiadomości oraz etykiet `ham` i `spam` w
`data/exercise.csv` i zapis podsumowania w `results/exercise-summary.json`.
W etapie 1 nie pozwalałem zmieniać kodu, danych ani testów. W etapie 2
dopuszczona była zmiana jednej etykiety z `ham` na `spam` i ponowne wykonanie
programu po zapisaniu przewidywania.

Przy odbiorze oczekiwałem rzeczywistego polecenia i wyniku procesu, wskazania
wejścia oraz miejsca zapisu, odczytania treści raportu, wyniku gotowego testu
i informacji o ewentualnych niewykonanych krokach. Agent odczytał README,
skrypt oraz wiadomość `exercise-001`. Jej pola oznaczają: `message_id` —
identyfikator, `text` — temat i treść, `label` — etykietę ze zbioru.

## 2. Powstanie i sprawdzenie raportu

Agent wykonał przez narzędzie terminala `exec_command` polecenie:

```powershell
.\.venv\Scripts\python.exe describe_data.py --data data/exercise.csv --out results/exercise-summary.json
```

Katalog roboczy jest w publikacji oznaczony neutralnym aliasem
`C:\projekt\Laboratorium 1\project`; rzeczywista ścieżka pozostaje w lokalnym oryginale.
Skrypt wykonał lokalnie Python 3.14.5 z interpretera
`.venv\Scripts\python.exe`. Zarówno pierwsze, jak i końcowe uruchomienie
zakończyło się kodem `0` i komunikatem `Report saved`.

Podział ról wyjaśniłem w sesji: model wybrał polecenie, Codex przekazał je
do terminala, a mój komputer uruchomił skrypt Pythona.

Po każdym uruchomieniu agent odczytał cały zapisany JSON; obie treści są
w pełnym eksporcie sesji. Końcowy raport wskazuje właściwy plik wejściowy,
a jego `input_sha256` zgadza się z sumą kontrolną aktualnego CSV. Wynik
to **8 wiadomości: 4 z etykietą `ham` i 4 z etykietą `spam`**. Program
zlicza istniejące etykiety; nie klasyfikuje sam wiadomości i nie trenuje modelu.

Plik podlinkowany na początku sprawozdania pochodzi z końcowego
`project/results/exercise-summary.json`. W kopii do publikacji zmieniono
wyłącznie ścieżki na jawnie opisane aliasy; liczebności, suma kontrolna wejścia
i wersja interpretera pozostały identyczne z wynikiem procesu.

## 3. Wyjaśniony fragment i przewidywanie

Wybrałem fragment `describe_data.py`, wiersz 29:

```python
counts[row["label"]] += 1
```

Moje wyjaśnienie: program wybiera licznik na podstawie pola `label`.
Długość tekstu ani wystąpienie w nim słowa „spam” nie wpływają na wynik.

Agent przedstawił zmieniony przypadek bez edycji plików projektu:

| Wiadomość | Treść | Etykieta |
| --- | --- | --- |
| A | Jeden wiersz zawierający słowo „spam” | `ham` |
| B | Pięć wierszy | `spam` |
| C | Dwa wiersze | `spam` |

Przed sprawdzeniem przewidziałem `total_messages = 3`, `ham = 1`, `spam = 2`.
Agent potwierdził wynik przez odczyt kodu: wiersz 15 używa `csv.DictReader`
do odczytu rekordów, wiersz 29 zwiększa licznik etykiety, a wiersz 37
sumuje oba liczniki. Przewidywanie było zgodne z kodem. Wniosek: jeden
rekord oznacza jedną wiadomość, nawet gdy jej tekst zajmuje wiele wierszy.

## 4. Gotowy test

Agent uruchomił z katalogu `project/`:

```powershell
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```

Pierwsza próba zakończyła się kodem `1`: proces nie miał uprawnień do zapisu
`summary.json` w domyślnym katalogu tymczasowym środowiska wykonania.
Agent ustawił `TEMP` i `TMP` wyłącznie w procesie terminala na
`project/results/test-temp` i ponowił ten sam test. Wynik: **`Ran 1 test`,
`OK`, kod zakończenia `0`**. Kod testu i jego oczekiwania pozostały bez zmian;
tymczasowy katalog po udanym teście został usunięty.

Test uruchamia skrypt na osobnym `tests/fixtures/messages.csv`, zawierającym
trzy wiadomości syntetyczne: dwie `ham` i jedną `spam`. Sprawdza powodzenie
procesu, zapis i odczyt JSON, właściwe wejście oraz oczekiwane liczebności.
Nie odczytuje ani nie nadpisuje raportu ćwiczeniowego. Wyjaśniłem w sesji,
że z tego powodu raport dla `data/exercise.csv` odczytaliśmy niezależnie.
Nieudana i udana próba pozostały w eksporcie sesji.

## 5. Zmiana etykiety i porównanie

Agent zmienił tylko pole `label` wiadomości `exercise-001`: **`ham` → `spam`**.
To celowa zmiana ćwiczeniowa. Pozostałe pola, wiadomości, kod, test i dane
testowe pozostały bez zmian, co sprawdzono względem oryginalnej paczki ZIP.

Po samej zmianie agent zatrzymał wykonanie: nie uruchomił skryptu ani testu
i zachował raport bez zmian. Następnie wyjaśniłem, że stary raport jest
nieaktualny, ponieważ nie odświeża się automatycznie po zmianie danych.
Przed ponownym wykonaniem przewidziałem **8 wiadomości, 4 `ham`, 4 `spam`**:
liczba wiadomości pozostaje taka sama, a jedna przechodzi z `ham` do `spam`.

Zleciłem ponowne uruchomienie skryptu dla tego samego CSV z zapisem do tego
samego JSON, a następnie odczyt i porównanie raportu. Uzasadniłem, że
uruchomienie utworzy aktualne podsumowanie, a odczyt pozwoli sprawdzić
właściwe wejście i uzyskane liczebności. Agent wykonał te operacje dopiero
po zapisaniu mojego przewidywania i zlecenia.

| Wartość | Pierwsze uruchomienie | Przewidywanie po zmianie | Końcowe uruchomienie |
| --- | ---: | ---: | ---: |
| `total_messages` | 8 | 8 | 8 |
| `ham` | 5 | 4 | 4 |
| `spam` | 3 | 4 | 4 |

Końcowy raport pochodzi z ponownego wywołania polecenia z punktu 2,
zakończonego kodem `0` po moim zleceniu. Wynik zgadza się z przewidywaniem.
Liczebności w JSON nie były poprawiane ręcznie; testu w etapie 2 nie ponawiano.
