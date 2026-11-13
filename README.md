# WAANDS-Discord-House-Bot

A customized Discord bot developed for my personal study group, featuring a Hogwarts-inspired house system based on our Dungeons & Dragons campaign.

The bot encourages community engagement, collaboration, and friendly competition through a house-based reward system, combining automated activity rewards with manual point management.

This project is adapted from [House-Bot by StylishPhoenix](https://github.com/StylishPhoenix/House-Bot) and modified to accommodate the needs of our community.

## Features

- **House Point Management:** Award and deduct points using predefined reward categories or custom amounts.
- **Automated Activity Rewards:** Reward members for eligible text and voice channel participation.
- **Anti-Spam Measures:** Discourage message spam through cooldowns, minimum message lengths, diminishing rewards, and hourly point limits.
- **Role-Based House Identification:** Automatically identify house membership through Discord server roles.
- **Point History:** Track individual and house transactions, including reasons and timestamps.
- **Leaderboards:** Display ranked member contributions within each house.
- **Interactive Embeds:** Navigate point histories and leaderboards using pagination buttons.
- **Persistent Data Storage:** Maintain house totals and SQLite transaction records across bot restarts.
- **Customizable Configuration:** Define house names, reward categories, and voice activity settings.

## Technologies

- JavaScript
- Node.js
- Discord.js v14
- SQLite
- better-sqlite3

## Project Structure

```
WAANDS-Discord-House-Bot/
├── House-Bot/
│   ├── HouseBot.js
│   ├── houseChoices.json
│   ├── examplepointChoices.json
│   └── exampleConfig.json
├── .gitignore
├── package.json
├── package-lock.json
└── README.md
```

Sensitive configuration files, installed dependencies, and generated data files are excluded from version control.

## Installation

### Prerequisites

- Node.js and npm
- A Discord application and bot token
- A Discord server with the appropriate bot permissions
- Message Content Intent enabled in the Discord Developer Portal

### Setup

Clone this repository and open a terminal in the repository's root directory.

Install the required dependencies:

```bash
npm ci
```

Create the local configuration files from the provided examples:

```bash
cp House-Bot/exampleConfig.json House-Bot/config.json
cp House-Bot/examplepointChoices.json House-Bot/pointChoices.json
```

Update `House-Bot/config.json` with your Discord bot token, server ID, and preferred voice reward settings.

Customize `House-Bot/houseChoices.json` and `House-Bot/pointChoices.json` to match your server's house system and reward categories.

Discord house role names must match the configured house names exactly.

**Security Note:** Never share or commit your Discord bot token. The local `config.json` file is excluded from version control.

## Configuration

### Bot Settings

The `House-Bot/config.json` file contains the following settings:

```json
{
    "token": "YOUR_BOT_TOKEN",
    "guildID": "YOUR_SERVER_ID",
    "timeInterval": 300000,
    "pointsPerInterval": 15,
    "minimumVoice": 2
}
```

| Setting | Description |
|---|---|
| `token` | Discord bot authentication token. |
| `guildID` | Discord server ID. |
| `timeInterval` | Voice reward interval in milliseconds. |
| `pointsPerInterval` | Points awarded per completed voice interval. |
| `minimumVoice` | Minimum qualifying voice channel participants required to begin tracking. |

### Text Activity Rewards

Text activity rewards encourage meaningful participation while discouraging spam.

- Messages must contain at least 10 characters.
- Messages must be separated by at least 30 seconds.
- Short or excessively frequent messages reset the cooldown.
- Messages sent by bots are excluded.
- Rewards decrease as additional eligible messages are sent.

| Eligible Message | Points Awarded |
|---|---:|
| 1 | 25 |
| 2 | 20 |
| 3 | 15 |
| 4–5 | 10 each |
| 6–9 | 5 each |
| 10+ | 0 |

Members can earn a maximum of 100 points per one-hour reward cycle. Accumulated points are awarded when the cycle ends.

### Voice Activity Rewards

Voice activity rewards are calculated using completed intervals of tracked participation.

With the example configuration, members earn 15 points per five-minute interval.

Tracking begins when the configured minimum number of non-bot, unmuted, and undeafened participants is present during a qualifying voice state change. Accumulated points are calculated when a tracked member leaves, switches channels, or changes their mute or deafen state.

## Usage

From the repository's root directory, start the bot using:

```bash
node House-Bot/HouseBot.js
```

The bot automatically registers its slash commands and loads saved house point totals upon connecting to Discord.

### Slash Commands

| Command | Description |
|---|---|
| `/add_points` | Award points to a house using predefined reward categories. |
| `/remove_points` | Deduct a specified number of points from a house with a recorded reason. |
| `/add_point_amount` | Award a custom number of points to a house with a recorded reason. |
| `/points` | Display current point totals for all houses. |
| `/point_history user` | Display a member's transaction history using their Discord ID or mention. |
| `/point_history house` | Display a specific house's transaction history. |
| `/leaderboard` | Rank members by their net point contributions to a selected house. |

The `/remove_points` and `/add_point_amount` commands require Ban Members permissions by default.

Point histories and leaderboards display up to 10 entries per page and include interactive Previous and Next navigation buttons.

## Data Persistence

The bot generates and maintains two local data files:

| File | Purpose |
|---|---|
| `house_points.txt` | Stores current house point totals. |
| `points_log.db` | SQLite database containing point transaction history. |

Each recorded transaction includes:

- Discord user ID
- House name
- Point amount
- Reason
- Timestamp

House totals are restored upon startup, and the SQLite database is automatically initialized if it does not exist.

These files are excluded from version control to protect community and member information.

## Project Purpose

This project serves as both a practical community tool and a personal development project, providing hands-on experience with:

- Event-driven JavaScript development
- Discord API integration
- Database management and persistence
- Automated reward systems
- Community-focused software customization

## Acknowledgments

The original House Points Bot was developed by [StylishPhoenix](https://github.com/StylishPhoenix).

**Original repository:** [StylishPhoenix/House-Bot](https://github.com/StylishPhoenix/House-Bot)

This project builds upon the original implementation to support the specific requirements of our study group's community.
