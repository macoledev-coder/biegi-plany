# Biegi — plany treningowe

Plany treningowe do pobrania w aplikacji **Biegi** (Android). Aplikacja czyta katalog
`catalog.json` z gałęzi `main`:

```
https://raw.githubusercontent.com/macoledev-coder/biegi-plany/main/catalog.json
```

Repozytorium jest publiczne do odczytu; zmiany wprowadzają tylko właściciel i dodani współpracownicy.

## Plany

| Plik | Plan | Dla kogo | Tygodnie × treningi |
|---|---|---|---|
| `first-5k.json` | Pierwsze 5 km | od zera do 30 min biegu | 8 × 3 |
| `first-5k-12w.json` | Pierwsze 5 km — 12 tygodni | od zera, łagodniej | 12 × 3 |
| `5k-50plus.json` | 5 km dla osób 50+ | 50+, start od 30 s biegu | 14 × 3 |
| `run-60min.json` | Godzina biegu | biegasz 30 min → 60 min | 10 × 3 |
| `first-10k.json` | Pierwsze 10 km | biegasz 5 km / 30 min | 10 × 3 |
| `10k-faster.json` | 10 km — szybciej | masz za sobą 10 km | 8 × 4 |
| `first-20k.json` | Pierwsze 20 km | biegasz 10 km / 60 min | 12 × 4 |
| `20k-steady.json` | 20 km — spokojnie | biegasz 60 min, 3 treningi | 16 × 3 |

Plany zostały opracowane samodzielnie na podstawie ogólnie przyjętych zasad treningu biegowego
(przeplatanie biegu z marszem na początku, dni odpoczynku między treningami, stopniowe wydłużanie
długiego biegu z lżejszymi tygodniami, wymagania wstępne dla 10 i 20 km), znanych m.in. z powszechnie
dostępnych planów typu „Couch to 5K” i planów na 10 km / półmaraton — nie są kopią żadnego z nich.
Mają charakter ogólny i nie zastępują porady lekarza ani trenera.

Format odcinków to rozgrzewka / bieg / marsz / schłodzenie na czas: szybsze odcinki (interwały,
bieg „w tempie”) są zapisane jako `run`, a ich intensywność opisuje tytuł i opis treningu.

## Struktura

```
catalog.json          lista planów dostępnych w aplikacji
plans/<id>.json       pojedyncze plany
```

### catalog.json

```json
{
  "formatVersion": 1,
  "updated": "2026-11-12",
  "plans": [
    {
      "id": "first-5k",
      "version": 1,
      "name": "Pierwsze 5 km",
      "description": "krótki opis na liście w aplikacji",
      "language": "pl",
      "weeks": 8,
      "url": "plans/first-5k.json",
      "sha256": "opcjonalnie: suma SHA-256 pliku planu (64 znaki hex)"
    }
  ]
}
```

- `id` i `version` muszą być **dokładnie takie same** jak w pliku planu — inaczej aplikacja odrzuci plik.
- `url` — ścieżka względem `catalog.json` (np. `plans/first-5k.json`) albo pełny adres `https://`.
- `id`: małe litery, cyfry i `-`; unikalne w katalogu.

### Plik planu

Format opisany w repozytorium aplikacji (`docs/ROADMAP.md` → „Format pliku planu”): `id`, `version`,
`name`, `description`, `language`, opcjonalny `weekPattern` (7 × `train`/`rest`) i `weeks` → `days` →
`segments` (`warmup` / `run` / `walk` / `cooldown` z `durationSec`, bloki `repeat`).

## Jak zaktualizować plan

1. Popraw `plans/<id>.json` i **zwiększ w nim `version`** o 1.
2. Ustaw tę samą `version` przy planie w `catalog.json` (i zaktualizuj `updated`).
3. Commit i push na `main`. Aplikacja pokaże „Aktualizuj” przy tym planie (GitHub może serwować
   starą wersję pliku jeszcze kilka minut).

## Jak dodać nowy plan

1. Dodaj `plans/<nowe-id>.json` z `"version": 1`.
2. Dopisz wpis do `plans` w `catalog.json`.
3. Commit i push.

Postęp użytkownika w aktywnym planie jest zapisany jako „tydzień–dzień”, więc przy aktualizacji
planu nie usuwaj ani nie przenumerowuj już istniejących tygodni i dni.
