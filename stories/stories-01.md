# Historyjka: Dodawanie propozycji zmiany ceny paliwa

## Opis

**Jako** zarejestrowany użytkownik aplikacji,  
**chcę** móc zaproponować aktualizację cen paliw na wybranej stacji benzynowej,  
**aby** pomóc innym kierowcom w znalezieniu najtańszego paliwa i zdobyć punkty za swój wkład.

## Kryteria akceptacji

- [ ] Użytkownik może dodać propozycję zmiany ceny paliwa z widoku wysyłania propozycji.
- [ ] Użytkownik widzi listę rodzajów paliw dostępnych na danej stacji wraz z polami liczbowymi do wprowadzenia nowych cen.
- [ ] Użytkownik może dodać jedno zdjęcie potwierdzające przedstawione ceny.
- [ ] System waliduje pola — nie można wprowadzić wartości ujemnych, pustych ani nieliczbowych.
- [ ] System waliduje zdjęcie — musi być w poprawnym formacie (np. JPG/PNG) i mieścić się w limicie rozmiaru.
- [ ] Po kliknięciu „Wyślij propozycję" formularz trafia do backendu poprzez REST API.
- [ ] Użytkownik otrzymuje komunikat potwierdzający pomyślne wysłanie propozycji.
- [ ] Jeśli którekolwiek pole jest błędne, system blokuje wysyłkę i informuje o błędzie.
- [ ] W przypadku braku uprawnień (np. użytkownik niezalogowany) system przekierowuje go do logowania.

## Scenariusze (Given – When – Then)

### Scenariusz 1: Pomyślne wysłanie propozycji

- **Mając (Given)** użytkownik jest zalogowany i otworzył widok wysyłania propozycji dla konkretnej stacji,
- **Kiedy (When)** wprowadzi ceny paliw, doda zdjęcie i kliknie „Wyślij",
- **Wtedy (Then)** system zapisze propozycję, wyświetli komunikat sukcesu i przekaże propozycję do dalszego przetworzenia.

### Scenariusz 2: Błąd walidacji ceny

- **Mając (Given)** użytkownik wprowadzi wartość nieliczbową lub ujemną,
- **Kiedy (When)** kliknie „Wyślij",
- **Wtedy (Then)** system wyświetli błąd walidacji i nie wyśle formularza.

### Scenariusz 3: Brak zdjęcia

- **Mając (Given)** użytkownik nie doda zdjęcia,
- **Kiedy (When)** kliknie „Wyślij",
- **Wtedy (Then)** system zwróci komunikat „Zdjęcie jest wymagane" i zablokuje wysyłkę.

### Scenariusz 4: Użytkownik niezalogowany

- **Mając (Given)** użytkownik nie jest zalogowany,
- **Kiedy (When)** spróbuje otworzyć widok wysyłania propozycji,
- **Wtedy (Then)** system przekieruje go do strony logowania.

## Dodatkowe informacje

### Zależności

- Wymaga istnienia systemu autoryzacji użytkowników
- Wymaga API do obsługi stacji benzynowych i rodzajów paliw

### Notatki techniczne

- Integracja z REST API do wysyłania propozycji (endpoint: `POST /api/proposals`)
- Walidacja po stronie frontendu i backendu
- Obsługa uploadu zdjęć (format: JPG/PNG, limit rozmiaru: 5MB)
- Biblioteka do walidacji formularzy (np. Formik, React Hook Form)

### Przypadki brzegowe

- Brak połączenia z internetem podczas wysyłania — wyświetlić komunikat o błędzie połączenia
- Limit rozmiaru zdjęcia przekroczony — wyświetlić komunikat przed próbą wysyłki
- Sesja użytkownika wygasła podczas wypełniania formularza — zapisać dane w localStorage i przekierować do logowania
- Użytkownik próbuje wysłać propozycję dla nieistniejącej stacji — zwrócić błąd 404

## Priorytet i estymacja

- **Priorytet:** Must Have
- **Estymacja (Story Points):** 5
