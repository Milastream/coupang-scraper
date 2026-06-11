[Coupang Scraper](https://apify.com/fatihtahta/coupang-scraper?fpr=data)

## Overview

Coupang Scraper quickly captures structured data from Coupang.com, South Korea’s leading e-commerce marketplace. Point the actor at search results, category listings, or individual product pages to automatically collect product titles, prices, discounts, ratings, review counts, and availability signals. Automated runs keep your dataset fresh and consistent, saving hours of manual research while delivering reliable results at scale.

## Why Use This Actor

- **Market researchers & analysts** gain visibility into trending products, pricing changes, and promotional activity across thousands of listings.
- **E-commerce teams & product managers** can benchmark competitors, track assortment changes, and monitor inventory status for strategic planning.
- **Developers & data teams** integrate up-to-date Coupang product data into dashboards, CRMs, or internal tools without building and maintaining their own scrapers.

Typical use cases include lead generation for marketplace sellers, market and price intelligence projects, product catalog enrichment, and building structured directories of Coupang listings for further analysis.

## Input Parameters

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| `startUrls` | Array of strings | Provide specific Coupang search, category, or product URLs to scrape directly. | – |
| `queries` | Array of strings | Supply one or more search terms; the actor will run Coupang searches and capture matching listings. | – |
| `limit` | Integer | Maximum number of listings to save per input, ideal for scoping or scaling runs. | `50000` |
| `proxyConfiguration` | Object | Choose how the actor connects to Coupang; the default Apify Residential proxy keeps runs dependable. | Residential proxy enabled |

## Example Input

```
{
  "queries": ["야구 모자"],
  "limit": 120,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

## Example Output

```
{
  "id": "86319754626",
  "url": "https://www.coupang.com/vp/products/7411117765?itemId=19202443650&vendorItemId=86319754626&sourceType=srp_product_ads&clickEventId=78e751f0-b650-11f0-ab1d-ce6a77f3bf30&korePlacement=15&koreSubPlacement=1&clickEventId=78e751f0-b650-11f0-ab1d-ce6a77f3bf30&korePlacement=15&koreSubPlacement=1",
  "title": "헬린이상점 숫자 로고 빅사이즈 대두 볼캡 모자",
  "price": "35%23,900원",
  "originalPrice": "36,900원",
  "discount": "35%",
  "rating": 4.5,
  "reviewCount": 671,
  "cashback": "최대 1,195원 적립",
  "soldOut": false,
  "isAd": true
}
```

Each record represents a single Coupang listing, including its unique identifier, detail page URL, current and original price data, promotional discount, rating metrics, cashback information, stock availability, and whether the listing is an advertisement.

## Notes & Limitations

Use this actor responsibly. Always review Coupang’s terms of service and ensure your data usage complies with applicable laws and internal policies.

## Support

Questions or custom needs? Open an issue on the **Issues** tab of the actor page in Apify Console and it will be resolved around the clock.

Happy Scraping,

- Fatih