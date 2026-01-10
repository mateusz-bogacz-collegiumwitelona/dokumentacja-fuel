# ADR-006: Wybór systemu cache

- **Status:** Zaakceptowana
- **Data:** 2025-10-06
- **Autorzy:** Mateusz Bogacz-Drewniak, Szymon Mikołajek

---

## 1. Kontekst

> W projekcie potrzebny jest mechanizm cache’owania, który przyspieszy obsługę często wykonywanych zapytań oraz zmniejszy obciążenie bazy danych. Cache ma działać zarówno lokalnie dla developerów, jak i w środowisku testowym oraz przyszłym środowisku produkcyjnym.

---

## 2. Rozważane opcje

### Opcja 1: Redis 8.2

- **Zalety:**
  - [+] Najpopularniejszy system cache dla aplikacji webowych.
  - [+] W pełni wspierany przez .NET (AddStackExchangeRedisCache).
  - [+] Bardzo łatwy do uruchomienia w Dockerze.
  - [+] Nadaje się do środowisk rozproszonych i produkcyjnych.
  - [+] Dużo materiałów, poradników i gotowych przykładów.
- **Wady:**
  - [-] Wymaga osobnego kontenera/instancji.
  - [-] Dodatkowa konfiguracja po stronie aplikacji.

### Opcja 2: MemoryCache (in-memory cache .NET)

- **Zalety:**
  - [+] Bardzo łatwe użycie.
  - [+] Brak potrzeby instalowania dodatkowych usług.
- **Wady:**
  - [-] Brak wsparcia dla środowisk wieloserwerowych.
  - [-] Nie nadaje się na cache współdzielony.
  - [-] Traci dane przy restarcie aplikacji.

---

## 3. Decyzja

> Zespół wybrał Redis jako system cache dla aplikacji.

---

## 4. Uzasadnienie

> Redis jest najpopularniejszym i najprostszym do wdrożenia systemem cache dla .NET. Posiada oficjalne wsparcie w bibliotece StackExchange.Redis, a integracja z ASP.NET Core ogranicza się do krótkiej konfiguracji w Program.cs. Dodatkowo, Redis ma bogatą dokumentację i wiele przykładów implementacji, co ułatwia pracę zespołowi, który nie ma dużego doświadczenia z systemami cache.

---

## 5. Konsekwencje

### Pozytywne

- [+] Wysoka wydajność operacji cache.
- [+] Łatwe wdrożenie lokalnie i w środowisku Docker.
- [+] Możliwość skalowania aplikacji bez zmiany implementacji cache.
- [+] Stabilne i powszechnie stosowane rozwiązanie.

### Negatywne

- [-] Wymaga dodatkowego serwera/instancji Redis.
- [-] Wymaga minimalnej konfiguracji po stronie .NET i Dockera.
