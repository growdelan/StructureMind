# AGENTS.md

Ten plik jest wyłącznie operacyjny: jak uruchamiać, testować i zarządzać zależnościami.
Decyzje produktowe/architektura: `spec.md` / `ROADMAP.md` / `STATUS.md`.

## Język
- Komunikacja wyłącznie po polsku.

## Środowisko i zależności
- Projekt jest statyczny i działa w przeglądarce, więc domyślnie nie wymaga dodatkowych zależności.
- Jeśli trzeba dodać bibliotekę front-endową lub narzędzie deweloperskie, uzasadnij to w `spec.md` w sekcji „Decyzje techniczne”.
- Nie dodawaj zależności „na zapas”; preferuj czysty HTML, CSS i JavaScript.

## Uruchamianie
- Główny entrypoint to `index.html`.
- Projekt uruchamiaj przez lokalny serwer statyczny albo bezpośrednio w przeglądarce, jeśli to nie psuje zachowania aplikacji.
- Komenda uruchomienia musi być opisana w `README.md`.

## Testy
- Preferuj smoke testy w przeglądarce dla `index.html` i `test.html`.
- Testy nie powinny zależeć od sieci ani zewnętrznych usług.
- Jeśli pojawią się automatyczne testy, trzymaj je w `tests/` lub jako proste pliki pomocnicze HTML/JS, zależnie od potrzeb.

## Konwencje repo
- Główna aplikacja może żyć w root jako pojedynczy plik HTML, bo taki jest charakter projektu.
- Jeśli rozdzielasz kod, trzymaj zasoby logicznie: HTML w root, a pomocnicze pliki JS/CSS w prostych katalogach tylko wtedy, gdy realnie poprawia to utrzymanie.
- Jeden główny entrypoint, opisany w `README.md`.

## Sekrety i konfiguracja
- Nie przechowuj sekretów w repo.
- Jeśli projekt kiedykolwiek zacznie korzystać z konfiguracji, dokumentuj ją w `README.md`.
- Bez ukrytych kluczy, sekretów i fallbacków.

## Styl
- Indentacja: 2 lub 4 spacje, ale zachowuj spójność w obrębie pliku.
- Nazwy: funkcje i zmienne `camelCase` albo `snake_case` konsekwentnie w obrębie projektu; klasy `PascalCase`.
- Kod prosty, bez „magii”; komentarze tylko przy nieoczywistej logice.
