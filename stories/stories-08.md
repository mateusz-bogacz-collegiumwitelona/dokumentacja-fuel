# Historyjka: Filtrowanie i sortowanie listy stacji benzynowych

## Opis

**Jako** zalogowany użytkownik aplikacji,  
**chcę** móc filtrować i sortować listę stacji benzynowych według różnych kryteriów,  
**aby** szybko znaleźć stacje odpowiadające moim konkretnym potrzebom i preferencjom cenowym.

## Kryteria akceptacji

- [ ] Użytkownik widzi panel filtrów nad lub obok listy stacji.
- [ ] Użytkownik może filtrować stacje według:
  - [ ] Rodzaju paliwa (np. Pb95, Pb98, ON, LPG)
  - [ ] Zakresu cen (minimalna i/lub maksymalna cena dla wybranego paliwa)
  - [ ] Marki stacji (wielokrotny wybór)
  - [ ] Lokalizacji (promień od aktualnej lokalizacji lub podany adres)
- [ ] Użytkownik może sortować listę według:
  - [ ] Nazwy marki (alfabetycznie A-Z lub Z-A)
  - [ ] Ceny określonego paliwa (rosnąco/malejąco)
  - [ ] Odległości od użytkownika (rosnąco/malejąco)
- [ ] System waliduje poprawność wprowadzonych danych:
  - [ ] Ceny muszą być wartościami liczbowymi
  - [ ] Minimalna cena nie może być większa niż maksymalna
  - [ ] Jeśli ustawiono zakres cen, rodzaj paliwa musi być wybrany
- [ ] Po kliknięciu przycisku „Wyszukaj" system zwraca przefiltrowaną i posortowaną listę stacji.
- [ ] System wyświetla liczbę znalezionych stacji pasujących do filtrów.
- [ ] Jeśli nie znaleziono żadnych stacji, system wyświetla komunikat „Brak wyników — spróbuj zmienić filtry".
- [ ] Użytkownik może zresetować wszystkie filtry i sortowanie jednym kliknięciem przycisku „Wyczyść filtry".

## Scenariusze (Given – When – Then)

### Scenariusz 1: Pomyślne filtrowanie i sortowanie stacji

