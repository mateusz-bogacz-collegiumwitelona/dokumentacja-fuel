# Dokumentacja Techniczna

- **Data:** 2025-12-01
- **Autorzy:** Mateusz Bogacz-Drewniak

---

## 1. Wprowadzenie

> Jest to aplikacja pozwalająca zgłaszać ceny paliw na stacjach, pozwalając znaleźć najniższą cenę za dany rodzaj paliwa w danej okolicy. Jest przeznaczona dla osób często jeżdżących w delegacje, kierowców tirów, osób chcących zaoszczędzić.

### 1.1. Technikalia

- **Stack technologiczny:**

  - Backend: .NET 8.0
  - Frontend: React + React Router
  - Baza danych: PostgreSQL 17 + PostGIS 3.5
  - Cache: Redis 8.2
  - Mailing: Mailpit v1.27
  - File Storage: Azure Blob
  - Azure Storage Emulator: Azurite
  - Reverse Proxy: Nginx

- **Integracje:**
  - Integracja z API Facebook (OAuth)
  - Integracja z API OpenStreetMap
  - Integracja z LeafLet (mapy)

### 1.2. Dokumentacja API

> Tu będą linki

### 1.3. ADR-y

> [Katalog ADR-ów](./adr)

---

## 2. Instrukcja uruchamiania

### 2.1. Wymagania

- **Docker** (z Docker Compose)
- **Node.js** (dla lokalnego developmentu frontendu)
- **.NET SDK 8.0** (tylko do tworzenia migracji lokalnie)

### 2.2. Instalacja lokalna

1. Sklonuj repozytorium na swój komputer:

```shell
git clone https://github.com/mateusz-bogacz-collegiumwitelona/fuel
cd fuel
```

2. Zmień nazwę pliku `.env.example` na `.env` oraz dostosuj go do siebie zmieniając hasła i porty:

```shell
cp .env.example .env
```

3. Uruchom wszystkie serwisy:

```shell
docker-compose build
docker-compose up
```

4. Wejdź do kontenera .NET i utwórz pierwszego użytkownika z rolą administratora:

```shell
# Wejście do kontenera
docker exec -it api-dev /bin/sh

# Tworzenie użytkownika
dotnet /app/bin/Debug/net8.0/Controllers.dll user:create

# Nadanie użytkownikowi roli admin
dotnet /app/bin/Debug/net8.0/Controllers.dll user:promote
```

5. Wejdź na stronę `http://localhost:4000` (lub inny port ustawiony w `CLIENT_PORT`)

6. Spróbuj się zalogować wcześniej utworzonymi danymi. Jeżeli się zalogowałeś, to wszystko działa poprawnie! ✅

### 2.3. Dostęp do paneli administracyjnych

Po uruchomieniu projektu dostępne są następujące panele:

| Panel          | URL                    | Opis                                          |
| -------------- | ---------------------- | --------------------------------------------- |
| **Aplikacja**  | http://localhost:4000  | Frontend aplikacji                            |
| **API**        | http://localhost:5111  | Backend API                                   |
| **Mailpit**    | http://localhost:63854 | Panel testowy do przeglądania wysłanych maili |
| **PostgreSQL** | localhost:5432         | Baza danych (pgAdmin/DBeaver)                 |

---

## 3. Architektura systemu

> Została zastosowana architektura **N-layer** (warstwowa)

### 3.1. Diagram architektury

![Diagram architektury](./img/architecture_diagram.png)

**Opis warstw:**

- **Database** - Baza danych PostgreSQL z rozszerzeniem PostGIS
- **Data** - Modele bazodanowe, migracje oraz repozytoria umożliwiające dostęp do bazy danych
- **Services** - Logika biznesowa aplikacji
- **Controllers** - Kontrolery API (endpoints REST)
- **Client** - Frontend React
- **DTO** - Data Transfer Objects - modele służące do przesyłania danych, używane w zależności od kontekstu

### 3.2. Szczegóły środowiska

#### Wersje technologii

- **.NET Runtime:** 8.0
- **PostgreSQL + PostGIS:** 17-3.5
- **Redis:** 8.2
- **Nginx:** latest
- **Mailpit:** v1.27.9
- **Azurite:** latest (Azure Storage Emulator)
- **Node.js:** 25

