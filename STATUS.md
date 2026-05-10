# Aktualny stan projektu

## Co działa
- Statyczna aplikacja `index.html` uruchamia się lokalnie w przeglądarce.
- Użytkownik może tworzyć mapę myśli z tekstowej struktury z wcięciami.
- Mapa jest renderowana lokalnie bez backendu, sieci i zewnętrznych zależności.
- Eksport TXT zapisuje bieżącą treść edytora do `structuremind-map.txt`.
- Eksport PNG zapisuje pełną mapę do `structuremind-map.png` z tłem i marginesem `64px`.
- Puste stany eksportu TXT i PNG są blokowane komunikatem w UI.

## Co jest skończone
- Milestone 0.5: minimalny działający slice aplikacji.
- Milestone 1: eksport mapy do TXT i PNG.
- Tymczasowy smoke test `test.html` został usunięty po walidacji, bo nie jest już potrzebny w repo.
- Ostatnia walidacja przed usunięciem testu: headless Chromium/Playwright, `5/5 testów zakończonych powodzeniem`.
- Self-review batcha: brak problemów krytycznych.

## Co jest w trakcie
- Brak aktywnej implementacji.

## Co jest następne
- Najbliższy sensowny krok: wybrać kolejny milestone produktowy przed rozpoczęciem implementacji, np. import TXT, dodatkowe formaty eksportu albo konfigurację parametrów eksportu.

## Blokery i ryzyka
- Brak znanych blockerów krytycznych.
- Projekt pozostaje statyczny i bez zależności; nowe biblioteki lub narzędzia wymagają uzasadnienia w `spec.md`.
- Eksport PNG działa przez Canvas API przeglądarki, więc przy zmianach renderowania mapy warto odtworzyć adekwatną walidację przeglądarkową.

## Ostatnie aktualizacje
- 2026-05-10: usunięto tymczasowy plik `test.html`, który nie będzie dalej utrzymywany w repo.
- 2026-05-10: zakończono Milestone 1, dodano lokalny eksport TXT/PNG; końcowa walidacja nie wykazała problemów krytycznych.
