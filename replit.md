# Telegram File Share Bot

## Overview
A fully-functional Telegram bot for sharing files with permanent links. Built with Python and Pyrogram, this bot allows users to upload files and receive shareable links that can be distributed to anyone.

**Bot Username:** @mraprguildtestbot

**Current Status:** Running and operational ✅

## Features

### Core Functionality
- **File Upload & Sharing**: Upload any file (up to 4GB) and receive a permanent share link
- **Multiple File Types**: Supports documents, videos, audio, images, and more
- **One-Click Sharing**: Built-in share buttons for easy distribution
- **Permanent Links**: All links remain active indefinitely
- **Force Subscription**: Optional channel subscription requirement (admin configurable)

### Technical Features
- **Health Monitoring**: Built-in health check server on port 8000
- **Keep-Alive System**: Automatic uptime monitoring and self-ping mechanism
- **JSON Database**: Persistent file storage using JSON (no external database needed)
- **Auto-Restart**: Resilient error handling with automatic recovery
- **Admin Commands**: Debug tools and force subscription management

## Project Structure

```
.
├── bot.py                 # Main bot application
├── config.py              # Configuration and environment variables
├── requirements.txt       # Python dependencies
├── templates/
│   └── index.html        # Web interface template
├── file_database.json    # Persistent storage (auto-generated)
└── replit.md            # This file
```

## Commands

### User Commands
- `/start` - Welcome message and file link retrieval
- `/help` - Detailed help guide
- `/stats` - Bot statistics (files stored, users, etc.)

### Admin Commands
- `/debug` - System debug information
- `/addfsub <channel_id>` - Add force subscription channel
- `/delfsub <channel_id>` - Remove force subscription channel

## Environment Variables

The following secrets are configured:
- `TELEGRAM_API_ID` - Telegram API ID from https://my.telegram.org
- `TELEGRAM_API_HASH` - Telegram API Hash
- `TELEGRAM_BOT_TOKEN` - Bot token from @BotFather

Optional:
- `ADMINS` - Space-separated list of admin user IDs
- `BOT_USERNAME` - Bot username (auto-detected if not set)

## How It Works

1. **User uploads a file** to the bot
2. **Bot stores file metadata** in JSON database with unique base64 key
3. **Share link is generated** using format: `https://t.me/<bot_username>?start=<key>`
4. **When link is clicked**, the `/start` command receives the key and sends the file

## Dashboard & Monitoring

The bot includes a built-in web dashboard with real-time status:
- **Dashboard**: http://0.0.0.0:8000/ - Modern dashboard with live stats
- **Health Endpoint**: http://0.0.0.0:8000/health - JSON health check
- **Stats Endpoint**: http://0.0.0.0:8000/stats - JSON statistics with API status
- **Status Endpoint**: http://0.0.0.0:8000/status - Simple status check

### Dashboard Features
- Real-time bot status (Running/Offline)
- Live uptime counter (Days, Hours, Minutes, Seconds)
- API Key connection status (API_ID, API_HASH, BOT_TOKEN)
- File and user statistics
- Auto-refresh every 5 seconds

## Database

Files are stored in `file_database.json` with the following structure:
```json
{
  "files": {
    "base64_key": {
      "file_id": "telegram_file_id",
      "file_name": "filename.ext",
      "file_size": 1234567,
      "uploader_user_id": 123456,
      "timestamp": 1234567890.0,
      "date_added": "2025-12-04 12:58:00"
    }
  },
  "users": {
    "user_id": {
      "joined_at": 1234567890.0,
      "first_seen": "2025-12-04 12:58:00"
    }
  }
}
```

## Recent Changes

**2025-12-04**: Initial deployment
- Cloned from GitHub repository: https://github.com/Mraprguild8133/Demo.git
- Installed Python 3.11 and all required dependencies
- Configured Telegram API credentials
- Set up workflow to run bot continuously
- Added .gitignore for Python project
- Bot successfully started as @mraprguildtestbot

## Development

### Running the Bot
The bot runs automatically via the "Telegram Bot" workflow. To restart manually:
```bash
python bot.py
```

### Adding Features
The bot is modular and easy to extend. Main handlers are in `bot.py`:
- Command handlers: `@app.on_message(filters.command("name"))`
- File handler: `@app.on_message(filters.document)`
- Callback handlers: `@app.on_callback_query()`

## User Preferences

None specified yet.

## Architecture Notes

- **Pyrogram** is used for the Telegram Bot API (async framework)
- **aiohttp** powers the health monitoring web server
- **JSON storage** keeps files persistent without external database
- All file sharing uses Telegram's file_id system (no file downloads to server)
- Base64 keys provide short, URL-safe share links
