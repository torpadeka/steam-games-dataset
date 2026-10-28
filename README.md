# Steam Games Dataset

Metadata for 11,130 games on the Steam store, stored as a single JSON file (`games_sample10.json`, ~78 MB). It is a random 10% sample (seed 42) of a full scrape of 111,301 games collected in December 2025 from the Steam Web API and SteamSpy. Release dates span 2001 to 2025.

The dataset was built for the paper *Diagnosing a Performance Ceiling in Text-Only Multi-Label Tag Prediction for Steam Games* (CENIM 2026), which predicts community tags from store descriptions.

## Format

The file is one JSON object keyed by Steam AppID. Each value is a game record:

```json
{
    "413150": {
        "name": "Stardew Valley",
        "release_date": "Feb 26, 2016",
        "price": 14.99,
        "detailed_description": "...",
        "genres": ["Indie", "RPG", "Simulation"],
        "tags": {"Farming Sim": 1234, "Pixel Graphics": 987, "...": 0},
        ...
    }
}
```

Values above are illustrative.

## Fields

| Group | Fields |
|---|---|
| Basic info | `name`, `release_date`, `required_age`, `price`, `discount`, `dlc_count` |
| Text | `detailed_description`, `about_the_game`, `short_description`, `reviews`, `notes` |
| Labels | `genres`, `categories`, `tags` (user-voted tag → vote count) |
| Platforms | `windows`, `mac`, `linux` |
| Languages | `supported_languages`, `full_audio_languages` |
| Companies | `developers`, `publishers` |
| Scores and reviews | `metacritic_score`, `metacritic_url`, `user_score`, `score_rank`, `positive`, `negative`, `recommendations`, `achievements` |
| Player activity (SteamSpy) | `estimated_owners`, `peak_ccu`, `average_playtime_forever`, `average_playtime_2weeks`, `median_playtime_forever`, `median_playtime_2weeks` |
| Media and links | `header_image`, `screenshots`, `movies`, `website`, `support_url`, `support_email`, `packages` |

Coverage in this sample: 11,118 games have a description and 8,131 have at least one user tag.

## Loading

```python
import json

with open("games_sample10.json", encoding="utf-8") as f:
    games = json.load(f)

for app_id, game in list(games.items())[:3]:
    print(app_id, game["name"], list(game["tags"])[:5])
```

## Collection

The data was scraped with the scripts in this repository, adapted from [FronkonGames/Steam-Games-Scraper](https://github.com/FronkonGames/Steam-Games-Scraper). `SteamGamesScraper.py` needs a Steam API key in a `.env` file (`STEAM_API_KEY=...`) and writes `games.json`; `ConvertToCSV.py` converts the output to CSV.

```
pip install requests
python SteamGamesScraper.py
```

## License

The scraper code is MIT licensed (see `LICENSE.md`, copyright Martin Bustos). Game metadata belongs to the respective publishers and Valve; use it for research purposes only.
