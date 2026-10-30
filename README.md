# Zgłoszenia OXM w ClickUp — skille dla Claude

Plugin `zgloszenia` pozwala w rozmowie z Claude:

| Komenda | Co robi |
|---|---|
| `/zgloszenia:start` | sprawdza połączenie z ClickUp i prowadzi przez jego podłączenie |
| `/zgloszenia:nowe` | zakłada zgłoszenie do działu — te same pytania co formularz „Zgłoszenia do działów”, z kontrolą, czy ktoś już tego nie zgłosił |
| `/zgloszenia:czytaj` | wczytuje zgłoszenie z polami, komentarzami i plikami |
| `/zgloszenia:szukaj` | wyszukuje zgłoszenia i podobne sprawy |

Komend nie trzeba pamiętać — wystarczy napisać np. „załóż zgłoszenie do logistyki” albo wkleić link do zadania.

**W repozytorium nie ma żadnych kluczy ani haseł.** Połączenie z ClickUp idzie przez oficjalny konektor ClickUp w Claude: logujesz się w oknie ClickUp, a Claude działa z Twoimi uprawnieniami.

## Instalacja

### 1. Podłącz ClickUp (raz)

Claude → **Ustawienia** → **Konektory** → **ClickUp** → **Połącz** → zaloguj się firmowym kontem, wybierz workspace **OXM** → **Zezwól**.

### 2. Dodaj plugin

**Aplikacja Claude (desktop, Cowork / Code):** **Ustawienia** → **Pluginy** (Customize → Plugins) → **Dodaj marketplace** → wpisz `michaloxm/public-clickup-skills` → zainstaluj plugin **zgloszenia**.

**Claude Code (terminal):**

```
/plugin marketplace add michaloxm/public-clickup-skills
/plugin install zgloszenia@oxm-zgloszenia
```

Aktualizacje: `/plugin` → **Marketplaces** → `oxm-zgloszenia` → **Enable auto-update**.

### 3. Sprawdź

Nowa rozmowa → `/zgloszenia:start`. Skill sprawdzi połączenie, a jeśli czegoś brakuje — poprowadzi krok po kroku.

## Rozwój

- Działy, listy i pytania formularza: [plugins/zgloszenia/DZIALY.md](plugins/zgloszenia/DZIALY.md) — zmiana działu albo pola w ClickUp = zmiana tutaj.
- Skille: `plugins/zgloszenia/skills/<skill>/SKILL.md`. Plugin bez pola `version`: wersją jest commit.
- Repozytorium jest publiczne: żadnych tokenów, haseł, danych klientów.
