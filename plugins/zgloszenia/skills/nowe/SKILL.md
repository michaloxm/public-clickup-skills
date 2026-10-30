---
name: nowe
description: Zakłada nowe zgłoszenie do działu w ClickUp — zadaje te same pytania co formularz „Zgłoszenia do działów” (dział, kategoria, wpływ, szczegóły, oczekiwany efekt, link, termin, załączniki), sprawdza czy podobnej sprawy już nie zgłoszono i zakłada zadanie na liście „Zgłoszenia do działu” właściwego działu. Używaj gdy użytkownik mówi „załóż zgłoszenie”, „zgłoś do IT / marketingu / logistyki”, „mam problem z…”, „trzeba poprawić…”, „nowy ticket”, „potrzebuję od działu X”. Trigger: /zgloszenia:nowe.
---

# zgloszenia:nowe

Rozmawiasz z osobą nietechniczną. Prowadź jak dobry formularz: prosto, po polsku, bez żargonu. Najpierw skill **zgloszenia:start** (tylko sprawdzenie połączenia). Działy, listy, pola i opcje: `${CLAUDE_PLUGIN_ROOT}/DZIALY.md`.

## Krok 1 — opis sprawy

Poproś: „Opisz własnymi słowami, czego potrzebujesz albo co nie działa.” Jeśli użytkownik już to napisał, nie pytaj drugi raz.

Z opisu wyciągnij, co się da, i dopytaj tylko o brakujące rzeczy — maksymalnie 2–3 pytania naraz:
- **co się dzieje / czego brakuje / gdzie** — konkret: numer zamówienia, produkt, rynek (PL/CZ/SK/RO), strona, osoba,
- **oczekiwany efekt** — co ma być zrobione, po czym poznać, że sprawa jest załatwiona,
- **link**, jeśli sprawa dotyczy konkretnej strony, produktu, zamówienia w panelu,
- **termin** — „Na kiedy potrzebujesz realizacji? Podaj realny termin.” Zamień „jutro”, „w piątek” na datę według `current_time` z `clickup_get_user`, nie według własnego zegara,
- **błąd** → kroki, jak go wywołać, i od kiedy występuje,
- **załączniki** → „Masz zrzut ekranu albo plik? Podaj ścieżkę do pliku na dysku albo przeciągnij go do rozmowy.”

Nie wymyślaj faktów. Czego użytkownik nie wie, to pomiń.

## Krok 2 — dział, kategoria, wpływ

Zaproponuj na podstawie opisu (tabele w DZIALY.md) i poproś o potwierdzenie — z przyciskami (AskUserQuestion), jeśli są dostępne:
- **dział** — jeden z listy formularza,
- **kategoria zadania**,
- **wpływ na pracę lub klientów** — opisz opcje prostymi słowami; nie zawyżaj („blokuje” tylko gdy naprawdę nie da się pracować / kupić).

Nie wiesz, który dział → zapytaj, podając 2–3 najbardziej prawdopodobne.

## Krok 3 — czy ktoś już tego nie zgłosił

Zanim założysz, wykonaj wyszukiwanie podobnych jak w skillu **zgloszenia:szukaj** (tryb „podobne”), ograniczone do otwartych zgłoszeń (`task_statuses: ["unstarted", "active"]`).

- Jest bardzo podobne otwarte zgłoszenie → pokaż je (`[IT-123 tytuł](url)`, status, kiedy założone) i zapytaj: zakładamy mimo to, czy wystarczy dopisać się w istniejącym (wtedy przerwij i podaj link — komentarz użytkownik doda w ClickUp).
- Nic podobnego → idź dalej bez komentarza.

## Krok 4 — podgląd i akceptacja

Pokaż całe zgłoszenie dokładnie tak, jak trafi do ClickUp:

```
Dział: Logistyka
Tytuł: Brak InPost w koszyku dla zamówienia 877S89W
Kategoria: Błąd · Wpływ: Blokuje sprzedaż lub klientów · Termin: 2026-11-04
Link: https://…
Szczegóły:
…
Oczekiwany efekt:
…
Załączniki: zrzut.png
```

Tytuł: krótki i konkretny, jak w formularzu („Uzupełnić wymiary szafy X na karcie produktu”, nie „Problem”). Załóż dopiero po wyraźnym „tak”. Poprawki → popraw i pokaż ponownie.

## Krok 5 — założenie

1. `list_id` działu z DZIALY.md. `clickup_get_custom_fields` (list_id) → id pól i id opcji.
2. `clickup_create_task`:
   - `list_id`, `name`,
   - `markdown_description`: szczegóły (markdown, z kroków do odtworzenia przy błędzie),
   - `status`: „czeka na akceptację”,
   - `due_date`: termin,
   - `custom_fields` — tylko pola, które lista ma:
     - „Do którego działu kierujesz zgłoszenie?” → id opcji działu,
     - „Space” → opcja działu (nazwy różnią się wielkością liter/znakami, np. „Obsługa Klienta”, „Zamowienia”),
     - „Kategoria zadania”, „Określ wpływ sprawy na pracę lub klientów” → id opcji,
     - „Oczekiwany efekt” → tekst, „Podaj link:” → url,
     - „Zgłaszający” → `{"add":["<id użytkownika z clickup_get_user>"]}`.
   Nie ustawiaj przypisanych ani innych pól — robi to lider działu.
3. Załączniki z dysku: `clickup_request_attachment_upload` (task_id) i wyślij plik według zwróconej instrukcji. Plik dostępny tylko jako obrazek w rozmowie (nie ma go na dysku) → poproś o zapisanie go i podanie ścieżki albo powiedz, że można go dodać w ClickUp po otwarciu linku.
4. Błąd przy polu (np. brak opcji) → powiedz, czego nie udało się ustawić; nie zakładaj zadania drugi raz.

## Wynik

```
Zgłoszenie założone: [IT-123 Brak InPost w koszyku…](https://app.clickup.com/t/…)
Trafiło do działu Logistyka ze statusem „czeka na akceptację” — lider działu zdecyduje o przyjęciu. Powiadomienia o zmianach dostaniesz w ClickUp.
```
