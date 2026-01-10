# Historyjka: Weryfikacja i punktowanie propozycji cenowych

## Opis

**Jako** administrator aplikacji,  
**chcę** móc weryfikować propozycje zmian cen paliw zgłoszone przez użytkowników,  
**aby** utrzymać wysoką jakość danych w aplikacji i nagradzać użytkowników za wartościowe zgłoszenia.

## Kryteria akceptacji

- [ ] Administrator widzi listę oczekujących propozycji w panelu administracyjnym.
- [ ] Każda propozycja zawiera: dane użytkownika, nazwę stacji, zaproponowane ceny paliw oraz zdjęcie potwierdzające.
- [ ] Administrator może zaakceptować propozycję — wtedy nowe ceny są aktualizowane w systemie.
- [ ] Administrator może odrzucić propozycję z opcjonalnym powodem odrzucenia.
- [ ] Po akceptacji propozycji użytkownik automatycznie otrzymuje punkty na swoje konto.
- [ ] Użytkownik otrzymuje powiadomienie o statusie swojej propozycji (zaakceptowana/odrzucona).
- [ ] W przypadku odrzucenia, użytkownik widzi powód (jeśli został podany).
- [ ] System loguje wszystkie akcje administratora (kto, kiedy, jaką decyzję podjął).
- [ ] Panel pokazuje historię wszystkich rozpatrzonych propozycji z możliwością filtrowania.

## Scenariusze (Given – When – Then)

### Scenariusz 1: Akceptacja propozycji

- **Mając (Given)** administrator zalogowany w panelu z listą oczekujących propozycji,
- **Kiedy (When)** kliknie „Zaakceptuj" przy wybranej propozycji,
- **Wtedy (Then)** ceny paliw na stacji zostaną zaktualizowane, użytkownik otrzyma punkty i powiadomienie o akceptacji.

### Scenariusz 2: Odrzucenie propozycji z powodem

- **Mając (Given)** administrator przegląda propozycję z błędnymi danymi,
- **Kiedy (When)** kliknie „Odrzuć", wprowadzi powód (np. „Nieczytelne zdjęcie") i potwierdzi,
- **Wtedy (Then)** propozycja zostanie odrzucona, a użytkownik otrzyma powiadomienie z podanym powodem.

### Scenariusz 3: Filtrowanie propozycji

- **Mając (Given)** administrator ma dostęp do panelu z wieloma propozycjami,
- **Kiedy (When)** zastosuje filtr (np. „tylko oczekujące" lub „odrzucone w ostatnim tygodniu"),
- **Wtedy (Then)** lista propozycji zostanie zawężona zgodnie z wybranym filtrem.

### Scenariusz 4: Brak uprawnień administratora

- **Mając (Given)** zwykły użytkownik próbuje uzyskać dostęp do panelu administracyjnego,
- **Kiedy (When)** otworzy URL panelu,
- **Wtedy (Then)** system wyświetli komunikat „Brak uprawnień" i przekieruje go do strony głównej.

## Dodatkowe informacje

### Zależności

- Wymaga ukończenia Historyjki 1: Dodawanie propozycji zmiany ceny paliwa
- Wymaga systemu uprawnień (role: admin, użytkownik)
- Wymaga systemu punktowego dla użytkowników

### Notatki techniczne

- Panel administracyjny dostępny pod `/admin/proposals`
- Endpoint API: `PATCH /api/proposals/:id/verify` (body: `{status: 'accepted'|'rejected', reason?: string}`)
- System powiadomień (e-mail lub push notifications)
- Audit log dla akcji administratora
- Liczba punktów za zaakceptowaną propozycję: 10 punktów (do konfiguracji)

### Przypadki brzegowe

- Propozycja została już rozpatrzona przez innego administratora — wyświetlić ostrzeżenie
- Użytkownik zgłaszający propozycję usunął konto — propozycja pozostaje, ale bez możliwości przyznania punktów
- Administrator przypadkowo zaakceptował błędną propozycję — dodać funkcję „Cofnij" (do 5 minut od akcji)
- Awaria systemu punktowego — zalogować błąd, ale nie blokować akceptacji propozycji

## Priorytet i estymacja

- **Priorytet:** Must Have
- **Estymacja (Story Points):** 5
