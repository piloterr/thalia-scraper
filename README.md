<p align="center">
  <a href="https://www.piloterr.com"><img src="images/banner.png" alt="Thalia Scraper API by Piloterr – scrape Thalia data as JSON" width="100%"></a>
</p>

# Thalia Scraper API

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white)](https://discord.gg/CFj3ph8aHU) [![Get API key](https://img.shields.io/badge/Piloterr-Get%20a%20free%20API%20key-111827)](https://app.piloterr.com) [![API docs](https://img.shields.io/badge/Docs-OpenAPI-0EA5E9)](https://app.piloterr.com/openapi) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The **Piloterr Thalia Scraper API** lets you extract public data from Thalia as **clean, structured JSON** — no proxies, headless browsers or HTML parsing to maintain. Endpoints available in production:

- **Thalia Product API** — Read one Thalia product from its URL.
- **Thalia Search API** — Search Thalia by keyword and page books, media, and gifts.

Use it for price monitoring, assortment tracking, competitor analysis, product research or catalog enrichment.

| Endpoint | Path | Credits | What you get |
|---|---|---|---|
| [Thalia Product API](#thalia-product-api) | `/v2/thalia/product` | 2 | Read one Thalia product from its URL. You get price, author or brand, and the product details. |
| [Thalia Search API](#thalia-search-api) | `/v2/thalia/search` | 2 | Search Thalia by keyword and page books, media, and gifts. Cards include title, price, and the product link. |

## Table of contents

- [How it works](#how-it-works)
- [Thalia Product API](#thalia-product-api)
- [Thalia Search API](#thalia-search-api)
- [Code examples in other languages](#code-examples-in-other-languages)
- [Practical use cases](#practical-use-cases)
- [Why Piloterr?](#why-piloterr)
- [FAQ](#faq)
- [Related scrapers](#related-scrapers)
- [Support](#support)

## How it works

1. **Create a free Piloterr account** and copy your API key from the [dashboard](https://app.piloterr.com).
2. **Send a `GET` (or `POST`) request** to `https://api.piloterr.com/v2/...` with the header `x-api-key: YOUR_API_KEY`.
3. **Receive structured JSON** — Piloterr handles rendering, proxies, retries and parsing.

```bash
export PILOTERR_API_KEY="your-api-key"
pip install requests        # Python
# TypeScript: Node.js 18+ (native fetch), run with `npx tsx file.ts`
```

> Every endpoint accepts both `GET` (query string) and `POST` (JSON body with the same fields).

## Thalia Product API

Read one Thalia product from its URL. You get price, author or brand, and the product details.

`GET | POST https://api.piloterr.com/v2/thalia/product` · **2 credits per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Product URL or Thalia article id. |
| `return_page_source` | boolean | — | When true, returns {"result": "<html>..."} instead of the parsed JSON. Default false. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/thalia/product",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "A1074644377",
    },
    timeout=60,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type ThaliaProduct = {
  id?: string;
  url?: string;
  isbn?: string;
  image?: string;
  price?: number;
  title?: string;
  author?: string;
  format?: string;
  rating?: number;
  series?: string;
  currency?: string;
  language?: string;
  subtitle?: string;
  publisher?: string;
  page_count?: number;
  description?: string;
  availability?: string;
  review_count?: number;
  price_display?: string;
  publication_date?: string;
};

const params = new URLSearchParams({
  "query": "A1074644377",
});

const response = await fetch(`https://api.piloterr.com/v2/thalia/product?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as ThaliaProduct;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/thalia/product
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "A1074644377"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "id": "A1074644377",
  "url": "https://www.thalia.de/shop/home/artikeldetails/A1074644377",
  "isbn": "978-3-551-55923-4",
  "image": "https://images.thalia.media/07/-/3aeeae4a5ce44ac39ca88cc122da4164/harry-potter-und-die-kammer-des-schreckens-harry-potter-2-gebundene-aus…",
  "price": 18,
  "title": "Harry Potter und die Kammer des Schreckens (Harry Potter 2)",
  "author": "example",
  "format": "Hardcover",
  "rating": 4.9,
  "series": "Harry Potter",
  "currency": "EUR",
  "language": "Deutsch",
  "subtitle": "Neuausgabe des Kinderbuch-Klassikers ab 10 Jahren über den bekanntesten Zauberer der Welt",
  "publisher": "Carlsen",
  "page_count": 352,
  "description": "Detailreiche und prachtvoll veredelte Neugestaltung aller sieben Bände Endlich wieder Schule! Einen solchen Seufzer kann nur der ausstoße…",
  "availability": "Sofort lieferbar",
  "review_count": 97,
  "price_display": "18,00 €",
  "publication_date": "2025-09-01"
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `id` | string |  |
| `url` | string |  |
| `isbn` | string |  |
| `image` | string |  |
| `price` | integer |  |
| `title` | string |  |
| `author` | string |  |
| `format` | string |  |
| `rating` | number |  |
| `series` | string |  |
| `currency` | string |  |
| `language` | string |  |
| `subtitle` | string |  |
| `publisher` | string |  |
| `page_count` | integer |  |
| `description` | string |  |
| `availability` | string |  |
| `review_count` | integer |  |
| `price_display` | string |  |
| `publication_date` | string |  |

## Thalia Search API

Search Thalia by keyword and page books, media, and gifts. Cards include title, price, and the product link.

`GET | POST https://api.piloterr.com/v2/thalia/search` · **2 credits per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Keyword or a thalia.de search URL (/suche or a category URL). A product URL returns 400. |
| `page` | number | — | Page number, from 1. Sent to Thalia as the query parameter p. Default 1. |
| `return_page_source` | boolean | — | When true, returns {"result": "<html>..."} instead of the parsed JSON. Default false. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/thalia/search",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "Harry Potter",
    },
    timeout=60,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type ThaliaSearch = {
  results?: {
    id?: string;
    url?: string;
    image?: string;
    price?: number;
    title?: string;
    author?: string;
    format?: string;
    rating?: number;
    currency?: string;
    review_count?: number;
    price_display?: string;
  }[];
  pagination?: {
    next?: string;
    page?: number;
    per_page?: number;
    total_pages?: number;
    total_results?: unknown;
  };
};

const params = new URLSearchParams({
  "query": "Harry Potter",
});

const response = await fetch(`https://api.piloterr.com/v2/thalia/search?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as ThaliaSearch;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/thalia/search
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "Harry Potter"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "results": [
    {
      "id": "A1074644377",
      "url": "https://www.thalia.de/shop/home/artikeldetails/A1074644377",
      "image": "https://images.thalia.media/03/-/3aeeae4a5ce44ac39ca88cc122da4164/harry-potter-und-die-kammer-des-schreckens-harry-potter-2-gebundene-aus…",
      "price": 18,
      "title": "Harry Potter und die Kammer des Schreckens (Harry Potter 2)",
      "author": "example",
      "format": "Buch (Gebundene Ausgabe)",
      "rating": 5,
      "currency": "EUR",
      "review_count": 97,
      "price_display": "18,00 €"
    }
  ],
  "pagination": {
    "next": "https://www.thalia.de/suche?sq=Harry%20potter&p=2",
    "page": 1,
    "per_page": 24,
    "total_pages": 258,
    "total_results": null
  }
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `results[]` | object[] |  |
| `results[].id` | string |  |
| `results[].url` | string |  |
| `results[].image` | string |  |
| `results[].price` | integer |  |
| `results[].title` | string |  |
| `results[].author` | string |  |
| `results[].format` | string |  |
| `results[].rating` | integer |  |
| `results[].currency` | string |  |
| `results[].review_count` | integer |  |
| `results[].price_display` | string |  |
| `pagination` | object |  |
| `pagination.next` | string |  |
| `pagination.page` | integer |  |
| `pagination.per_page` | integer |  |
| `pagination.total_pages` | integer |  |
| `pagination.total_results` | object |  |

## Code examples in other languages

Ready-to-run examples live in the [`examples/`](examples) folder. Each one reads your key from the `PILOTERR_API_KEY` environment variable.

| Language | Product | Search |
|---|---|---|
| cURL (Bash) | [`product`](examples/curl/product.sh) | [`search`](examples/curl/search.sh) |
| PHP | [`product`](examples/php/product.php) | [`search`](examples/php/search.php) |
| C# | [`product`](examples/csharp/Product.cs) | [`search`](examples/csharp/Search.cs) |
| Go | [`product`](examples/go/product.go) | [`search`](examples/go/search.go) |
| Java | [`product`](examples/java/ProductExample.java) | [`search`](examples/java/SearchExample.java) |
| Ruby | [`product`](examples/ruby/product.rb) | [`search`](examples/ruby/search.rb) |
| Node.js | [`product`](examples/nodejs/product.mjs) | [`search`](examples/nodejs/search.mjs) |

## Practical use cases

- **Price monitoring** — Track prices, promotions and availability over time to react to competitors.
- **Assortment & catalog tracking** — Follow new products, delisted items and category changes.
- **Competitor analysis** — Benchmark ranges, brands and positioning across retailers.
- **Product research** — Spot best-sellers and trends from search results.
- **Catalog enrichment** — Pull titles, images and attributes into your PIM or marketplace listings.

## Why Piloterr?

- **No infrastructure** — no proxies, headless browsers or CAPTCHA solving to run yourself.
- **Structured JSON** — stable, documented fields instead of brittle HTML selectors.
- **Pay per request** — transparent credit cost per endpoint, failed requests are retried for you.
- **One API key, hundreds of sources** — Thalia plus many other e-commerce, search and company data endpoints in the [Piloterr library](https://www.piloterr.com/library).
- **Any language** — plain HTTPS + JSON, with examples for Python, TypeScript, cURL, PHP, C#, Go, Java, Ruby and Node.js.

## FAQ

### Is it legal to scrape Thalia?

Collecting publicly available data is generally allowed in many jurisdictions, but rules depend on your country, the data you collect and how you use it. Avoid personal data unless you have a lawful basis, respect the website's terms where they apply, and get legal advice for your specific use case. This is not legal advice.

### Do I need proxies or a headless browser?

No. Piloterr handles rendering, proxy rotation and retries on its side. You only send an HTTPS request with your `x-api-key` header and receive JSON.

### How much does a request cost?

Credits per call: `/v2/thalia/product` **2**, `/v2/thalia/search` **2**. See the [Piloterr website](https://www.piloterr.com) for plans.

### What data does the Thalia API return?

Each endpoint returns structured JSON: **Thalia Product** — `id`, `url`, `isbn`, `image`, `price`, `title`; **Thalia Search** — `results[]`, `results[].id`, `results[].url`, `results[].image`, `results[].price`, `results[].title`.

### How do I paginate Thalia results?

Use the `page` parameter and keep requesting while the response indicates more results.

### Can I get the raw HTML instead of JSON?

Yes. Set `return_page_source=true` and the response becomes `{"result": "..."}` with the page source.

### Which languages are supported?

Any language that can make an HTTP request. This README shows Python and TypeScript; the [`examples/`](examples) folder contains cURL, PHP, C#, Go, Java, Ruby and Node.js.

## Related scrapers

- [About You Scraper API](https://github.com/piloterr/aboutyou-scraper)
- [Action Scraper API](https://github.com/piloterr/action-scraper)
- [Aldi Scraper API](https://github.com/piloterr/aldi-scraper)
- [Alibaba Scraper API](https://github.com/piloterr/alibaba-scraper)
- [AliExpress Scraper API](https://github.com/piloterr/aliexpress-scraper)
- [Allegro Scraper API](https://github.com/piloterr/allegro-scraper)
- [All Piloterr scrapers on GitHub](https://github.com/piloterr)

## Support

- 💬 Join the [Piloterr Discord](https://discord.gg/CFj3ph8aHU) to ask questions and share what you build.
- 📚 Browse the [API reference (OpenAPI)](https://app.piloterr.com/openapi) and the [Piloterr library](https://www.piloterr.com/library).
- 🔑 [Get your API key](https://app.piloterr.com).

---

<p align="center">
  <a href="https://www.piloterr.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="images/logo-dark.svg">
      <img src="images/logo.svg" alt="Piloterr" height="32">
    </picture>
  </a>
  <br>
  <sub>Built by <a href="https://www.piloterr.com">Piloterr</a> — web scraping APIs for e-commerce, search, real estate and more, one API key.</sub>
</p>

<sub>Thalia is a trademark of its respective owner. This repository is not affiliated with or endorsed by Thalia. Examples are released under the MIT license.</sub>
