# Działy i listy zgłoszeń (workspace OXM, id 90122134419)

Wspólne źródło dla skilli `zgloszenia:*`. Pytania i opcje odpowiadają formularzowi „Zgłoszenia do działów” w ClickUp.

## Dział → lista „Zgłoszenia do działu”

| Dział (opcja w formularzu) | Czego dotyczy (podpowiedź) | list_id |
|---|---|---|
| Rozwój oprogramowania | sklep, panel, błędy systemów, importy, integracje, dostępy do systemów | 901223368266 |
| Produkt | karty produktów, opisy, wymiary, zdjęcia, ceny | 901223367204 |
| Marki własne | produkty marek własnych OXM | 901223367384 |
| Marketing | kampanie, banery, newslettery, treści, social media | 901223367409 |
| Sprzedaż | oferta, rabaty, klienci biznesowi, marketplace'y | 901223367492 |
| Księgowość | faktury, płatności, rozliczenia, zwroty pieniędzy | 901223367532 |
| HR | kadry, rekrutacja, umowy, urlopy | 901223364429 (lista ogólna, patrz niżej) |
| Obsługa klienta | sprawy klientów, reklamacje, rekompensaty | 901223367562 |
| Zamówienia | realizacja zamówień, producenci, terminy | 901223367573 |
| Logistyka | wysyłki, kurierzy, magazyn, transport | 901223367587 |
| Utrzymanie budynku | biuro, sprzęt biurowy, naprawy, sprzątanie | 901223367597 |
| Wdrożenia | wdrożenia nowych procesów i narzędzi | 901223376725 |
| Architekci kierunku | kierunek rozwoju, decyzje architektoniczne | 901223367621 |
| Wsparcie Clickup | konfiguracja ClickUp, listy, automatyzacje, uprawnienia | 901223367845 |

HR nie ma własnej listy: zgłoszenie trafia na listę ogólną „Zgłoszenia do działów” w przestrzeni Grupa OXM General (901223364429) z polem „Do którego działu kierujesz zgłoszenie?” = HR.

Lista nie odpowiada (404, brak dostępu) → znajdź ją `clickup_search` (słowa „Zgłoszenia do działu” + nazwa działu) albo przez `clickup_get_workspace_hierarchy`; nie zgaduj innej listy.

## Pytania formularza → pola zadania

| Pytanie | Pole w ClickUp | Typ | Wymagane |
|---|---|---|---|
| Krótko opisz czego dotyczy zgłoszenie | nazwa zadania | tekst | tak |
| Do którego działu kierujesz zgłoszenie? | „Do którego działu kierujesz zgłoszenie?” (+ „Space”, jeśli lista ma) | lista wyboru | tak |
| Kategoria zadania | „Kategoria zadania” | lista wyboru | tak |
| Określ wpływ sprawy na pracę lub klientów | „Określ wpływ sprawy na pracę lub klientów” | lista wyboru | tak |
| Podaj szczegóły — co się dzieje, czego brakuje, gdzie? | opis zadania | markdown | nie (ale zawsze dopytaj) |
| Podaj link | „Podaj link:” | url | nie |
| Oczekiwany efekt — co trzeba zrobić? | „Oczekiwany efekt” | tekst | nie (ale zawsze dopytaj) |
| Zgłaszający | „Zgłaszający” | osoba | tak — osoba zakładająca |
| Na kiedy potrzebujesz realizacji? | termin (due date) | data | tak |
| Załączniki | załączniki zadania | pliki | nie |

Status nowego zgłoszenia: „czeka na akceptację”. Lider działu decyduje o przyjęciu.

### Kategoria zadania

Błąd · Wsparcie operacyjne · Brak/korekta informacji · Nowa funkcjonalność · Ulepszenie/pomysł · Dostępy i uprawnienia · UX · Integracja · Bezpieczeństwo · Dane i raporty · Infrastruktura/DevOps · Dług techniczny · Wydajność · e2e

### Wpływ sprawy

| Opcja | Kiedy |
|---|---|
| Blokuje sprzedaż lub klientów | klient nie może kupić, zapłacić, dostać towaru; sklep nie działa |
| Blokuje pracę | ktoś w firmie nie może wykonać swojej pracy |
| Utrudnia pracę lub korzystanie ze sklepu | da się obejść, ale jest uciążliwe |
| Dotyczy zaplanowanej pracy lub potrzebnego wsparcia | zaplanowane zadanie, prośba o pomoc |
| Jest propozycją usprawnienia | pomysł, coś mogłoby działać lepiej |

Opcje list wyboru ustawiasz po **id opcji** z `clickup_get_custom_fields` (list_id), dopasowując nazwę bez względu na wielkość liter, polskie znaki i spacje na końcu (np. „Blokuje pracę ” ma spację).
