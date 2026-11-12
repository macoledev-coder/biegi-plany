# Biegi — plany treningowe

Plany treningowe do pobrania w aplikacji **Biegi** (Android). Aplikacja czyta katalog
`catalog.json` z gałęzi `main`:

```
https://raw.githubusercontent.com/macoledev-coder/biegi-plany/main/catalog.json
```

Repozytorium jest publiczne do odczytu; zmiany wprowadzają tylko właściciel i dodani współpracownicy.

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
