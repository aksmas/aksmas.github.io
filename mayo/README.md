# Happy Birthday, Mayo 💛

A full-screen, swipeable birthday carousel for Mayuri's 31st —
**31 moments to cherish, one for every year of your life.**

Flow: **passcode lock → "tap to begin" → carousel**
Carousel = a start slide, 31 moment slides (photo + Year 1–31 + message), and a
heartfelt closing slide with a tap-to-open gift.

---

## 1. Messages + photo order  →  `messages.json`

```json
[
  {"filename.jpg": "message for this moment"},
  {"another.jpg":  "next message"}
]
```

- **Year number = position in the list.** 1st entry = Year 1, 2nd = Year 2, …
- **Reorder** the list to reorder the carousel.
- Filenames are looked up inside `photos/web/` (see `CONFIG.photoDir` in `index.html`).
- The messages currently in the file are placeholders — edit them to your own words.

### Adding / replacing photos

1. Put the full-size original in `photos/` (kept local, not published — see below).
2. Make a web-sized copy:
   ```bash
   cd mayo/photos
   for f in *.jpg; do sips -Z 1600 -s formatOptions 70 "$f" --out "web/$f"; done
   ```
3. Add/rename its entry in `messages.json`.

Originals in `photos/` are git-ignored (they were ~119 MB); only the ~13 MB
`photos/web/` copies are published.

---

## 2. Music  →  `CONFIG.music` in `index.html`

```js
music: {
  src: "",          // paste a YouTube link OR a direct audio URL (.mp3/.m4a/.ogg)
  volume: 0.5,
  startSeconds: 0,  // YouTube only
}
```

- Paste **either** a YouTube link (`https://youtu.be/…`) **or** a direct audio file
  URL. The page auto-detects which and shows a mute/unmute button.
- YouTube caveat: the video must allow embedding and may play a short ad. A direct
  `.mp3` link is the most reliable / ad-free option.
- Music starts on the "tap to begin" tap (browsers block autoplay before a tap).

---

## 3. Passcode  →  `CONFIG.lock` in `index.html`

The page is gated so you can send the link early and the passcode at midnight.

```js
lock: {
  enabled: true,
  hashHex: "…",       // SHA-256 of the passcode (lowercased, trimmed)
  unlockAt: "",       // optional backup, e.g. "2026-09-23T00:00:00" (auto-opens then)
}
```

- ⚠️ **Current passcode is a placeholder: `happy31`.** Change it!
- To set your own code, hash it and paste the result into `hashHex`:
  ```bash
  printf '%s' 'yourcode' | shasum -a 256
  ```
- Note: this is a *soft* lock (client-side), perfect for a surprise but not real
  security — a determined dev could read the page source. The code is stored as a
  hash so a casual "view source" won't reveal it.
- `unlockAt` is optional: set it to auto-unlock at a moment (e.g. midnight) even if
  she doesn't have the code yet. Leave `""` for passcode-only.

---

## 4. Start / end words  →  `CONFIG.hero` and `CONFIG.finale` in `index.html`

Edit the kicker, name, tagline (start) and the gift line, closing message, and
signature (end).

---

## View / publish

- Local: run a tiny server (needed because `messages.json` is fetched):
  ```bash
  cd mayo && python3 -m http.server 8777
  ```
  then open `http://localhost:8777/`.
- Her phone: publish via GitHub Pages and send `https://aksmas.github.io/mayo/`.

No build step, no dependencies.
