# Aktualny stan projektu

## Co działa
- Statyczna aplikacja `index.html` uruchamia się lokalnie w przeglądarce.
- Użytkownik może tworzyć mapę myśli z tekstowej struktury z wcięciami.
- Mapa jest renderowana lokalnie bez backendu, sieci i zewnętrznych zależności.
- Poziomy mapy są wyróżniane stałymi kolorami kropek i połączeń, z cyklicznym powtarzaniem palety dla poziomów 7+.
- Eksport TXT zapisuje bieżącą treść edytora do `structuremind-map.txt`.
- Eksport PNG zapisuje pełną mapę do `structuremind-map.png` z tłem, marginesem `64px` i kolorami poziomów zgodnymi z widokiem.
- Puste stany eksportu TXT i PNG są blokowane komunikatem w UI.
- Aplikacja ma konfigurację publikacji na GitHub Pages pod adresem `https://growdelan.github.io/StructureMind/`.

## Co jest skończone
- Milestone 0.5: minimalny działający slice aplikacji.
- Milestone 1: eksport mapy do TXT i PNG.
- Milestone 2: kolorowanie poziomów mapy myśli.
- Publikacja GitHub Pages przez workflow `.github/workflows/deploy-pages.yml`.
- Tymczasowy smoke test `test.html` został usunięty po walidacji, bo nie jest już potrzebny w repo.
- Ostatnia walidacja przed usunięciem testu: headless Chromium/Playwright, `5/5 testów zakończonych powodzeniem`.
- Ostatnia walidacja Milestone 2: składnia skryptu, smoke test przeglądarkowy poziomów 0-9, przełączanie motywów, eksport PNG i brak błędów konsoli.
- Ostatnia poprawka eksportu PNG: osobne metryki Canvas dla węzłów zapobiegają nakładaniu etykiet i metadanych `Poziom X`.
- Ostatnia poprawka układu PNG: eksport przelicza pionowe pozycje węzłów na podstawie metryk Canvas, żeby większe węzły nie kolidowały przy rozbudowanych mapach.
- Self-review batcha Milestone 2: brak problemów krytycznych.

## Co jest w trakcie
- Brak aktywnej implementacji.

## Co jest następne
- Brak zaplanowanych milestone'ów. Najbliższy sensowny krok: wybrać kolejny milestone produktowy przed rozpoczęciem implementacji, np. import TXT, dodatkowe formaty eksportu albo konfigurację parametrów eksportu.

## Blokery i ryzyka
- Brak znanych blockerów krytycznych.
- Projekt pozostaje statyczny i bez zależności; nowe biblioteki lub narzędzia wymagają uzasadnienia w `spec.md`.
- Eksport PNG działa przez Canvas API przeglądarki, więc przy zmianach renderowania mapy warto odtworzyć adekwatną walidację przeglądarkową.
- Publikacja GitHub Pages działa z brancha `main`, zgodnie z regułami środowiska `github-pages`.

## Ostatnie aktualizacje
- 2026-05-10: doprecyzowano układ eksportu PNG, aby rozbudowane mapy nie rozsypywały się po zwiększeniu wysokości węzłów.
- 2026-05-10: naprawiono regresję eksportu PNG, w której etykieta węzła i metadane poziomu mogły nachodzić na siebie.
- 2026-05-10: zakończono Milestone 2, dodano kolorowanie kropek i połączeń poziomów oraz zgodność kolorów w eksporcie PNG.
- 2026-05-10: dodano publikację GitHub Pages przez GitHub Actions i zaktualizowano screenshot w README.
- 2026-05-10: usunięto tymczasowy plik `test.html`, który nie będzie dalej utrzymywany w repo.
- 2026-05-10: zakończono Milestone 1, dodano lokalny eksport TXT/PNG; końcowa walidacja nie wykazała problemów krytycznych.
