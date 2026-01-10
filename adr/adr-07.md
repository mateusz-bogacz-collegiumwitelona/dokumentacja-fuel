# ADR-007: Wybór narzędzia do testowania wysyłki e-mail

- **Status:** Zaakceptowana
- **Data:** 2025-10-06
- **Autorzy:** Mateusz Bogacz-Drewniak, Szymon Mikołajek

---

## 1. Kontekst

> Aplikacja wysyła wiadomości e-mail w takich scenariuszach jak:

- reset hasła,
- potwierdzenie rejestracji,
- inne komunikaty systemowe.

Na potrzeby developmentu oraz testów automatycznych wymagane jest narzędzie, które:

- przechwyci wysyłane e-maile,
- pozwoli na ich podgląd w przeglądarce,
- działa w środowisku lokalnym i testowym,
- jest łatwe do uruchomienia w Dockerze,
- nie wymaga konfiguracji prawdziwego SMTP.

## Celem jest przyspieszenie pracy zespołu oraz możliwość testowania logiki mailingowej bez faktycznego wysyłania e-maili.

## 2. Rozważane opcje

### Opcja 1: MailPit v1.27

- **Zalety:**
  - [+] Bardzo łatwa konfiguracja (jeden kontener Docker).
  - [+] Interfejs WWW do podglądu wysłanych wiadomości.
  - [+] Wsparcie dla API do pobierania i testowania maili automatycznie.
  - [+] Niska waga i wysoka stabilność.
  - [+] Aktywnie rozwijany projekt.
- **Wady:**
  - [-] Narzędzie nie jest oficjalnym standardem (wciąż rozwijane).

### Opcja 2: MailHog 1.0.1

- **Zalety:**
  - [+] Popularne i długo obecne na rynku.
  - [+] Prosty interfejs WWW.
- **Wady:**
  - [-] Słabsze wsparcie API niż w MailPit.
  - [-] Brak aktywnego rozwoju w ostatnich latach.

---

## 3. Decyzja

> Zespół wybrał MailPit jako narzędzie do testowania wysyłki e-mail.

---

## 4. Uzasadnienie

> MailPit jest nowoczesnym narzędziem do przechwytywania wiadomości e-mail wysyłanych przez aplikację. Posiada interfejs dostępny przez przeglądarkę, dzięki czemu możliwa jest szybka weryfikacja wysyłanych treści podczas developmentu. Najważniejszym argumentem była łatwość integracji z Dockerem — MailPit uruchamia się w jednym kontenerze i nie wymaga dodatkowej konfiguracji, co znacząco ułatwia pracę zespołu. Narzędzie posiada również proste API, które pozwala wykorzystać je w testach automatycznych.

---

## 5. Konsekwencje

### Pozytywne

- [+] Łatwy podgląd wysyłanych maili w przeglądarce.
- [+] Brak ryzyka przypadkowego wysyłania maili na realne adresy.
- [+] Stabilne środowisko do testowania mailingu.
- [+] Pełna integracja z Dockerem i CI/CD.

### Negatywne

- [-] Niewielkie ryzyko zmian API związanych z szybkim rozwojem projektu.
- [-] W produkcji wymagany będzie prawdziwy serwer SMTP.
