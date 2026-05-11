# Specyfikacja techniczna

## Cel
StructureMind to statyczna aplikacja przeglądarkowa do tworzenia map myśli na podstawie tekstowej struktury wpisanej przez użytkownika. Aplikacja rozwiązuje problem szybkiego przekształcania hierarchicznego tekstu w czytelną mapę wizualną oraz zachowania tej mapy poza aplikacją.

Projekt jest przeznaczony dla osób, które chcą szybko zaplanować, uporządkować albo udostępnić strukturę informacji bez konta, serwera i konfiguracji.

Zakres aplikacji obejmuje:
- wpisywanie i edycję tekstowej struktury mapy,
- wizualizację pełnej mapy myśli,
- ukrywanie i ponowne pokazywanie panelu edytora,
- kolorowanie poziomów hierarchii w mapie myśli,
- lokalny zapis źródłowej struktury jako pliku TXT,
- lokalny zapis pełnej mapy jako pliku PNG,
- utrzymanie kodu aplikacji bez potwierdzonych martwych elementów i nieaktualnych komentarzy technicznych.

Poza zakresem są eksport SVG/PDF, import plików, zapis do chmury, wybór nazwy pliku oraz zewnętrzne usługi lub zależności.

---

## Zakres funkcjonalny (high-level)
Kluczowe use-case’y:
- użytkownik wpisuje strukturę tekstową z wcięciami i widzi odpowiadającą jej mapę myśli,
- użytkownik szybciej rozpoznaje poziomy zagnieżdżenia dzięki kolorom kropek i połączeń,
- użytkownik dopracowuje wygląd i układ mapy w aktualnym interfejsie,
- użytkownik ukrywa panel edytora, aby skupić się na mapie i wykorzystać pełną przestrzeń aplikacji,
- użytkownik zapisuje dokładną treść edytora do pliku `structuremind-map.txt`,
- użytkownik zapisuje pełną mapę do pliku `structuremind-map.png`.

Główne przepływy użytkownika:
- wpisanie lub zmiana tekstu w edytorze aktualizuje mapę,
- każdy poziom wcięcia otrzymuje spójny kolor kropki i połączenia prowadzącego do węzła,
- kliknięcie `Ukryj edytor` chowa panel edytora i uchwyt zmiany rozmiaru, a mapa zajmuje dostępną przestrzeń,
- kliknięcie `Pokaż edytor` przywraca panel edytora w poprzednim rozmiarze,
- kliknięcie `Zapisz TXT` pobiera plik tekstowy, jeśli edytor nie jest pusty,
- kliknięcie `Zapisz PNG` pobiera obraz pełnej mapy, jeśli mapa nie jest pusta,
- przy pustym edytorze albo pustej mapie aplikacja blokuje eksport i pokazuje krótki komunikat w UI.

Aplikacja nie wykonuje normalizacji treści TXT, nie wysyła danych poza przeglądarkę i nie oferuje konfiguracji parametrów eksportu, palety kolorów poziomów ani zaawansowanych trybów panelu edytora.

---

## Architektura i przepływ danych
Architektura pozostaje statyczna i lokalna. Głównym entrypointem jest `index.html`, który zawiera interfejs, style oraz logikę działania aplikacji.

1. Główne komponenty systemu
   - edytor tekstowy struktury mapy,
   - renderer mapy myśli,
   - reguła kolorowania poziomów hierarchii,
   - kontrola widoczności panelu edytora,
   - warstwa interakcji użytkownika,
   - mechanizm lokalnego eksportu TXT,
   - mechanizm lokalnego eksportu PNG,
   - warstwa komunikatów statusu.

2. Przepływ danych między komponentami
   - użytkownik wpisuje tekst w edytorze,
   - aplikacja interpretuje tekst jako hierarchię węzłów,
   - renderer prezentuje mapę w widoku i przypisuje kolory na podstawie poziomu węzła,
   - użytkownik może przełączyć widoczność panelu edytora bez zmiany treści mapy,
   - eksport TXT korzysta bezpośrednio z bieżącej wartości edytora,
   - eksport PNG korzysta z aktualnie wyrenderowanej pełnej mapy, jej wyglądu i reguły kolorowania poziomów,
   - komunikaty statusu informują o sukcesie albo blokadzie eksportu.

