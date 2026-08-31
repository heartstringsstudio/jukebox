# Studio Jukebox

A single-page, browsable sampler of songs Heartstrings Studio has produced.
Pure HTML/CSS/vanilla JS — no build step, no frameworks. Songs load from
`songs.json` in this same folder.

**Live site:** https://heartstringsstudio.github.io/jukebox/
**Share link:** https://tinyurl.com/hsjukebox (use this one on Facebook and in messages)

## The Control Panel (the easy way)

**https://heartstringsstudio.github.io/jukebox/admin.html**

Everything below can be done by hand in `songs.json`, but you don't have to.
The control panel is a page in this same repo that gives you:

- **Add a song** — a form. Paste the YouTube link and it pulls the ID out for
  you. No JSON, no commas to get wrong.
- **Keep / ★ Featured / ♪ This Week's Song / Retire** — a tap each, on any song.
  Featured and This Week wear the same two colours as their ribbons on the live
  page, so the panel looks like what you're about to publish. The panel also
  counts your featured songs and says so when you get past three — when
  everything is starred, nothing stands out.
  Starring a song keeps it automatically, so its Keep shows as a dashed gold
  outline: already protected, nothing to do.
- **A live picture of what's on the page** — every song is labelled *On the
  page*, *Rotated off*, or *Retired*, and each category shows how many of its
  eight slots are used and how many you've locked with Keep. Toggle something
  and the whole picture updates instantly, so you can see whether a change is
  about to push a song you love off the page **before** you publish it.

The panel can't change the live site by itself — the site is plain files on
GitHub, with nothing running behind it. So it does the writing and hands the
file to you:

1. Tap **Save changes** — the whole corrected file goes to your clipboard and
   GitHub opens in a new tab.
2. Select everything in the GitHub editor (Ctrl+A / Cmd+A) and paste over it.
3. Commit. The live site catches up in a minute or two.

That's the only "coding" left, and it's a paste. Nothing is saved until you do
it, so you can toggle things freely to see what happens — closing the tab
throws it all away.

The page is `noindex`, so it won't show up in Google. Anyone with the link can
open it, but it's a worksheet: it can't change the jukebox, and nothing on it is
private.

## How to add a song

The control panel above does this for you. To do it by hand instead:

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
     Featured songs never rotate out (see below).

3. Commit the change. Done — the site updates itself within a minute or two.
   No other file needs to change.

## Rotation — how songs age off the page

The jukebox is a sampler, not a catalog. A client should hear our range in one
screen, so the page shows only the **newest 8 songs in each category**. Once a
category is full, adding a new song to it pushes the oldest one off the page.

Two things make that safe:

- **Nothing is ever deleted.** Songs stay in `songs.json` forever. A song that
  rotates off is one edit away from coming back.
- **The cap is per category, not per page.** Four new memorials for a funeral
  home campaign can only push out older *memorials* — the weddings, milestones,
  tributes and holidays don't move. Every category keeps its own eight slots.

### Keeping a song you never want to lose

Add `"keep": true` to any song and it stays on the page no matter how old it
gets:

```json
{
  "title": "I'm Still Here",
  "occasion": "memorial",
  "year": 2026,
  "blurb": "One sentence about the song.",
  "youtubeId": "a50GgjTQago",
  "featured": false,
  "keep": true
}
```

Keepers claim their slot first, and the newest songs fill whatever is left. So
in a category capped at 8 with three keepers, the five newest non-keepers show
alongside them. Songs marked `"featured": true` are kept automatically — no need
to add both.

Use `keep` on the two or three songs per category that sell the studio best: the
one you'd play for a client who only has time for one. If you mark eight
memorials as keepers, that category stops rotating entirely — which is allowed,
but then a new memorial won't appear on the page until you unmark one.

### Benching a song on purpose

To pull a song off the page without deleting it, add `"retired": true`. It
disappears from the grid and from the filter counts, and stays in the file.
Delete the line to bring it back.

### Changing the cap

The number lives in `index.html` — search for `MAX_PER_OCCASION` near the top of
the script:

```js
var MAX_PER_OCCASION = 8;
```

Raise it if the page feels thin, lower it if it feels long. Nothing else needs
to change.

**How "oldest" is decided:** by position in `songs.json`, not by the `year`
field. New songs go in at the **top** of the file, so the bottom of each
category is the oldest. Keep adding new entries at the top and rotation takes
care of itself.

## This Week's Song (the weekly spotlight)

The "Just Released — This Week's Song" section at the top of the page shows
whichever song has `"spotlight": true` in `songs.json`. This is the song your
Facebook posts point to.

To change it each week:

1. Add the new song as usual (see above), and give it `"spotlight": true`.
2. Remove the `"spotlight": true` line from last week's song (or set it to
   `false`).

Exactly one song should have `"spotlight": true` at a time — if none does,
the section simply hides itself. The spotlight song also appears in its
regular category below, so nothing else needs to change.

### Spotlight without adding it to the jukebox

If you want a song at the top **only** — no card in the grid below, and no
effect on the filter counts — add `"spotlightOnly": true` alongside
`"spotlight": true`:

```json
{
  "title": "Don't Go Quiet",
  "occasion": "tribute",
  "year": 2026,
  "blurb": "One sentence about the song.",
  "youtubeId": "Cp6tOij60c8",
  "featured": false,
  "spotlight": true,
  "spotlightOnly": true
}
```

The entry still needs `occasion`, `year`, and `blurb` — the spotlight card
shows all three. When you're ready to move it into the rotation, delete the
`"spotlightOnly"` line.

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
| `admin.html` | The control panel — add songs and set Keep/Retire without touching JSON |
| `favicon.png` | Browser-tab icon, copied from the main site |
