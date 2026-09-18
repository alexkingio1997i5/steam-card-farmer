# steam-card-farmer

I wrote this because I wanted a lightweight, headless card farmer that doesn't require a heavy setup like ArchiSteamFarm or keeping the official Steam client running. It connects directly to Steam, checks which of your games still have trading card drops remaining, and idles them.

It runs entirely in the console and caches your session credentials locally so you only have to log in once.

## Installation

1. Install Python 3.11 or later.
2. Clone this repository.
3. Install the dependencies:

   pip install -r requirements.txt

## How to use

Run the script and follow the prompts to log in.

    python farmer.py --username your_steam_username

If you have Steam Guard (2FA) enabled, the script will prompt you for the code.

### Options

- `--username`: Your Steam account login name.
- `--max-idle`: Maximum number of games to idle simultaneously (default is 3, Steam starts slowing down drop rates if you idle too many at once).
- `--check-interval`: How often to re-scan the badges page for remaining drops, in minutes (default is 15). Required if you want to swap games automatically as drops finish.

## How it works

1. It authenticates with Steam using the `steam` library. This establishes a headless client session.
2. It fetches your Steam badges page via `httpx` to parse which games have card drops remaining.
3. It sends a `games_played` packet to Steam for those AppIDs. Steam registers you as playing them, triggering the drop timer.
4. Every 15 minutes, it checks your badges page again. Once a game has 0 drops left, it drops it from the active list and picks up the next one.

<!-- checked: 2026-09-18 -->