3. Granice odpowiedzialności
   - edytor odpowiada za źródłową strukturę tekstową,
   - renderer odpowiada za wizualną reprezentację mapy, w tym kolory kropek i połączeń poziomów,
   - kontrola widoczności panelu odpowiada za ukrycie lub pokazanie edytora i uchwytu zmiany rozmiaru bez modyfikowania danych mapy,
   - eksport TXT odpowiada za pobranie dokładnej treści edytora bez modyfikacji,
   - eksport PNG odpowiada za obraz pełnej mapy z tłem, marginesem i kolorami poziomów zgodnymi z widokiem,
   - aplikacja nie odpowiada za przechowywanie plików po stronie serwera ani synchronizację danych.

---

## Komponenty techniczne
- `index.html`: główny plik aplikacji, entrypoint uruchamiany w przeglądarce.
- Edytor struktury: przechowuje bieżącą treść mapy i stan wejściowy dla renderowania oraz eksportu TXT.
- Parser struktury: interpretuje tekst z wcięciami jako hierarchię węzłów mapy.
- Renderer mapy: tworzy wizualną mapę z węzłami, połączeniami, tłem, aktualnym motywem i kolorami poziomów.
- Reguła kolorów poziomów: przypisuje poziomowi 0 kolor turkusowy, poziomom 1-6 stałą paletę kolorów, a poziomom 7+ kolory powtarzane cyklicznie od poziomu 1.
- Kontrolki UI: obsługują akcje użytkownika, w tym przyciski `Ukryj edytor` / `Pokaż edytor`, `Zapisz TXT` i `Zapisz PNG`.
- Kontrola widoczności panelu: chowa lub pokazuje panel edytora razem z uchwytem zmiany rozmiaru oraz zapamiętuje ostatni stan w `localStorage`.
- Eksporter TXT: generuje lokalny plik `structuremind-map.txt` z dokładną treścią edytora.
- Eksporter PNG: generuje lokalny plik `structuremind-map.png` obejmujący pełną mapę z marginesem `64px` i kolorowaniem poziomów zgodnym z widokiem.
- Komunikaty statusu: pokazują krótkie informacje o sukcesie lub zablokowanym eksporcie.

---

## Decyzje techniczne
- Decyzja: aplikacja pozostaje statyczna i działa w czystym HTML, CSS i JavaScript.
- Uzasadnienie: PRD wymaga działania lokalnego, bez internetu, bez zewnętrznych usług i bez dodatkowych zależności.
- Konsekwencje: eksport musi korzystać z możliwości przeglądarki i nie może opierać się na bibliotekach ani backendzie.

- Decyzja: eksport TXT zapisuje dokładną wartość edytora bez normalizacji.
- Uzasadnienie: użytkownik oczekuje zachowania struktury dokładnie tak, jak została wpisana.
- Konsekwencje: aplikacja nie poprawia automatycznie wcięć, pustych linii ani znaków listy przy eksporcie.

- Decyzja: eksport PNG zachowuje aktualny wygląd UI i obejmuje pełną mapę.
- Uzasadnienie: obraz ma odzwierciedlać mapę dopracowaną przez użytkownika, niezależnie od aktualnego viewportu.
- Konsekwencje: mechanizm eksportu musi uwzględniać pełne granice mapy, tło, motyw, połączenia, węzły i margines.

- Decyzja: publiczna wersja aplikacji jest publikowana przez GitHub Pages z użyciem GitHub Actions.
- Uzasadnienie: aplikacja jest statyczna, więc może być hostowana bez backendu i bez dodatkowych zależności runtime.
- Konsekwencje: publikacja zależy od workflow `.github/workflows/deploy-pages.yml`, a lokalne działanie aplikacji pozostaje bez zmian.

- Decyzja (dotyczy PRD: 001-level-colors-prd.md): poziomy mapy są kolorowane stałą paletą bez konfiguracji użytkownika.
- Uzasadnienie: kolor ma poprawiać szybkie skanowanie hierarchii bez rozbudowy interfejsu i bez dodatkowego stanu aplikacji.
- Konsekwencje: poziom 0 pozostaje turkusowy, poziomy 1-6 mają stałe kolory, a poziomy 7+ powtarzają paletę cyklicznie.

