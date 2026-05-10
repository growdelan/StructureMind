# PRD: Kolorowanie poziomów mapy myśli

## Kontekst
StructureMind prezentuje mapę myśli na podstawie tekstowej struktury z wcięciami. Aktualnie każdy węzeł pokazuje etykietę `Poziom X` oraz kropkę obok tej etykiety, ale poza poziomem głównym kolejne poziomy używają tego samego fioletowego koloru.

Przy mapach z kilkoma poziomami utrudnia to szybkie rozpoznanie głębokości węzłów. Użytkownik widzi numer poziomu w tekście, ale kolor nie pomaga w skanowaniu hierarchii.

Nowa funkcjonalność ma dodać stałe kolorowanie poziomów mapy:
- kropka przy etykiecie poziomu ma kolor odpowiadający poziomowi wcięcia,
- połączenie prowadzące do węzła ma kolor poziomu tego węzła,
- eksport PNG zachowuje te same kolory co widok aplikacji.

## Cel
Celem funkcji jest poprawienie czytelności mapy myśli przez wizualne rozróżnienie poziomów hierarchii.

Użytkownik powinien móc szybciej ocenić głębokość i strukturę mapy bez zmiany sposobu wpisywania danych, bez konfiguracji i bez dodatkowych elementów interfejsu.

Funkcja ma działać w pełni lokalnie w przeglądarce, bez wysyłania danych poza komputer użytkownika i bez dodawania zewnętrznych zależności.

## Użytkownik docelowy
Użytkownikiem jest osoba tworząca mapy myśli w StructureMind, która chce:
- łatwiej odróżniać poziomy zagnieżdżenia podczas pracy z mapą,
- szybciej skanować rozbudowane struktury,
- zachować tę samą czytelność w wyeksportowanym obrazie PNG.

## Zakres funkcjonalny

### Kolorowanie kropek poziomów
Każdy węzeł mapy ma kropkę przy etykiecie `Poziom X`. Kolor tej kropki musi wynikać z poziomu wcięcia węzła.

Wymagania:
- poziom 0 pozostaje turkusowy jak obecnie,
- poziomy 1-6 używają stałej palety kolorów,
- poziomy 7+ powtarzają paletę poziomów 1-6 cyklicznie,
- tekst `Poziom X` pozostaje bez zmian,
- kolorowanie działa tak samo w motywie jasnym, ciemnym i auto,
- użytkownik nie wybiera ani nie konfiguruje kolorów.

Paleta kolorów:
- poziom 0: turkus `#22d3ee`,
- poziom 1: fiolet `#8b5cf6`,
- poziom 2: niebieski `#3b82f6`,
- poziom 3: zieleń `#10b981`,
- poziom 4: bursztyn `#f59e0b`,
- poziom 5: róż `#f43f5e`,
- poziom 6: indygo `#6366f1`.

Przykład cyklicznego powtarzania:
- poziom 7 używa koloru poziomu 1,
- poziom 8 używa koloru poziomu 2,
- poziom 9 używa koloru poziomu 3.

### Kolorowanie połączeń
Połączenie między rodzicem a dzieckiem ma używać koloru poziomu dziecka.

Wymagania:
- linia prowadząca do węzła poziomu 1 używa koloru poziomu 1,
- linia prowadząca do węzła poziomu 2 używa koloru poziomu 2,
- ta sama reguła działa dla wszystkich poziomów, także przy cyklicznym powtarzaniu palety,
- poziom 0 nie ma połączenia rodzica, więc reguła dotyczy tylko połączeń do węzłów potomnych.

### Eksport PNG
Eksport PNG musi odzwierciedlać kolorowanie poziomów tak samo jak widok mapy w aplikacji.

Wymagania:
- kropki w pliku PNG mają takie same kolory jak kropki w widoku mapy,
- połączenia w pliku PNG mają takie same kolory jak połączenia w widoku mapy,
- eksport nadal obejmuje pełną mapę, tło i margines `64px`,
- eksport TXT pozostaje bez zmian.

### Interfejs użytkownika
Funkcja nie dodaje nowych kontrolek ani ustawień.

