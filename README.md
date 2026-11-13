# MLDS 422 Homework 5 — API Practice

This project combines Last.fm track information and listening statistics with Genius lyrics and songwriting credits for Jay Chou. Albums are selected automatically using Last.fm's popularity ranking. The saved dataset contains 54 songs from five albums: 11月的蕭邦, 七里香, 我很忙, 葉惠美, and 魔杰座.

## APIs and collected fields

**Last.fm** supplies the album selection, tracklists, and listening statistics through three methods:

- `artist.getTopAlbums` selects the five most popular albums.
- `album.getInfo` retrieves each album's tracks, track numbers, and durations.
- `track.getInfo` retrieves each song's play count and listener count.

The Last.fm CSV contains `album`, `song_name`, `track_number`, `duration_seconds`, `track_playcount`, and `track_listeners`.

**Genius** supplies song metadata through its search and song endpoints. LyricsGenius retrieves lyrics from Genius song pages. Search results are filtered to retain entries whose primary artist is Jay Chou before the song details are requested.

The Genius CSV contains `album`, `song_name`, `lyrics`, `writer_artists`, and `producer_artists`.

## Project structure

```text
hw5/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 01_lastfm.ipynb
│   ├── 02_genius.ipynb
│   └── 03_eda.ipynb
└── data/
    ├── lastfm.csv
    ├── genius.csv
    └── combined.csv
```

## Environment setup

The project was developed using Python 3.14. From the `hw5` directory, create a virtual environment and install the dependencies.

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Open the notebooks in VS Code with the Python and Jupyter extensions installed, and select the interpreter from `.venv` as the notebook kernel.

Alternatively, install the Jupyter Notebook interface and launch it from the activated environment.

```bash
python -m pip install notebook
cd notebooks
python -m notebook
```

## API credentials

Create a `.env` file in the `hw5` directory and supply your own credentials.

```dotenv
LASTFM_API_KEY=your_lastfm_api_key
GENIUS_ACCESS_TOKEN=your_genius_client_access_token
```

Get a Last.fm API key through [Last.fm API registration](https://www.last.fm/api/account/create). Create a Genius API client and generate its Client Access Token through [Genius API Clients](https://genius.com/api-clients).

The notebooks call `load_dotenv()` to load these values. Keep the credentials in the local `.env` file.

## Run order

Run each notebook from top to bottom in the following order. Save the notebooks with their outputs displayed.

1. **`01_lastfm.ipynb`** selects five popular Jay Chou albums, retrieves their tracks, and collects per-track listening statistics. It writes `data/lastfm.csv`.
2. **`02_genius.ipynb`** reads the Last.fm track list, searches Genius, and collects lyrics, writers, and producers. It preserves the Last.fm album and song names as the keys for the later merge and writes `data/genius.csv`.
3. **`03_eda.ipynb`** joins the two datasets on `album` and `song_name`, validates the one-to-one relationship, and writes `data/combined.csv`. It also produces summary tables and two insights by EDA.
