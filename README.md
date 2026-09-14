# Reeder

A reading aid for when your eyes glaze over a wall of text. One HTML file, no build, no server required.

Paste text, press **Read**, pick a mode:

| Mode | What you get |
|---|---|
| **Speed** | One word (or up to 20) flashed in the centre of the screen at a set WPM, Spritz-style focus letter so your eyes never move. Silent. |
| **Speech** | The text laid out as readable paragraphs, read aloud by an Azure neural voice, current word or sentence highlighted and kept in view. Tap a word to jump there. |
| **Speed speech** | The flashing word, flipped exactly as the voice says it. Ears set the pace, eyes have nowhere else to go. |

Each mode keeps its own settings. Position is remembered per text, so you can close it and pick up where you stopped.

## Run it

Open `index.html` in a browser. That's it.

To reach it from a phone on the same network:

```sh
python3 -m http.server 8080 -d /path/to/reeder
```

then open `http://<your-ip>:8080`.

## Voice (optional)

Speech modes use [Azure Speech](https://portal.azure.com) neural voices. Create a *Speech service* resource (free tier F0 is 500k characters/month), then paste **key 1** and the **region** (e.g. `eastus`) under *Azure Speech* in the settings. The key is stored in your browser's localStorage and is only ever sent to Azure.

Without a key, the speech modes still work — they pace by timer instead of voice.

## Controls

| Key / tap | Action |
|---|---|
| Space, or tap the stage | Play / pause. Paused shows the whole sentence with your spot bolded. |
| ← / → | Previous / next sentence |
| ↑ / ↓ | Speed up / slow down |
| Esc / Done | Back to the paste screen |

## How the sync works

Text is split into ~150-word chunks. Each chunk is synthesized with the Speech SDK, which fires a `wordBoundary` event with the audio offset of every word; those offsets drive the display against `audio.currentTime`. The next chunk is fetched while the current one plays. Changing the speed mid-read adjusts `playbackRate` (pitch preserved) until the next chunk is synthesized at the new pace.

## Self-check

Open `index.html?test` and look at the console — asserts for the tokenizer, frame grouping and sentence navigation.
