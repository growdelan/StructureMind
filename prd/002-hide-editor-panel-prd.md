# PRD: Ukrywanie lewego panelu edytora

## Kontekst
StructureMind pokazuje edytor struktury w lewym panelu oraz mapę myśli w głównym obszarze aplikacji. Panel edytora jest stale widoczny: użytkownik może zmieniać jego szerokość na desktopie albo wysokość na wąskich ekranach, ale nie może go całkowicie schować.

Przy pracy z większą mapą lub podczas przeglądania gotowej struktury stały panel ogranicza przestrzeń dostępną dla wizualizacji. Użytkownik potrzebuje prostego sposobu na tymczasowe ukrycie edytora i powrót do niego bez utraty wpisanej treści ani ustawionej szerokości panelu.

Nowa funkcjonalność ma dodać przycisk w górnym pasku mapy, który przełącza widoczność lewego panelu edytora.

## Cel
Celem funkcji jest zwiększenie przestrzeni roboczej dla mapy myśli wtedy, gdy użytkownik chce skupić się na podglądzie, prezentacji albo dopracowaniu układu mapy.

Użytkownik powinien móc jednym kliknięciem:
- schować panel edytora i uchwyt zmiany rozmiaru,
- oddać całą dostępną przestrzeń obszarowi mapy,
- ponownie pokazać panel edytora w poprzednim rozmiarze.

Funkcja ma działać lokalnie w przeglądarce, bez dodawania zależności i bez zmiany sposobu tworzenia, renderowania ani eksportowania mapy.

## Użytkownik docelowy
Użytkownikiem jest osoba tworząca lub przeglądająca mapę myśli w StructureMind, która chce:
- chwilowo skupić się na samej mapie,
- uzyskać więcej miejsca na rozbudowaną wizualizację,
- ukryć edytor podczas prezentowania mapy,
- szybko wrócić do edycji bez przeładowywania aplikacji.

## Zakres funkcjonalny

### Przełączanie widoczności panelu
Użytkownik może kliknąć przycisk w górnym pasku mapy, aby schować albo ponownie pokazać panel edytora.

Wymagania:
- przycisk znajduje się w istniejącym topbarze mapy, obok obecnych kontrolek,
- gdy panel jest widoczny, przycisk ma etykietę `Ukryj edytor`,
- gdy panel jest schowany, przycisk ma etykietę `Pokaż edytor`,
- kliknięcie `Ukryj edytor` całkowicie chowa panel edytora,
- kliknięcie `Pokaż edytor` przywraca panel edytora,
- wpisana treść edytora nie jest czyszczona ani modyfikowana przy chowaniu i pokazywaniu panelu.

### Zachowanie układu mapy
Po schowaniu panelu obszar mapy ma zająć całą dostępną przestrzeń aplikacji.

Wymagania:
- na desktopie znika lewy panel oraz pionowy uchwyt zmiany szerokości,
- na wąskich ekranach znika górny panel oraz poziomy uchwyt zmiany wysokości,
- główny obszar mapy rozszerza się na wolne miejsce,
- istniejące kontrolki mapy pozostają dostępne,
- zoom, przesuwanie, dopasowanie widoku i centrowanie działają tak jak wcześniej.

### Zapamiętanie stanu
Aplikacja ma zapamiętywać ostatni stan widoczności panelu.

Wymagania:
- jeśli użytkownik schowa panel, po odświeżeniu strony panel pozostaje schowany,
- jeśli użytkownik pokaże panel, po odświeżeniu strony panel pozostaje widoczny,
- zapamiętanie stanu korzysta z `localStorage`,
- istniejący zapis szerokości panelu pozostaje niezależny od stanu widoczności,
- po ponownym pokazaniu panelu zachowana wcześniej szerokość lub wysokość panelu nie ginie.

### Responsywne działanie
Funkcja ma działać spójnie na desktopie i na wąskich ekranach.

Wymagania:
- na desktopie panel jest częścią poziomego układu aplikacji i chowa się z lewej strony,
- na mobile panel jest częścią pionowego układu aplikacji i chowa się z góry,
- ten sam przycisk w topbarze obsługuje oba układy,
- aplikacja nie pokazuje osobnego mobilnego trybu panelu ani nakładki.

## Poza zakresem
Ta funkcja nie obejmuje:
- skrótu klawiaturowego do przełączania panelu,
- osobnego trybu overlay dla panelu,
- zwężania panelu do paska lub ikony,
- animowanych albo zaawansowanych trybów panelu,
- zmiany treści, parsera lub zasad wcięć edytora,
- zmian w renderowaniu węzłów i połączeń mapy,
- zmian w eksporcie TXT,
- zmian w eksporcie PNG,
- nowych ustawień użytkownika poza zapamiętaniem widoczności panelu,
- dodawania zewnętrznych bibliotek.

