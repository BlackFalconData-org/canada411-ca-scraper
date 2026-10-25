# Canada411 People Search Scraper

Extract structured data from [canada411.ca](https://canada411.ca) — Canadian residential phone listings from canada411.ca by name and city. Each record gives the person's name, phone number and street address with postal code, plus GPS coordinates. Search by name and city or paste a canada411.ca results URL. CRM-ready rows.

**[Canada411 People Search Scraper on Apify →](https://apify.com/blackfalcondata/canada411-ca-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch contact information, structured metadata for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N records (up to 10.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from canada411.ca on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from canada411.ca.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "Smith",
  "location": "Toronto ON",
  "maxResults": 100,
  "includeDetails": false,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Listed name to search for — surname, or first + last name (e.g. "Smith" or "Shelley Smith"). A name search needs a 📍 City + province or a postal code. Use a JSON array for several names: ["Smith","Tremblay"]. |
| `location` | string | — | Where to search, as city and two-letter province (e.g. "Toronto ON", "Vancouver BC"). Required — canada411.ca has no nationwide name search. Use a JSON array for several places: ["Toronto ON","Ottawa ON"]. |
| `postalCodes` | array | `[]` | Postal codes, e.g. "M9N 3L3". With a name in 🔍 Name(s) to look up, the name is searched in these postal codes instead of (or as well as) a city. Without a name, every listing in each postal code is returned. |
| `addresses` | array | `[]` | Street addresses to look up, as street, city and province separated by commas, e.g. "48 Howbert, North York, ON". A postal code may stand in for the province ("48 Howbert, M9N 3L3"). Returns the listings at and around each address: the site also lists neighbours on the same street, so compare street_address to keep only the exact address. Works alongside a name search or on its own. |
| `phoneNumbers` | array | `[]` | 10-digit Canadian phone numbers to look up, e.g. "416-243-0072". Each listed number returns the name and address behind it; an unlisted number returns nothing. Works alongside a name search or on its own. |
| `maxResults` | integer | `100` | Maximum total listings (0 = unlimited). |
| `startUrls` | array | `[]` | Paste canada411.ca search-result URLs or listing pages (https://www.canada411.ca/res/...). Each URL becomes its own search task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the name/location fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `false` | Visit every listing's own page as well. The result list already carries name, phone, full address and coordinates, so leave this off unless you want the listing page re-read — it makes the run slower and more expensive without adding fields. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (names, locations, startUrls) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When Incremental Mode is on, also emit listings with no detected changes (changeType=UNCHANGED). |
| `emitExpired` | boolean | `false` | When Incremental Mode is on, also emit listings that disappeared since the last run (changeType=EXPIRED). |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches a listing that disappeared in a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "14165550123"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every record returns the same 32-field schema. Missing values are `null` — never omitted.

- `listing_id`
- `listing_type`
- `full_name`
- `phone`
- `phone_e164`
- `street_address`
- `city`
- `province`
- `postal_code`
- `latitude`
- `longitude`
- `listing_url`
- `profileUrl`
- `portalUrl`
- `query_name`
- `query_location`
- `result_rank`
- `scraped_at`
- `phone_formatted`
- `address`
- `source_url`
- `source_mode`
- `lookup_status`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `profile_verified_at`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "listing_id": "462614471",
  "listing_type": "residential",
  "full_name": "GLORIA SMITH SMITH",
  "phone": "4162430072",
  "phone_e164": "+14162430072",
  "street_address": "48 Howbert",
  "city": "North York",
  "province": "ON",
  "postal_code": "M9N 3L3",
  "latitude": 43.711978912353516,
  "longitude": -79.52639770507812,
  "listing_url": "https://www.canada411.ca/res/4162430072/GLORIA+SMITH-SMITH/462614471.html"
}
```

*Truncated — full records contain 32 fields. See Output fields for the complete schema.*

**[Try Canada411 People Search Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/canada411-ca-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.0009 |

See the [actor on Apify](https://apify.com/blackfalcondata/canada411-ca-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape canada411.ca?**
Use this actor on Apify to extract structured data from canada411.ca. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get canada411.ca data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape canada411.ca?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check canada411.ca's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Canada411 People Search Scraper](https://apify.com/blackfalcondata/canada411-ca-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
