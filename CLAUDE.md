# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Greed is a customizable, multilingual Telegram shop bot with Telegram Payments support. It allows users to create orders, add funds to their wallet, and manage a virtual store through Telegram.

## Development Commands

### Running the Bot
```bash
# Install dependencies (use virtual environment)
pip install -r requirements.txt

# Optional: Install coloredlogs for colored console output
pip install coloredlogs

# Generate configuration file (first run)
python -OO core.py

# Start the bot (after configuration)
python -OO core.py
```

### Configuration Setup
- Configuration is managed via `config/config.toml` (generated from `config/template_config.toml`)
- Never modify `template_config.toml` directly
- Configuration includes Telegram bot token, payment provider tokens, database settings, and appearance options

### Docker Alternative
```bash
# Run with Docker
docker run --volume "$(pwd)/config:/etc/greed" --volume "$(pwd)/strings:/usr/src/greed/strings" --volume "$(pwd)/data:/var/lib/greed" ghcr.io/steffo99/greed
```

## Architecture

### Core Components
- **core.py**: Main entry point that handles Telegram communication and dispatches updates to workers
- **worker.py**: Handles conversation flow for individual users, runs in separate threads for each conversation
- **database.py**: SQLite/PostgreSQL database interactions using SQLAlchemy
- **duckbot.py**: Telegram bot wrapper and utilities
- **localization.py**: Language management system

### Support Files
- **utils.py**: Utility methods and helper functions
- **nuconfig.py**: Configuration loader and management
- **strings/**: Localization files for multiple languages (en.py, it.py, es_mx.py, etc.)

### Threading Model
- Main thread (Core) handles Telegram API communication
- Each user conversation runs in a separate Worker thread
- Worker threads are named as "Worker {chat_id}" for debugging

### Database
- Uses SQLAlchemy ORM
- Supports both SQLite (default) and PostgreSQL
- Database engine configurable via `config.toml` or `DB_ENGINE` environment variable
- Default SQLite database: `database.sqlite`

### Localization
- Multi-language support with fallback system
- Available languages: it, en, uk, ru, zh_cn, he, es_mx, pt_br, hi
- Language files in `strings/` directory as Python modules
- Configurable default and fallback languages

### Configuration System
- TOML-based configuration in `config/config.toml`
- Environment variable support (e.g., `CONFIG_PATH`, `DB_ENGINE`)
- Automatic template generation on first run
- Sections: Language, Database, Telegram, Payments, Appearance, Logging

### Payment Integration
- Telegram Payments API integration
- Support for both cash payments and credit card payments
- Configurable currency, fees, and payment presets
- Payment provider token configuration required for credit card payments