# ADR-009: Wybór usługi do wysyłki maili

- **Status:** Proponowana
- **Data:** 2025-12-20
- **Autorzy:** Mateusz Bogacz-Drewniak, Paweł Kruk, Szymon Mikołajek, Michał Nocuń

---

## 1. Kontekst

> W projekcie konieczne jest wysyłanie wiadomości e-mail do użytkowników (np. potwierdzenia konta, reset hasła, powiadomienia).  
> Zespół potrzebuje usługi, która oferuje **darmowy plan** lub **niskie koszty początkowe**, łatwą integrację z aplikacją .NET oraz wsparcie zarówno dla **SMTP**, jak i **API**.  
> Wymagana jest również dobra dokumentacja i niezawodność dostarczania wiadomości.

---

## 2. Rozważane opcje

### Opcja 1: Mailjet

- **Zalety:**

  - [+] Darmowy plan: **do 6 000 maili miesięcznie** (limit ~200 dziennie)
  - [+] Obsługa **SMTP i REST API**
  - [+] Dobre wsparcie dla .NET (biblioteka Mailjet SDK)
  - [+] Łatwa konfiguracja i wdrożenie

- **Wady:**
  - [-] Limit darmowych maili może być niewystarczający przy większym ruchu
  - [-] Niektóre funkcje premium wymagają płatnej subskrypcji

---

### Opcja 2: Brevo (dawniej SendinBlue)

- **Zalety:**

  - [+] Darmowy plan: **do 300 maili dziennie**
  - [+] Obsługa **SMTP i REST API**
  - [+] Wsparcie dla transakcyjnych maili i marketingu
  - [+] Dobry interfejs do monitorowania wysyłek

- **Wady:**
  - [-] Niewielki limit dzienny w darmowym planie
  - [-] Niektóre zaawansowane funkcje (segmentacja, automatyzacje) są płatne

---

### Opcja 3: SparkPost

- **Zalety:**

  - [+] Darmowy plan: **do 15 000 maili miesięcznie**
  - [+] Obsługa **SMTP i API**
  - [+] Bardzo dobra dostarczalność maili
  - [+] Możliwość monitorowania i raportowania wysyłek

- **Wady:**
  - [-] Darmowy plan wymaga rejestracji i konfiguracji konta
  - [-] Wysokie limity i zaawansowane funkcje są płatne
  - [-] Zespół nie posiada doświadczenia z tą usługą

---

## 3. Decyzja

> Wybieramy **Mailjet** jako usługę do wysyłki maili w projekcie.

---

## 4. Uzasadnienie

> Mailjet został wybrany ze względu na **wysoki miesięczny limit darmowych maili**, łatwą integrację z .NET (SMTP i API) oraz dobrą dokumentację.  
> Usługa pozwala na szybkie rozpoczęcie prac i prostą konfigurację transakcyjnych wiadomości e-mail bez dodatkowych kosztów na wczesnym etapie projektu.

---

## 5. Konsekwencje

### Pozytywne

- [+] Szybki start projektu dzięki prostemu wdrożeniu Mailjet
- [+] Stabilna dostarczalność wiadomości e-mail
- [+] Łatwa integracja z backendem .NET
- [+] Możliwość monitorowania statystyk wysyłek
- [+] Możliwość późniejszej rozbudowy i przejścia na płatny plan

### Negatywne

- [-] Limit darmowych maili może być niewystarczający przy większej liczbie użytkowników
- [-] Niektóre funkcje premium wymagają dodatkowej opłaty
- [-] Uzależnienie projektu od zewnętrznej usługi (vendor lock-in)
