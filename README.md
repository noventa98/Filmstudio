# Film Library

A personal film catalog delivered as a single static web page, hosted on GitHub Pages and usable from any device, including a phone in a shop.

**Live site:** https://noventa98.github.io/Filmstudio/ (password-protected). The page is encrypted with [StatiCrypt](https://github.com/robinmoisson/staticrypt), so this repository holds no readable catalog data.

As of September 2026 the catalog covers about 2,600 films and 3,400 editions (DVD, Blu-ray, 4K and digital).

## What it does

- **Cards.** One card per film with poster, year, director, a 4K / HD / SD badge, a Plex badge, the number of editions owned, and a marker when the disc owned is higher resolution than what Plex actually streams.
- **Detail view.** Summary, cast, director, screenplay, cinematography and music; ratings from IMDb, Rotten Tomatoes, Metacritic and TMDB side by side; embedded trailer; a direct link that opens the film in Plex; watched status and personal rating.
- **Editions.** Every copy owned, with audio languages, subtitles, publisher, publication year, extras, audio commentaries, aspect ratio, sound format and storage location.
- **Sorting.** Shuffle (the default when the page opens), title, year, runtime, recently added, director, IMDb rating, Rotten Tomatoes, entry number.
- **Filters that combine.** Search by title, director or actor; click any person to see everything with them; release year range; on Plex; watched or not watched; watchlist; resolution; storage location; genre; country; audio language; subtitle language.
- **Cross-device state.** Watched status and ratings are synced from Plex through a GitHub Gist, so the phone and the desktop show the same picture.
- **Responsive layout** for phone and desktop.

## How it is built

The pipeline is a Python script that runs locally and is not part of this repository. It:

1. Reads the master Excel spreadsheet, one row per edition. The spreadsheet is the source of truth for what is owned and where it is stored.
2. Fetches metadata from [TMDB](https://www.themoviedb.org) (cast, crew, posters, backdrops, trailers, ratings) and [OMDb](https://www.omdbapi.com) (summaries, IMDb, Rotten Tomatoes and Metacritic ratings), caching results locally so that only new films trigger API calls.
3. Reads the Plex library to link films to the server and to collect watched status, ratings and dates added. That state is published to a Gist the page reads on load.
4. Groups editions by film, embeds the resulting JSON in the HTML template, encrypts the page with StatiCrypt and pushes `index.html` here.

The browser never talks to the Plex server directly. At runtime the page reads only the Gist, TMDB and OMDb.

## Repository contents

| File | Purpose |
|---|---|
| `index.html` | The encrypted, self-contained catalog (about 7 MB) |
| `SECURITY.md`, `.well-known/security.txt` | How to report a security issue |

The Excel master, the unencrypted template, the build script and the API caches stay local and are gitignored.

## Stack

Plain HTML, CSS and JavaScript in one file, no framework, no backend. Python 3 with `openpyxl` and `requests` for the pipeline. StatiCrypt for the password gate. Designed and written with Claude.

## Credits

This product uses the TMDB API but is not endorsed or certified by TMDB. Ratings and summaries are provided by OMDb.
