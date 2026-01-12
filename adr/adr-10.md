# ADR-010: Zastąpienie usługi deploy

- **Status:** Zaakceptowane
- **Data:** 2026-01-11
- **Autorzy:** Mateusz Bogacz-Drewniak, Paweł Kruk,

---

## 1. Kontekst

> Ze względu na brak dobre znajomości Microsoft Azure team zadecydował że bezpieczniej będzie postawić cała aplikacje jako jeden kontener na vps. Głównie zależy na łatwym panelu admina i znajomości, oraz żeby maszyna pracowała na Linuxie

---

## 2. Rozważane opcje

### Opcja 1: Azure VPS

- **Zalety:**

  - [+] Uznay dostawca
  - [+] Dużo dokumentacji

- **Wady:**
  - [-] Cena przekraczająca budżet
  - [-] Trudności z konfiguracją wersji Linux

---

### Opcja 2: OHVCloude

- **Zalety:**

  - [+] Tani
  - [+] DevOps team zna już tą platformę
  - [+] Różne wersje Linuxa do wyboru

- **Wady:**
  - [-] Mało dokumentacji

---

## 3. Decyzja

> Wybieramy **OHVCloude** jako usługę do wysyłki maili w projekcie.

---

## 4. Uzasadnienie

> OHVCloude zostało wybrane ze względu na znajomość środowiska, łatwe ogowanie i możliwość wybrania dowolnego Linuxa

---

## 5. Konsekwencje

### Pozytywne

- [+] Szybki start projektu dzięki prostemu wdrożeniu

### Negatywne

- [+] Trzeba było zapłacić