- Decyzja (dotyczy PRD: 001-level-colors-prd.md): połączenie prowadzące do węzła używa koloru poziomu tego węzła.
- Uzasadnienie: linia i kropka węzła powinny wspólnie komunikować ten sam poziom hierarchii.
- Konsekwencje: renderer DOM/SVG i eksporter PNG muszą korzystać z tej samej reguły wyboru koloru poziomu.

- Decyzja (dotyczy PRD: 002-hide-editor-panel-prd.md): panel edytora jest chowany całkowicie razem z uchwytem zmiany rozmiaru, a przełącznik pozostaje w topbarze mapy.
- Uzasadnienie: użytkownik ma odzyskać pełną przestrzeń dla mapy i nadal mieć dostęp do przywrócenia edytora.
- Konsekwencje: układ musi obsługiwać stany `Ukryj edytor` i `Pokaż edytor` na desktopie oraz mobile bez zmiany danych mapy.

- Decyzja (dotyczy PRD: 002-hide-editor-panel-prd.md): stan widoczności panelu edytora jest zapamiętywany lokalnie.
- Uzasadnienie: zachowanie jest spójne z istniejącym lokalnym zapisem ustawień interfejsu, w tym szerokości panelu.
- Konsekwencje: aplikacja musi przechowywać widoczność panelu w `localStorage`, niezależnie od zapamiętanego rozmiaru panelu.

- Decyzja (dotyczy PRD: 003-code-cleanup-prd.md): techniczne sprzątanie martwego kodu i komentarzy nie zmienia zachowania aplikacji.
- Uzasadnienie: wykryte elementy są potwierdzonymi pozostałościami implementacyjnymi, a nie częścią funkcjonalności użytkownika.
- Konsekwencje: zmiany porządkowe muszą być minimalne, ograniczone do wskazanych elementów i potwierdzone smoke testami regresji.

---

## Jakość i kryteria akceptacji
- Aplikacja działa lokalnie bez sieci i bez zewnętrznych usług.
- Dane użytkownika nie opuszczają przeglądarki.
- Panel edytora można całkowicie ukryć i ponownie pokazać przyciskiem w topbarze mapy.
- Po ukryciu panelu znika również uchwyt zmiany rozmiaru, a mapa zajmuje dostępną przestrzeń.
- Stan widoczności panelu oraz wcześniej ustawiony rozmiar panelu są zachowywane lokalnie.
- Eksport TXT pobiera `structuremind-map.txt` tylko wtedy, gdy edytor zawiera niepustą treść.
- Zawartość TXT jest identyczna z treścią edytora.
- Eksport PNG pobiera `structuremind-map.png` tylko wtedy, gdy istnieje mapa do zapisania.
- PNG zawiera pełną mapę, nie tylko aktualnie widoczny fragment viewportu.
- PNG zawiera aktualny wygląd mapy, tło i margines `64px`.
- Kropki i połączenia na mapie używają kolorów zgodnych z poziomami hierarchii.
- PNG zawiera te same kolory kropek i połączeń poziomów co widok mapy.
- Puste stany są blokowane i komunikowane w UI bez wyskakujących alertów.
- Kod aplikacji nie zawiera potwierdzonych martwych elementów wskazanych w aktualnym PRD porządkowym.
- Walidacja przeglądarkowa powinna obejmować uruchomienie `index.html` oraz podstawowe przepływy eksportu.

---

## Zasady zmian i ewolucji
- zmiany funkcjonalne → aktualizacja `ROADMAP.md`
- zmiany architektoniczne → aktualizacja tej specyfikacji
- nowe zależności → wpis do `## Decyzje techniczne`
- refactory tylko w ramach aktualnego milestone’u
- sprzątanie martwego kodu → brak zmian zachowania oraz adekwatna walidacja regresji

---

## Powiązanie z roadmapą
- Szczegóły milestone’ów i ich statusy znajdują się w `ROADMAP.md`.

---

## Status specyfikacji
- Data utworzenia: 2026-05-10
- Ostatnia aktualizacja: 2026-05-11
- Aktualny zakres obowiązywania: struktura tekstowa mapy, wizualizacja mapy, ukrywanie panelu edytora, kolorowanie poziomów hierarchii, lokalny eksport TXT i PNG oraz techniczne utrzymanie kodu bez potwierdzonych martwych elementów.
