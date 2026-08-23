# Music Maker

A tiny step-sequencer for making synth melodies, hosted as a free Google Apps Script
(GAS) web app. Tap steps across a grid of synthesized drum/synth sounds, hit play, and
export the loop as a real `.mp3`.

## How it works

- **`Code.gs`** — `doGet()` serves `Index.html` as the web app's page.
- **`Index.html`** — the entire app: step-sequencer grid, a Web Audio synth engine
  (kick/snare/hats/bass/lead/chime, all generated in-browser with oscillators and
  filtered noise — no audio sample files), a lookahead scheduler for tight playback
  timing, and an MP3 export path (`OfflineAudioContext` render → `lamejs` encode →
  browser download).
- **`appsscript.json`** — Apps Script manifest / web app deployment config.

No backend storage, no external assets — everything runs client-side once the page
loads (the only network dependency is the `lamejs` encoder, loaded from a CDN).

## Local development

Since this is plain HTML/JS, you can iterate without deploying to Apps Script at all:
open `Index.html` directly in a browser, or serve the folder with any static file
server, to test the grid, playback, and export.

## Deploying to Apps Script

This project is managed with [`clasp`](https://github.com/google/clasp) so the code
lives as normal text files in git instead of the Apps Script web editor.

1. Install clasp and log in with your Google account:
   ```bash
   npm install -g @google/clasp
   clasp login
   ```
2. Either create a fresh Apps Script project bound to this folder, or clone an existing
   one you already made in script.google.com:
   ```bash
   clasp create --type webapp --title "Music Maker"
   # or, if you already have a project:
   clasp clone <script-id>
   ```
   This generates a `.clasp.json` with your project's script ID (not committed here
   since it's account-specific — add it locally).
3. Push the code and deploy:
   ```bash
   clasp push
   clasp deploy
   ```
4. Open the web app URL clasp prints out.

## Sample tracks

- **`track-001.mp3`** — 110 BPM, 1 bar (16 steps), four-on-the-floor kick with a
  backbeat snare, syncopated bass and lead lines, and a sparse chime accent:

  | Row | Steps |
  |---|---|
  | Kick | 1, 5, 9, 13 |
  | Snare | 5, 13 |
  | Hi-hat | 1, 3, 5, 7, 9, 11, 13 |
  | Open Hat | 15 |
  | Bass | 1, 4, 9, 11 |
  | Lead | 3, 7, 11, 15 |
  | Chime | 1, 9 |

## Roadmap

- Chaining multiple patterns into a longer song (currently a single loopable pattern).
- Saving/reloading patterns (currently in-memory only for the session).
