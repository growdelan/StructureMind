# PRD: Sprzątanie martwego kodu i komentarzy technicznych

## Kontekst
StructureMind jest jednoplikową aplikacją statyczną, która rozwijała się iteracyjnie przez kolejne milestone'y. Po przeglądzie kodu wykryto kilka potwierdzonych pozostałości technicznych w `index.html`: nieużywane pola stanu, nieużywane zmienne CSS, nieużywany identyfikator DOM, nieużywany atrybut dynamiczny oraz komentarze implementacyjne `FIX #1` / `FIX #2`.

Te elementy nie zmieniają działania aplikacji, ale utrudniają czytanie kodu i mogą sugerować, że istnieje dodatkowe zachowanie albo zależność, której w rzeczywistości nie ma.

## Cel
Celem zmiany jest uporządkowanie kodu bez zmiany zachowania aplikacji.

Po sprzątaniu aplikacja ma dalej działać tak samo dla użytkownika: renderowanie mapy, ukrywanie panelu, eksport TXT/PNG, zoom, centrowanie, dopasowanie widoku i motywy pozostają bez zmian.

## Użytkownik docelowy
Bezpośrednim odbiorcą zmiany jest osoba utrzymująca kod StructureMind, która chce:
- szybciej rozumieć aktualny stan implementacji,
- unikać mylących pozostałości po poprzednich poprawkach,
- mieć pewność, że kod nie zawiera znanych, potwierdzonych martwych fragmentów.

## Zakres funkcjonalny

### Usunięcie martwych elementów kodu
Z `index.html` należy usunąć tylko elementy potwierdzone jako nieużywane:
- pola `interaction.nodeStartX` i `interaction.nodeStartY`,
- zmienne CSS `--muted2`, `--accent`, `--accent2` i `--indent` wraz z wariantami motywów,
- identyfikator `id="status"` z elementu statusu, przy zachowaniu klasy `.status`,
- przypisanie `el.dataset.id = String(node.id)`, jeśli przed implementacją nadal nie ma odwołania do `data-id`.

### Usunięcie nieaktualnych komentarzy technicznych
Należy usunąć komentarze `FIX #1` i `FIX #2`, które opisują historyczne poprawki, a nie aktualną logikę domenową.

Sprzątanie komentarzy nie może usuwać komentarzy, które nadal wyjaśniają nieoczywistą logikę aplikacji.

### Zachowanie działania aplikacji
Zmiana nie może wprowadzać nowych funkcji ani zmieniać istniejących przepływów użytkownika.

Wymagania:
- struktura UI pozostaje taka sama,
- eksport TXT i PNG działają tak jak wcześniej,
- ukrywanie i przywracanie panelu działa tak jak wcześniej,
- kolorowanie poziomów pozostaje bez zmian,
- zapis w `localStorage` i `sessionStorage` pozostaje bez zmian,
- aplikacja nie dostaje nowych zależności.

## Poza zakresem
Ta zmiana nie obejmuje:
- refaktoru architektury,
- podziału `index.html` na osobne pliki,
- zmian wizualnych,
- zmian parsera mapy,
- zmian eksportu TXT lub PNG,
- zmian nazw kluczy `localStorage` albo `sessionStorage`,
- nowych testów frameworkowych,
- dodawania bibliotek lub narzędzi.

## Wymagania techniczne
- Implementacja ma pozostać w czystym HTML, CSS i JavaScript.
- Głównym entrypointem pozostaje `index.html`.
- Przed usunięciem każdego elementu należy potwierdzić, że nie ma realnego odwołania w kodzie.
- Sprzątanie ma być minimalne i ograniczone do elementów wskazanych w tym PRD.
- Brak zmian zachowania musi być potwierdzony smoke testem przeglądarkowym.

## Kryteria akceptacji
- `index.html` nie zawiera pól `nodeStartX` ani `nodeStartY`.
- `index.html` nie zawiera nieużywanych zmiennych CSS `--muted2`, `--accent`, `--accent2` ani `--indent`.
- Element statusu nadal ma klasę `.status`, ale nie ma nieużywanego `id="status"`.
- Kod nie ustawia nieużywanego `data-id` dla węzłów mapy.
- Komentarze `FIX #1` i `FIX #2` zostały usunięte.
- Renderowanie mapy działa poprawnie.
- Ukrywanie i ponowne pokazywanie panelu edytora działa poprawnie.
- Zoom, dopasowanie widoku i centrowanie działają poprawnie.
- Eksport TXT i PNG działają poprawnie.
- W konsoli przeglądarki nie pojawiają się błędy po uruchomieniu aplikacji.

## Smoke testy

### Smoke test statyczny
1. Sprawdzić, że w kodzie aplikacji nie ma pozostałości po usuwanych elementach:
   - `nodeStartX`
   - `nodeStartY`
   - `--muted2`
   - `--accent`
   - `--accent2`
   - `--indent`
   - `id="status"`
   - `dataset.id`
   - `FIX #`
2. Potwierdzić, że ewentualne trafienia w dokumentacji nie są realnymi pozostałościami w kodzie aplikacji.

### Smoke test przeglądarkowy
1. Uruchomić aplikację lokalnie.
2. Sprawdzić, że mapa renderuje przykładową strukturę.
3. Kliknąć `Ukryj edytor` i sprawdzić, że panel znika.
4. Kliknąć `Pokaż edytor` i sprawdzić, że panel wraca.
5. Sprawdzić zoom, dopasowanie widoku i centrowanie.
6. Sprawdzić eksport TXT.
7. Sprawdzić eksport PNG.
8. Potwierdzić brak błędów konsoli.

## Otwarte kwestie
Brak. Zakres jest techniczny i ograniczony do potwierdzonych elementów sprzątania.