## Wymagania techniczne
- Implementacja ma pozostać w czystym HTML, CSS i JavaScript.
- Nie dodawać zależności front-endowych ani narzędzi deweloperskich.
- Funkcja ma działać bez sieci i bez zewnętrznych usług.
- Dane mapy nie mogą opuszczać przeglądarki użytkownika.
- Głównym entrypointem pozostaje `index.html`.
- Implementacja powinna używać istniejącego układu `sidebar`, `resizer` i `main`.
- Stan widoczności panelu powinien być przechowywany w `localStorage`, zgodnie z obecnym wzorcem zapisu ustawień aplikacji.
- Istniejący zapis szerokości panelu powinien dalej działać po ukryciu i ponownym pokazaniu panelu.

## Kryteria akceptacji
- Użytkownik widzi przycisk `Ukryj edytor`, gdy panel edytora jest widoczny.
- Kliknięcie `Ukryj edytor` chowa cały panel edytora.
- Kliknięcie `Ukryj edytor` chowa uchwyt zmiany szerokości lub wysokości panelu.
- Po schowaniu panelu mapa zajmuje całą dostępną przestrzeń aplikacji.
- Po schowaniu panelu przycisk zmienia etykietę na `Pokaż edytor`.
- Kliknięcie `Pokaż edytor` przywraca panel edytora i uchwyt zmiany rozmiaru.
- Treść edytora pozostaje bez zmian po schowaniu i ponownym pokazaniu panelu.
- Ostatni stan widoczności panelu jest zachowany po odświeżeniu strony.
- Zapisana szerokość lub wysokość panelu pozostaje zachowana po schowaniu i ponownym pokazaniu panelu.
- Funkcja działa na desktopie i na wąskich ekranach.
- Eksport TXT i PNG działają tak jak wcześniej.
- Zoom, dopasowanie widoku, centrowanie mapy i zmiana motywu działają tak jak wcześniej.
- Aplikacja nadal działa lokalnie i nie wymaga internetu.

## Smoke testy

### Smoke test ukrywania panelu
1. Uruchomić aplikację lokalnie.
2. Sprawdzić, że panel edytora jest widoczny.
3. Kliknąć `Ukryj edytor`.
4. Sprawdzić, że panel edytora zniknął.
5. Sprawdzić, że uchwyt zmiany rozmiaru panelu zniknął.
6. Sprawdzić, że mapa zajmuje wolną przestrzeń po panelu.

### Smoke test przywracania panelu
1. Schować panel edytora.
2. Sprawdzić, że przycisk ma etykietę `Pokaż edytor`.
3. Kliknąć `Pokaż edytor`.
4. Sprawdzić, że panel edytora wrócił.
5. Sprawdzić, że uchwyt zmiany rozmiaru panelu wrócił.
6. Sprawdzić, że treść edytora nie została zmieniona.

### Smoke test zapamiętania stanu
1. Schować panel edytora.
2. Odświeżyć stronę.
3. Sprawdzić, że panel pozostaje schowany.
4. Kliknąć `Pokaż edytor`.
5. Odświeżyć stronę.
6. Sprawdzić, że panel pozostaje widoczny.

### Smoke test zachowania rozmiaru panelu
1. Zmienić szerokość panelu na desktopie albo wysokość panelu na wąskim ekranie.
2. Schować panel edytora.
3. Ponownie pokazać panel edytora.
4. Sprawdzić, że panel wraca w zapamiętanym rozmiarze.

### Smoke test mobile
1. Uruchomić aplikację w wąskim viewportcie.
2. Sprawdzić, że panel edytora znajduje się nad mapą.
3. Kliknąć `Ukryj edytor`.
4. Sprawdzić, że panel i poziomy uchwyt zmiany wysokości zniknęły.
5. Sprawdzić, że mapa zajmuje pełną dostępną wysokość.
6. Kliknąć `Pokaż edytor` i sprawdzić, że panel wrócił.

### Smoke test regresji istniejących funkcji
1. Wpisać strukturę mapy z kilkoma poziomami.
2. Schować panel i pokazać go ponownie.
3. Sprawdzić, że mapa nadal renderuje się poprawnie.
4. Sprawdzić działanie zoomu, dopasowania widoku, centrowania i zmiany motywu.
5. Kliknąć `Zapisz TXT` i sprawdzić, że eksport działa jak wcześniej.
6. Kliknąć `Zapisz PNG` i sprawdzić, że eksport działa jak wcześniej.

## Otwarte kwestie
Brak. Zakres funkcji został ustalony:
- panel edytora ma być chowany całkowicie,
- uchwyt zmiany rozmiaru znika razem z panelem,
- mapa zajmuje całą dostępną przestrzeń,
- przycisk znajduje się w topbarze mapy,
- etykieta przycisku zmienia się między `Ukryj edytor` i `Pokaż edytor`,
- stan widoczności panelu jest zapamiętywany w `localStorage`,
- funkcja działa tak samo na desktopie i mobile,
- skrót klawiaturowy jest poza zakresem,
- implementacja bez nowych zależności.
