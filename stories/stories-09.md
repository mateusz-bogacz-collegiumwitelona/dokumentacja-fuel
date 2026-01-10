# Historyjka: Przeglądanie i filtrowanie listy stacji benzynowych

## Opis

**Jako** zalogowany użytkownik aplikacji,  
**chcę** przeglądać tabelę wszystkich stacji benzynowych z możliwością filtrowania i sortowania,  
**aby** łatwo porównać ceny paliw i znaleźć najbardziej odpowiednią dla mnie stację.

## Kryteria akceptacji

- [ ] Użytkownik widzi listę wszystkich dostępnych stacji benzynowych w formie tabeli.
- [ ] Każda pozycja na liście zawiera:
  - [ ] Nazwę marki stacji (np. Orlen, Shell, BP)
  - [ ] Aktualne ceny dostępnych paliw
  - [ ] Lokalizację (adres)
  - [ ] Odległość od użytkownika (jeśli dostępna geolokalizacja)
  - [ ] Przyciski akcji: „Szczegóły" i „Pokaż na mapie"
- [ ] Użytkownik może filtrować stacje według:
  - [ ] Rodzaju paliwa (np. Pb95, Pb98, ON, LPG)
  - [ ] Zakresu cen (minimalna i/lub maksymalna cena dla wybranego paliwa)
  - [ ] Marki stacji (wielokrotny wybór)
  - [ ] Lokalizacji (promień od aktualnej lokalizacji lub podany adres)
- [ ] Użytkownik może sortować listę według:
  - [ ] Nazwy marki (alfabetycznie)
  - [ ] Ceny określonego paliwa (rosnąco/malejąco)
  - [ ] Odległości od użytkownika (rosnąco/malejąco)
- [ ] System waliduje poprawność wprowadzonych danych:
  - [ ] Ceny muszą być wartościami liczbowymi
  - [ ] Minimalna cena nie może być większa niż maksymalna
  - [ ] Jeśli ustawiono zakres cen, rodzaj paliwa musi być wybrany
- [ ] Po kliknięciu przycisku „Wyszukaj" system zwraca przefiltrowaną i posortowaną listę stacji.
- [ ] Jeśli nie znaleziono żadnych stacji, system wyświetla komunikat „Brak wyników — spróbuj zmienić filtry".
- [ ] Użytkownik może kliknąć przycisk „Szczegóły" przy stacji, aby przejść do jej pełnego profilu.
- [ ] Użytkownik może kliknąć przycisk „Pokaż na mapie", aby zobaczyć lokalizację stacji na mapie.
- [ ] System wyświetla liczbę znalezionych stacji pasujących do filtrów.
- [ ] Lista jest responsywna i działa zarówno na desktopie, jak i urządzeniach mobilnych.

## Scenariusze (Given – When – Then)

### Scenariusz 1: Pomyślne wyszukanie, filtrowanie i posortowanie stacji

