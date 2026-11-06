# Autom CLI

Official command-line interface for the [Autom](https://www.autom.dev) SERP API.

Google, Bing and Brave search results as structured JSON — web, maps, news, shopping, images, jobs and AI Mode.

Every documented endpoint is a command — **23 commands** — and `request` reaches any other path. JSON responses are pretty-printed; files (images, PDFs…) are saved with `-o`.

## Requirements

- Node.js 20 or newer
- a Autom [API key](https://app.autom.dev)

## Install

```sh
npm install --global @automdev/cli
autom --help
```

Or run it without installing:

```sh
npx @automdev/cli --help
```

## Configure your API key

The CLI reads the key from the environment; it does not load `.env` files.

```sh
export AUTOM_API_KEY="YOUR_API_KEY"
```

PowerShell:

```powershell
$env:AUTOM_API_KEY = "YOUR_API_KEY"
```

## Usage

```sh
autom google-search --query 'web scraping api'
```

Parameters use the names of the [API reference](https://docs.autom.dev) as flags: `--full-page` and `--full_page` are equivalent. Boolean flags can be bare (`--flag`) or explicit (`--flag=false`); array parameters accept commas or repeated flags.

```sh
autom google-search --help     # parameters of one command
autom endpoints                # every command and its HTTP path
autom endpoints search         # filter the list
```

### JSON parameters

```sh
echo '{"query": "web scraping api"}' | autom google-search --params -
autom google-search --params params.json --compact
```

Flags override values read with `--params`.

### Any endpoint

```sh
autom request get /v1/usage
autom request post /v1/some/new/endpoint --params body.json
```

### Pipe into jq

```sh
autom google-search --query 'web scraping api' | jq .
```

## Exit codes

| Code | Meaning                                                                             |
| ---- | ----------------------------------------------------------------------------------- |
| `0`  | Success.                                                                            |
| `1`  | The API returned an error (details on standard error) or the network failed.        |
| `2`  | Invalid usage: unknown command or parameter, missing API key or required parameter. |

## Commands

| Command                      | HTTP                                        | Description                                                                                                              |
| ---------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `bing-search`                | GET / POST `/v1/bing/search`                | Query Bing Search via API and retrieve structured organic results (title, link, snippet, position).                      |
| `brave-images`               | GET / POST `/v1/brave/images`               | Search Brave Images via API and retrieve structured image results with pagination and SafeSearch.                        |
| `brave-search`               | GET / POST `/v1/brave/search`               | Search Brave Search via API and retrieve structured organic results with pagination.                                     |
| `brave-videos`               | GET / POST `/v1/brave/videos`               | Search Brave Videos via API and retrieve structured video results with pagination and SafeSearch.                        |
| `duckduckgo-search`          | GET / POST `/v1/duckduckgo/search`          | Query DuckDuckGo via API and retrieve structured organic results with region and language.                               |
| `finder-google-countries`    | GET `/v1/finder/google-countries`           | Search Google countries and retrieve country codes (free Finder endpoint).                                               |
| `finder-google-languages`    | GET `/v1/finder/google-languages`           | Search Google languages and retrieve language codes (free Finder endpoint).                                              |
| `finder-google-locations`    | GET `/v1/finder/google-locations`           | Find supported Google locations and retrieve location IDs and country codes (free endpoint).                             |
| `google-images`              | GET / POST `/v1/google/images`              | Search Google Images via API and retrieve image results (URLs, titles, sources, dimensions).                             |
| `google-jobs`                | GET / POST `/v1/google/jobs`                | Scrape Google Jobs results via API and retrieve structured job listings with title, company, location, and pagination.   |
| `google-maps`                | GET / POST `/v1/google/maps`                | Search Google Maps via API and retrieve structured place results (address, website, phone).                              |
| `google-news`                | GET / POST `/v1/google/news`                | Search Google News via API and retrieve structured news results with pagination.                                         |
| `google-search`              | GET / POST `/v1/google/search`              | Scrape Google Search results via API and retrieve structured SERP data with pagination.                                  |
| `google-search-ai`           | GET / POST `/v1/google/search/ai`           | Query Google AI Mode via API. JSON with text_blocks, references, and shopping_results.                                   |
| `google-search-autocomplete` | GET / POST `/v1/google/search/autocomplete` | Get Google Search autocomplete suggestions via API for a given query.                                                    |
| `google-search-light`        | GET / POST `/v1/google/search/light`        | Scrape lightweight Google Search results via API: fast, cost-efficient, organic results only.                            |
| `google-shopping`            | GET / POST `/v1/google/shopping`            | Scrape Google Shopping results via API and retrieve structured product listings with prices, ratings, and merchant data. |
| `google-videos`              | GET / POST `/v1/google/videos`              | Search Google Videos via API and retrieve structured video results with metadata.                                        |
| `jobs`                       | GET `/v1/jobs/{id}`                         | Poll the status, progress (0–100) and result of a job started with is_async=true. Polling is free. Poll until status is… |
| `search`                     | GET / POST `/v1/search`                     | One call walks engines in order and returns the first organic results. engines=auto tries Google, then Brave, Bing, Duc… |
| `stats`                      | GET `/v1/stats`                             | Infrastructure health for an engine or endpoint over the last hour. Not scoped to your account or tenant. Enabled per a… |
| `usage`                      | GET `/v1/usage`                             | Retrieve real-time usage statistics and credit balance for your Autom account.                                           |
| `yahoo-search`               | GET / POST `/v1/yahoo/search`               | Query Yahoo via API and retrieve structured organic results with country and language.                                   |

## Built on the SDK

The CLI uses the official [Node.js SDK](https://github.com/automdev/autom-node) (`@automdev/sdk`). Use the SDK directly for programmatic access.

## License

MIT — see [LICENSE](LICENSE).
