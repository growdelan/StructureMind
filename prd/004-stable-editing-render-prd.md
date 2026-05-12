# PRD: Stabilne renderowanie mapy podczas edycji

## Kontekst
StructureMind renderuje mapę myśli na podstawie tekstowej struktury wpisywanej w edytorze. Aktualnie przy każdej zmianie tekstu mapa jest widocznie przebudowywana w obszarze po prawej stronie. Przy dopisywaniu nowych kategorii i podkategorii użytkownik widzi miganie w miejscu, w którym pojawia się węzeł.

Miganie rozprasza podczas pisania i sprawia wrażenie, że ekran traci stabilność przy każdej edycji. Problem jest szczególnie widoczny przy szybkim dopisywaniu kolejnych węzłów, bo istniejąca mapa znika albo odtwarza animację zamiast spokojnie aktualizować aktualny widok.

Nowa funkcjonalność ma ustabilizować renderowanie mapy podczas edycji tekstu:
- istniejące węzły pozostają widoczne,
- kamera nie resetuje się podczas pisania,
- nowy albo zmieniony węzeł pojawia się w docelowym miejscu bez migania,
- ręcznie przesunięte węzły zachowują swoje pozycje, jeśli nadal istnieją po edycji.

## Cel
Celem funkcji jest poprawienie komfortu pracy w edytorze przez usunięcie migania mapy podczas wpisywania i modyfikowania struktury.

Użytkownik powinien móc swobodnie dopisywać, usuwać, zmieniać nazwy, wcinać oraz wklejać węzły bez efektu przebudowy całego widoku po prawej stronie.

Funkcja ma zachować natychmiastową aktualizację mapy po zmianie tekstu, ale bez widocznego czyszczenia i ponownego animowania całej mapy.

## Użytkownik docelowy
Użytkownikiem jest osoba tworząca mapę myśli w StructureMind, która chce:
- pisać strukturę bez rozpraszającego migania wizualizacji,
- widzieć aktualizacje mapy natychmiast po zmianie tekstu,
- zachować stabilny obszar mapy podczas dopisywania kolejnych kategorii i podkategorii,
- nie tracić ręcznie ustawionych pozycji węzłów przy dalszej edycji tekstu.

## Zakres funkcjonalny

### Stabilna aktualizacja podczas edycji
Mapa ma aktualizować się stabilnie przy każdej zmianie treści edytora.

Wymagania:
- dopisywanie tekstu w istniejącym węźle nie powoduje migania mapy,
- dodanie nowego węzła nie powoduje znikania istniejących węzłów,
- usunięcie węzła aktualizuje mapę bez odtworzenia animacji całej struktury,
- zmiana nazwy węzła aktualizuje widoczny tekst bez resetowania widoku,
- `Enter`, `Tab`, cofnięcie wcięcia i wklejanie większej struktury aktualizują mapę bez migania,
- mapa reaguje natychmiast po zmianie tekstu, bez opóźniania aktualizacji widoku.

### Zachowanie stabilnego widoku
Podczas edycji tekstu obszar mapy ma pozostać stabilny.

Wymagania:
- kamera nie resetuje pozycji ani zoomu przy zwykłej edycji tekstu,
- istniejące węzły pozostają widoczne podczas aktualizacji,
- nowe węzły pojawiają się od razu w docelowym miejscu, bez animacji wejścia,
- istniejące węzły pozostają możliwie nieruchome,
- dopuszczalny jest minimalny ruch istniejących węzłów tylko wtedy, gdy jest potrzebny do uniknięcia kolizji albo zachowania czytelnego układu.

### Zachowanie ręcznie przesuniętych węzłów
Jeśli użytkownik ręcznie przesunął węzeł, aplikacja powinna zachować tę pozycję po kolejnych edycjach tekstu, o ile ten węzeł nadal istnieje.

Wymagania:
- ręcznie przesunięty węzeł nie wraca automatycznie do pozycji wyliczonej przez układ po zmianie nazwy,
- ręcznie przesunięty węzeł nie wraca automatycznie do pozycji wyliczonej przez układ po dopisaniu innego węzła,
- po usunięciu węzła jego pozycja nie musi być zachowywana,
- po utworzeniu nowego węzła aplikacja może nadać mu pozycję wynikającą z aktualnego układu mapy.

### Zachowanie obecnych akcji mapy
Funkcja dotyczy stabilizacji podczas edycji tekstu. Pozostałe akcje mapy mogą zachować dotychczasowe zachowanie, jeśli nie wprowadzają regresji.

Wymagania:
- pierwszy render mapy może używać obecnych animacji,
- zoom może działać jak dotychczas,
- centrowanie mapy może działać jak dotychczas,
- dopasowanie widoku może działać jak dotychczas,
- przeciąganie tła i węzłów może działać jak dotychczas,
- eksport TXT i PNG pozostają bez zmian funkcjonalnych.

## Poza zakresem
Ta funkcja nie obejmuje:
- zmiany formatu tekstowej struktury mapy,
- zmiany parsera wcięć,
- dodawania debouncowania renderowania mapy,
- dodawania nowych ustawień stabilizacji widoku,
- dodawania przełącznika animacji w UI,
- zmiany działania eksportu TXT,
- zmiany działania eksportu PNG,
- zmiany kolorowania poziomów,
- przebudowy całej architektury aplikacji,
- dodawania zewnętrznych bibliotek lub narzędzi.

