---
name: szukaj
description: Wyszukuje zgłoszenia (zadania) w ClickUp po słowach, dziale, osobie, statusie i dacie oraz znajduje zgłoszenia podobne do opisanej sprawy albo do wskazanego zgłoszenia (np. czy ktoś już to zgłaszał, czy był podobny błąd). Używaj gdy użytkownik mówi „znajdź zgłoszenie”, „szukaj w ClickUp”, „moje zgłoszenia”, „czy ktoś już zgłaszał…”, „były podobne sprawy?”, „podobne do IT-123”, „zgłoszenia logistyki z tego tygodnia”. Trigger: /zgloszenia:szukaj.
---

# zgloszenia:szukaj

Tylko odczyt. Najpierw skill **zgloszenia:start** (tylko sprawdzenie połączenia). Działy i listy: `${CLAUDE_PLUGIN_ROOT}/DZIALY.md`.

Narzędzie: `clickup_search` z `filters.asset_types: ["task"]`. Gdy jest `next_cursor`, pobieraj kolejne strony (do ok. 50 wyników; więcej → zaproponuj zawężenie).

## Tryb 1 — wyszukiwanie

Przełóż prośbę na filtry:
- słowa → `keywords`: 1–3 najbardziej charakterystyczne słowa (numer zamówienia, nazwa produktu, nazwa systemu), nie całe zdanie,
- „moje”, „zgłoszone przeze mnie” → `creators: ["me"]`; „przypisane do mnie” → `assignees: ["me"]`; konkretna osoba → imię i nazwisko,
- dział → `location.projects` (przestrzeń działu) albo `location.subcategories` z list_id z DZIALY.md, gdy chodzi tylko o zgłoszenia,
- otwarte → `task_statuses: ["unstarted", "active"]`; zamknięte → `["done", "closed"]`,
- czas („w tym tygodniu”, „od września”) → `created_date_from/to`, licząc od `current_time` z `clickup_get_user`, nie od własnego zegara.

Brak wyników → spróbuj raz szerzej (mniej słów, odmiana słowa, bez polskich znaków, bez filtra statusu) i powiedz, co zmieniłeś.

## Tryb 2 — podobne zgłoszenia

Wejście: opis sprawy albo zgłoszenie (link / IT-123 → najpierw `clickup_get_task` z `include: ["description", "custom_fields"]`).

1. Ułóż 3–5 krótkich zapytań z różnych stron sprawy: rzeczownik + system/miejsce („InPost koszyk”), identyfikatory (numer zamówienia, produktu, nazwa producenta), komunikat błędu, synonimy (zwrot/reklamacja, dostawa/wysyłka/kurier). Uwzględnij odmianę („import”/„importu”).
2. Każde zapytanie → `clickup_search` (bez filtra statusu, żeby znaleźć też sprawy już załatwione). Pomiń samo zgłoszenie wejściowe.
3. Połącz, usuń duplikaty, oceń podobieństwo po tytule i miejscu (dział, lista). Dla 3–5 najlepszych kandydatów, gdy tytuł nie przesądza, sprawdź opis (`clickup_get_task`, `include: ["description"]`).
4. Pokaż najwyżej 10, od najbardziej podobnych, z krótkim „dlaczego podobne”. Nie ma nic naprawdę podobnego → powiedz to wprost, nie dopychaj luźnych trafień.

## Wynik

```
Znalazłem 4 zgłoszenia:

1. [IT-685 Proszę o sprawdzenie importu z Unique…](url) — czeka na akceptację · Rozwój oprogramowania · 2026-10-29 · Joanna Walczyk
   dlaczego podobne: ten sam import stanów Unique, brak aktualizacji produktów
2. …
```

Na końcu jedna podpowiedź: „Chcesz, żebym wczytał któreś w całości?” (**zgloszenia:czytaj**) albo przy szukaniu duplikatu: „Nic podobnego — założyć nowe zgłoszenie?” (**zgloszenia:nowe**).
