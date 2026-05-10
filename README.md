# StructureMind
Aplikacja do tworzenia map myśli w oparciu o struktury tekstowe, mieszcząca się w jednym pliku HTML. Do działania wymaga wyłącznie przeglądarki.

<div align="center">
  <img src="img/img1.png" alt="alt text" width="820">
</div>

## Uruchamianie

Projekt jest statyczny. Można uruchomić go bezpośrednio w przeglądarce przez `index.html` albo przez lokalny serwer:

```bash
python3 -m http.server 8000
```

Po uruchomieniu serwera aplikacja jest dostępna pod adresem `http://localhost:8000/index.html`.

## Smoke testy

Smoke test eksportu TXT/PNG znajduje się w `test.html`. Najprostsze uruchomienie:

```bash
python3 -m http.server 8000
```

Następnie otwórz `http://localhost:8000/test.html` w przeglądarce. Oczekiwany wynik po poprawnym przebiegu to `5/5 testów zakończonych powodzeniem`.