## Wymagania techniczne
- Implementacja ma pozostać w czystym HTML, CSS i JavaScript.
- Nie dodawać zależności front-endowych ani narzędzi deweloperskich.
- Funkcja ma działać bez sieci i bez zewnętrznych usług.
- Dane mapy nie mogą opuszczać przeglądarki użytkownika.
- Głównym entrypointem pozostaje `index.html`.
- Aktualizacja po `input` powinna preferować ponowne użycie istniejących elementów DOM/SVG zamiast widocznego czyszczenia całych warstw mapy.
- Aktualizacja po `input` nie powinna odtwarzać animacji wejścia dla istniejących węzłów.
- Nowe węzły dodane podczas edycji tekstu powinny pojawiać się bez animacji wejścia.
- Mechanizm stabilizacji powinien działać na desktopie i w wąskim viewportcie.
- Implementacja nie może pogorszyć działania eksportu, zoomu, centrowania, dopasowania widoku, ukrywania panelu ani kolorowania poziomów.

## Kryteria akceptacji
- Dopisywanie tekstu w edytorze nie powoduje migania prawej części ekranu.
- Dodanie nowej kategorii nie powoduje znikania istniejących węzłów.
- Dodanie nowej podkategorii nie powoduje znikania istniejących węzłów.
- Zmiana nazwy istniejącego węzła aktualizuje tekst bez resetowania kamery.
- Usunięcie węzła aktualizuje mapę bez odtworzenia animacji całej mapy.
- Wklejenie większej struktury aktualizuje mapę bez widocznego czyszczenia całego obszaru mapy.
- `Enter` i `Tab` w edytorze aktualizują mapę bez migania.
- Nowy węzeł pojawia się od razu w docelowym miejscu podczas edycji tekstu.
- Ręcznie przesunięty węzeł zachowuje pozycję po edycji tekstu, jeśli nadal istnieje.
- Kamera zachowuje aktualny zoom i przesunięcie podczas zwykłej edycji tekstu.
- Funkcja działa na desktopie i na wąskich ekranach.
- Eksport TXT działa tak jak wcześniej.
- Eksport PNG działa tak jak wcześniej.
- Kolorowanie poziomów działa tak jak wcześniej.
- Ukrywanie i ponowne pokazywanie panelu edytora działa tak jak wcześniej.
- Zoom, centrowanie i dopasowanie widoku działają tak jak wcześniej.
- W konsoli przeglądarki nie pojawiają się błędy po edycji mapy.

## Smoke testy

### Smoke test desktop
1. Uruchomić aplikację lokalnie w szerokim viewportcie.
2. Wpisać strukturę z kategorią główną i kilkoma podkategoriami.
3. Dopisać nową kategorię.
4. Dopisać nową podkategorię.
5. Sprawdzić, że prawa część ekranu nie miga podczas wpisywania.
6. Sprawdzić, że istniejące węzły nie znikają przy aktualizacji mapy.

### Smoke test mobile
1. Uruchomić aplikację lokalnie w wąskim viewportcie.
2. Wpisać strukturę z kilkoma poziomami.
3. Dopisać nowy węzeł i podwęzeł.
4. Sprawdzić, że mapa aktualizuje się bez migania.
5. Sprawdzić, że edytor i mapa pozostają używalne w układzie mobilnym.

### Smoke test edycji tekstu
1. Zmienić nazwę istniejącego węzła.
2. Dodać nowy węzeł przez `Enter`.
3. Zmienić poziom węzła przez `Tab`.
4. Usunąć istniejący węzeł.
5. Wkleić większą strukturę z kilkoma poziomami.
6. Sprawdzić, że żadna z tych akcji nie powoduje widocznego czyszczenia całej mapy ani resetu kamery.

### Smoke test ręcznych pozycji
1. Wpisać strukturę z kilkoma węzłami.
2. Ręcznie przesunąć jeden z węzłów.
3. Zmienić nazwę innego węzła w edytorze.
4. Dopisać nowy węzeł.
5. Sprawdzić, że ręcznie przesunięty węzeł zachował swoją pozycję, jeśli nadal istnieje.

### Smoke test regresji istniejących funkcji
1. Wpisać strukturę zawierającą kilka poziomów.
2. Sprawdzić kolorowanie kropek i połączeń poziomów.
3. Sprawdzić zoom, centrowanie i dopasowanie widoku.
4. Schować i ponownie pokazać panel edytora.
5. Kliknąć `Zapisz TXT` i sprawdzić, że eksport działa jak wcześniej.
6. Kliknąć `Zapisz PNG` i sprawdzić, że eksport działa jak wcześniej.
7. Potwierdzić brak błędów konsoli.

## Otwarte kwestie
Brak. Zakres funkcji został ustalony:
- stabilizacja dotyczy wszystkich edycji tekstu,
- mapa aktualizuje się natychmiast,
- nowe węzły pojawiają się bez animacji wejścia podczas edycji,
- istniejące węzły pozostają widoczne i możliwie nieruchome,
- ręcznie przesunięte węzły zachowują pozycje, jeśli nadal istnieją,
- pierwszy render oraz akcje zoomu, centrowania, dopasowania i przeciągania mogą zachować obecne zachowanie,
- implementacja bez nowych zależności.
