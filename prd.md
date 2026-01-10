# PRD: Fuel APP

- **Autor:** Mateusz Bogacz-Drewniak - Team Leader
- **Data:** 2025-10-06
- **Status:** Zatwierdzony
- **Zespół:**
  - Mateusz Bogacz-Drewniak – Team Leader / Backend Developer / DevOps
  - Szymon Mikołajek – Tester / Project Manager
  - Mateusz Chimkowski – Frontend Developer / UX/UI Designer
  - Paweł Kruk – Frontend Developer / DevOps
  - Michał Nocuń - Tester

---

## 1. Podsumowanie

> Jest to aplikacja pozwalająca zgłaszać ceny paliw na stacjach, pozwalając znależć najniższą cene za dany rodzaj paliwa w danej okolicy.Jest przeznaczona dla osób często jeżdzących w delegacje, kierowców tirów, osób chcących zaoszczędzić.

---

## 2. Opis Problemu

### Jaki problem rozwiązujemy?

> Większośc aplikacji bazuje na cenach paliw podanych przez zewnętrzne API, które mają problemy z dokładnością danych. Nasza aplikacja pozwala na przesłanie ceny przez użytykownika co zapewnia najdokladniejsze dane.

### Dla kogo jest ten produkt?

- **Główna persona:** Anna, przedsotawicielka handlowa
- **Główna persona:** Jan, kierowca tira
- **Poboczne persony:** Marek, student informatyki chcący zaoszczędzić

---

## 3. Cele i Mierniki Sukcesu

### Cele projektu

- **Cel 1:** Ułatwienei możliwości znalezienia najtańszego paliwa w danej okolicy
- **Cel 2:** Stworzenie szybko reagującej społeczności
- **Cel 3:** Stworzenie alternatywy dla istniejących rozwiązań

### Mierniki sukcesu

- **Miernik 1:** Liczba aktywnych użytkowników tygodniowo (WAU) na poziomie 100
- **Miernik 2:** Średni czas wyszukiwania stacji 30 sekund
- **Miernik 3:** Średnia ilośc dziennych zgłoszeń nowej centy wpowyżej 10000

---

## 4. Wymagania i Zakres

### Kluczowe historyjki użytkownika

- **US-001:** Jako student, chcę zaoszczędzić na paliwie w drodze na uczelnie
- **US-002:** Jako głowa rodziny chce dobrze zaplanować budżet na wakacje samochodem
- **US-003:** Jako kierowaca tira chcę zmiejszy ć wydatki na paliwo

### Zakres projektu

**Co jest w zakresie (In Scope):**

- Wyszukiwanie najbliższych stacji.
- Lista stacji z rozbudowanymi filtrami.
- Zgłaszanie nowej ceny danego paliwa przez użytkownika.

**Co jest poza zakresem (Out of Scope):**

- Aplikacja mobilna.
- Integracja z chatbotem.

---

## 5. Doświadczenie Użytkownika (UX) i Projekt Interfejsu (UI)

- **Link do projektu w Figmie:** https://www.figma.com/design/eiGZkQhRhr8innkWQd7J56/Untitled?node-id=0-1&p=f
---

## 6. Kwestie Techniczne

- **Stack technologiczny:**
  - Backend: .NET
  - Frontend: React Router
  - Baza danych: PostgreSQL + PostGIS
  - Cache: Redis
  - Mailing: Mailpit
  - File Host: Azurite
- **Integracje:**
  - Integracja z API Facebook OAuth
  - Integracja z API Google OAuth
  - Integracja z API OpenStreetMap
  - Integracja z LeafLet

---

## 7. Otwarte Pytania i Ryzyka

- **Otwarte pytanie:** Czy powinniśmy dać użytkownikowi możliwość dodawania stacji?
- **Ryzyko:** Użytkownicy mogą nadużywać tej funkcji (spam)

  - **Plan mitygacji:** Skupienie się na funkcjach bardziej użytecznych

- **Otwarte pytanie:** Dodawanie loga stacji na jej profilu?
- **Ryzyko:** Spowolni to działanie aplikacji przez obowiązek trzymania na zewnętrzej usłudze każdego dostępnego loga
  - **Plan mitygacji:** Skupienie się na funkcjach bardziej użytecznych
