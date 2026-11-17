# Сундучки (DuoChest)

A phone/tablet web game for a 3–4 year old learning Russian, built around the
Duolingo-style reward chest: do three small tasks, get a chest, tap it three
times for a chance to upgrade its rarity, open it, and collect the animal,
dinosaur or gem inside.

The game is `index.html` plus the recorded voice in `voice-ru.js`. No server, no accounts.
Open it in any modern browser (Safari on iPhone/iPad, Chrome on Android) and
add it to the home screen.

## What's inside

**Tasks** (rotate randomly; each can be switched off in the parents' screen):

| Task | What the child does | Difficulty curve |
|---|---|---|
| Счёт 1–10 | Hears «Где три яблока?» and taps the group of three, or counts a group and taps the numeral. Nouns agree with the numeral: одна рыбка, две рыбки, пять рыбок | Starts with 1–3, adds one number for every 5 correct answers up to 10 |
| Животные | Hears «Где лошадка?» and taps the right animal out of 3 (30 animals, in the diminutive forms kids use: курочка, лошадка, хрюшка…) | Fixed; this is practice |
| Буквы | New letters get an intro card («Это буква эм. Мяч.»), then «Найди букву эм» among 3 letters. Tapping a wrong letter still teaches it | Starts with А О У, unlocks one letter per 4 correct answers, in primer order |
| Найди предмет | A scene (room, park, beach, kitchen) with 15 objects; find three in turn («Найди носок!») | Fixed; about one round in three |

**Chest** – three taps, each with a chance to upgrade: Common → Rare → Epic → Legendary.
Per-tap upgrade probabilities are 50 % / 35 % / 35 %. Simulated over two million chests:

| Tier | Share of chests | Gems |
|---|---|---|
| Обычный | 12.6 % | 1 |
| Редкий | 49.9 % | 3 |
| Эпический | 31.5 % | 5 |
| Легендарный | 6.1 % | 10 |

Tune `UPGRADE_P` near the top of the script to change this.

**Chest animation** – the tap wobble and orbiting comet arc are modelled on a
Duolingo screen recording. Opening is the big moment, in four beats of about
three seconds: the screen dims while the chest shakes harder and harder and
light leaks from under the lid; it bursts open with a white flash, shockwaves,
a screen shake and confetti; the reward rises out of the chest in a beam of
light; the chest sinks away and the reward lands on a stage of spinning rays,
its name popping in letter by letter while confetti rains. Rarer chests get
more confetti and fireworks. An upgrade swells and flashes the chest, slams
the new rarity label in and fires a coloured burst.

**Collection** – 40 stickers (10 per tier) saved in the browser's `localStorage`,
plus a gem counter. Tap a sticker to hear its name.

**Voice** – every phrase the game can say (530 clips, about 11 minutes) is
recorded ahead of time and shipped in `voice-ru.js`, so it sounds the same on
every phone and does not depend on the device's speech engine. The recordings
are made by [RHVoice](https://rhvoice.org/) (voice «Арина») with the stress of
every word marked by hand. The device's own speech engine is used only as a
fallback, and only if it has a real Russian voice.

To change wording or add words, edit the block between `VOICE-DATA-BEGIN` and
`VOICE-DATA-END` in `index.html` (a `+` goes before each stressed vowel) and
re-record:

```
sudo apt-get install rhvoice rhvoice-russian ffmpeg
node tools/build-voice.mjs            # or: node tools/build-voice.mjs victoria 95
```

**Sound effects** are synthesised with Web Audio, so there are no other audio
files. Voice and effects can be turned off separately in the parents' screen.

## Parents' screen (⚙️ Родителям)

Toggle task types, voice and sounds; see rounds played, accuracy, which
numbers and letters are currently unlocked; reset all progress.

## Deploying

Any static host works. For GitHub Pages: Settings → Pages → Source: **GitHub Actions**.
The workflow in `.github/workflows/pages.yml` publishes the repository root on
every push to `main`.

## Editing content

All word lists are plain arrays at the top of the `<script>` in `index.html`:
`ANIMALS`, `COUNT_ITEMS` (with gender and three number forms), `LETTERS`,
`HIDDEN_ITEMS` (with the form used in «Найди …»), `SCENES` and `REWARDS`.
After changing any spoken word, re-run `tools/build-voice.mjs`.
