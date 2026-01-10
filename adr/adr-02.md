# ADR-2: Wybór bazy danych

- **Status:** Zaakceptowana
- **Data:** 2025-10-06
- **Autorzy:** Mateusz Bogacz-Drewniak, Szymon Mikołajek

---

## 1. Kontekst

> Nasza aplikacja ma przechowywać dane o stacji paliw (w tym i dane geolokalizacyjne), użytkownikach oraz paliwa. Oczekujemy szybkiego wzrostu liczby użytkowników. Zespół ma największe doświadczenie w pracy z relacyjnymi bazami danych. Kluczowe wymagania to wparcie GIS, skalowalność oraz prostota wdrożenia

---

## 2. Rozważane opcje

### Opcja 1: PostgresSQL 17

- **Zalety:**
  - [+] Dojrzała technologia z dużym wsparciem społeczności
  - [+] Wsparcie GIS
  - [+] Jest na licencji Open Source
  - [+] Dobra dokumentacja
  - [+] Wpsarcie dla enumeratrów jako kolumny
  - [+] Najlepiej znany dialekt sql w zespole
  - [+] Dobrze udokumentowany dodattek do obsługi GIS (PostGIS 3.5)
- **Wady:**
  - [-] Mniejsza elastyczność schematu w porównaniu do baz NoSQL.
  - [-] Jeżeli baza danych produkcyjna nie będzie w kontenerze tylko na dedykowanym serwerze to trzeba zainstalować PostGIS

### Opcja 2: Microsoft SQL Server 2025

- **Zalety:**
  - [+] Dojrzała, dobrze udkomumentowana baza
  - [+] Niezawodna
  - [+] Wbudowane wsparcie dla danych geoloklaizacyjnych
  - [+] Najlepiej udokumnetowany
- **Wady:**
  - [-] Płatna
  - [-] Brak możliwośći konteneryzacji na prodkukcji
  - [-] Słaba znajomość T-SQL przez zespół

### Opcja 3: Użycie MongoDB Geospatial 8.0

- **Zalety:**
  - [+] Elastyczny schemat, łatwy do modyfikacji
  - [+] Łatwa w konteneryzacji
  - [+] Dobra dokumenacja
- **Wady:**
  - [-] Wymaga od zespołu nauki nowej technologii

---

## 3. Decyzja

> Wybieramy **PostgresSQL** jako framework backend naszej aplikacji.

---

## 4. Uzasadnienie

> Wybraliśmy PostgresSQL ze względu na dobrą znajomość bazy dancyh przez zespół, dobrą dokumetnacje PostGIS oraz łatwą możliwość uruchomienia jako kontener. Doświadczenie zespołu w technologiach SQL pozwoli na szybsze wdrożenie.

---

## 5. Konsekwencje

### Pozytywne

- [+] Szybszy start projektu dzięki znajomości technologii przez zespół.
- [+] Wysoka stabilność i przewidywalność – baza rozwijana od wielu lat, rzadkie breaking changes.

### Negatywne

- [-] Trzeba było znależć odpoweidnią paczkę obsługującą PostGIS w .NET
