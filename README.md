# GRU Flight Reliability Monitor

> Automated web scraper for delayed and cancelled flights at São Paulo Guarulhos International Airport (GRU).

## Overview

This project monitors flight disruptions at GRU Airport through automated data collection. It generates a CSV file with real-time information about delayed and cancelled flights, which can be consumed by external applications for passenger rights awareness and compensation claims.

## Features

- 🔄 Automated scraping of GRU Airport flight data
- 📊 CSV export with standardized flight disruption data
- 🤖 GitHub Actions integration for scheduled updates
- ☁️ Supabase integration for persistent data storage
- 🔍 6-month data retention policy

## How It Works

The scraper:
1. Downloads the latest flight data from GRU Airport sources
2. Processes and normalizes flight information (delays, cancellations, airlines)
3. Merges with existing historical data stored in Supabase
4. Generates `voos_atrasados_gru.csv` with filtered results
5. Removes flight records older than 6 months

## Output Format

The generated CSV (`voos_atrasados_gru.csv`) contains:

| Column | Description |
|--------|-------------|
| `data_captura` | Capture/scraping date |
| `flight_number` | Flight number (normalized) |
| `scheduled_time` | Scheduled departure time |
| `companhia` / `airline` | Airline name |
| `status` | Flight status (delayed/cancelled) |
| `destino` / `destination` | Destination airport |
| `destination_city` | Destination city name |
| `delay_hours` | Delay duration in hours |

## Usage

### Local Execution

```bash
# Install dependencies
pip install -r requirements.txt

# Set up environment variables (optional for Supabase)
export SUPABASE_URL="your_supabase_url"
export SUPABASE_KEY="your_supabase_key"

# Run the scraper
python voos_proximos_finalbuild.py
```

### Automated Execution

The project includes a GitHub Actions workflow (`.github/workflows/update-flights.yml`) that:
- Runs automatically via scheduled cron jobs
- Can be manually triggered from the Actions tab
- Commits the updated CSV file to the repository

### Data Access

The CSV file is publicly accessible via GitHub Raw:
```
https://raw.githubusercontent.com/jonechelon/gru-flight-reliability-monitor/main/voos_atrasados_gru.csv
```

## Technical Stack

- **Python 3.12+** - Core scripting language
- **Pandas** - Data processing and CSV manipulation
- **Supabase** - Cloud database for persistent storage
- **GitHub Actions** - CI/CD automation

## Data Sources

Flight data is collected from publicly available GRU Airport information systems.

## Related Projects

This scraper provides data for [**MatchFly**](https://github.com/jonechelon/matchfly-pseo), a programmatic SEO project that generates static pages for flight disruption information and passenger compensation rights (ANAC 400 / EC 261).

## Requirements

- Python 3.12 or higher
- Active internet connection
- Supabase account (optional, for data persistence)

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `SUPABASE_URL` | Optional | Supabase project URL |
| `SUPABASE_KEY` | Optional | Supabase service role key |

*If Supabase credentials are not provided, the scraper will work in local-only mode.*

## Data Retention

Flight records are automatically purged after 6 months to comply with data minimization best practices. Historical data older than this threshold is removed from the in-memory dataset before processing.

## License

This project is provided as-is for educational and informational purposes.

## Contributing

This is a personal project. For questions or suggestions, please open an issue.

***

**Note:** This scraper respects rate limits and implements proper error handling to ensure responsible data collection.
