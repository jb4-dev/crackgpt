# CrackGPT

A single-file Discord bot that chats using a local **Ollama** model and can
optionally enrich replies with **website previews** and **Spotify track** details.

```
CrackGPT
by Pengu
```

## Features

- **Local LLM** via Ollama (`ollama.chat`), configurable model name
- **Discord** integration (discord.py)
- **Per-channel style toggle** (`!crackgpt toggle`)
- **Conversation memory** per channel
- **Optional**: scrape linked webpages for short previews
- **Optional**: fetch Spotify track details from links
- **Optional**: periodic, style-matched **random chatter**
- **Single file**; production-ready structure, logging, timeouts, retries

## Quick Start

1. **Install prerequisites** (Python 3.10+ recommended)

   ```bash
   pip install --upgrade discord aiohttp beautifulsoup4 ollama spotipy python-dotenv
   ```

2. **Install and run Ollama** (choose a model, e.g. `gemma3:12b`)

   - Install from https://ollama.com
   - Pull a model:
     ```bash
     ollama pull gemma3:12b
     ```
   - Make sure `ollama serve` is running (usually automatic), or the Ollama app is running.

3. **Create a Discord bot**

   - Go to https://discord.com/developers/applications → New Application
   - Add a bot, copy the **Bot Token**
   - Enable **Message Content Intent**
   - Invite the bot to your server with appropriate permissions (Send Messages, Read Message History).

4. **Configure environment**

   Create a `.env` file (optional but convenient) in the same folder:

   ```ini
   DISCORD_BOT_TOKEN=your_discord_token_here

   # Model
   OLLAMA_MODEL=llama3
   OLLAMA_TIMEOUT_SEC=60

   # Allowed channels (empty = all). Comma-separated Discord channel IDs.
   # ALLOWED_CHANNELS=123456789012345678,234567890123456789

   # Features
   ENABLE_WEB_SCRAPING=true
   ENABLE_SPOTIFY_FEATURES=true
   HISTORY_MAX_TURNS=12
   RESPOND_TO_BOTS=false

   # Random chatter (disabled by default)
   RANDOM_MESSAGE_ENABLED=false
   RANDOM_INTERVAL_MIN_S=900
   RANDOM_INTERVAL_MAX_S=1800

   # Scraping
   USER_AGENT=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36
   MAX_CONTENT_LENGTH=2000
   HTTP_TOTAL_TIMEOUT=10

   # Spotify (optional)
   SPOTIFY_CLIENT_ID=
   SPOTIFY_CLIENT_SECRET=

   # Prompting
   TOGGLE_KEYWORD=!crackgpt toggle

   # Logging
   LOG_LEVEL=INFO
   ```

5. **Run it**

   ```bash
   python crackgpt.py
   ```

## Usage in Discord

- Chat as normal; the bot replies in allowed channels.
- Use `!crackgpt toggle` to turn **style guidance** ON/OFF for that channel.
- Use `!crackgpt help` for a mini command list.
- If you paste a link, the bot may preview the page or Spotify track (if enabled).

## Customization (single file)

Open `crackgpt.py` and edit:

- **`DEFAULT_MASTER_INSTRUCTION`** for the system behavior.
- **Environment variables** (via `.env` or your process env) to change model, features, limits.
- **Random chatter**: enable and tweak intervals.
- **Allowed channels**: set `ALLOWED_CHANNELS` in `.env` to restrict where it talks.

## Production notes

- **Timeouts & retries** are enabled for Ollama and HTTP fetching.
- **Logging** is structured; set `LOG_LEVEL=DEBUG` for more detail.
- Handles **graceful shutdown** on SIGINT/SIGTERM.
- Keeps a rolling **per-channel history** (bounded by `HISTORY_MAX_TURNS`).

## FAQ

**Q: Does it need Spotify?**  
A: No. If Spotify creds or the library are missing, that feature is silently disabled.

**Q: Can I keep everything in one file?**  
A: Yes—this repo is designed as a single `crackgpt.py` file. The `.env` file is optional.

**Q: Can it run without web scraping?**  
A: Yes—set `ENABLE_WEB_SCRAPING=false`.

---

©2025 Pengu