- **Mając (Given)** użytkownik jest na widoku listy stacji, lista stacji zawiera rekordy z marką, cenami dla poszczególnych paliw i lokalizacją,
- **I (And)** użytkownik wybrał typ paliwa (np. „ON"), ustawił zakres cen (min i/lub max) i/lub filtr marki oraz/lub promień lokalizacji (albo podał adres),
- **I (And)** użytkownik wybrał sortowanie (np. „cena ON rosnąco" lub „odległość rosnąco"),
- **Kiedy (When)** kliknie przycisk „Wyszukaj",
- **Wtedy (Then)** system zweryfikuje poprawność pól (wartości liczbowe, zakresy),
- **I (And)** zwróci listę stacji spełniających filtry, posortowaną zgodnie z wyborem (endpoint `POST /api/station/list`),
- **I (And)** każda pozycja listy wyświetli nazwę marki, aktualne ceny dostępnych paliw, odległość (jeśli dotyczy) i przyciski: „Szczegóły" oraz „Pokaż na mapie",
- **I (And)** jeśli nie znaleziono żadnych stacji, system pokaże informację „Brak wyników — spróbuj zmienić filtry".

### Scenariusz 2: Błąd walidacji — ustawiono zakres cen bez wyboru rodzaju paliwa

- **Mając (Given)** użytkownik ustawił minimalną i/lub maksymalną cenę w polach filtra cen,
- **Ale (But)** nie wybrał rodzaju paliwa,
- **Kiedy (When)** kliknie „Wyszukaj",
- **Wtedy (Then)** system nie wykona zapytania i wyświetli komunikat błędu: „Proszę wybrać rodzaj paliwa przy ustawionym zakresie cen",
- **I (And)** podświetli pola wymagające poprawy (np. lista paliw),
- **I (And)** nie pokaże wyników do momentu poprawienia błędu.

### Scenariusz 3: Przejście do strony szczegółów stacji

- **Mając (Given)** lista wyników wyszukiwania jest widoczna i zawiera co najmniej jedną stację,
- **Kiedy (When)** użytkownik kliknie przycisk „Szczegóły" przy wybranej stacji,
- **Wtedy (Then)** system przekieruje go do strony profilu stacji (endpoint `GET /api/station/profile`),
- **I (And)** użytkownik zobaczy szczegółowe informacje: adres, pełną listę paliw i cen, godziny otwarcia, historię cen, dane kontaktowe.

### Scenariusz 4: Wyświetlenie stacji na mapie

- **Mając (Given)** lista wyników wyszukiwania jest widoczna,
- **Kiedy (When)** użytkownik kliknie przycisk „Pokaż na mapie" przy danej stacji,
- **Wtedy (Then)** system otworzy widok mapy z zaznaczonym markerem wybranej stacji,
- **I (And)** pokaże odległość od lokalizacji użytkownika (jeśli geolokalizacja jest dostępna),
- **I (And)** wyświetli przycisk „Jak dojechać" (link do nawigacji),
- **I (And)** jeżeli aplikacja nie ma pozwolenia na geolokalizację, poprosi o podanie lokalizacji ręcznie.

### Scenariusz 5: Sortowanie po cenie paliwa

- **Mając (Given)** użytkownik wybrał typ paliwa „Pb95" w filtrach,
- **Kiedy (When)** wybierze sortowanie „Cena Pb95 rosnąco" i kliknie „Wyszukaj",
- **Wtedy (Then)** system wyświetli listę stacji posortowaną od najtańszej do najdroższej ceny Pb95.

### Scenariusz 6: Sortowanie po odległości

- **Mając (Given)** użytkownik udostępnił swoją lokalizację,
- **Kiedy (When)** wybierze sortowanie „Odległość rosnąco" i kliknie „Wyszukaj",
- **Wtedy (Then)** system wyświetli listę stacji posortowaną od najbliższej do najdalszej od użytkownika.

### Scenariusz 7: Filtrowanie po promieniu lokalizacji

- **Mając (Given)** użytkownik udostępnił swoją lokalizację lub podał adres,
- **Kiedy (When)** ustawi promień „5 km" i kliknie „Wyszukaj",
- **Wtedy (Then)** system wyświetli tylko stacje znajdujące się w promieniu 5 km od podanej lokalizacji.

### Scenariusz 8: Brak wyników wyszukiwania

- **Mając (Given)** użytkownik ustawił bardzo restrykcyjne filtry (np. cena ON poniżej 1 zł),
- **Kiedy (When)** kliknie „Wyszukaj",
- **Wtedy (Then)** system nie zwróci żadnych wyników,
- **I (And)** wyświetli komunikat „Brak wyników — spróbuj zmienić filtry",
- **I (And)** pokaże sugestie rozszerzenia kryteriów wyszukiwania.

## Dodatkowe informacje

### Notatki techniczne

- **Endpointy:**
  - `POST /api/station/list` - główny endpoint do pobierania listy stacji z filtrami i sortowaniem
  - `GET /api/station/all-brands` - lista wszystkich marek do filtrów
  - `GET /api/station/fuel-codes` - lista kodów paliw do filtrów
  - `GET /api/station/profile` - szczegóły wybranej stacji
- **Geolokalizacja:** Browser Geolocation API dla obliczania odległości
- **Paginacja:** Rozważyć implementację dla dużej liczby wyników
- **Performance:** Cache'owanie często wybieranych filtrów w Redis

### Przypadki brzegowe

- Użytkownik nie wyraził zgody na geolokalizację - możliwość ręcznego wprowadzenia adresu
- Brak aktualnych cen dla wybranego paliwa na stacji - wyświetlenie „Brak danych"
- Długi czas ładowania przy dużej liczbie stacji - wyświetlenie loadera/spinnera
- Użytkownik zmienia filtry bez klikania „Wyszukaj" - opcjonalnie: auto-refresh przy zmianie filtrów
- Niepoprawny adres przy ręcznym wprowadzaniu lokalizacji - walidacja i komunikat błędu
- Sortowanie po cenie paliwa, którego nie ma w filtrach - system automatycznie dodaje to paliwo do filtrów lub wyświetla komunikat

### Zależności

- Ta historyjka wymaga wcześniejszej implementacji:
  - Systemu logowania i autoryzacji
  - Endpointu `POST /api/station/list` z obsługą filtrów i sortowania
  - Endpointu `GET /api/station/all-brands`
  - Endpointu `GET /api/station/fuel-codes`
  - Endpointu `GET /api/station/profile`
  - Funkcjonalności wyświetlania stacji na mapie

## Priorytet i estymacja

- **Priorytet:** Must Have (kluczowa funkcjonalność aplikacji - porównywarka cen)
- **Estymacja (Story Points):** 8 (średnia/wysoka złożoność - filtry, sortowanie, walidacja, integracja z mapą)
