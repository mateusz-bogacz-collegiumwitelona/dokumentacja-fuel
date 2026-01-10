# Product Backlog - Fuel App

**Data:** 2025-12-06  
**Zespół:** Mateusz Bogacz-Drewniak, Szymon Mikołajek, Mateusz Chimkowski, Paweł Kruk, Michał Nocuń

---

## Legenda priorytetów (MoSCoW)

- **M** - Must Have (kluczowe dla MVP)
- **S** - Should Have (ważne, ale nie krytyczne)
- **C** - Could Have (nice to have)
- **W** - Won't Have (poza zakresem na razie)

---

## Sprint 0 - Infrastruktura i setup (DONE)

| ID       | Zadanie                                                           | Priorytet | Story Points | Status |
| -------- | ----------------------------------------------------------------- | --------- | ------------ | ------ |
| INFRA-01 | Konfiguracja Docker Compose (PostgreSQL, Redis, Azurite, Mailpit) | M         | 3            | Done   |
| INFRA-02 | Setup projektu .NET (Controllers, Services, Data, DTO)            | M         | 5            | Done   |
| INFRA-03 | Setup projektu React + React Router                               | M         | 3            | Done   |
| INFRA-04 | Konfiguracja PostGIS dla danych geograficznych                    | M         | 2            | Done   |
| INFRA-05 | Integracja z Facebook OAuth                                       | S         | 3            | Done   |
| INFRA-06 | System logowania i rejestracji (email + Facebook)                 | M         | 8            | Done   |

**Podsumowanie Sprint 0:** 24 Story Points

---

## Sprint 1 - Podstawowe funkcjonalności (MVP Core)

| ID      | Zadanie                                              | Priorytet | Story Points | Status      |
| ------- | ---------------------------------------------------- | --------- | ------------ | ----------- |
| US-01   | Dodawanie propozycji zmiany ceny paliwa              | M         | 5            | In Progress |
| US-02   | Przeglądanie mapy stacji z podstawowymi filtrami     | M         | 8            | To Do       |
| TECH-01 | API endpoint: `POST /api/station/price-proposal/add` | M         | 3            | In Progress |
| TECH-02 | API endpoint: `POST /api/station/map/all`            | M         | 3            | To Do       |
| TECH-03 | Integracja Leaflet w React                           | M         | 2            | To Do       |

**Podsumowanie Sprint 1:** 21 Story Points

---

## Sprint 2 - Panel administratora i moderacja

| ID      | Zadanie                                                 | Priorytet | Story Points | Status |
| ------- | ------------------------------------------------------- | --------- | ------------ | ------ |
| US-03   | Zarządzanie propozycjami cen przez administratora       | M         | 5            | To Do  |
| TECH-04 | API endpoint: `GET /api/admin/proposal/list`            | M         | 2            | To Do  |
| TECH-05 | API endpoint: `PATCH /api/admin/proposal/change-status` | M         | 3            | To Do  |
| TECH-06 | System punktów dla użytkowników                         | M         | 5            | To Do  |
| TECH-07 | Panel administracyjny - UI                              | M         | 5            | To Do  |

**Podsumowanie Sprint 2:** 20 Story Points

---

## Sprint 3 - Lista stacji i zaawansowane filtrowanie

| ID      | Zadanie                                           | Priorytet | Story Points | Status |
| ------- | ------------------------------------------------- | --------- | ------------ | ------ |
| US-04   | Przeglądanie i filtrowanie listy stacji           | M         | 8            | To Do  |
| TECH-08 | API endpoint: `POST /api/station/list` z filtrami | M         | 5            | To Do  |
| TECH-09 | Geolokalizacja użytkownika (Browser API)          | S         | 3            | To Do  |
| TECH-10 | Sortowanie po cenie/odległości                    | S         | 3            | To Do  |

**Podsumowanie Sprint 3:** 19 Story Points

---

## Sprint 4 - Profil stacji i szczegóły

| ID      | Zadanie                                  | Priorytet | Story Points | Status |
| ------- | ---------------------------------------- | --------- | ------------ | ------ |
| US-05   | Wyświetlanie profilu stacji              | S         | 5            | To Do  |
| TECH-11 | API endpoint: `GET /api/station/profile` | S         | 3            | To Do  |
| TECH-12 | Historia cen dla stacji                  | S         | 5            | To Do  |
| TECH-13 | Wykresy cen (Recharts/Chart.js)          | C         | 3            | To Do  |

