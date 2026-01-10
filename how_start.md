# Instalacja i konfiguracja

## 1. Wymagania

- **Docker** (z Docker Compose)
- **Node.js** (dla lokalnego developmentu frontendu)
- **.NET SDK 8.0** (opcjonalnie - tylko do tworzenia migracji lokalnie)

## 2. Instalacja lokalna

### 2.1. Klonowanie repozytorium

Sklonuj projekt na swój komputer:

```shell
git clone https://github.com/mateusz-bogacz-collegiumwitelona/fuel
cd fuel
```

### 2.2. Konfiguracja środowiska

#### Tworzenie pliku .env

Skopiuj przykładowy plik konfiguracyjny i dostosuj go do swoich potrzeb:

```shell
cp .env.example .env
```

> **💡 Wskazówka:** Upewnij się, że zmieniłeś wszystkie domyślne hasła w pliku `.env` przed uruchomieniem w środowisku produkcyjnym.

#### Najważniejsze zmienne środowiskowe

Pełna lista zmiennych znajduje się w pliku `.env.example`. Poniżej najważniejsze z nich:

| Zmienna                       | Opis                     | Wartość domyślna |
| ----------------------------- | ------------------------ | ---------------- |
| **API_PORT**                  | Port dla .NET API        | 5111             |
| **CLIENT_PORT**               | Port dla frontendu React | 4000             |
| **POSTGRES_USER**             | Użytkownik PostgreSQL    | postgres         |
| **POSTGRES_PASSWORD**         | Hasło do bazy danych     | **Zmień!**       |
| **POSTGRES_DB**               | Nazwa bazy danych        | fuel_db          |
| **REDIS_PASSWORD**            | Hasło do Redis           | **Zmień!**       |
| **FACEBOOK_OAUTH_APP_ID**     | Facebook App ID          | Twój App ID      |
| **FACEBOOK_OAUTH_APP_SECRET** | Facebook App Secret      | **Zmień!**       |
| **GOOGLE_CLIENT_ID**          | Google App ID            | **Zmień!**       |
| **GOOGLE_CLIENT_SECRET**      | Google App Secret        | **Zmień!**       |
| **AZURITE_BLOB_PORT**         | Port dla Azurite Blob    | 10000            |
| **AZURITE_QUEUE_PORT**        | Port dla Azurite Queue   | 10001            |
| **AZURITE_TABLE_PORT**        | Port dla Azurite Table   | 10002            |

#### Różnice między środowiskami

Każde środowisko (development, staging, production) powinno mieć własny plik `.env` z odpowiednimi wartościami:

**Development (.env.development):**

```env
API_PORT=5111
CLIENT_PORT=4000
POSTGRES_PASSWORD=dev_password_123
FACEBOOK_OAUTH_APP_ID=dev_app_id
```

**Production (.env.production):**

```env
API_PORT=443
CLIENT_PORT=443
POSTGRES_PASSWORD=strong_secure_password_XYZ789!
FACEBOOK_OAUTH_APP_ID=production_app_id
# Użyj certyfikatów SSL
# Różne adresy URL dla Azurite i innych serwisów
```

> **Bezpieczeństwo:**
>
> - Nigdy nie commituj plików `.env` do repozytorium
> - Użyj silnych, unikalnych haseł dla każdego środowiska
> - Regularnie rotuj klucze API i hasła
> - W produkcji użyj secrets management (np. Azure Key Vault, AWS Secrets Manager)

#### Konfiguracja Logowania Społecznościowego (OAuth2)

Aplikacja wspiera logowanie przez Facebook i Google. Aby te funkcje działały, należy uzupełnić poniższe klucze w `.env`:

| Zmienna                          | Opis                    | Gdzie znaleźć?                                            |
| :------------------------------- | :---------------------- | :-------------------------------------------------------- |
| **FACEBOOK_OAUTH_CLIENT_ID**     | ID Aplikacji Facebook   | [FB Developers](https://developers.facebook.com/)         |
| **FACEBOOK_OAUTH_CLIENT_SECRET** | Klucz (Secret) Facebook | [FB Developers](https://developers.facebook.com/)         |
| **GOOGLE_CLIENT_ID**             | Client ID Google        | [Google Cloud Console](https://console.cloud.google.com/) |
| **GOOGLE_CLIENT_SECRET**         | Client Secret Google    | [Google Cloud Console](https://console.cloud.google.com/) |

**Wymagane ustawienia u dostawców:**

- **Facebook:** W sekcji "Logowanie przez Facebooka" dodaj domenę `http://localhost:4000` (lub Twoją domenę produkcyjną).
- **Google:** W sekcji "Credentials" dodaj `http://localhost:4000` do "Authorized JavaScript origins".

### 2.3. Uruchomienie aplikacji

Zbuduj i uruchom wszystkie serwisy za pomocą Docker Compose:

```shell
docker-compose build
docker-compose up
```

Alternatywnie, możesz uruchomić w trybie detached (w tle):

```shell
docker-compose up -d
```

> **Sprawdzanie statusu:** Aby zobaczyć status wszystkich kontenerów, użyj: `docker-compose ps`

### 2.4. Utworzenie pierwszego użytkownika (admin)

Wejdź do kontenera API i utwórz użytkownika z uprawnieniami administratora:

```shell
# Wejście do kontenera
docker exec -it api-dev /bin/sh

# Tworzenie użytkownika
dotnet /app/bin/Debug/net8.0/Controllers.dll user:create

# Nadanie roli administratora
dotnet /app/bin/Debug/net8.0/Controllers.dll user:promote
```

> **Notatka:** Zapamiętaj dane logowania wprowadzone podczas tworzenia użytkownika.

### 2.5. Weryfikacja instalacji

1. Otwórz przeglądarkę i przejdź do: `http://localhost:4000` (lub inny port ustawiony w `CLIENT_PORT`)
2. Zaloguj się używając wcześniej utworzonych danych
3. Jeśli logowanie przebiegło pomyślnie - gratulacje, wszystko działa poprawnie!

---

## 3. Rozwiązywanie problemów

### Problem: Kontenery nie startują

**Możliwe przyczyny:**

- Porty w `.env` są zajęte przez inne aplikacje
- Docker Desktop nie jest uruchomiony
- Niewystarczające zasoby (RAM/CPU) przydzielone dla Dockera

**Rozwiązanie:**

```shell
# Sprawdź, które porty są zajęte
netstat -ano | findstr :4000  # Windows
lsof -i :4000                 # Linux/Mac

# Sprawdź logi konkretnego kontenera
docker logs api-dev

# Sprawdź wszystkie logi
docker-compose logs
```

### Problem: Nie mogę się zalogować

**Możliwe przyczyny:**

- Użytkownik nie został poprawnie utworzony
- Błąd w bazie danych
- Problemy z hasłem

**Rozwiązanie:**

```shell
# Sprawdź logi API
docker logs api-dev

# Sprawdź, czy baza danych działa
docker exec -it postgres-container psql -U postgres -d fuel_db

# Spróbuj utworzyć użytkownika ponownie
docker exec -it api-dev /bin/sh
dotnet /app/bin/Debug/net8.0/Controllers.dll user:create
```

### Problem: Błędy połączenia z bazą danych

**Rozwiązanie:**

```shell
# Zaczekaj kilka sekund po uruchomieniu - PostgreSQL potrzebuje czasu na inicjalizację
docker-compose up -d
sleep 10

# Sprawdź, czy kontener PostgreSQL działa
docker ps | grep postgres

# Zweryfikuj konfigurację w .env
cat .env | grep POSTGRES
```

### Problem: Facebook OAuth nie działa

**Możliwe przyczyny:**

- Nieprawidłowe credentials w `.env`
- Aplikacja Facebook nie jest skonfigurowana poprawnie
- Redirect URL nie pasuje

**Rozwiązanie:**

1. Sprawdź credentials w Facebook Developers Console
2. Upewnij się, że redirect URL w ustawieniach FB to: `http://localhost:4000/auth/callback`
3. Zweryfikuj zmienne w `.env`:

```shell
cat .env | grep FACEBOOK
```