#### Serwisy i porty (domyślne)

| Serwis         | Port wewnętrzny | Port zewnętrzny | Opis                |
| -------------- | --------------- | --------------- | ------------------- |
| API (.NET)     | 8080            | 5111            | Backend API         |
| PostgreSQL     | 5432            | 5432            | Baza danych         |
| Redis          | 6379            | 6379            | Cache               |
| Mailpit UI     | 8025            | 63854           | Panel testowy maili |
| Mailpit SMTP   | 1025            | 1025            | Serwer SMTP         |
| Nginx          | 80              | 8080            | Reverse proxy       |
| Client (React) | 4000            | 4000            | Frontend            |
| Azurite Blob   | 10000           | 10000           | Azure Blob Storage  |
| Azurite Queue  | 10001           | 10001           | Azure Queue Storage |
| Azurite Table  | 10002           | 10002           | Azure Table Storage |

> Porty można dostosować w pliku `.env`

#### Struktura katalogów

```
fuel/
├── Controllers/          # API Controllers + Dockerfile
├── Services/            # Logika biznesowa
├── Data/                # Modele, migracje, repozytoria
├── DTO/                 # Data Transfer Objects
├── Tests/               # Testy + Dockerfile
├── Client/              # Frontend React
├── templates/           # Konfiguracja Nginx
├── logs/                # Logi aplikacji (wolumen)
├── docker-compose.yml   # Konfiguracja Docker
├── .env                 # Zmienne środowiskowe
└── .env.example         # Przykładowa konfiguracja
```

#### Wolumeny Docker

- **db-data:** Dane PostgreSQL (persystentne)
- **redis-data:** Dane Redis (persystentne)
- **blob_data:** Dane Azurite (persystentne)
- **shared-volume:** Współdzielony wolumen między kontenerami

### 3.3. Przepływ danych

![Diagram przepływu danych](./img/dataflow_diagram.png)

---

## 4. Baza danych

### 4.1. Diagram ERD

![Diagram ERD bazy danych](./img/erd-diagram.png)

### 4.2. Główne encje

Baza danych zawiera następujące kluczowe tabele:

- **Users** - Użytkownicy systemu z integracją Facebook OAuth, przechowuje dane logowania i profile
- **FuelStations** - Stacje paliw z danymi geograficznymi (wykorzystuje PostGIS do lokalizacji)
- **FuelPrices** - Zgłoszenia cen paliw przez użytkowników, zawiera cenę, datę zgłoszenia i referencje do stacji
- **FuelTypes** - Rodzaje paliw (benzyna 95, benzyna 98, diesel, LPG, etc.)
- **Roles** - Role użytkowników (user, admin)
- **UserRoles** - Tabela łącząca użytkowników z rolami

### 4.3. PostGIS

Baza wykorzystuje rozszerzenie **PostGIS** do obsługi danych geograficznych:

- Przechowywanie lokalizacji stacji paliw jako punkty geograficzne
- Wyszukiwanie najbliższych stacji w określonym promieniu
- Obliczanie odległości między użytkownikiem a stacjami

### 4.4. Migracje

#### Tworzenie migracji

**Ważne:** Migracje tworzy się **lokalnie na PC** (nie w kontenerze), ponieważ kontener zawiera tylko .NET Runtime, a do tworzenia migracji potrzebny jest .NET SDK.

```shell
# Lokalnie na PC (wymaga .NET SDK 8.0)
dotnet ef migrations add NazwaMigracji --project Data

# Migracje są automatycznie wykonywane przy starcie aplikacji w kontenerze
```

#### Rollback migracji

```shell
# Cofnięcie do poprzedniej migracji
dotnet ef database update PreviousMigrationName --project Data

# Cofnięcie wszystkich migracji
dotnet ef database update 0 --project Data
```

#### Lista migracji

```shell
# Wyświetlenie wszystkich migracji
dotnet ef migrations list --project Data
```

---

## 5. Testowanie

### 5.1. Uruchamianie testów

