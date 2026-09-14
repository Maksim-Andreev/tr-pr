# Три причины — setup

Three files matter. **`reasons.json` is the only one you will ever edit again.**

```
index.html            the app (don't touch)
reasons.json          your reasons  ← this is the one
studio.html           the recording tool
audio/                your voice: 001.wav, 002.wav ...
manifest.webmanifest  makes it install as an app
sw.js                 makes it work offline
icons/                home screen icon
```

Every reason now has a permanent `id`. The recording for it is `audio/<id>.wav`.
The id is what ties the two together, so **never change an id** — reorder the list
freely, but an id belongs to its line forever.

---

## 1. Check `reasons.json` (already filled in)

Your 118 reasons are in, deduplicated, with a few spelling and comma fixes.
Three are pinned to 17 September — they open the app on her birthday and every
birthday after. Open the file in any text editor to change anything.

```json
"her":       "Anna",          ← her name, or leave "" for no name
"signature": "Maksim",        ← how you sign off
"startDate": "2026-09-17",    ← her birthday. Day 1.
```

Adding more is just another line in the list — comma at the end of every line except
the last:

```json
"reasons": [
  "Потому что ты спасатель улиток",
  "Потому что ты любишь большие кружки"
]
```

**Rules that matter:**

- Straight double quotes only. If a reason contains a quote mark, write it `\"like this\"`.
- No comma after the last one. This is the only thing that will break the file.
- Paste the whole file into **jsonlint.com** before you commit if you're unsure. Takes 5 seconds.

`special` pins a reason to a date — `"09-17"` fires every year (birthday, anniversary),
`"2027-09-17"` fires once. It becomes reason *one* that day.

**How many do you need?** Not 1,095. The app deals a shuffled deck: nothing repeats
until the whole list has been used once, then it reshuffles into a new order.

| Reasons in the file | Days before anything repeats |
|---|---|
| 118 (now) | **39** |
| 180 | 60 |
| 250 | 83 |
| 365 | 121 |

You have 39 days covered. Add a line whenever something occurs to you — that is the
whole point of building it this way.

---

## 2. Put it online (15 minutes, free, permanent)

1. Make a free account at **github.com**.
2. **New repository** → name it something she won't guess, e.g. `tr-app`
   → **Public** → Create. (It must be public for free Pages hosting. Nobody finds it
   without the link.)
3. On the repo page: **Add file → Upload files**. Drag in everything — `index.html`,
   `reasons.json`, `studio.html`, `sw.js`, `manifest.webmanifest`, and the `icons` and
   `audio` folders. Commit.
4. **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main`, folder `/ (root)`
   → Save.
5. Wait ~2 minutes. Your app is at:
   `https://YOURNAME.github.io/tr-app/`

Open that on your phone to check it.

### Adding reasons later

On github.com, open `reasons.json` → pencil icon → add lines → **Commit changes**.
Live in about a minute. No app update, nothing on her phone to do.

---

## 3. Record your voice

Open **`https://YOURNAME.github.io/tr-app/studio.html`** on a laptop. Use the deployed
URL, not the file on your disk — browsers only hand the microphone to a real web page.
(Local alternative: run `python3 -m http.server 8000` inside the folder and open
`localhost:8000/studio.html`.)

Then:

1. **Choose audio folder** → pick the `audio` folder. In Chrome or Edge every take is
   written straight into it. In Safari or Firefox takes land in Downloads instead and
   you move them across at the end.
2. Press **Record**, read the line, press **Record** again to stop. It jumps to the next
   one on its own.
3. <kbd>Space</kbd> record/stop, <kbd>P</kbd> to hear it back, <kbd>→</kbd> to skip.
   Re-recording a line overwrites it.

Each take is trimmed of silence at both ends, levelled to a consistent volume, and saved
as mono WAV — the one format every iPhone plays with no conversion. About 60 KB per
second, so all 119 come to roughly 40 MB. That is fine for GitHub and it downloads once
on her phone, then stays cached.

The progress bar doubles as a level meter while recording — if it doesn't move, the
microphone isn't live. Check that before you get twenty takes in.

**You don't have to finish.** Any reason without a recording simply shows no play button.
Record thirty tonight and the rest whenever.

Then upload the `audio` folder to the repo the same way as everything else.

---

## 4. Put it on her home screen

On **her iPhone**, in **Safari** (this does not work in Chrome):

1. Open the link.
2. Share button → **Add to Home Screen** → Add.

It now has its own icon, opens full screen with no browser bars, and works with no
signal. It looks and behaves like an app because, as far as iOS is concerned, it is one.

---

## 5. A daily nudge (optional, 2 minutes)

A web app can't reliably push notifications without a server, so use Shortcuts instead —
it's actually better, because it opens the thing rather than just mentioning it.

On her phone: **Shortcuts → Automation → + → Time of Day → 8:00 AM, Daily →**
**Run Immediately** (turn *Notify When Run* off) **→ Add Action → Open App →**
pick **Три причины**.

Every morning at 8 it opens itself.

---

## The bit worth knowing

The app contains no reasons and no audio. It fetches `reasons.json` every time it opens,
pulls each recording the first time it is played, and caches both. That's the whole design, and it's why you can keep writing for
years without ever touching the app again.

Each day's three are dealt deterministically from the date — so the same day always
shows the same three, on any device, and she can swipe back through every day since
her birthday. Days she has already seen are locked to what she actually saw, so adding
new reasons never rewrites her history.
