# Dane

Źródłem wiadomości jest [publiczny korpus Apache SpamAssassin](https://spamassassin.apache.org/old/publiccorpus/):
archiwa `20030228_easy_ham.tar.bz2` i `20030228_spam.tar.bz2`.
Pliki CSV ćwiczenia nie są obecnie dołączone do repozytorium.
Poniższy opis określa dane potrzebne do odtworzenia raportów.

- `message_id`: identyfikator wiadomości.
- `text`: odkodowany temat i pełna treść wiadomości.
- `label`: etykieta `ham` lub `spam`.

`exercise-original.csv` jest identyczną kopią wejściowego CSV z paczki zadania.
W `exercise.csv` zmieniono tylko etykietę `exercise-001` z `ham` na `spam`.
Teksty wiadomości oraz pozostałe pola pozostały bez zmian.

Wiadomości są historycznymi przykładami z 2002 roku. Nazwiska, podpisy i adresy
w ich treści należą do źródłowego korpusu, a nie do osoby wykonującej ćwiczenie.
To mała próbka edukacyjna; nie jest reprezentatywnym zbiorem do oceny filtra.

Zgodnie z [opisem korpusu](https://spamassassin.apache.org/old/publiccorpus/readme.html)
prawa do treści pozostają przy autorach wiadomości. Licencja oprogramowania
SpamAssassin nie stanowi licencji tych wiadomości. Pliki czytaj jako tekst;
nie wysyłaj wiadomości i nie otwieraj zawartych w nich linków.
