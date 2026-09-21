# 🎬 MovieMood

Tell it your mood, not a genre checklist — get back one movie, picked for tonight.

**Repo:** https://github.com/Sakhibhagat/moviemood

![MovieMood screenshot](screenshot.png)
<!-- Add a real screenshot.png to this folder before pushing, or this line will show a broken image. -->

## Why

Most recommendation demos are "enter a movie → get 5 similar ones." MovieMood skips the data-entry step entirely: tap one of 9 mood tiles (😭 I want to cry, 😱 Scary, 🧠 Mind-blowing, 🌙 Late-night...) and get a single considered pick, with a real rating/runtime and a one-line reason it was chosen — not a wall of results to sift through.

It's also intentionally not Hollywood-only. The catalog spans English, Hindi, and Gujarati cinema, pulled live from a real movie database rather than a hand-picked list.

## Features

- **Mood-first UX** — one tap (😭😂🥹😱🧠✨🕵️🔥🌙) instead of a multi-question quiz
- **Optional filters** — language (English / Hindi / Gujarati / Any), genre, era, minimum rating, runtime (including a quick "under 90 minutes" toggle)
- **Single-pick reveal** — a "Tonight's Pick" card with rating, runtime, tags, and an editorial-style tagline, instead of a generic top-5 list
- **Live data** — pulls from [The Movie Database (TMDB)](https://www.themoviedb.org/) API in real time, so the catalog is the actual, current set of released films — not a fixed list that runs out
- **🎲 Surprise Me** — re-rolls to a different strong match without restarting the flow
- **❤️ Save** — persists to `localStorage`, so saved picks survive a reload
- **↗ Share** — uses the Web Share API on mobile, falls back to clipboard copy on desktop
- **Zero build step** — one HTML file, no framework, no bundler

## How the recommendation logic works

Each of the 9 moods maps to a genre bias (e.g. *Scary → Horror*, *Mystery/Thriller → Thriller*). On request:

1. The app queries TMDB's `/discover/movie` endpoint for the selected language(s) — either one specific language, or all three (English, Hindi, Gujarati) in parallel if "Any" is chosen.
2. Any active filters (genre, era, minimum rating, runtime) are applied as query parameters.
3. If a strict combination returns nothing, the app progressively relaxes the least essential filters (runtime → rating → era → genre) and retries, so an unusual combination never dead-ends into a blank screen.
4. From the results, the app leans toward higher-rated matches while keeping some randomness, so repeated requests for the same mood don't always surface the identical film.
5. Once a pick is chosen, a second call fetches its runtime (not included in the discovery results) to complete the reveal card.

## Tech stack

- Vanilla JavaScript (no framework)
- Plain CSS (no Tailwind/build step)
- [TMDB API](https://developer.themoviedb.org/reference/intro/getting-started) for live movie data
- `localStorage` for saved-picks persistence

## Setup

This app calls the TMDB API directly from the browser, so it needs a free API key:

1. Create a free account at [themoviedb.org](https://www.themoviedb.org/signup)
2. Go to **Settings → API → Request an API Key** (choose "Developer")
3. Open `index.html` and replace the `TMDB_API_KEY` constant near the top of the `<script>` block with your own key

**Note on the key:** since this is a static site with no backend, the API key is visible to anyone who views the page source. That's the standard, accepted trade-off for a personal/portfolio project using TMDB's free tier — it is *not* appropriate for a commercial product, which would need a backend proxy to keep the key private.

## Running it locally

```bash
git clone https://github.com/Sakhibhagat/moviemood.git
cd moviemood
# add your TMDB API key first — see Setup above
open index.html   # or just double-click the file
```

## Project structure

```
moviemood/
├── index.html      # the entire app — markup, styles, and logic
└── README.md
```

## Possible next steps

- Add a lightweight backend proxy so the API key isn't exposed client-side
- Expand beyond English/Hindi/Gujarati to more languages
- Split `index.html` into `index.html` / `styles.css` / `app.js`
- Add a "Movie vs. Series" filter using TMDB's TV endpoints
- Server-synced saved lists instead of per-browser `localStorage`

## Attribution

This product uses the TMDB API but is not endorsed or certified by TMDB.

## License

MIT — see [LICENSE](LICENSE).
