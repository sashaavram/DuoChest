# Сундучки (DuoChest)

A phone/tablet web game for a 3–4 year old learning Russian, built around the
Duolingo-style reward chest: do three small tasks, get a chest, tap it three
times for a chance to upgrade its rarity, open it, and collect the animal,
dinosaur or gem inside.

Everything is one file, `index.html`. No build step, no server, no accounts.
Open it in any modern browser (Safari on iPhone/iPad, Chrome on Android) and
add it to the home screen; it works offline after the first load.

## What's inside

**Tasks** (rotate randomly; each can be switched off in the parents' screen):

| Task | What the child does | Difficulty curve |
|---|---|---|
| Цифры 1–10 | Hears «Покажи, где три» and taps the group with 3 items, or counts a group and taps the numeral | Starts with 1–3, adds one number for every 5 correct answers up to 10 |
| Животные | Hears «Где лошадка?» and taps the right animal out of 3 (30 animals, in the diminutive forms kids use: курочка, лошадка, хрюшка…) | Fixed; this is practice |
| Буквы | New letters get an intro card («Это буква А. А — арбуз»), then «Найди букву А» among 3 letters. Tapping a wrong letter still teaches it («Это буква Б, банан») | Starts with А О У, unlocks one letter per 4 correct answers, in primer order |
| Найди предмет | A scene (room, park, beach, kitchen) with 15 objects; find three in turn («Найди носок!») | Fixed; about one round in three |

**Chest** – three taps, each with a chance to upgrade: Common → Rare → Epic → Legendary.
Per-tap upgrade probabilities are 50 % / 35 % / 35 %. Simulated over one million chests:

| Tier | Share of chests | Gems |
|---|---|---|
| Обычный | 12.5 % | 1 |
| Редкий | 50.0 % | 3 |
| Эпический | 31.4 % | 5 |
| Легендарный | 6.1 % | 10 |

Tune `UPGRADE_P` near the top of the script to change this.

**Collection** – 40 stickers (10 per tier) saved in the browser's `localStorage`,
plus a gem counter. Tap a sticker to hear its name.

**Audio** – prompts and praise use the browser's built-in Russian speech
voice (Web Speech API; on iOS this is the «Milena» voice). Sound effects are
synthesised with Web Audio, so there are no audio files to download. Both can
be turned off in the parents' screen.

## Parents' screen (⚙️ Родителям)

Toggle task types, voice and sounds; see rounds played, accuracy, which
numbers and letters are currently unlocked; reset all progress.

## Deploying

Any static host works. For GitHub Pages: Settings → Pages → Source: **GitHub Actions**.
The workflow in `.github/workflows/pages.yml` publishes the repository root on
every push to `main`.

## Editing content

All word lists are plain arrays at the top of the `<script>` in `index.html`:
`ANIMALS`, `COUNT_ITEMS`, `LETTERS` (with a picture word per letter),
`HIDDEN_ITEMS` (with the accusative form used in «Найди …»), `SCENES` and `REWARDS`.