Testy są uruchamiane w dedykowanym kontenerze Docker:

```shell
# Zbuduj i uruchom kontener testowy
docker-compose up tests

# Lub wejdź do kontenera i uruchom ręcznie
docker exec -it tests /bin/sh
dotnet test
```

### 5.2. Środowisko testowe

Testy wykorzystują:

- Oddzielną konfigurację z `ASPNETCORE_ENVIRONMENT=Test`
- Te same serwisy co środowisko deweloperskie (PostgreSQL, Redis)
- Osobną bazę danych lub transakcje z rollbackiem (zależnie od konfiguracji)

---

## 6. Deployment i środowiska

### 6.1. Środowiska

Projekt jest przygotowany do pracy w następujących środowiskach:

- **Development** (lokalne) - Pełne środowisko z wszystkimi narzędziami deweloperskimi
- **Staging** - Środowisko testowe przed produkcją
- **Production** - Środowisko produkcyjne

### 6.2. Konfiguracja środowisk

Każde środowisko powinno mieć własny plik `.env` z odpowiednimi wartościami:

- Inne hasła i klucze API
- Różne porty (jeśli potrzebne)
- Różne credentials dla Facebook OAuth

### 6.3. Zmienne środowiskowe

Pełna lista zmiennych środowiskowych znajduje się w pliku `.env.example`. Najważniejsze:

- **API_PORT** - Port dla .NET API (domyślnie: 5111)
- **POSTGRES\_\*** - Konfiguracja PostgreSQL
- **REDIS\_\*** - Konfiguracja Redis
- **FACEBOOK*OAUTH*\*** - Credentials dla Facebook OAuth
- **CLIENT_PORT** - Port dla frontendu React (domyślnie: 4000)

---

## 7. Integracje zewnętrzne

### 7.1. Autentykacja i Integracje OAuth2

System wykorzystuje hybrydowy model autentykacji, łącząc oficjalne SDK dostawców na froncie z manualną weryfikacją tokenów na backendzie.

#### 7.1.1 Przepływ logowania (Social Login Flow)

Proces został zaimplementowany w sposób zapewniający maksymalne bezpieczeństwo:

1.  **Inicjalizacja (Frontend):** Komponenty `FacebookLoginButton` i `GoogleLoginButton` ładują zewnętrzne skrypty SDK.
2.  **Pozyskanie Tokenu:** Po interakcji użytkownika, frontend otrzymuje `accessToken` (FB) lub `idToken` (Google).
3.  **Weryfikacja (Backend):** Token jest przesyłany do API. Usługa `LoginRegisterServices` dokonuje weryfikacji:
    - **Google:** Używamy biblioteki `GoogleJsonWebSignature` do kryptograficznego sprawdzenia podpisu tokena Google.
    - **Facebook:** Backend wykonuje bezpośrednie zapytanie serwer-serwer do Facebook Graph API, aby potwierdzić tożsamość użytkownika i pobrać jego email.
4.  **Just-in-Time Provisioning:** Jeśli użytkownik loguje się pierwszy raz, system automatycznie tworzy konto w bazie PostgreSQL, przypisuje rolę `User` i inicjalizuje statystyki.
5.  **Sesja aplikacji:** Po poprawnej weryfikacji, backend generuje własny token **JWT** oraz **RefreshToken**.

#### 7.1.2 Bezpieczeństwo sesji

- **HttpOnly Cookies:** Token JWT nie jest przechowywany w `localStorage`, lecz w bezpiecznym ciasteczku `HttpOnly`. Dzięki temu jest on niedostępny dla skryptów JavaScript, co eliminuje ryzyko kradzieży tokenu przez ataki XSS.
- **Refresh Token Pattern:** Implementacja `IRefreshTokenRepository` pozwala na odświeżanie sesji bez konieczności ponownego logowania przez użytkownika, przy jednoczesnym zachowaniu krótkiego czasu życia głównego tokena dostępowego.
- **Rate Limiting:** Endpointy autentykacji są objęte limitem żądań (konfiguracja w `Program.cs`), co chroni system przed atakami typu Brute Force.

### 7.2. OpenStreetMap + LeafLet

