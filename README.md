# IKEA dataset: 1,000 products in 50 countries (free sample)

A free sample of **IKEA product data**: 1,000 random articles from IKEA's UK range (every top-level category,
in proportion), each with its listing in every country that sells it. That is 21,016 rows, so you can compare what
the same article costs in 35 currencies.

Every row has the local price and previous price, price labels (new, lower price, IKEA Family, limited offer),
product name, type, colour and measurements in the local language, variant options, rating and number of ratings,
online and last-chance flags, category path, IKEA business and product areas, and image URLs.

**The full dataset** (1,200,000+ listings of 140,000+ articles in 50
countries): **[cartpie.com/datasets/ikea](https://cartpie.com/datasets/ikea)**.

Also on [GitHub](https://github.com/votiakov/ikea-dataset), [Hugging Face](https://huggingface.co/datasets/votiakov/ikea-products-sample) and [Kaggle](https://www.kaggle.com/datasets/votiakov/ikea-products-sample).

Made by [Andrii Votiakov](https://votiakov.com).

## Files

| File | Format | Rows |
|---|---|---|
| [`data/ikea_sample_2026-11-13.csv`](data/ikea_sample_2026-11-13.csv) | CSV (UTF-8, header; lists separated by `\|`) | 21,016 |
| [`data/ikea_sample_2026-11-13.parquet`](data/ikea_sample_2026-11-13.parquet) | Parquet (zstd) | 21,016 |
| [`data/ikea_sample_2026-11-13.jsonl`](data/ikea_sample_2026-11-13.jsonl) | JSON Lines | 21,016 |
| [`data/schema.json`](data/schema.json) | Field names, types and descriptions | |

## One article in every country

PÅVEFINK Lantern for pillar candle, bamboo, 25 cm, article `60609976`:

| Country | Product type (local) | Price | Previous price |
|---|---|---|---|
| United Kingdom | Lantern for pillar candle | £25 | |
| Australia | Lantern for pillar candle | 35.10 AUD | ~~39 AUD~~ |
| Austria | Laterne für Blockkerze | €24.99 | |
| Bahrain | Lantern for pillar candle | 9.90 BHD | |
| Belgium | Lantaarn voor stompkaars | €29.99 | |
| Canada | Lantern for pillar candle | 34.99 CAD | |
| Colombia | Farol para vela | 119,990 COP | |
| Croatia | Fenjer za svijeću | €24.99 | |
| Czechia | Lucerna na sloupovou svíčku | 599 CZK | |
| Denmark | Lanterne til bloklys | 199 DKK | |
| Egypt | Lantern for pillar candle | 3,299 EGP | |
| Estonia | Küünlalatern | €24.99 | |
| Finland | Lyhty pöytäkynttilälle | €24.99 | |
| France | Lanterne bougie bloc | €24.99 | |
| Germany | Laterne für Blockkerze | €24.99 | |
| Hungary | Lámpás tömbgyertyához | 8,990 HUF | |
| Ireland | Lantern for pillar candle | €25 | |
| Israel | עששית לנר בלוק | 145 ILS | |
| Italy | Lanterna per cero | €29.95 | |
| Jordan | Lantern for pillar candle | 24.99 JOD | |
| Kuwait | Lantern for pillar candle | 8.50 KWD | |
| Latvia | Laterna lielajai svecei | €24.99 | |
| Lithuania | Cilindrinės žvakės žibintas | €24.99 | |
| Mexico | Farol para vela | 699 MXN | |
| Morocco | Lanterne bougie bloc | 329 MAD | |
| Netherlands | Lantaarn voor stompkaars | €24.99 | |
| New Zealand | Lantern for pillar candle | 49.99 NZD | |
| Norway | Lykt til kubbelys | 279 NOK | |
| Oman | Lantern for pillar candle | 8.50 OMR | |
| Poland | Latarenka na świece kolumnowe | 99.99 PLN | |
| Portugal | Lanterna p/vela grossa | €25 | |
| Qatar | Lantern for pillar candle | 95 QAR | |
| Romania | Felinar lumânare bloc | 129.90 RON | |
| Saudi Arabia | Lantern for pillar candle | 129 SAR | |
| Serbia | Fenjer za top sveću | 2,999 RSD | |
| Slovakia | Lampáš na vysokú sviečku | €19.99 | |
| Slovenia | Laterna za svečo | €29.99 | |
| Spain | Farol para vela | €29.99 | |
| Sweden | Lykta för blockljus | 299 SEK | |
| Switzerland | Laterne für Blockkerze | 24.95 CHF | |
| United Arab Emirates | Lantern for pillar candle | 85 AED | |
| United States | Lantern for pillar candle | $34.99 | |

## Preview (UK)

| Name | Type | Colour / design | Size | Price | Was | Category | Rating |
|---|---|---|---|---|---|---|---|
| JÄTTEBO | Cover for armrest | Samsala grey-beige |  | £30 |  | Living room seating | 4.3 |
| METOD | Corner base cab w pull-out fit | white/Kallarp high-gloss dar | 128x68 cm | £269 |  | Kitchens | 4.0 |
| HULTARP | Rail | polished/brass-colour | 80 cm | £10 |  | Kitchens | 4.7 |
| VIKANES | Door with hinges | red-brown | 50x195 cm | £77 |  | Storage furniture |  |
| METOD / MAXIMERA | High cabinet f built-in applia | white/Sinarp oak veneer | 60x60x140 cm | £159 |  | Kitchens | 5.0 |
| METOD | High cab f oven w 2 doors/shel | white/Sinarp oak veneer | 60x60x220 cm | £279 |  | Kitchens | 3.8 |
| UPPÅKRA | Modular corner sofa, 3-seat | Axvall dark grey-blue |  | £1,430 |  | Living room seating | 1.8 |
| BILLY | Bookcase | oak effect | 80x28x202 cm | £70 |  | Storage furniture | 4.5 |

## Quick start

```python
import pandas as pd
df = pd.read_parquet("data/ikea_sample_2026-11-13.parquet")
# one article, every country
print(df[df.item_no == "60609976"][["country", "currency", "price", "type_name"]])
# number of articles and median price by country
print(df.groupby(["country", "currency"]).price.agg(["count", "median"]))
```

```sql
-- DuckDB: share of articles with a lowered price, by country
SELECT country, count(*) AS articles, avg((original_price IS NOT NULL)::INT) AS share_reduced
FROM 'data/ikea_sample_2026-11-13.parquet'
GROUP BY 1 ORDER BY articles DESC;
```

## Fields

| Field | Type | Description |
|---|---|---|
| `item_no` | string | IKEA article number (8 digits). The same number is the same article in every country. |
| `item_type` | string | ART = single article; SPR = combination of several articles sold as one product. |
| `parent_item_no` | string | Article number of the product this row is a colour/size variant of (equals item_no for the main product). |
| `is_main_variant` | boolean | True for the product's main variant, false for its other colour/size variants. |
| `name` | string | Product range name, e.g. KALLAX. |
| `type_name` | string | Product type, e.g. 'Shelving unit', in the country's language. |
| `design` | string | Colour / design text, e.g. 'white stained oak effect'. |
| `measure` | string | Main measurement as shown, e.g. '77x147 cm'. |
| `option_colour` | string | Colour option this variant represents among the product's variants. |
| `option_size` | string | Size option this variant represents among the product's variants. |
| `variant_count` | integer | Number of variants of the product (main variant included). |
| `product_url` | string | Product page on the country's IKEA website. |
| `country` | string | ISO country code. |
| `country_name` | string | Country name. |
| `language` | string | Language of names, types and categories. |
| `currency` | string | ISO currency of the prices. |
| `price` | number | Current selling price in local currency. |
| `original_price` | number | Previous price when the price was lowered; empty otherwise. |
| `discount_pct` | number | 1 - price / original_price, rounded to 4 decimals; 0 when not reduced. |
| `price_tags` | list of strings | Price labels: NEW_PRODUCT, NEW_LOWER_PRICE, TIME_RESTRICTED_OFFER, FAMILY_PRICE (IKEA Family member price), DISCONTINUED_PRODUCT, LOWER_CURRENT_REGULAR_PRICE_IN_STORE, COMING_SOON. Every reduced price carries one of them. |
| `price_valid_from` | date | Start of the current price's validity, when published. |
| `price_valid_to` | date | End of the current price's validity, when published. |
| `rating` | number | Average customer rating (1-5). |
| `rating_count` | integer | Number of customer ratings. |
| `online_sellable` | boolean | False when the article is sold in stores only. |
| `last_chance` | boolean | True when the article is being discontinued. |
| `top_category` | string | Top-level category, e.g. 'Storage furniture'. |
| `category` | string | Leaf category. |
| `category_path` | string | Category path from top level to leaf, separated by ' > '. |
| `business_area` | string | IKEA home furnishing business, e.g. 'Bedroom furniture'. |
| `product_range_area` | string | IKEA product range area, e.g. 'Chests & other furniture'. |
| `product_area` | string | IKEA product area, e.g. 'Chest of drawers'. |
| `image_url` | string | Main product image URL. |
| `image_urls` | list of strings | All product image URLs. |
| `image_count` | integer | Number of product images. |
| `collected_at` | timestamp | When this record was collected (UTC). |

## Notes

- `item_no` is the same article in every country, so countries join on it. `item_type` SPR rows are combinations sold as one product.
- Prices are what shoppers in each country see, in local currency. US and Canadian prices exclude sales tax; most other countries include VAT.
- Names, product types, colours and categories are in each country's language.
- Some markets, mainly in Asia-Pacific, sell many articles under their own article numbers.
- Image URLs only. No image files are included.
- Independent data. Not affiliated with, endorsed by or sponsored by IKEA or Inter IKEA Systems B.V. IKEA is a trademark of its owner.

## Terms

Free to use for evaluation, research and journalism, with attribution to [cartpie.com/datasets/ikea](https://cartpie.com/datasets/ikea).
Not for resale or redistribution as a dataset. For commercial use and the full data, get a licence at
[cartpie.com/datasets/ikea](https://cartpie.com/datasets/ikea).
