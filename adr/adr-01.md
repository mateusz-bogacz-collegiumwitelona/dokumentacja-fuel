# ADR-1: Wybór framework backend

- **Status:** Zaakceptowana
- **Data:** 2025-10-06
- **Autorzy:** Mateusz Bogacz-Drewniak, Szymon Mikołajek

---

## 1. Kontekst

> Nasza aplikacja ma mieć możliwość zwracania danych w formacie REST API. Ma mieć możliwość integracji z jaknajprzyjemniejszym w programowaniu bibiotekami integrującymi logowanie za pomocą Facebook, ORM oraz łatwy system tworzenia systemu logowania. Dodoatkową zaletą będzie dobra znajomość języka którego używa dany framework.

---

## 2. Rozważane opcje

### Opcja 1: Laravel 12

- **Zalety:**
  - [+] Dojrzały framewrok
  - [+] Przyjemny ORM dostępny out-of-the-box
  - [+] Backend pisał już aplikacje w tym
  - [+] Dobre wsparcie dla OOP
  - [+] Dobrze udokumentowy ORM oraz system autoryzacji
- **Wady:**
  - [-] Ciężej zastosować architekrurę n-latyer
  - [-] Najsłabsza znajomośc w porównianiu do pozostałych propozycji
  - [-] Mniejsza baza dodatkowych bibiotek
  - [-] Docker + Laravel działa słabo na Windows co jest stanowi problem dla reszty team

### Opcja 2: FastAPI 0.126

- **Zalety:**
  - [+] Lekki Framework
  - [+] Szybki deployment
  - [+] Szybkie działanie po deployment
  - [+] Duża baza dodatkowych bibiotek
  - [+] Łatowść tworznia asynchronicznych akcji
- **Wady:**
  - [-] Młody framework, ma nie dopracowane bibioteki
  - [-] Mnogość nie sprawdzonych bibiotek
  - [-] Python nie wspiera w dużym stopniu OOP
  - [-] Ciężej zastosować architekrurę n-latyer
  - [-] Python ma swoje własne wirtualne środowisko przez co ciężko stworzyć środowisko docker

### Opcja 3: .NET 8.0

- **Zalety:**
  - [+] Dojrzały framewrok
  - [+] Łatowść integracji z api Facebook
  - [+] Backend ma już doświadczenie w aplikacji enterprice
  - [+] Łatowś pisania kodu logowania użytkownika
  - [+] IDE samo tworzy podstawowego docker-compose
  - [+] Łatowść tworznia asynchronicznych akcji
  - [+] Duża baza otwartych bibiotek
  - [+] Bardzo dobre wsparcie dla OOP
  - [+] Bardzo łatwe zastosowanie architektury n-layer
  - [+] Dobrze udokumentowy ORM oraz system autoryzacji
- **Wady:**
  - [-] Cięższy deployment
  - [-] Cięższy kod w porównaiu do innych opcji

---

## 3. Decyzja

> Wybieramy **.NET** jako framework backend naszej aplikacji.

---

## 4. Uzasadnienie

> Wybraliśmy .NET ze względu na dojrzałość frameworka oraz dobrze udokumentowy ORM i system uatoryzacji. Dodaktowo zespół ma w tym największe dośiwadczenie co pozwla na szybsze przygotowanie działającej aplikacji.

---

## 5. Konsekwencje

### Pozytywne

- [+] Szybszy start projektu dzięki znajomości technologii przez zespół.
- [+] Wysoka stabilność i przewidywalność – framework rozwijany od wielu lat, rzadkie breaking changes.

### Negatywne

- [-] Szybko narastająca baza kodu
