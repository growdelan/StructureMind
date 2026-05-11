# Roadmapa (milestones)

## Statusy milestone’ów
Dozwolone statusy:
- planned
- in_progress
- done
- blocked

---

## Milestone 0.5: Minimal end-to-end slice (done)

Cel:
- aplikacja uruchamia się
- wykonuje jedno bardzo proste zadanie
- zwraca poprawny wynik

Definition of Done:
- aplikację da się uruchomić jednym poleceniem (opisanym w README.md)
- istnieje co najmniej jeden smoke test
- testy przechodzą lokalnie
- brak placeholderów w kodzie

Zakres:
- minimalny entrypoint aplikacji
- minimalna logika domenowa
- minimalna obsługa IO (jeśli dotyczy)
- smoke test end-to-end

---

## Milestone 1: Eksport mapy do TXT i PNG (done)

Cel:
- umożliwić użytkownikowi lokalne zapisanie źródłowej struktury mapy jako TXT
- umożliwić użytkownikowi lokalne zapisanie pełnej mapy jako PNG
- zachować prosty, statyczny charakter aplikacji bez zewnętrznych zależności

Definition of Done:
- w UI są dostępne przyciski `Zapisz TXT` i `Zapisz PNG`
- `Zapisz TXT` pobiera plik `structuremind-map.txt` z dokładną treścią edytora
- pusty edytor blokuje eksport TXT i pokazuje komunikat w UI
- `Zapisz PNG` pobiera plik `structuremind-map.png` z pełną mapą
- PNG zawiera aktualny wygląd mapy, tło i margines `64px`
- pusta mapa blokuje eksport PNG i pokazuje komunikat w UI
- eksport działa lokalnie, bez sieci i bez dodatkowych zależności
- smoke testy dla eksportu TXT i PNG przechodzą lokalnie

Zakres:
- dodanie dwóch akcji eksportu do istniejącego UI
- lokalny eksport TXT bez normalizacji treści
- lokalny eksport PNG pełnej mapy niezależnie od aktualnego zoomu i przesunięcia widoku
- komunikaty sukcesu i błędów w istniejącym interfejsie
- smoke testy dla poprawnego eksportu i pustych stanów

Uwagi:
- poza zakresem są SVG, PDF, import TXT, wybór nazwy pliku i konfiguracja parametrów eksportu
- implementacja nie może dodawać zewnętrznych zależności

---

## Milestone 2: Kolorowanie poziomów mapy myśli (done)

Cel:
- poprawić czytelność mapy przez wizualne rozróżnienie poziomów hierarchii
- nadać kropkom poziomów stałe kolory zgodne z poziomem wcięcia
- nadać połączeniom kolor poziomu węzła docelowego
- zachować zgodność kolorów między widokiem aplikacji i eksportem PNG

Definition of Done:
- poziom 0 zachowuje turkusowy kolor `#22d3ee`
- poziomy 1-6 używają stałej palety kolorów z PRD `001-level-colors-prd.md`
- poziomy 7+ powtarzają paletę poziomów 1-6 cyklicznie
- kropka przy każdym węźle używa koloru poziomu tego węzła
- połączenie do każdego węzła używa koloru poziomu tego węzła
- eksport PNG zawiera te same kolory kropek i połączeń co widok mapy
- eksport TXT pozostaje bez zmian
- funkcja działa lokalnie, bez sieci i bez dodatkowych zależności
- smoke testy kolorów poziomów, motywów i eksportu PNG przechodzą lokalnie

Zakres:
- dodanie wspólnej reguły wyboru koloru poziomu
- kolorowanie kropek poziomów w widoku mapy
- kolorowanie połączeń SVG kolorem poziomu węzła docelowego
- odtworzenie tej samej reguły kolorów w eksporcie PNG
- walidacja działania w motywie jasnym, ciemnym i auto
- smoke test cyklicznego powtarzania palety dla poziomów głębszych niż 6

