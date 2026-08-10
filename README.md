# How this site works now

Two files matter:

- **`index.html`**: the page itself: layout, styling, and the logic that reads and renders content. You basically never touch this again.
- **`content.json`**: everything you'd actually want to change: entries in `life`, `art`, `tech`, plus your `letterboxd` list.

## Your workflow

1. Open `content.json`.
2. Add a new object to the relevant array (`life`, `art`, or `tech`), or edit an existing one.
3. Give it a `"date": "YYYY-MM-DD"`, this is what controls ordering. Newest date always sorts to the top of its section, and the site automatically pulls the 5 most recent items *across all three categories* into the "Latest" feed on the home page. You don't sort anything by hand.
4. Save, commit, push. If it's on GitHub Pages / Netlify / Vercel, the site updates automatically within seconds of the push.

## Entry formats

**life** (just a running log entry):
```json
{ "date": "2026-06-01", "title": "Reading: ...", "body": "..." }
```

**tech** (a project card):
```json
{
  "date": "2026-06-01",
  "tag": "cli tool",
  "title": "Project name",
  "body": "What it is, one or two sentences.",
  "meta": ["python", "2026"],
  "repo": "https://github.com/you/project",
  "demo": "https://project.example.com"
}
```
`"repo"` and `"demo"` are both optional. Whichever are present show up as "code →" / "live →" links on the card.

**art** (a photo/film card, same as tech, plus either an image or ASCII thumbnail):
```json
{
  "date": "2026-06-01",
  "tag": "35mm",
  "title": "Title",
  "body": "Caption.",
  "meta": ["35mm", "2026"],
  "image": "images/rooftop.jpg",
  "link": "https://www.instagram.com/p/xxxxxxxxxxx/"
}
```
`"link"` is optional. If present, the thumbnail becomes clickable and opens that URL in a new tab, e.g. point it at the original Instagram post the photo came from.

**letterboxd** (the "recently watched" list on the life tab, up to 5 shown, most recent first):
```json
"letterboxd": {
  "username": "yourname",
  "recent": [
    { "title": "Film Title", "year": 2026, "rating": 4, "watched": "2026-08-01", "url": "https://letterboxd.com/film/film-title/" }
  ]
}
```
This isn't pulled live from Letterboxd (the site has no backend to fetch it for you); add an entry by hand each time you log a film. `"rating"` supports halves (e.g. `3.5`). The footer link always points at `letterboxd.com/{username}`.

## About images

JSON can't hold a photo directly, don't try to paste image bytes into it. Instead:

1. Put the actual photo file in an `images/` folder next to `index.html`.
2. In the JSON entry, set `"image"` to that relative path, e.g. `"images/rooftop.jpg"`.

If you'd rather keep the ASCII-art placeholder look for an entry instead of a real photo, use `"ascii"` instead of `"image"` (see the existing art entries in `content.json` for the format; it's a single string with `\n` between lines).

## Testing locally before you push

Opening `index.html` directly by double-clicking it won't load `content.json`, browsers block that for local files. Instead, from the folder in a terminal:

```
python3 -m http.server 8000
```

then open `http://localhost:8000/index.html`. This isn't needed once it's actually hosted (GitHub Pages etc. serve it over real HTTP, so it just works).