Wymagania:
- brak legendy kolorów w głównym widoku,
- brak wyboru palety,
- brak własnych kolorów użytkownika,
- istniejąca etykieta `Poziom X` nadal wyjaśnia znaczenie poziomu.

## Poza zakresem
Ta funkcja nie obejmuje:
- edytora palety kolorów,
- presetów palet,
- osobnej legendy kolorów w UI,
- zmiany tekstów etykiet poziomów,
- zmiany sposobu parsowania wcięć,
- zmiany układu mapy,
- zmiany eksportu TXT,
- eksportu SVG lub PDF,
- dodawania zewnętrznych bibliotek.

## Wymagania techniczne
- Implementacja ma pozostać w czystym HTML, CSS i JavaScript.
- Nie dodawać zależności front-endowych ani narzędzi deweloperskich.
- Funkcja ma działać bez sieci i bez zewnętrznych usług.
- Dane mapy nie mogą opuszczać przeglądarki użytkownika.
- Głównym entrypointem pozostaje `index.html`.
- Reguła wyboru koloru poziomu powinna być wspólna dla renderowania DOM/SVG i eksportu PNG, żeby uniknąć rozjazdu między widokiem a obrazem.

## Kryteria akceptacji
- Poziom 0 ma turkusową kropkę `#22d3ee`.
- Poziomy 1-6 mają różne kolory zgodnie ze stałą paletą.
- Poziomy 7+ powtarzają kolory poziomów 1-6 cyklicznie.
- Kropka przy każdym węźle używa koloru poziomu tego węzła.
- Połączenie do każdego węzła używa koloru poziomu tego węzła.
- Kolory działają w motywie jasnym, ciemnym i auto.
- Eksport PNG zawiera te same kolory kropek i połączeń co widok mapy.
- Eksport TXT działa tak jak wcześniej i nie zmienia zawartości pliku.
- Aplikacja nadal działa lokalnie i nie wymaga internetu.

## Smoke testy

### Smoke test kolorów poziomów
1. Uruchomić aplikację lokalnie.
2. Wpisać strukturę zawierającą poziomy od 0 do 6.
3. Sprawdzić, że kropki dla poziomów 0-6 mają kolory zgodne z paletą.
4. Sprawdzić, że połączenia do węzłów mają kolor poziomu węzła docelowego.

### Smoke test cyklicznego powtarzania palety
1. Wpisać strukturę zawierającą poziomy od 0 do 9.
2. Sprawdzić, że poziom 7 używa koloru poziomu 1.
3. Sprawdzić, że poziom 8 używa koloru poziomu 2.
4. Sprawdzić, że poziom 9 używa koloru poziomu 3.

### Smoke test motywów
1. Wpisać strukturę z kilkoma poziomami.
2. Przełączyć motyw aplikacji na jasny.
3. Sprawdzić czytelność kropek i połączeń.
4. Przełączyć motyw aplikacji na ciemny.
5. Sprawdzić czytelność kropek i połączeń.
6. Przełączyć motyw aplikacji na auto i sprawdzić, że kolorowanie nadal działa.

### Smoke test PNG
1. Wpisać strukturę zawierającą poziomy od 0 do 6.
2. Kliknąć `Zapisz PNG`.
3. Sprawdzić, że pobrany plik `structuremind-map.png` zawiera pełną mapę.
4. Sprawdzić, że kropki i połączenia w PNG mają takie same kolory jak w widoku aplikacji.

### Smoke test TXT
1. Wpisać dowolną strukturę mapy.
2. Kliknąć `Zapisz TXT`.
3. Sprawdzić, że plik `structuremind-map.txt` zawiera dokładnie tę samą treść co edytor.

## Otwarte kwestie
Brak. Zakres funkcji został ustalony:
- kolorowanie dotyczy kropek i połączeń,
- połączenie przyjmuje kolor poziomu dziecka,
- poziom 0 zostaje turkusowy,
- poziomy 1-6 używają stałej palety,
- poziomy 7+ powtarzają paletę cyklicznie,
- PNG musi być zgodny z widokiem aplikacji,
- implementacja bez nowych zależności i bez dodatkowej konfiguracji UI.