Uwagi:
- poza zakresem są edytor palety, presety palet, legenda kolorów, zmiana parsera wcięć oraz dodatkowe formaty eksportu
- implementacja nie może dodawać zewnętrznych zależności

---

## Milestone 3: Ukrywanie lewego panelu edytora (done)

Cel:
- umożliwić użytkownikowi schowanie panelu edytora, aby mapa mogła zająć całą dostępną przestrzeń
- umożliwić szybkie ponowne pokazanie panelu edytora bez utraty treści i rozmiaru panelu
- zachować prosty, statyczny charakter aplikacji bez zewnętrznych zależności

Definition of Done:
- w topbarze mapy jest dostępny przycisk `Ukryj edytor`, gdy panel jest widoczny
- kliknięcie `Ukryj edytor` całkowicie chowa panel edytora i uchwyt zmiany rozmiaru
- po ukryciu panelu mapa zajmuje całą dostępną przestrzeń aplikacji
- po ukryciu panelu przycisk zmienia etykietę na `Pokaż edytor`
- kliknięcie `Pokaż edytor` przywraca panel edytora i uchwyt zmiany rozmiaru
- treść edytora pozostaje bez zmian po schowaniu i ponownym pokazaniu panelu
- ostatni stan widoczności panelu jest zapamiętywany w `localStorage`
- wcześniej zapisany rozmiar panelu pozostaje zachowany po schowaniu i ponownym pokazaniu
- funkcja działa na desktopie i na wąskich ekranach
- eksport TXT/PNG, zoom, dopasowanie widoku, centrowanie i zmiana motywu pozostają bez regresji
- smoke testy ukrywania panelu, przywracania panelu, zapamiętania stanu, zachowania rozmiaru, mobile i regresji istniejących funkcji przechodzą lokalnie

Zakres:
- dodanie przełącznika widoczności panelu edytora do istniejącego topbara mapy
- pełne ukrywanie i pokazywanie panelu edytora razem z uchwytem zmiany rozmiaru
- rozszerzanie obszaru mapy po ukryciu panelu na desktopie i mobile
- zapamiętanie stanu widoczności panelu w `localStorage`
- zachowanie istniejącego zapisu szerokości lub wysokości panelu
- walidacja działania bez zmian w parserze, renderowaniu mapy oraz eksportach TXT/PNG

Uwagi:
- poza zakresem są skróty klawiaturowe, tryb overlay, zwężanie panelu do paska, animowane tryby panelu i nowe zależności
- implementacja nie może dodawać zewnętrznych zależności

---

## Milestone 4: Sprzątanie martwego kodu i komentarzy technicznych (done)

Cel:
- usunąć potwierdzone martwe elementy z `index.html`
- usunąć nieaktualne komentarze implementacyjne `FIX #1` i `FIX #2`
- zachować dotychczasowe zachowanie aplikacji bez zmian UI, eksportu i interakcji

Definition of Done:
- pola `interaction.nodeStartX` i `interaction.nodeStartY` są usunięte
- nieużywane zmienne CSS `--muted2`, `--accent`, `--accent2` i `--indent` są usunięte wraz z wariantami motywów
- element statusu nadal działa przez klasę `.status`, ale nie ma nieużywanego `id="status"`
- kod nie ustawia nieużywanego `data-id` dla węzłów mapy
- komentarze `FIX #1` i `FIX #2` są usunięte
- nie dodano nowych zależności ani nowych funkcji
- smoke test statyczny potwierdza brak usuwanych symboli w kodzie aplikacji
- smoke test przeglądarkowy potwierdza render mapy, ukrywanie panelu, zoom, dopasowanie, centrowanie oraz eksport TXT/PNG

Zakres:
- minimalne sprzątanie `index.html` zgodnie z PRD `003-code-cleanup-prd.md`
- weryfikacja braku referencji przed usunięciem elementów
- walidacja regresji podstawowych przepływów aplikacji

Uwagi:
- poza zakresem są refaktor architektury, podział pliku `index.html`, zmiany wizualne, zmiany parsera, zmiany eksportów oraz nowe zależności