- **OpenStreetMap** - Źródło danych geograficznych dla stacji paliw
- **LeafLet** - Biblioteka JavaScript do renderowania interaktywnych map w interfejsie użytkownika

---

## 8. Rozwiązywanie problemów

### 8.1. Częste problemy

#### Problem: Kontener API nie może połączyć się z bazą danych

```
Błąd: "could not connect to server: Connection refused"
```

**Rozwiązanie:**

1. Sprawdź czy kontener postgis jest w stanie `healthy`:

```shell
docker ps
```

2. Jeśli nie, sprawdź logi:

```shell
docker logs postgres
```

3. Upewnij się, że `POSTGRES_HOST` w `.env` jest ustawiony na `postgis`

#### Problem: Facebook OAuth nie działa

```
Błąd: "Invalid OAuth credentials"
```

**Rozwiązanie:**

1. Wypełnij poprawnie `FACEBOOK_OAUTH_CLIENT_ID` i `FACEBOOK_OAUTH_CLIENT_SECRET` w pliku `.env`
2. Skontaktuj się z Mateuszem po aktualne credentials
3. Sprawdź czy w ustawieniach aplikacji Facebook dodano prawidłowy Redirect URI

#### Problem: Frontend nie widzi API

```
Błąd: "Network Error" lub "Failed to fetch"
```

**Rozwiązanie:**

1. Sprawdź czy wszystkie kontenery są uruchomione:

```shell
docker ps
```

2. Zweryfikuj czy porty w `.env` są poprawne
3. Sprawdź logi API:

```shell
docker logs api-dev
```

#### Problem: Redis connection failed

```
Błąd: "Connection to Redis failed"
```

**Rozwiązanie:**

1. Sprawdź czy Redis jest uruchomiony i zdrowy:

```shell
docker ps | grep redis
```

2. Upewnij się że `REDIS_HOST` w `.env` jest ustawiony na `redis`
3. Sprawdź logi Redis:

```shell
docker logs redis-dev
```

#### Problem: Migracje nie działają

```
Błąd przy starcie: "Migration failed"
```

**Rozwiązanie:**

1. Sprawdź logi API aby zobaczyć dokładny błąd migracji
2. Upewnij się że baza danych jest dostępna i pusta (przy pierwszym uruchomieniu)
3. Jeśli potrzeba, usuń wolumen bazy i utwórz od nowa:

```shell
docker-compose down -v
docker volume rm fuel_db-data
docker-compose up
```

### 8.2. Logi

#### Logi aplikacji .NET

Logi aplikacji .NET znajdują się w katalogu `./logs` (zamontowany wolumen z hosta)

#### Logi kontenerów

```shell
docker logs api-dev        # Logi API
docker logs postgres       # Logi bazy danych
docker logs redis-dev      # Logi Redis
docker logs azurite        # Logi Azurite
docker logs nginx-dev      # Logi Nginx
docker logs client         # Logi frontendu
docker logs mailpit-dev    # Logi Mailpit
```

#### Live logs (śledzenie w czasie rzeczywistym)

```shell
docker logs -f api-dev     # Śledź logi API na żywo
```

### 8.3. Restart serwisów

```shell
# Restart pojedynczego serwisu
docker-compose restart api-dev

# Restart wszystkich serwisów
docker-compose restart

# Pełne przeładowanie (rebuild)
docker-compose down
docker-compose up --build
```

### 8.4. Czyszczenie środowiska

```shell
# Zatrzymaj wszystkie kontenery
docker-compose down

# Usuń również wolumeny (UWAGA: usuwa dane!)
docker-compose down -v

# Usuń nieużywane obrazy
docker image prune -a

# Pełne czyszczenie Dockera (UWAGA: usuwa wszystko!)
docker system prune -a --volumes
```

---

## 9. Kontakt i wsparcie

W przypadku pytań lub problemów:

- **Autor:** Mateusz Bogacz-Drewniak
- **Repozytorium:** https://github.com/mateusz-bogacz-collegiumwitelona/fuel
- **ADR-y:** [Katalog decyzji architektonicznych](./adr/)

---

**Ostatnia aktualizacja:** 2025-12-01
