---
name: start
description: Pierwsze uruchomienie i sprawdzenie połączenia z ClickUp dla skilli zgłoszeń — prowadzi krok po kroku przez podłączenie konektora ClickUp (bez kluczy i tokenów) i pokazuje, co można robić dalej. Używaj na /zgloszenia:start, gdy użytkownik pyta „jak zacząć”, „czy mam połączenie z ClickUp”, „nie działa ClickUp”, „podłącz ClickUp”, oraz jako pierwszy krok skilli zgloszenia:nowe, zgloszenia:czytaj i zgloszenia:szukaj.
---

# zgloszenia:start

Rozmawiasz z osobą nietechniczną. Krótkie zdania, bez żargonu (nie mów „MCP”, „API”, „token” — mów „połączenie z ClickUp”). Jedna instrukcja na raz, czekaj na potwierdzenie.

## Krok 1 — czy jest połączenie

Sprawdź, czy w tej rozmowie są narzędzia ClickUp (nazwy kończące się na `clickup_get_user`, `clickup_search`, `clickup_get_task`, `clickup_create_task`).

- Są → wywołaj `clickup_get_user`. Działa → krok 3.
- Nie ma albo `clickup_get_user` zwraca błąd autoryzacji → krok 2.

## Krok 2 — podłączenie ClickUp (raz na zawsze)

Logowanie odbywa się w oknie ClickUp. Nikt nie wkleja żadnych haseł ani kluczy do rozmowy — jeśli użytkownik spróbuje wkleić hasło albo klucz `pk_…`, poproś, żeby tego nie robił.

Poprowadź po kolei:

1. W aplikacji Claude (albo na claude.ai) kliknij swoje inicjały w lewym dolnym rogu → **Ustawienia** (Settings) → **Konektory** (Connectors).
2. Znajdź **ClickUp** i kliknij **Połącz** (Connect). Jeśli go nie ma na liście: **Przeglądaj konektory** (Browse connectors) → wyszukaj „ClickUp”.
3. Otworzy się okno ClickUp: zaloguj się firmowym kontem i wybierz workspace **OXM**, potem **Allow / Zezwól**.
4. Wróć do Claude i zacznij **nową rozmowę** (konektor jest widoczny w rozmowach rozpoczętych po podłączeniu). W nowej rozmowie wpisz `/zgloszenia:start`.

Jeśli w trakcie rozmowy narzędzia ClickUp są wyłączone: przycisk **+** (albo ikona suwaków) pod polem wiadomości → **Konektory** → włącz **ClickUp**.

Nie ma ClickUp w konektorach albo organizacja go blokuje → powiedz, żeby napisać do działu IT (Rozwój oprogramowania), i przerwij.

Po podłączeniu przerwij skill, który cię wywołał — użytkownik wraca w nowej rozmowie.

## Krok 3 — sprawdzenie workspace'u

`clickup_get_user` zwraca osobę i strefę czasową. Zgłoszenia są w workspace OXM (id `90122134419`). Gdy narzędzia zgłaszają kilka workspace'ów, przekazuj `workspace_id: "90122134419"`.

## Raport

Jedno zdanie + co dalej:

```
Połączenie z ClickUp działa — jesteś zalogowany jako Imię Nazwisko.

Co możesz zrobić:
• /zgloszenia:nowe — założyć zgłoszenie do działu (jak formularz w ClickUp)
• /zgloszenia:czytaj — wczytać zgłoszenie z polami, komentarzami i plikami (wklej link albo numer, np. IT-123)
• /zgloszenia:szukaj — znaleźć zgłoszenia albo sprawdzić, czy ktoś już zgłosił podobną sprawę
```

Wywołany jako krok innego skilla → nie pokazuj listy, tylko kontynuuj tamten skill.
