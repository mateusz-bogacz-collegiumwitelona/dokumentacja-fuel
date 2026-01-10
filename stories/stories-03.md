# Historyjka: Przeglądanie mapy stacji benzynowych

## Opis

**Jako** zalogowany użytkownik aplikacji,  
**chcę** przeglądać interaktywną mapę stacji benzynowych,  
**aby** szybko znaleźć najbliższą stację i zobaczyć jej podstawowe informacje.

## Kryteria akceptacji

- [ ] Użytkownik widzi mapę z zaznaczonymi wszystkimi stacjami benzynowymi w obszarze widoku.
- [ ] Każda stacja jest reprezentowana przez marker (np. Orlen, Shell, BP).
- [ ] Użytkownik może kliknąć w marker stacji, aby zobaczyć podstawowe informacje (nazwa, adres, aktualne ceny).
- [ ] Mapa automatycznie centruje się na lokalizacji użytkownika (jeśli wyrazi zgodę na udostępnienie lokalizacji).
- [ ] Po załadowaniu mapy system wyświetla 3 najbliższe stacje od użytkownika.
- [ ] Użytkownik może kliknąć przycisk "Zobacz szczegóły" w popup'ie, aby przejść do pełnego profilu stacji.
- [ ] Mapa jest responsywna i działa zarówno na desktopie, jak i urządzeniach mobilnych.
- [ ] System wyświetla loader podczas ładowania danych map.

## Scenariusze (Given – When – Then)

### Scenariusz 1: Wyświetlenie wszystkich stacji na mapie

- **Mając (Given)** użytkownik jest zalogowany i otworzył widok mapy,
- **Kiedy (When)** mapa się załaduje,
- **Wtedy (Then)** system wyświetli wszystkie stacje benzynowe z danymi pobranymi z endpointu `POST /api/station/map/all`.

### Scenariusz 2: Wyświetlenie szczegółów stacji w popup

- **Mając (Given)** użytkownik widzi markery stacji na mapie,
- **Kiedy (When)** kliknie w marker konkretnej stacji,
- **Wtedy (Then)** system wyświetli popup z podstawowymi informacjami (nazwa, adres, aktualne ceny paliw, data ostatniej aktualizacji).

### Scenariusz 3: Przejście do profilu stacji

- **Mając (Given)** użytkownik otworzył popup ze szczegółami stacji,
- **Kiedy (When)** kliknie przycisk "Zobacz szczegóły",
- **Wtedy (Then)** system przekieruje go do pełnego profilu stacji (endpoint `GET /api/station/profile`).

### Scenariusz 4: Centrowanie mapy na lokalizacji użytkownika

- **Mając (Given)** użytkownik wyraził zgodę na udostępnienie lokalizacji,
- **Kiedy (When)** mapa się załaduje,
- **Wtedy (Then)** system automatycznie wycentruje mapę na aktualnej lokalizacji użytkownika i pokaże 3 najbliższe stacje (endpoint `GET /api/station/map/nearest`).

### Scenariusz 5: Brak zgody na geolokalizację

- **Mając (Given)** użytkownik nie wyraził zgody na udostępnienie lokalizacji,
- **Kiedy (When)** mapa się załaduje,
- **Wtedy (Then)** system wycentruje mapę na domyślnej lokalizacji (np. centrum Polski) i wyświetli wszystkie dostępne stacje.

### Scenariusz 6: Stacja bez aktualnych cen

- **Mając (Given)** użytkownik kliknął w marker stacji, która nie ma aktualnych danych cenowych,
- **Kiedy (When)** popup się otworzy,
- **Wtedy (Then)** system wyświetli informację "Brak aktualnych danych cenowych" zamiast cen.

## Dodatkowe informacje

### Zależności

- Wymaga systemu logowania i autoryzacji
- Wymaga endpointu `POST /api/station/map/all`
- Wymaga endpointu `GET /api/station/map/nearest`
- Wymaga endpointu `GET /api/station/profile`

### Notatki techniczne

- **Endpointy:**
  - `POST /api/station/map/all` - pobieranie danych wszystkich stacji do mapy
  - `GET /api/station/map/nearest` - pobieranie 3 najbliższych stacji
  - `GET /api/station/profile` - szczegóły wybranej stacji
- **Biblioteka map:** Leaflet lub Google Maps (do ustalenia z zespołem)
- **Geolokalizacja:** Browser Geolocation API
- **Caching:** Rozważyć cache'owanie danych map w Redis dla lepszej wydajności
- **Ikonki markerów:** Przygotować zestaw ikon dla różnych marek stacji

### Przypadki brzegowe

- Użytkownik nie wyraził zgody na udostępnienie lokalizacji - mapa centruje się na domyślnej lokalizacji (np. centrum Polski)
- Brak połączenia z internetem - wyświetlenie komunikatu o braku dostępu do danych
- Długi czas ładowania danych map - wyświetlenie loadera/spinnera z komunikatem "Ładowanie stacji..."
- Stacja nie ma aktualnych cen - wyświetlenie informacji "Brak aktualnych danych cenowych"
- API geolokalizacji nie jest dostępne w przeglądarce - pominąć centrowanie i użyć domyślnej lokalizacji

## Priorytet i estymacja

- **Priorytet:** Must Have (kluczowa funkcjonalność aplikacji)
- **Estymacja (Story Points):** 5
