# Historyjka: Przeglądanie listy stacji benzynowych

## Opis

**Jako** zalogowany użytkownik aplikacji,  
**chcę** przeglądać tabelę wszystkich stacji benzynowych z podstawowymi informacjami,  
**aby** łatwo porównać ceny paliw i znaleźć najbardziej odpowiednią dla mnie stację.

## Kryteria akceptacji

- [ ] Użytkownik widzi listę wszystkich dostępnych stacji benzynowych w formie tabeli.
- [ ] Każda pozycja na liście zawiera:
  - [ ] Nazwę marki stacji (np. Orlen, Shell, BP)
  - [ ] Aktualne ceny dostępnych paliw
  - [ ] Lokalizację (adres)
  - [ ] Odległość od użytkownika (jeśli dostępna geolokalizacja)
  - [ ] Przyciski akcji: „Szczegóły" i „Pokaż na mapie"
- [ ] System wyświetla liczbę wszystkich dostępnych stacji.
- [ ] Użytkownik może kliknąć przycisk „Szczegóły" przy stacji, aby przejść do jej pełnego profilu.
- [ ] Użytkownik może kliknąć przycisk „Pokaż na mapie", aby zobaczyć lokalizację stacji na mapie.
- [ ] Jeśli użytkownik udostępnił geolokalizację, system oblicza i wyświetla odległość do każdej stacji.
- [ ] Lista jest responsywna i działa zarówno na desktopie, jak i urządzeniach mobilnych.
- [ ] Jeśli stacja nie ma aktualnych cen dla danego paliwa, system wyświetla „Brak danych".

## Scenariusze (Given – When – Then)

### Scenariusz 1: Wyświetlenie listy wszystkich stacji

- **Mając (Given)** użytkownik jest zalogowany i otworzył widok listy stacji,
- **Kiedy (When)** strona się załaduje,
- **Wtedy (Then)** system wyświetli listę wszystkich stacji pobranych z endpointu `POST /api/station/list`,
- **I (And)** każda pozycja zawiera nazwę marki, aktualne ceny dostępnych paliw, adres i przyciski akcji.

### Scenariusz 2: Wyświetlenie odległości przy włączonej geolokalizacji

- **Mając (Given)** użytkownik udostępnił swoją lokalizację,
- **Kiedy (When)** lista stacji się załaduje,
- **Wtedy (Then)** system obliczy i wyświetli odległość do każdej stacji w kolumnie „Odległość",
- **I (And)** odległości są wyświetlane w km z dokładnością do jednego miejsca po przecinku.

### Scenariusz 3: Brak geolokalizacji

- **Mając (Given)** użytkownik nie udostępnił swojej lokalizacji,
- **Kiedy (When)** lista stacji się załaduje,
- **Wtedy (Then)** system wyświetli stacje bez kolumny „Odległość",
- **I (And)** wyświetli informację „Włącz geolokalizację, aby zobaczyć odległości do stacji".

### Scenariusz 4: Przejście do strony szczegółów stacji

- **Mając (Given)** lista stacji jest widoczna,
- **Kiedy (When)** użytkownik kliknie przycisk „Szczegóły" przy wybranej stacji,
- **Wtedy (Then)** system przekieruje go do strony profilu stacji (endpoint `GET /api/station/profile`),
- **I (And)** użytkownik zobaczy szczegółowe informacje: adres, pełną listę paliw i cen, godziny otwarcia, historię cen, dane kontaktowe.

### Scenariusz 5: Wyświetlenie stacji na mapie

- **Mając (Given)** lista stacji jest widoczna,
- **Kiedy (When)** użytkownik kliknie przycisk „Pokaż na mapie" przy danej stacji,
- **Wtedy (Then)** system otworzy widok mapy z zaznaczonym markerem wybranej stacji,
- **I (And)** jeśli geolokalizacja jest dostępna, pokaże odległość od lokalizacji użytkownika.

### Scenariusz 6: Brak aktualnych cen dla paliwa

- **Mając (Given)** stacja nie ma aktualnych danych cenowych dla konkretnego rodzaju paliwa,
- **Kiedy (When)** lista się wyświetli,
- **Wtedy (Then)** system wyświetli „Brak danych" w kolumnie tego paliwa dla tej stacji.

### Scenariusz 7: Długi czas ładowania

- **Mając (Given)** użytkownik otworzył widok listy stacji,
- **Kiedy (When)** ładowanie trwa dłużej niż 1 sekundę,
- **Wtedy (Then)** system wyświetli loader/spinner z komunikatem „Ładowanie stacji...".

## Dodatkowe informacje

### Zależności

- Wymaga systemu logowania i autoryzacji
- Wymaga endpointu `POST /api/station/list`
- Wymaga endpointu `GET /api/station/profile`
- Wymaga funkcjonalności wyświetlania stacji na mapie (Historia 3)

### Notatki techniczne

- **Endpointy:**
  - `POST /api/station/list` - główny endpoint do pobierania listy stacji
  - `GET /api/station/profile` - szczegóły wybranej stacji
- **Geolokalizacja:** Browser Geolocation API dla obliczania odległości
- **Paginacja:** Rozważyć implementację dla dużej liczby wyników (np. 50 stacji na stronę)
- **Performance:** Cache'owanie listy stacji w Redux/Context API
- **Responsive:** Tabela powinna zmieniać się w karty na urządzeniach mobilnych

### Przypadki brzegowe

- Użytkownik nie wyraził zgody na geolokalizację - nie pokazywać kolumny odległości
- Brak aktualnych cen dla wybranego paliwa na stacji - wyświetlenie „Brak danych"
- Długi czas ładowania przy dużej liczbie stacji - wyświetlenie loadera/spinnera
- Brak stacji w systemie - wyświetlić komunikat „Brak dostępnych stacji"
- API geolokalizacji nie jest dostępne w przeglądarce - pominąć obliczanie odległości
- Bardzo duża liczba stacji (500+) - implementacja paginacji lub wirtualnego scrollingu
- Stacja została usunięta podczas przeglądania - odświeżyć listę i wyświetlić komunikat

## Priorytet i estymacja

- **Priorytet:** Must Have (podstawowa funkcjonalność aplikacji)
- **Estymacja (Story Points):** 3
