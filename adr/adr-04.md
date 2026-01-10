# ADR-004: Wybór rozwiązania mapowego

- **Status:** Zaakceptowana
- **Data:** 2025-10-06
- **Autorzy:** Mateusz Bogacz-Drewniak, Mateusz Chimkowski, Paweł Krók

---

## 1. Kontekst

Nasz projekt porównywarki cen paliw wymaga integracje z mapami po stronie clienta. Jest to kluczowy elementem interfejsu użytkownika, pozwalającym na wizualizacje dostępnych stacji paliw.

**Kluczowe wymagania:**

- **Interaktywność**: Użytkownicy muszą mieć możliwość przesuwania mapy (panning) i przybliżania/oddalania (zooming).
- **Wyświetlanie znaczników (Markers)**: Mapa musi obsługiwać wyświetlanie pinezek reprezentujących stacje paliw.
- **Koszty**: Ze względu na to że jest to projekt studencki, koszty muszą być jak najniższe
- **Technologia**: Rozwiązanie musi być łatwe do zintegrowania z naszym frontendem opartym na JavaScript (React/Vue).

---

## 2. Rozważane opcje

### Opcja 1: Leaflet + OpenStreetMap (OSM)

- **Zalety:**
  - [+] Leaflet i OSM są darmowe
  - [+] Nie jesteśmy uzależnieni od jednego komercyjnego dostawcy. Jeśli domyślne serwery OSM staną się niewydajne, możemy łatwo przełączyć źródło kafelków (np. na Mapbox, Stadia Maps) bez zmiany biblioteki Leaflet.
  - [+] Dobra dokumentacja
  - [+] Zespół zna tę technologię, co przyspieszy prace.
  - [+] Duża liczba wtyczek
  - [+] Leaflet jest bardzo mały i szybki
  - [+] Łatwa integracja z React
- **Wady:**
  - [-] Trzeba mocno wystylizować by wyglądało nowocześnie

### Opcja 2: Google Maps JavaScript API

- **Zalety:**
  - [+] Google posiada bardzo dokładne dane mapowe, w tym zdjęcia satelitarne i Street View, oraz doskonałe pokrycie globalne.
  - [+] Użytkownicy są przyzwyczajeni do wyglądu i działania map Google.
  - [+] Łatwy dostęp do dodatkowych płatnych usług, takich jak autouzupełnianie adresów (Places API), wyznaczanie tras czy informacje o ruchu drogowym.
  - [+] Bardzo wysoka dostępność i szybkość ładowania map
- **Wady:**
  - [-] Model płatności "pay-as-you-go" może być kosztowny przy dużej skali ruchu. Wymaga podpięcia karty kredytowej od samego początku.
  - [-] Użycie danych Google jest ściśle powiązane z ich mapą. Istnieją ograniczenia dotyczące cache'owania danych i łączenia ich z innymi źródłami.
  - [-] Chociaż możliwa jest stylizacja mapy, jest ona bardziej ograniczona niż w przypadku rozwiązań opartych na Leaflet/OSM.

---

## 3. Decyzja

**Wybieramy Leaflet + OpenStreetMap jako główne rozwiązanie mapowe dla naszego projektu.**

---

## 4. Uzasadnienie

Wybraliśmy Leaflet w połączeniu z danymi OpenStreetMap, ponieważ:

1. **Spełnia wymagania funkcjonalne**: Zestaw ten w pełni umożliwia realizację interaktywnej mapy z niestandardowymi znacznikami nieruchomości i dymkami informacyjnymi.
2. **Optymalizacja kosztów**: Na etapie startu projektu kluczowe jest unikanie stałych wysokich kosztów. Rozwiązanie to pozwala nam wystartować za darmo.
3. **Kontrola i elastyczność**: Zespół deweloperski ceni sobie otwartość biblioteki Leaflet. Daje nam to pewność, że w razie potrzeby możemy dostosować rozwiązanie pod siebie, a w przyszłości – jeśli ruch wzrośnie – łatwo zmienić dostawcę warstwy kafelkowej (np. na płatny plan Mapbox) bez konieczności przepisywania całego kodu frontendowego odpowiedzialnego za obsługę mapy.
4. **Wystarczająca jakość danych**: Do celów wyświetlania lokalizacji nieruchomości w obszarach zurbanizowanych dokładność danych OpenStreetMap jest w zupełności wystarczająca.

---

## 5. Konsekwencje

### Pozytywne

- Brak początkowych kosztów licencyjnych za korzystanie z mapy.
- Pełna kontrola nad kodem źródłowym komponentu mapy dzięki otwartej bibliotece.
- Możliwość łatwej zmiany dostawcy kafelków w przyszłości bez zmiany biblioteki TS.

### Negatywne

- Może być wymagany dodatkowy czas pracy frontendowców na dostosowanie stylu mapy i znaczników, aby osiągnąć "nowoczesny" wygląd, który Google oferuje "z pudełka"
