[Coupang Scraper](https://apify.com/abotapi/coupang-scraper?fpr=data)

# Coupang Korea Product Scraper

Pull product listings from Coupang Korea by query, by category, or by direct URL. Returns 20+ structured fields per item: title, brand, price (current and original), discount %, currency, rating, review count, image URL, product URL, Rocket Delivery flag, Tomorrow Delivery flag, availability, full description, and image gallery.

## Why this scraper

- 20+ fields per item, including price (current and original), discount %, average rating, review count, Rocket / Tomorrow / Fresh / Global delivery badges, and full image gallery.
- Two run modes: search (query + filters) and url (multi-URL walk).
- Forward-walking pagination with per-search budget cap so multiple queries each get fair coverage.
- Korean locale and UTC-9 timezone baked into the worker so price formatting and delivery promises match what Korean shoppers see.
- Network discipline: fonts, media, and ad / analytics domains are blocked at the worker level. Bandwidth and wall time per page are noticeably lower than a default run.

## Data you get

> Sample shape, values are illustrative placeholders, not from a live listing.

| Field | Example |
| --- | --- |
| `productId` | `0000000001` |
| `title` | `Sample Product Title` |
| `brand` | `Sample Brand` |
| `url` | `https://www.coupang.com/vp/products/0000000001?itemId=0&vendorItemId=0` |
| `image` | `https://thumbnail.coupangcdn.com/thumbnails/remote/230x230ex/sample.jpg` |
| `images` | `["https://thumbnail.coupangcdn.com/thumbnails/remote/492x492ex/sample-1.jpg", "...sample-2.jpg"]` |
| `price` | `3490` |
| `originalPrice` | `5500` |
| `discountPercent` | `36` |
| `unitPrice` | `(1개당 1,745원)` |
| `currency` | `KRW` |
| `rating` | `4.7` |
| `reviewCount` | `0` |
| `rocketDelivery` | `true` |
| `rocketFresh` | `false` |
| `rocketGlobal` | `false` |
| `tomorrowDelivery` | `true` |
| `freeShipping` | `true` |
| `shippingCost` | `0` |
| `availability` | `InStock` |
| `deliveryInfo` | `내일(목) 도착 보장` |
| `categoryId` | `178155` |
| `categoryPath` | `쿠팡 홈 > Sample Category` |
| `description` | `Full product description appears here when fetchDetails = true.` |
| `query` | `노트북` |
| `scrapedAt` | `2026-01-01T00:00:00.000Z` |

## How to use

Search by Korean keyword:

```
{
  "mode": "search",
  "queries": ["노트북"],
  "maxPages": 3,
  "maxListings": 60,
  "proxy": { "useApifyProxy": true, "apifyProxyGroups": ["RESIDENTIAL"], "apifyProxyCountry": "KR" }
}
```

Search by English keyword with filters:

```
{
  "mode": "search",
  "queries": ["wireless mouse"],
  "minPrice": 10000,
  "maxPrice": 50000,
  "rocketOnly": true,
  "minRating": 4,
  "sortBy": "salePriceAsc",
  "maxPages": 5,
  "maxListings": 100,
  "fetchDetails": true,
  "proxy": { "useApifyProxy": true, "apifyProxyGroups": ["RESIDENTIAL"], "apifyProxyCountry": "KR" }
}
```

Walk multiple categories or saved searches:

```
{
  "mode": "url",
  "urls": [
    "https://www.coupang.com/np/categories/178255",
    "https://www.coupang.com/np/categories/178255?page=2"
  ],
  "maxPages": 2,
  "maxListings": 120
}
```

Scrape a specific product detail page:

```
{
  "mode": "url",
  "urls": ["https://www.coupang.com/vp/products/0000000001?itemId=00000000&vendorItemId=00000000"],
  "fetchDetails": true
}
```

Pull customer reviews for one or more products (one record per review):

```
{
  "mode": "reviews",
  "productIds": ["0000000002", "0000000001"],
  "maxReviewsPerProduct": 50,
  "reviewSortBy": "DATE_DESC",
  "proxy": { "useApifyProxy": true, "apifyProxyGroups": ["RESIDENTIAL"], "apifyProxyCountry": "KR" }
}
```

Reviews mode returns 17 fields per review including title, content, rating, date, helpful counts, photos, videos, vendor name, the variant the reviewer bought, and Coupang's structured survey answers (사용 목적 / 무게 / 성능 buckets). Pull only 1-star reviews for negative-feedback analysis:

```
{
  "mode": "reviews",
  "productIds": ["0000000002"],
  "reviewRatingFilter": 1,
  "maxReviewsPerProduct": 50
}
```

## Input parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `mode` | string | `search` | `search` for query + filters, `url` for direct URL walking, `reviews` for customer reviews by product ID |
| `queries` | array | `["노트북"]` | One or more Korean or English search terms |
| `categoryId` | integer | (none) | Optional category id to narrow the search |
| `minPrice` | integer | (none) | Minimum item price in KRW |
| `maxPrice` | integer | (none) | Maximum item price in KRW |
| `rocketOnly` | boolean | `false` | Keep only items eligible for the Rocket Delivery service |
| `minRating` | integer | (none) | Minimum average rating, 1 to 5 |
| `sortBy` | string | `scoreDesc` | One of `scoreDesc`, `salePriceAsc`, `salePriceDesc`, `latest`, `saleCountDesc` |
| `urls` | array | (none) | One or more `coupang.com/np/search`, `/np/categories/...`, or `/vp/products/...` URLs |
| `productIds` | array | (none) | (Reviews mode) One or more product IDs or product URLs |
| `maxReviewsPerProduct` | integer | `50` | (Reviews mode) Reviews per product to fetch via the paginated API |
| `reviewSortBy` | string | `ORDER_SCORE_ASC` | (Reviews mode) `ORDER_SCORE_ASC` (Best) or `DATE_DESC` (Newest) |
| `reviewRatingFilter` | integer | (none) | (Reviews mode) Only return reviews with this exact star rating (1 to 5) |
| `maxPages` | integer | `3` | Pages to walk per query / URL (60 items per page) |
| `maxListings` | integer | `0` | Stop after this many items (0 = unlimited) |
| `fetchDetails` | boolean | `false` | Visit each product page for brand, full description, image gallery, shipping |
| `proxy` | object | Apify Residential KR | Proxy configuration |

## Output example

> Sample record, values are illustrative placeholders, not from a live listing.

```
{
  "productId": "0000000001",
  "title": "Sample Product Title",
  "brand": "Sample Brand",
  "url": "https://www.coupang.com/vp/products/0000000001?itemId=0&vendorItemId=0",
  "image": "https://thumbnail.coupangcdn.com/thumbnails/remote/230x230ex/sample.jpg",
  "images": [
    "https://thumbnail.coupangcdn.com/thumbnails/remote/492x492ex/sample-1.jpg",
    "https://thumbnail.coupangcdn.com/thumbnails/remote/492x492ex/sample-2.jpg"
  ],
  "price": 3490,
  "originalPrice": 5500,
  "discountPercent": 36,
  "unitPrice": "(1개당 1,745원)",
  "currency": "KRW",
  "rating": 4.7,
  "reviewCount": 0,
  "rocketDelivery": true,
  "rocketFresh": false,
  "rocketGlobal": false,
  "tomorrowDelivery": true,
  "freeShipping": true,
  "shippingCost": 0,
  "availability": "InStock",
  "deliveryInfo": "내일(목) 도착 보장",
  "categoryId": "178155",
  "categoryPath": "쿠팡 홈 > Sample Category",
  "description": "Full seller description text appears here when fetchDetails = true.",
  "query": "노트북",
  "scrapedAt": "2026-01-01T00:00:00.000Z"
}
```

## Plan requirement

Coupang serves Korean shoppers. The upstream edge filter accepts very little non-Korean traffic, so the actor needs **Apify Residential proxy with country `KR`**. This is included in the Apify Starter plan and above.

If you are on the free plan and pick a non-residential proxy, the actor falls back to a small datacenter pool. Throughput is much lower and many pages will not return data; for production runs the upgrade is essential.

Default memory is 1024 MB. Increase to 2048 MB for runs that walk more than 200 pages so the worker has headroom across the full session.