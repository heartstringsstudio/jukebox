# Studio Jukebox

A single-page, browsable wall of every song Heartstrings Studio has produced.
Pure HTML/CSS/vanilla JS — no build step, no frameworks. Songs load from
`songs.json` in this same folder.

**Live site:** https://heartstringsstudio.github.io/jukebox/
**Share link:** https://tinyurl.com/hsjukebox (use this one on Facebook and in messages)

## How to add a song

1. Open `songs.json` (in GitHub: click the file, then the pencil icon to edit).
2. Copy an existing entry and paste it into the list (watch the commas between
   entries), then fill in the fields:

   ```json
   {
     "title": "Song Title Here",
     "occasion": "memorial",
     "year": 2026,
     "blurb": "One warm sentence about the story behind the song.",
     "youtubeId": "dQw4w9WgXcQ",
     "featured": false
   }
   ```

   - **occasion** must be exactly one of: `memorial`, `celebration-of-life`,
     `wedding`, `milestone`, `tribute`, `holiday`
   - **youtubeId** is just the ID, not the full link. For
     `https://youtu.be/qdAYOA_1ok0` the ID is `qdAYOA_1ok0`.
   - **featured** — set `true` to pin the song to the top with a ★ ribbon.
     Keep it to two or three songs so "featured" still means something.

3. Commit the change. Done — the site updates itself within a minute or two.
   No other file needs to change.

**Tip:** if the page ever shows "We couldn't load the jukebox," the last edit
probably broke the JSON (usually a missing or extra comma). Paste the file into
https://jsonlint.com to find the exact spot.

## How to deploy (one-time setup)

This repo is a GitHub Pages project site for the `heartstringsstudio` account:

1. In this repo, go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to "Deploy from a branch,"
   choose branch `main` and folder `/ (root)`, and save.
3. After a minute, the site is live at
   `https://heartstringsstudio.github.io/jukebox/`.

Every commit to `main` after that redeploys automatically.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole site — layout, styles, and player logic |
| `songs.json` | The song list (the only file you edit day-to-day) |
| `favicon.png` | Browser-tab icon, copied from the main site |
