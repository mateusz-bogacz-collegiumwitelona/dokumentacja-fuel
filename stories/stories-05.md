# Historyjka: Przeglądanie i filtrowanie propozycji cen przez administratora

## Opis

**Jako** administrator aplikacji,  
**chcę** przeglądać listę propozycji cen paliw od użytkowników z możliwością filtrowania i sortowania,  
**aby** efektywnie zarządzać dużą liczbą zgłoszeń i szybko znajdować propozycje wymagające weryfikacji.

## Kryteria akceptacji

- [ ] Administrator widzi listę wszystkich propozycji cen z podstawowymi informacjami (użytkownik, stacja, data dodania, status).
- [ ] Administrator może filtrować propozycje według:
  - [ ] Daty dodania
  - [ ] Użytkownika
  - [ ] Stacji benzynowej
  - [ ] Statusu (oczekująca, zaakceptowana, odrzucona)
- [ ] Administrator może sortować listę według daty, użytkownika lub stacji.
- [ ] Administrator może kliknąć w propozycję, aby zobaczyć wszystkie szczegóły:
  - [ ] Dane użytkownika, który dodał propozycję
  - [ ] Nazwę i adres stacji
  - [ ] Proponowane ceny dla każdego rodzaju paliwa
  - [ ] Zdjęcie potwierdzające ceny (jeśli zostało dodane)
  - [ ] Datę i godzinę dodania propozycji
  - [ ] Aktualny status propozycji
- [ ] Administrator może otworzyć zdjęcie w pełnym rozmiarze w nowym oknie/modalu.
- [ ] System wyświetla liczbę propozycji oczekujących na weryfikację.
- [ ] System zabezpiecza endpoint przed nieautoryzowanym dostępem (tylko admin).

## Scenariusze (Given – When – Then)

### Scenariusz 1: Przeglądanie listy propozycji

- **Mając (Given)** administrator jest zalogowany i otworzył panel administracyjny,
- **Kiedy (When)** przejdzie do sekcji "Propozycje cen",
- **Wtedy (Then)** system wyświetli listę wszystkich propozycji pobranych z endpointu `GET /api/admin/proposal/list`.

### Scenariusz 2: Wyświetlenie szczegółów propozycji

- **Mając (Given)** administrator widzi listę propozycji,
- **Kiedy (When)** kliknie w konkretną propozycję,
- **Wtedy (Then)** system wyświetli panel ze wszystkimi szczegółami: dane użytkownika, stacja, proponowane ceny, zdjęcie potwierdzające (pobrane przez `GET /api/admin/proposal?token=abc123def456`).

### Scenariusz 3: Filtrowanie propozycji według statusu

- **Mając (Given)** administrator widzi listę wszystkich propozycji,
- **Kiedy (When)** wybierze filtr "Status: Oczekująca",
- **Wtedy (Then)** system wyświetli tylko propozycje ze statusem "oczekująca", a licznik pokaże ich liczbę.

### Scenariusz 4: Sortowanie propozycji według daty

- **Mając (Given)** administrator widzi nieposortowaną listę propozycji,
- **Kiedy (When)** kliknie nagłówek kolumny "Data dodania",
- **Wtedy (Then)** lista zostanie posortowana od najnowszych do najstarszych (lub odwrotnie przy ponownym kliknięciu).

### Scenariusz 5: Przeglądanie zdjęcia w pełnym rozmiarze

- **Mając (Given)** administrator przegląda szczegóły propozycji ze zdjęciem,
- **Kiedy (When)** kliknie na miniaturę zdjęcia,
- **Wtedy (Then)** zdjęcie otworzy się w pełnym rozmiarze w modalu/nowym oknie, umożliwiając dokładną weryfikację cen.

### Scenariusz 6: Próba dostępu przez zwykłego użytkownika

- **Mając (Given)** zalogowany użytkownik bez uprawnień administratora,
- **Kiedy (When)** spróbuje uzyskać dostęp do `GET /api/admin/proposal/list`,
- **Wtedy (Then)** system zwróci błąd 403 Forbidden i przekieruje do strony głównej z komunikatem "Brak uprawnień".

### Scenariusz 7: Wyświetlanie statystyk

- **Mając (Given)** administrator otworzył panel propozycji,
- **Kiedy (When)** strona się załaduje,
- **Wtedy (Then)** system wyświetli podstawowe statystyki pobrane z `GET /api/proposal-statistic` (np. liczba oczekujących, zaakceptowanych dzisiaj, odrzuconych).

## Dodatkowe informacje

### Zależności

- Wymaga systemu ról i uprawnień (ADMIN)
- Wymaga endpointu dodawania propozycji przez użytkowników (Historia 1)
- Wymaga przechowywania obrazów w Azure Blob

### Notatki techniczne

- **Endpointy:**
  - `GET /api/admin/proposal/list` - lista wszystkich propozycji
  - `GET /api/admin/proposal?token=abc123def456` - szczegóły propozycji przez token
  - `GET /api/proposal-statistic` - statystyki dla dashboardu
- **Autoryzacja:** Middleware sprawdzający rolę "ADMIN"
- **Obrazy:** Integracja z Azure Blob do wyświetlania zdjęć
- **Paginacja:** Rozważyć paginację dla dużej liczby propozycji (np. 50 na stronę)

### Przypadki brzegowe

- Zdjęcie nie załadowało się/zostało usunięte - wyświetlić placeholder z informacją "Brak zdjęcia"
- Użytkownik usunął swoje konto po dodaniu propozycji - propozycja nadal widoczna z oznaczeniem "użytkownik usunięty"
- Stacja została usunięta - propozycja pozostaje z historycznymi danymi stacji
- Bardzo duża liczba propozycji (1000+) - implementacja paginacji i lazy loading
- Brak propozycji w systemie - wyświetlić komunikat "Brak propozycji do wyświetlenia"
- Wolne połączenie - pokazać loader podczas ładowania listy

## Priorytet i estymacja

- **Priorytet:** Must Have (podstawa dla moderacji treści)
- **Estymacja (Story Points):** 3
