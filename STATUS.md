# Aktualny stan projektu

## Co działa
- Statyczna aplikacja `index.html` uruchamia się lokalnie w przeglądarce.
- Użytkownik może tworzyć mapę myśli z tekstowej struktury z wcięciami.
- Mapa jest renderowana lokalnie bez backendu, sieci i zewnętrznych zależności.
- Panel edytora można ukryć i ponownie pokazać z topbara mapy; stan widoczności oraz rozmiar panelu są zapamiętywane lokalnie.
- Poziomy mapy są wyróżniane stałymi kolorami kropek i połączeń, z cyklicznym powtarzaniem palety dla poziomów 7+.
- Mapa aktualizuje się stabilnie podczas edycji tekstu: dopisywanie, usuwanie, wcinanie i wklejanie węzłów nie czyści całego widoku ani nie resetuje kamery.
- Ręcznie przesunięte węzły zachowują pozycje po edycji tekstu, jeśli nadal istnieją w strukturze.
- Eksport TXT zapisuje bieżącą treść edytora do `structuremind-map.txt`.
- Eksport PNG zapisuje pełną mapę do `structuremind-map.png` z tłem, marginesem `64px` i kolorami poziomów zgodnymi z widokiem.
- Puste stany eksportu TXT i PNG są blokowane komunikatem w UI.
- Kod `index.html` został przeczyszczony z potwierdzonych martwych elementów i nieaktualnych komentarzy technicznych `FIX #1` / `FIX #2`.
- Aplikacja ma konfigurację publikacji na GitHub Pages pod adresem `https://growdelan.github.io/StructureMind/`.

## Co jest skończone
- Milestone 0.5: minimalny działający slice aplikacji.
- Milestone 1: eksport mapy do TXT i PNG.
- Milestone 2: kolorowanie poziomów mapy myśli.
- Milestone 3: ukrywanie lewego panelu edytora.
- Milestone 4: sprzątanie martwego kodu i komentarzy technicznych.
- Milestone 5: stabilne renderowanie mapy podczas edycji.
- Ostatnia walidacja Milestone 5: składnia skryptu, smoke test przeglądarkowy desktop/mobile dla dopisywania węzłów, ukrywanie i przywracanie panelu, zoom, dopasowanie, centrowanie oraz eksport TXT/PNG.
- Self-review batcha Milestone 5: wykryto i poprawiono jedną drobną regresję wcięcia ostatniej linii przykładu; po poprawce rewalidacja przeszła.
- Ostatnia walidacja Milestone 4: statyczny check braku usuwanych symboli w `index.html` oraz headless Chromium/Playwright dla renderowania mapy, ukrywania i przywracania panelu, zoomu, dopasowania, centrowania, eksportu TXT/PNG i braku błędów konsoli.
- Self-review batcha Milestone 4: brak problemów krytycznych.
- Publikacja GitHub Pages przez workflow `.github/workflows/deploy-pages.yml`.
- Ostatnia walidacja Milestone 3: składnia skryptu, headless Chromium/Playwright dla desktop/mobile, ukrywanie i przywracanie panelu, zapamiętanie stanu, zachowanie szerokości, eksport TXT/PNG, zoom, dopasowanie, centrowanie, motyw i brak błędów konsoli.
- Self-review batcha Milestone 3: wykryto i poprawiono jedną drobną kwestię jakościową dotyczącą komunikatu statusu przy inicjalizacji ukrytego panelu; po poprawce rewalidacja przeszła.
- Tymczasowy smoke test `test.html` został usunięty po walidacji, bo nie jest już potrzebny w repo.
- Ostatnia walidacja przed usunięciem testu: headless Chromium/Playwright, `5/5 testów zakończonych powodzeniem`.
- Ostatnia walidacja Milestone 2: składnia skryptu, smoke test przeglądarkowy poziomów 0-9, przełączanie motywów, eksport PNG i brak błędów konsoli.
- Ostatnia poprawka eksportu PNG: osobne metryki Canvas dla węzłów zapobiegają nakładaniu etykiet i metadanych `Poziom X`.
- Ostatnia poprawka układu PNG: eksport przelicza pionowe pozycje węzłów na podstawie metryk Canvas, żeby większe węzły nie kolidowały przy rozbudowanych mapach.
- Ostatnia poprawka przepływu PNG: przycisk `Zapisz PNG` wykonuje pełny reload i automatycznie eksportuje mapę po ponownym renderze.
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
- 2026-05-12: zakończono Milestone 5, dodano PRD `004-stable-editing-render-prd.md`, zaktualizowano dokumentację i ustabilizowano renderowanie mapy podczas edycji tekstu.
- 2026-05-11: zakończono Milestone 4, dodano PRD `003-code-cleanup-prd.md`, zaktualizowano dokumentację i przeczyszczono potwierdzone martwe elementy oraz komentarze techniczne z `index.html`.
- 2026-05-11: zakończono Milestone 3, dodano ukrywanie i przywracanie panelu edytora z zapamiętaniem stanu oraz rozmiaru panelu.
- 2026-05-11: dodano PRD `002-hide-editor-panel-prd.md` oraz zaktualizowano `spec.md` i `ROADMAP.md` dla ukrywania panelu edytora.
- 2026-05-10: dodano pełne odświeżenie strony przed eksportem PNG oraz auto-eksport po ponownym renderze.
- 2026-05-10: doprecyzowano układ eksportu PNG, aby rozbudowane mapy nie rozsypywały się po zwiększeniu wysokości węzłów.
- 2026-05-10: naprawiono regresję eksportu PNG, w której etykieta węzła i metadane poziomu mogły nachodzić na siebie.
- 2026-05-10: zakończono Milestone 2, dodano kolorowanie kropek i połączeń poziomów oraz zgodność kolorów w eksporcie PNG.
- 2026-05-10: dodano publikację GitHub Pages przez GitHub Actions i zaktualizowano screenshot w README.
- 2026-05-10: usunięto tymczasowy plik `test.html`, który nie będzie dalej utrzymywany w repo.
- 2026-05-10: zakończono Milestone 1, dodano lokalny eksport TXT/PNG; końcowa walidacja nie wykazała problemów krytycznych.
