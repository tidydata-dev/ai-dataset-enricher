# Bulk ChatGPT & Claude for Datasets — Python, Node.js & cURL examples

Run an **AI model on every row** of your data and get the answers back as **new columns**: **classify** into your categories, **sentiment analysis**, **extract fields** (price, brand, email…), **summarize**, **translate** or your **own prompt** with `{field}` placeholders. Works on Apify datasets, CSV, Excel and Google Sheets. **No API key needed** (built-in AI, **$1 per 1,000 rows**) or bring your own OpenAI, Claude, Gemini or OpenRouter key.

Hosted on Apify: **[Bulk ChatGPT & Claude for Datasets](https://apify.com/tidydata/ai-dataset-enricher)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/ai-dataset-enricher").call(run_input={
    "task": "classify",
    "labels": [
        "Complaint",
        "Question",
        "Praise",
        "Spam"
    ],
    "texts": [
        "Where is my invoice?",
        "Love the new update!"
    ]
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['text', 'ai_label', 'ai_confidence']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/ai-dataset-enricher').call({"task": "sentiment", "texts": ["Great product, slow delivery."]});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~ai-dataset-enricher/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"task": "sentiment", "texts": ["Great product, slow delivery."]}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor) · [Google News](https://apify.com/tidydata/google-news-scraper) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.
