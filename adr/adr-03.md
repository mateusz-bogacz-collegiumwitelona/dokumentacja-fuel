# ADR-3: Wybór framework frontend

- **Status:** Zaakceptowana
- **Data:** 2025-10-06
- **Autorzy:** Mateusz Bogacz-Drewniak, Mateusz Chimkowski, Paweł Kruk

---

## 1. Kontekst

> Nasza aplikacja ma posiadać framework wspierający SPA, najpopularniejsze frameworki CSS, duża bazę gotowych komponentów oraz wsparcie popularnych rozwiązań wyświtlania mapy. Zespół nie posiada dużego doświadczenia w tworzeniu frontendu więc framework musi być dobrze udokumentowny i oraz ugruntowany w świecie technologii. Dobrze by było żeby było można pisać w TypeScrpit, ze względu na typowanie danych i możliwośc tworzenia interfejsów i klas.

---

## 2. Rozważane opcje

### Opcja 1: Użycie Angular 20

- **Zalety:**
  - [+] Dojrzały framewrok
  - [+] Dobra dokumentacja
  - [+] Dobry wybór bibiotek
  - [+] Dobre wsparcie dla framework CSS
  - [+] Dużo poradników w internecie
- **Wady:**
  - [-] Skomplikowany w porównaniu do reszty
  - [-] Większa ilość kodu
  - [-] Stracił na popularności w ostatnich latach

### Opcja 2: Użycie React Route 7

- **Zalety:**
  - [+] Dojrzały framewrok
  - [+] Dobra dokumentacja
  - [+] Największy wybór bibiotek
  - [+] Duże wsparcie dla framework CSS
  - [+] Obecnie najpopularniejszy framework JavaScript\TypeScript
  - [+] Dużo poradników w internecie
- **Wady:**
  - [-] Trudniejszy od Vue ale łatwiejszy od Angular

### Opcja 3: Użycie Vue.js 3.5

- **Zalety:**
  - [+] Dobra krzywa uczenia
  - [+] Zyskuje na popularności
- **Wady:**
  - [-] Nie jest jeszcze standardem branżowym
  - [-] Mniej poradników niż przy React czy Angular

---

## 3. Decyzja

> Wybieramy **React Routet** jako framework frontend naszej aplikacji.

---

## 4. Uzasadnienie

> Wybraliśmy React Route ze względu na dojrzałość frameworka, dobrą dokumentacje, szeroką bazę dodtakowych bibiotek oraz ze względu przyszłościowych.

---

## 5. Konsekwencje

### Pozytywne

- [+] Szybka nauka zespołu
- [+] Ogólne zadowolenie zespołu

### Negatywne

- [-] Błędy nowicjuszy
