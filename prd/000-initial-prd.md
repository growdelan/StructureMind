# PRD: Eksport mapy do TXT i PNG

## Kontekst
StructureMind pozwala tworzyć mapę myśli na podstawie tekstowej struktury wpisanej w edytorze. Aktualnie użytkownik może opracować mapę w przeglądarce, ale nie ma prostego sposobu zapisania źródłowej struktury ani wygenerowania pliku graficznego z pełną mapą.

Nowa funkcjonalność ma dodać lokalny eksport mapy w dwóch formach:
- plik TXT z dokładną treścią edytora,
- plik PNG z pełną mapą.

## Cel
Celem funkcji jest umożliwienie użytkownikowi zachowania mapy poza aplikacją:
- jako edytowalnej struktury tekstowej,
- jako gotowego obrazu do udostępnienia, dokumentacji lub prezentacji.

Funkcja ma działać w pełni lokalnie w przeglądarce, bez wysyłania danych poza komputer użytkownika i bez dodawania zewnętrznych zależności.

## Użytkownik docelowy
Użytkownikiem jest osoba tworząca mapy myśli w StructureMind, która chce:
- zachować aktualną strukturę tekstową do późniejszej edycji,
- wyeksportować wizualną pełną mapę jako obraz,
- wykonać eksport szybko, bez konfiguracji i bez konta.

## Zakres funkcjonalny

### Eksport TXT
Użytkownik może kliknąć przycisk `Zapisz TXT`, aby pobrać plik tekstowy.

Wymagania:
- eksport zapisuje dokładnie bieżącą zawartość edytora znak w znak,
- aplikacja nie normalizuje wcięć, spacji, tabów, pustych linii ani znaków listy,
- proponowana nazwa pliku to `structuremind-map.txt`,
- eksport działa lokalnie w przeglądarce,
- jeśli edytor jest pusty lub zawiera wyłącznie białe znaki, eksport jest blokowany,
- przy zablokowanym eksporcie aplikacja pokazuje krótki komunikat w UI.

### Eksport PNG
Użytkownik może kliknąć przycisk `Zapisz PNG`, aby pobrać obraz pełnej mapy.

Wymagania:
- eksport obejmuje pełną mapę, niezależnie od aktualnego zoomu i przesunięcia widoku,
- eksport zachowuje aktualny wygląd mapy z UI, w tym motyw, kolory, tło, węzły, połączenia i ręczne przesunięcia węzłów,
- eksport zawiera tło mapy, nie jest przezroczysty,
- obraz ma margines `64px` wokół pełnej mapy,
- proponowana nazwa pliku to `structuremind-map.png`,
- eksport działa lokalnie w przeglądarce,
- jeśli mapa jest pusta, eksport jest blokowany,
- przy zablokowanym eksporcie aplikacja pokazuje krótki komunikat w UI.

### Interfejs użytkownika
W UI mają pojawić się dwa osobne przyciski:
- `Zapisz TXT`,
- `Zapisz PNG`.

Przyciski powinny być umieszczone w istniejącym interfejsie w sposób spójny z obecnym paskiem akcji lub panelem edytora.

Komunikaty powinny być krótkie i widoczne w aplikacji, bez wyskakujących alertów przeglądarki.

Przykładowe komunikaty:
- `Zapisano plik TXT`,
- `Zapisano plik PNG`,
- `Brak tekstu do zapisania`,
- `Brak mapy do zapisania`.

## Poza zakresem
Ta funkcja nie obejmuje:
- eksportu SVG,
- eksportu PDF,
- wyboru nazwy pliku przez użytkownika,
- wyboru jakości, skali lub rozdzielczości PNG,
- zapisu do chmury,
- importu plików TXT,
- zewnętrznych bibliotek do eksportu,
- osobnego stylu eksportu niezależnego od wyglądu UI.

## Wymagania techniczne
- Implementacja ma pozostać w czystym HTML, CSS i JavaScript.
- Nie dodawać zależności front-endowych ani narzędzi deweloperskich.
- Funkcja ma działać bez sieci i bez zewnętrznych usług.
- Dane mapy nie mogą opuszczać przeglądarki użytkownika.
- Głównym entrypointem pozostaje `index.html`.

## Kryteria akceptacji
- Użytkownik widzi przyciski `Zapisz TXT` i `Zapisz PNG`.
- Kliknięcie `Zapisz TXT` przy niepustym edytorze pobiera plik `structuremind-map.txt`.
- Plik TXT zawiera dokładnie ten sam tekst, który był w edytorze.
- Kliknięcie `Zapisz TXT` przy pustym edytorze nie pobiera pliku i pokazuje komunikat.
- Kliknięcie `Zapisz PNG` przy niepustej mapie pobiera plik `structuremind-map.png`.
- Plik PNG zawiera pełną mapę, nie tylko aktualny widok.
- Plik PNG zawiera tło i aktualny wygląd mapy z aplikacji.
- Plik PNG ma widoczny margines wokół mapy.
- Kliknięcie `Zapisz PNG` przy pustej mapie nie pobiera pliku i pokazuje komunikat.
- Eksport działa lokalnie i nie wymaga internetu.

## Smoke testy

### Smoke test TXT
1. Uruchomić aplikację lokalnie.
2. Wpisać strukturę z kilkoma poziomami wcięć, pustą linią i znakiem listy.
3. Kliknąć `Zapisz TXT`.
4. Sprawdzić, że pobrany plik nazywa się `structuremind-map.txt`.
5. Sprawdzić, że zawartość pliku jest identyczna z zawartością edytora.

### Smoke test pustego TXT
1. Wyczyścić edytor.
2. Kliknąć `Zapisz TXT`.
3. Sprawdzić, że plik nie został pobrany.
4. Sprawdzić, że aplikacja pokazuje komunikat o braku tekstu do zapisania.

### Smoke test PNG
1. Wpisać strukturę tworzącą mapę z kilkoma poziomami.
2. Zmienić motyw lub układ mapy, jeśli jest to dostępne w UI.
3. Przesunąć lub powiększyć widok tak, aby część mapy nie była widoczna w viewportcie.
4. Kliknąć `Zapisz PNG`.
5. Sprawdzić, że pobrany plik nazywa się `structuremind-map.png`.
6. Sprawdzić, że obraz zawiera pełną mapę, tło i margines wokół mapy.

### Smoke test pustego PNG
1. Wyczyścić edytor.
2. Kliknąć `Zapisz PNG`.
3. Sprawdzić, że plik nie został pobrany.
4. Sprawdzić, że aplikacja pokazuje komunikat o braku mapy do zapisania.

## Otwarte kwestie
Brak. Zakres funkcji został ustalony:
- TXT zapisuje dokładną treść edytora,
- PNG zapisuje pełną mapę,
- PNG zachowuje aktualny wygląd UI,
- eksport działa lokalnie,
- implementacja bez zewnętrznych zależności.