- **Mając (Given)** użytkownik jest na widoku listy stacji z aktywnym panelem filtrów,
- **I (And)** użytkownik wybrał typ paliwa (np. „ON"), ustawił zakres cen (min 5.50 zł, max 6.00 zł), wybrał markę „Orlen" i promień lokalizacji 10 km,
- **I (And)** użytkownik wybrał sortowanie „Cena ON rosnąco",
- **Kiedy (When)** kliknie przycisk „Wyszukaj",
- **Wtedy (Then)** system zweryfikuje poprawność pól,
- **I (And)** zwróci listę stacji spełniających wszystkie filtry, posortowaną od najtańszej ceny ON (endpoint `POST /api/station/list`),
- **I (And)** wyświetli komunikat „Znaleziono X stacji" na górze listy.

### Scenariusz 2: Błąd walidacji — zakres cen bez wyboru rodzaju paliwa

- **Mając (Given)** użytkownik ustawił minimalną cenę 5.00 zł i maksymalną cenę 6.00 zł,
- **Ale (But)** nie wybrał rodzaju paliwa,
- **Kiedy (When)** kliknie „Wyszukaj",
- **Wtedy (Then)** system nie wykona zapytania i wyświetli komunikat błędu: „Proszę wybrać rodzaj paliwa przy ustawionym zakresie cen",
- **I (And)** podświetli pole wyboru rodzaju paliwa,
- **I (And)** nie pokaże wyników do momentu poprawienia błędu.

### Scenariusz 3: Błąd walidacji — minimalna cena większa niż maksymalna

- **Mając (Given)** użytkownik wybrał rodzaj paliwa „Pb95",
- **I (And)** ustawił minimalną cenę 6.50 zł i maksymalną cenę 5.50 zł,
- **Kiedy (When)** kliknie „Wyszukaj",
- **Wtedy (Then)** system wyświetli komunikat błędu: „Cena minimalna nie może być większa niż maksymalna",
- **I (And)** podświetli pola cen,
- **I (And)** zablokuje wysyłkę zapytania.

### Scenariusz 4: Sortowanie po cenie paliwa

- **Mając (Given)** użytkownik wybrał typ paliwa „Pb95" w filtrach,
- **Kiedy (When)** wybierze sortowanie „Cena Pb95 rosnąco" i kliknie „Wyszukaj",
- **Wtedy (Then)** system wyświetli listę stacji posortowaną od najtańszej do najdroższej ceny Pb95,
- **I (And)** pokaże komunikat z liczbą znalezionych stacji.

### Scenariusz 5: Sortowanie po odległości

- **Mając (Given)** użytkownik udostępnił swoją lokalizację,
- **Kiedy (When)** wybierze sortowanie „Odległość rosnąco" i kliknie „Wyszukaj",
- **Wtedy (Then)** system wyświetli listę stacji posortowaną od najbliższej do najdalszej od użytkownika.

### Scenariusz 6: Filtrowanie po promieniu lokalizacji

- **Mając (Given)** użytkownik udostępnił swoją lokalizację lub podał adres,
- **Kiedy (When)** ustawi promień „5 km" i kliknie „Wyszukaj",
- **Wtedy (Then)** system wyświetli tylko stacje znajdujące się w promieniu 5 km od podanej lokalizacji,
- **I (And)** pokaże komunikat „Znaleziono X stacji w promieniu 5 km".

### Scenariusz 7: Filtrowanie po wielu markach

- **Mając (Given)** użytkownik wybrał marki „Orlen" i „Shell" z wielokrotnego wyboru,
- **Kiedy (When)** kliknie „Wyszukaj",
- **Wtedy (Then)** system wyświetli tylko stacje marek Orlen i Shell.

### Scenariusz 8: Brak wyników wyszukiwania

- **Mając (Given)** użytkownik ustawił bardzo restrykcyjne filtry (np. cena ON poniżej 1.00 zł),
- **Kiedy (When)** kliknie „Wyszukaj",
- **Wtedy (Then)** system nie zwróci żadnych wyników,
- **I (And)** wyświetli komunikat „Brak wyników — spróbuj zmienić filtry",
- **I (And)** pokaże sugestie: „Spróbuj rozszerzyć zakres cen lub promień wyszukiwania".

### Scenariusz 9: Reset filtrów

- **Mając (Given)** użytkownik ma aktywne kilka filtrów i sortowanie,
- **Kiedy (When)** kliknie przycisk „Wyczyść filtry",
- **Wtedy (Then)** wszystkie filtry i sortowanie zostaną zresetowane do wartości domyślnych,
- **I (And)** lista wyświetli ponownie wszystkie dostępne stacje,
- **I (And)** licznik pokaże całkowitą liczbę stacji.

### Scenariusz 10: Filtrowanie po lokalizacji bez geolokalizacji

- **Mając (Given)** użytkownik nie udostępnił geolokalizacji,
- **I (And)** próbuje ustawić filtr promienia lokalizacji,
- **Kiedy (When)** kliknie „Wyszukaj",
- **Wtedy (Then)** system wyświetli komunikat: „Włącz geolokalizację lub podaj adres, aby używać filtra odległości",
- **I (And)** zablokuje aplikację tego filtra.

## Dodatkowe informacje

### Zależności

- Wymaga ukończenia Historyjki 7: Przeglądanie listy stacji benzynowych
- Wymaga endpointu `GET /api/station/all-brands` - lista marek do filtrów
- Wymaga endpointu `GET /api/station/fuel-codes` - lista kodów paliw do filtrów
- Wymaga geolokalizacji użytkownika dla filtrów odległości

### Notatki techniczne

- **Endpointy:**
  - `POST /api/station/list` - główny endpoint z obsługą parametrów filtrowania i sortowania
  - `GET /api/station/all-brands` - lista wszystkich marek do filtrów
  - `GET /api/station/fuel-codes` - lista kodów paliw do filtrów
- **Query parameters dla POST /api/station/list:**
  - `fuelType`: string (np. "ON", "Pb95")
  - `minPrice`: number
  - `maxPrice`: number
  - `brands`: array of strings
  - `radius`: number (w km)
  - `location`: { lat: number, lng: number }
  - `sortBy`: string (np. "price_asc", "distance_asc", "name_asc")
- **State management:** Context API lub Redux do zarządzania filtrów i sortowania
- **Validation:** Biblioteka walidacji (np. Yup, Zod) po stronie frontendu i backendu
- **Performance:** Debouncing dla pól tekstowych w filtrach

### Przypadki brzegowe

- Użytkownik nie ma włączonej geolokalizacji, ale próbuje użyć filtra promienia - wyświetlić komunikat o konieczności włączenia geolokalizacji lub podania adresu
- Użytkownik wprowadził niepoprawny adres przy ręcznym podawaniu lokalizacji - walidacja i komunikat błędu
- Sortowanie po cenie paliwa, którego nie ma w filtrach - system automatycznie dodaje to paliwo do filtrów lub wyświetla komunikat
- Bardzo duża liczba stacji po filtrowaniu (200+) - implementacja paginacji
- Brak dostępnych marek w systemie - wyświetlić komunikat w sekcji filtra marek
- Użytkownik zmienia filtry wielokrotnie bez klikania „Wyszukaj" - opcjonalnie: auto-refresh z debouncing
- Wartości nieliczbowe w polach cen - blokada wprowadzania znaków innych niż cyfry i przecinek/kropka
- Timeout API podczas filtrowania - wyświetlić komunikat błędu i możliwość ponowienia

## Priorytet i estymacja

- **Priorytet:** Must Have (kluczowa funkcjonalność dla UX - porównywarka cen)
- **Estymacja (Story Points):** 5