**Podsumowanie Sprint 4:** 16 Story Points

---

## Sprint 5 - Statystyki i rankingi

| ID      | Zadanie                                                     | Priorytet | Story Points | Status |
| ------- | ----------------------------------------------------------- | --------- | ------------ | ------ |
| US-06   | Wyświetlanie rankingu użytkowników (top users)              | S         | 3            | To Do  |
| US-07   | Statystyki użytkownika (zaakceptowane/odrzucone propozycje) | S         | 3            | To Do  |
| TECH-14 | API endpoint: `GET /api/proposal-statistic`                 | S         | 2            | To Do  |
| TECH-15 | API endpoint: `GET /api/proposal-statistic/top-users`       | S         | 2            | To Do  |
| TECH-16 | Dashboard użytkownika z punktami                            | S         | 5            | To Do  |

**Podsumowanie Sprint 5:** 15 Story Points

---

## Sprint 6 - Zarządzanie stacjami i markami (Admin)

| ID      | Zadanie                                            | Priorytet | Story Points | Status |
| ------- | -------------------------------------------------- | --------- | ------------ | ------ |
| US-08   | Zarządzanie stacjami (dodawanie, edycja, usuwanie) | S         | 8            | To Do  |
| US-09   | Zarządzanie markami stacji                         | S         | 5            | To Do  |
| US-10   | Zarządzanie typami paliw                           | S         | 3            | To Do  |
| TECH-17 | API endpoints dla zarządzania stacjami             | S         | 3            | To Do  |
| TECH-18 | API endpoints dla zarządzania markami              | S         | 2            | To Do  |

**Podsumowanie Sprint 6:** 21 Story Points

---

## Sprint 7 - Zarządzanie użytkownikami (Admin)

| ID      | Zadanie                                             | Priorytet | Story Points | Status |
| ------- | --------------------------------------------------- | --------- | ------------ | ------ |
| US-11   | Zarządzanie użytkownikami (lista, blokowanie, role) | S         | 5            | To Do  |
| US-12   | System reportowania użytkowników                    | C         | 5            | To Do  |
| TECH-19 | API endpoints dla zarządzania użytkownikami         | S         | 3            | To Do  |
| TECH-20 | Panel moderacji reportów                            | C         | 5            | To Do  |

**Podsumowanie Sprint 7:** 18 Story Points

---

## Backlog - Nice to Have (Could Have / Won't Have)

| ID      | Zadanie                                              | Priorytet | Story Points | Status  |
| ------- | ---------------------------------------------------- | --------- | ------------ | ------- |
| FEAT-01 | Powiadomienia email o zaakceptowaniu propozycji      | C         | 3            | Backlog |
| FEAT-02 | Powiadomienia o spadku cen na ulubionych stacjach    | C         | 5            | Backlog |
| FEAT-03 | Eksport danych do Excel (dla biznesu)                | C         | 3            | Backlog |
| FEAT-04 | Dark mode                                            | C         | 2            | Backlog |
| FEAT-05 | Aplikacja mobilna (React Native)                     | W         | 21           | Future  |
| FEAT-06 | Chatbot integracja                                   | W         | 13           | Future  |
| FEAT-07 | Możliwość dodawania nowych stacji przez użytkowników | W         | 8            | Future  |

---

## Podsumowanie

**Łączna liczba Story Points (Sprinty 0-7):** 154 SP  
**Szacowany czas realizacji:** 7 sprintów (14 tygodni przy 2-tygodniowych sprintach)  
**MVP (Must Have):** Sprinty 0-3 = 84 Story Points ≈ 6 tygodni

---

## Metryki sukcesu (z PRD)

- **WAU (Weekly Active Users):** 100+ użytkowników
- **Średni czas wyszukiwania stacji:** < 30 sekund
- **Dzienne zgłoszenia cen:** 10,000+ propozycji

---

## Notatki

- Priorytety mogą się zmieniać na podstawie feedbacku użytkowników
- Story Points są estymowane przez zespół i mogą być dostosowane
- Po każdym sprincie należy przeprowadzić retrospekcję i dostosować backlog
