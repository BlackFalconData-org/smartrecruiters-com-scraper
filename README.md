# SmartRecruiters Job Scraper [Just 💰$0.02]

Extract structured data from [smartrecruiters.com](https://smartrecruiters.com) — smartrecruiters.com — records. Every record carries address, company and posted date the source publishes. Posted date is included where the source lists it. Structured rows, ready to import into a CRM or a spreadsheet.

**[SmartRecruiters Job Scraper [Just 💰$0.02] on Apify →](https://apify.com/blackfalcondata/smartrecruiters-com-scraper?fpr=1h3gvi)**

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

**Detail enrichment** — Fetch full descriptions, salary data, direct apply URLs for each job.

**Incremental mode** — Only get new or changed jobs since your last run. Content hash per job — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how jobs evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N jobs (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every job returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured jobs from smartrecruiters.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor jobs, track trends, and analyze market dynamics with structured, deduplicated data from smartrecruiters.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Compensation benchmarking**
Aggregate salary ranges across roles, industries, and locations on smartrecruiters.com to inform pricing decisions, hiring plans, or candidate negotiations.

**AI / LLM training data**
Structured JSON per job is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "company": "ServiceNow",
  "query": "engineer",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `company` | string | — | The company whose job board you want, as it appears in its careers URL: for jobs.smartrecruiters.com/ServiceNow, enter `ServiceNow`. Use a JSON array for several companies, e.g. ["ServiceNow","Bosch"]. Optional: leave empty and enter a search term to search across all companies (this returns only the newest ~100 matching postings, so add a company for a complete list). Provide a company, a search term or career-page URLs. |
| `query` | string | — | Optional keyword filter (job title, skill, team). With a company it filters that company's board; without a company it searches across all companies and returns only the newest ~100 matches. Leave empty (with a company) to get every open posting. Use a JSON array for several keywords. |
| `location` | string | — | Optional. A city name (e.g. `Berlin`), or a 2-letter country code (e.g. `DE`) to filter by country. In a search without a company the filter applies to those newest matches. Use a JSON array for several locations. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste company career-page URLs, e.g. https://jobs.smartrecruiters.com/ServiceNow (a single posting URL works too — it scans that company). Each URL becomes its own task; results are merged and deduped across URLs. When provided, these REPLACE the company/query/location fields; unsupported URLs are skipped, and if none is supported the run stops with an input error. |
| `includeDetails` | boolean | `true` | Fetch full job details. |
| `enrichEmails` | boolean | `true` | Collect the email addresses written into each job description (recruiter or HR contacts) into extractedEmails. Needs Include Full Details. On by default; it adds no extra requests. |
| `includeRawJson` | boolean | `false` | Add the complete, unmodified SmartRecruiters posting (list record and full detail) to each result as rawJson — every field the source returns, including ones not mapped to their own column. Off by default; it roughly doubles the record size. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Which description representations to emit. `all` keeps description + descriptionText + descriptionHtml + descriptionMarkdown (default, back-compat). `text` / `html` / `markdown` keep only one variant — typically halves output size. |
| `excludeEmptyFields` | boolean | `false` | Drop null / empty-string / empty-array fields (and fully-null nested objects like socialProfiles) from each output record. Off by default to preserve dataset schema compatibility; turn on to slim output for LLM / agent consumption. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (companies, queries, locations, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | Also emit jobs that have not changed since the last run (incrementalMode only). |
| `emitExpired` | boolean | `false` | Also emit jobs that have disappeared since the last run (incrementalMode only). |
| `skipReposts` | boolean | `false` | When incremental, skip jobs whose content matches an expired job from a prior run (cross-run repost detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED jobs. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of jobs included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |

---

## Output fields

Every job returns the same 57-field schema. Missing values are `null` — never omitted.

- `id`
- `jobId`
- `uuid`
- `refNumber`
- `company`
- `companySlug`
- `sourceCompanySlug`
- `title`
- `department`
- `function`
- `industry`
- `employmentType`
- `experienceLevel`
- `language`
- `languageCode`
- `customFields`
- `location`
- `locations`
- `city`
- `region`
- `country`
- `address`
- `postalCode`
- `remote`
- `hybrid`
- `workplace`
- `postedDate`
- `postedAt`
- `url`
- `portalUrl`
- `jobUrl`
- `applyUrl`
- `description`
- `descriptionText`
- `descriptionHtml`
- `descriptionMarkdown`
- `salaryMin`
- `salaryMax`
- `salaryCurrency`
- `salaryType`
- `extractedEmails`
- `extractedPhones`
- `extractedUrls`
- `socialProfiles`
- `sourceAts`
- `source`
- `keyword`
- `searchQuery`
- `scrapedAt`
- `contentQuality`
- `detailFetched`
- `contentHash`
- `changeType`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`
- `rawJson`

---

## Sample output

One object per job. Here is a real example from a production run:

```json
{
  "id": "744000156137939",
  "jobId": "744000156137939",
  "uuid": "dc304c4c-4a16-4862-b67d-701470a7714b",
  "refNumber": "JB0074948",
  "company": "ServiceNow",
  "companySlug": "ServiceNow",
  "sourceCompanySlug": "ServiceNow",
  "title": "Forward Deployed Solution Engineer – Applied AI FDE",
  "department": "Product",
  "function": "Engineering",
  "industry": "Information Technology And Services",
  "employmentType": "Full-time"
}
```

*Truncated — full records contain 57 fields. See Output fields for the complete schema.*

**[Try SmartRecruiters Job Scraper [Just 💰$0.02] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/smartrecruiters-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.00002 |

See the [actor on Apify](https://apify.com/blackfalcondata/smartrecruiters-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape smartrecruiters.com?**
Use this actor on Apify to extract structured data from smartrecruiters.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get smartrecruiters.com data as JSON, CSV, or Excel?**
The actor writes each job to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape smartrecruiters.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check smartrecruiters.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each job gets a content hash. On subsequent runs, only new or changed jobs are emitted — saving time, compute, and storage. Expired jobs can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [SmartRecruiters Job Scraper [Just 💰$0.02]](https://apify.com/blackfalcondata/smartrecruiters-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
