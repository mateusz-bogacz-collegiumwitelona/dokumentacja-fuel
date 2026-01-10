# Historyjka: Filtrowanie stacji benzynowych na mapie

## Opis

**Jako** zalogowany użytkownik aplikacji,  
**chcę** móc filtrować stacje benzynowe na mapie według wybranych kryteriów,  
**aby** szybko znaleźć stację odpowiadającą moim konkretnym potrzebom (marka, odległość).

## Kryteria akceptacji

- [ ] Użytkownik widzi panel filtrów nad lub obok mapy.
- [ ] Użytkownik może filtrować stacje według:
  - [ ] Marki stacji (wielokrotny wybór, np. Orlen, Shell, BP)
  - [ ] Odległości od aktualnej lokalizacji
- [ ] Po zastosowaniu filtrów mapa pokazuje tylko stacje spełniające wybrane kryteria.
- [ ] System wyświetla liczbę znalezionych stacji pasujących do filtrów (np. "Znaleziono 15 stacji").
- [ ] Użytkownik może zresetować wszystkie filtry jednym kliknięciem przycisku "Wyczyść filtry".
- [ ] Filtry są zachowane podczas nawigacji w obrębie mapy (przesuwanie, zoom).
- [ ] Po zastosowaniu filtrów markery na mapie aktualizują się w czasie rzeczywistym.
- [ ] System wyświetla komunikat, jeśli żadna stacja nie spełnia wybranych kryteriów.

## Scenariusze (Given – When – Then)

### Scenariusz 1: Filtrowanie stacji według marki

- **Mając (Given)** użytkownik widzi mapę ze wszystkimi stacjami,
- **Kiedy (When)** wybierze markę "Orlen" z listy filtrów,
- **Wtedy (Then)** mapa wyświetli tylko stacje marki Orlen, a licznik pokaże liczbę znalezionych stacji (np. "Znaleziono 12 stacji").

### Scenariusz 2: Filtrowanie według wielu marek

- **Mając (Given)** użytkownik zastosował filtr marki "Orlen",
- **Kiedy (When)** dodatkowo zaznaczy markę "Shell",
- **Wtedy (Then)** mapa wyświetli stacje obu marek, a licznik zaktualizuje się odpowiednio.

### Scenariusz 3: Filtrowanie według odległości

- **Mając (Given)** użytkownik ma włączoną geolokalizację,
- **Kiedy (When)** wybierze filtr "Do 5 km",
- **Wtedy (Then)** mapa wyświetli tylko stacje znajdujące się w promieniu 5 km od użytkownika.

### Scenariusz 4: Brak wyników filtrowania

- **Mając (Given)** użytkownik zastosował bardzo restrykcyjne filtry (np. marka "BP" w promieniu 1 km),
- **Kiedy (When)** żadna stacja nie spełnia kryteriów,
- **Wtedy (Then)** system wyświetli komunikat "Nie znaleziono stacji spełniających wybrane kryteria. Spróbuj rozszerzyć filtry" i pokaże "Znaleziono 0 stacji".

### Scenariusz 5: Reset filtrów

- **Mając (Given)** użytkownik ma aktywne kilka filtrów,
- **Kiedy (When)** kliknie przycisk "Wyczyść filtry",
- **Wtedy (Then)** wszystkie filtry zostaną usunięte, mapa wyświetli ponownie wszystkie stacje, a licznik pokaże całkowitą liczbę dostępnych stacji.

### Scenariusz 6: Kombinacja filtrów

- **Mając (Given)** użytkownik zastosował filtr marki "Orlen" i odległości "Do 10 km",
- **Kiedy (When)** oba filtry są aktywne,
- **Wtedy (Then)** mapa wyświetli tylko stacje Orlen znajdujące się w promieniu 10 km od użytkownika.

## Dodatkowe informacje

### Zależności

- Wymaga ukończenia Historyjki 3: Przeglądanie mapy stacji benzynowych
- Wymaga endpointu `GET /api/station/all-brands` - lista wszystkich marek do filtrów
- Wymaga geolokalizacji użytkownika dla filtra odległości

### Notatki techniczne

- **Endpointy:**
  - `GET /api/station/all-brands` - pobieranie listy wszystkich marek stacji
  - Filtrowanie może być realizowane po stronie frontendu (na pobranych danych z `POST /api/station/map/all`) lub przez dedykowany endpoint z parametrami query
- **State management:** Rozważyć użycie Context API lub Redux do zarządzania stanem filtrów
- **Performance:** Optymalizacja renderowania markerów przy dużej liczbie stacji (clustering)
- **UX:** Dodać debouncing dla filtrów, aby uniknąć zbyt częstych rerenderów mapy

### Przypadki brzegowe

- Użytkownik nie ma włączonej geolokalizacji, ale próbuje użyć filtra odległości - wyświetlić komunikat "Włącz geolokalizację, aby używać filtra odległości"
- Brak dostępnych marek w systemie - wyświetlić komunikat "Brak dostępnych marek do filtrowania"
- Użytkownik zmienia lokalizację podczas aktywnego filtra odległości - automatycznie przeliczyć odległości i zaktualizować wyniki
- Bardzo duża liczba stacji (1000+) - implementacja clusteringu markerów dla lepszej wydajności
- Filtry zwracają tylko 1-2 stacje - upewnić się, że są dobrze widoczne na mapie (odpowiedni zoom)

## Priorytet i estymacja

- **Priorytet:** Must Have (kluczowa funkcjonalność dla UX)
- **Estymacja (Story Points):** 3
