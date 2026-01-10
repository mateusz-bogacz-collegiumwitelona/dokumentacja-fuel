# ADR-005: Wybór framework testów

- **Status:** Zaakceptowana
- **Data:** 2025-10-06
- **Autorzy:** Mateusz Bogacz-Drewniak, Szymon Mikołajek

---

## 1. Kontekst

> Projekt wymaga przygotowania zestawu testów jednostkowych dla .NET Web API. Testy mają obejmować podstawowe elementy logiki biznesowej oraz warstwę kontrolerów. Zespół ma ograniczone doświadczenie w pisaniu testów, dlatego ważne jest wybranie narzędzia prostego, popularnego oraz dobrze udokumentowanego.

---

## 2. Rozważane opcje

### Opcja 1: xUnit 2.5

- **Zalety:**
  - [+] Domyślnie wspierany i promowany w oficjalnych szablonach .NET.
  - [+] Bardzo prosty w użyciu, niski próg wejścia.
  - [+] Niski próg wejścia
  - [+] Przyjazny model testów (konstruktor jako setup, brak klasy testowej globalnie)
- **Wady:**
  - [-] Mniej zaawansowanych funkcji integracyjnych w porównaniu do NUnit.
  - [-] Brak atrybutów typu `[TestCase]`, co wydłuża niektóre testy parametryzowane.
  -

### Opcja 2: NUnit 4.4

- **Zalety:**
  - [+] Bardzo rozbudowany zestaw atrybutów testowych (np. [TestCase], [Sequential])
  - [+] Niski próg wejścia dla osób korzystających z VS.
- **Wady:**
  - [-] Większa złożoność w stosunku do xUnit.
  - [-] Mniej popularny w nowych projektach .NET 6/7/8.

### Opcja 3: MSTest v4

- **Zalety:**
  - [+] Oficjalne narzędzie Microsoft, stabilne i zgodne z Visual Studio.
  - [+] Dobrze znany w starszych projektach .NET Framework.
  - [+] Bogate możliwości konfiguracji testów.
- **Wady:**
  - [-] Mniej nowoczesny styl pisania testów.
  - [-] Najmniejsza popularność we współczesnych projektach .NET.
  - [-] Mniej materiałów i przykładów niż xUnit.

---

## 3. Decyzja

> Wybieramy xUnit jako framework testów jednostkowych dla projektu .NET Web API.

---

## 4. Uzasadnienie

> xUnit zapewnia najprostszy start dla zespołu, który nie ma dużego doświadczenia w pisaniu testów. Framework jest domyślnie wspierany przez .NET, dobrze integruje się z test runnerami oraz narzędziami CI, a liczba dostępnych tutoriali i przykładów minimalizuje czas potrzebny na naukę. W przeciwieństwie do NUnit i MSTest, xUnit oferuje prostszy model inicjalizacji testów oraz bardziej nowoczesną architekturę odpowiadającą trendom w .NET 8. Jest również standardem w społeczności, co ułatwia w przyszłości utrzymanie i rozwój testów.

---

## 5. Konsekwencje

[Opisz pozytywne i negatywne skutki podjętej decyzji.]

### Pozytywne

- [+] Szybsze wdrożenie testów dzięki niskiemu progowi wejścia.
- [+] Dostęp do wielu gotowych przykładów i materiałów edukacyjnych.
- [+] Dobra integracja z popularnymi bibliotekami do mockowania (Moq, NSubstitute).
- [+] Zgodność z aktualnymi standardami w .NET.

### Negatywne

- [-] Brak natywnego wsparcia dla [TestCase] może powodować bardziej rozbudowany kod testów parametryzowanych.
- [-] Nieco mniejsza elastyczność konfiguracji testów w porównaniu z NUnit.
