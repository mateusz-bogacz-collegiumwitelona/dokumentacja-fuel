# ADR-008: Wybór serwera do deployu

- **Status:** Zaakceptowana
- **Data:** 2025-10-06
- **Autorzy:** Mateusz Bogacz-Drewniak, Paweł Kruk

---

## 1. Kontekst

> Ze względu na ograniczone zasoby finansowe zespołu, dostawca usług do deployu aplikacji musi być względnie tani, a najlepiej darmowy dla studentów.  
> Dodatkową zaletą jest duża ilość materiałów edukacyjnych oraz dokumentacji, które mogą wspomóc zespół DevOps.  
> Kluczowym wymaganiem jest możliwość przechowywania plików (object storage) oraz opcja uruchomienia własnych usług **Redis** oraz **PostgreSQL z rozszerzeniem PostGIS**.

---

## 2. Rozważane opcje

### Opcja 1: Oracle Cloud Infrastructure (OCI)

- **Zalety:**

  - [+] Darmowy dostęp do dwóch maszyn w ramach Oracle Cloud Free Tier
  - [+] Część zespołu posiada doświadczenie w pracy z OCI

- **Wady:**
  - [-] Bardziej złożona konfiguracja w porównaniu do innych dostawców
  - [-] Specyficzny model przydzielania zasobów – trudność w przewidzeniu dostępności VPS
  - [-] Niewielkie doświadczenie zespołu z OCI Object Storage
  - [-] Usługi **OCI Cache with Redis** oraz **OCI PostgreSQL Service** nie są dostępne w wersji darmowej

---

### Opcja 2: Microsoft Azure

- **Zalety:**

  - [+] Bardzo duża ilość dokumentacji i materiałów edukacyjnych
  - [+] Darmowe **100 USD kredytów** dla studentów
  - [+] Znajomość **Azure Blob Storage** w zespole
  - [+] **Azurite** jako dobry lokalny emulator usług typu Blob Storage
  - [+] Naturalna integracja z backendem opartym o **.NET**

- **Wady:**
  - [-] Problemy organizacyjne związane z aktywacją i utrzymaniem licencji studenckiej
  - [-] Koszty usług po wykorzystaniu darmowych kredytów szybko rosną

---

### Opcja 3: Amazon Web Services (AWS)

- **Zalety:**

  - [+] Najbardziej popularna chmura – ogromna ilość dokumentacji i przykładów
  - [+] Standard branżowy (S3, EC2, RDS)
  - [+] Dobrze udokumentowany **Amazon S3** z możliwością lokalnej emulacji przy użyciu **MinIO**
  - [+] **AWS Free Tier** umożliwia uruchomienie podstawowych zasobów na potrzeby projektu
  - [+] Duża elastyczność w doborze usług i architektury

- **Wady:**
  - [-] Brak dedykowanej, stałej darmowej wersji dla studentów (jedynie Free Tier / czasowe kredyty)
  - [-] Ograniczenia czasowe Free Tier (12 miesięcy)
  - [-] Zarządzane usługi **Amazon ElastiCache (Redis)** oraz **Amazon RDS (PostgreSQL + PostGIS)** są płatne poza Free Tier
  - [-] Złożony model rozliczeń wymagający stałego monitorowania kosztów
  - [-] Ograniczone doświadczenie zespołu z AWS poza usługą S3

---

## 3. Decyzja

> Wybrano **Microsoft Azure** jako platformę do deployu aplikacji.

---

## 4. Uzasadnienie

> Wybór platformy **Microsoft Azure** został podyktowany dostępnością **100 USD darmowych kredytów studenckich**, dużą ilością dokumentacji oraz łatwiejszym procesem deployu aplikacji backendowej opartej na technologii **.NET**.  
> Znajomość usług Azure przez część zespołu pozwala na szybsze rozpoczęcie prac oraz zmniejszenie ryzyka problemów konfiguracyjnych na wczesnym etapie projektu.

---

## 5. Konsekwencje

### Pozytywne

- [+] Szybszy start projektu dzięki znajomości platformy Azure przez zespół
- [+] Dobra integracja z backendem opartym o .NET
- [+] Wysoka jakość dokumentacji oraz wsparcia społeczności
- [+] Możliwość lokalnego developmentu dzięki emulatorom (Azurite)
- [+] Skalowalność rozwiązania w przypadku dalszego rozwoju projektu

### Negatywne

- [-] Ryzyko przekroczenia darmowych limitów i wzrostu kosztów po wykorzystaniu kredytów
- [-] Uzależnienie projektu od jednego dostawcy chmurowego (vendor lock-in)
- [-] Konieczność stałego monitorowania zużycia zasobów
- [-] Potencjalnie bardziej skomplikowana migracja do innej chmury w przyszłości
