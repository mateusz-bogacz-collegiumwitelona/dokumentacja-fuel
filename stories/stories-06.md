# Historyjka: Weryfikacja i zmiana statusu propozycji cen przez administratora

## Opis

**Jako** administrator aplikacji,  
**chcę** móc zatwierdzać lub odrzucać propozycje cen paliw od użytkowników,  
**aby** utrzymać wysoką jakość danych w systemie i nagradzać użytkowników za wartościowe zgłoszenia.

## Kryteria akceptacji

- [ ] Administrator może zmienić status propozycji na "zaakceptowana" lub "odrzucona" z poziomu szczegółów propozycji.
- [ ] Po zaakceptowaniu propozycji:
  - [ ] Ceny w systemie zostają zaktualizowane dla danej stacji
  - [ ] Użytkownik automatycznie otrzymuje punkty na swoje konto
  - [ ] Status propozycji zmienia się na "zaakceptowana"
  - [ ] System wyświetla komunikat potwierdzający: "Propozycja zaakceptowana, użytkownik otrzymał punkty"
- [ ] Po odrzuceniu propozycji:
  - [ ] Status zmienia się na "odrzucona"
  - [ ] Ceny w systemie pozostają bez zmian
  - [ ] Użytkownik nie otrzymuje punktów
  - [ ] System wyświetla komunikat: "Propozycja odrzucona"
- [ ] Administrator nie może zmienić statusu propozycji, która już została przetworzona (zaakceptowana lub odrzucona).
- [ ] Wszystkie akcje administratora są logowane w systemie (kto, kiedy, jaką decyzję podjął).
- [ ] System waliduje, czy administrator ma odpowiednie uprawnienia przed wykonaniem akcji.

## Scenariusze (Given – When – Then)

### Scenariusz 1: Pomyślne zaakceptowanie propozycji

- **Mając (Given)** administrator otworzył szczegóły propozycji ze statusem "oczekująca",
- **Kiedy (When)** zweryfikuje dane, sprawdzi zdjęcie i kliknie "Zaakceptuj",
- **Wtedy (Then)** system:
  - Wyśle żądanie `PATCH /api/admin/proposal/change-status` ze statusem "true"
  - Zaktualizuje ceny dla danej stacji
  - Doda punkty użytkownikowi
  - Wyświetli komunikat "Propozycja zaakceptowana, użytkownik otrzymał punkty"
  - Zmieni status propozycji na "zaakceptowana"

### Scenariusz 2: Odrzucenie propozycji z nieprawidłowymi danymi

- **Mając (Given)** administrator otworzył propozycję z podejrzanymi cenami lub nieczytelnym zdjęciem,
- **Kiedy (When)** kliknie "Odrzuć",
- **Wtedy (Then)** system:
  - Wyśle żądanie `PATCH /api/admin/proposal/change-status` ze statusem "false"
  - Zmieni status propozycji na "odrzucona"
  - Wyświetli komunikat "Propozycja odrzucona"
  - Nie zaktualizuje cen ani nie nada punktów użytkownikowi

### Scenariusz 3: Próba zmiany statusu już przetworzonej propozycji

- **Mając (Given)** administrator otworzył propozycję ze statusem "zaakceptowana",
- **Kiedy (When)** spróbuje kliknąć "Zaakceptuj" lub "Odrzuć",
- **Wtedy (Then)** przyciski akcji są nieaktywne/niewidoczne, a system wyświetla informację "Propozycja została już przetworzona".

### Scenariusz 4: Konflikt - propozycja przetworzona przez innego admina

- **Mając (Given)** administrator przegląda propozycję ze statusem "oczekująca",
- **Kiedy (When)** inny administrator przetworzy tę propozycję w międzyczasie, a pierwszy admin kliknie akcję,
- **Wtedy (Then)** system wyświetli komunikat "Propozycja została już przetworzona przez innego administratora" i odświeży dane.

### Scenariusz 5: Błąd podczas akceptacji

- **Mając (Given)** administrator próbuje zaakceptować propozycję,
- **Kiedy (When)** wystąpi błąd połączenia lub błąd serwera,
- **Wtedy (Then)** system wyświetli komunikat błędu "Nie udało się przetworzyć propozycji. Spróbuj ponownie" i umożliwi ponowną próbę.

### Scenariusz 6: Akceptacja propozycji dla usuniętej stacji

- **Mając (Given)** administrator próbuje zaakceptować propozycję dla stacji, która została usunięta,
- **Kiedy (When)** kliknie "Zaakceptuj",
- **Wtedy (Then)** system wyświetli błąd "Stacja nie istnieje. Propozycja nie może być zaakceptowana" i zachowa status "oczekująca".

## Dodatkowe informacje

### Zależności

- Wymaga ukończenia Historyjki 5: Przeglądanie i filtrowanie propozycji cen przez administratora
- Wymaga systemu punktów dla użytkowników
- Wymaga mechanizmu aktualizacji cen stacji
- Wymaga systemu logowania akcji administracyjnych (audit log)

### Notatki techniczne

- **Endpointy:**
  - `PATCH /api/admin/proposal/change-status` - zmiana statusu propozycji
    - Body: `{ proposalId: string, status: boolean }` (true = zaakceptowana, false = odrzucona)
    - Response: `{ success: boolean, message: string, updatedProposal: Proposal }`
- **Autoryzacja:** Middleware sprawdzający rolę "ADMIN"
- **Transakcje:** Akceptacja propozycji powinna być operacją atomową (aktualizacja cen + dodanie punktów + zmiana statusu)
- **Audit Log:** Każda akcja zapisywana z danymi: admin_id, proposal_id, action, timestamp
- **Liczba punktów:**Stała liczba 1 pkt

### Przypadki brzegowe

- Propozycja została już przetworzona przez innego admina (konflikt) - wyświetlić komunikat i odświeżyć dane
- Użytkownik usunął swoje konto po dodaniu propozycji - propozycja może być przetworzona, ale punkty nie zostaną przyznane
- Stacja została usunięta - zablokować akceptację i wyświetlić błąd
- Brak połączenia podczas akceptacji - wyświetlić błąd i umożliwić ponowną próbę
- Awaria systemu punktowego - zalogować błąd, ale nie blokować całkowicie akceptacji (punkty mogą być dodane manualnie później)
- Administrator próbuje zmienić status propozycji, która już ma status "zaakceptowana" lub "odrzucona" - system blokuje i informuje o konflikcie
- Równoczesna akceptacja tej samej propozycji przez dwóch adminów - pierwszy wygrywa, drugi otrzymuje błąd konfliktu

## Priorytet i estymacja

- **Priorytet:** Must Have (kluczowa funkcjonalność dla moderacji)
- **Estymacja (Story Points):** 5
