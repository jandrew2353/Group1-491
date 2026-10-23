# Data dictionary: Topic 2, The Empty Shelves Paradox

The data come from a regional grocery chain with ten stores in East Texas, North Louisiana and South Arkansas. They cover 1,200 tracked products over 730 days, from Wednesday 3 January 2024 to Thursday 1 January 2026. The daily files follow the layout of the M5 Forecasting data (daily unit sales, weekly prices, a calendar with events and SNAP days). The order files follow the layout of the Instacart Market Basket data (orders and their product lines). All numbers are simulated. They are not the records of any real company.

Every file is comma-separated and has a header row. Files ending in `.gz` are gzip-compressed, and pandas reads them directly. Each file also has a `*_sample.csv` copy that holds its first 1,000 rows, so you can take a quick look.

## How the files fit together

```
stores.csv    (store_id) ──────────────┐
products.csv  (item_id) ───────────────┼──────────────┬───────────────┐
calendar.csv  (date, ad_week) ──┐      │              │               │
                                ▼      ▼              ▼               ▼
sales_train.csv.gz   (date, store_id, item_id)        one row per store, product and day
inventory.csv.gz     (date, store_id, item_id)        one row per store, product and day
prices.csv           (store_id, item_id, ad_week)     one row per store, product and ad week
orders.csv.gz        (order_id; store_id, date)       one row per sampled order (receipt)
order_lines.csv.gz   (order_id, item_id)              one row per tracked product on a sampled order
```

- `sales_train` and `inventory` share the key (`date`, `store_id`, `item_id`).
- To find the price on a given day, look up that day's `ad_week` in `calendar.csv`. Then join `prices.csv` on (`store_id`, `item_id`, `ad_week`).
- Department, category and unit cost are in `products.csv`, which joins on `item_id`.
- The state of a store is in `stores.csv`. Its SNAP column in `calendar.csv` is `snap_TX`, `snap_LA` or `snap_AR`.

## stores.csv: one row per store (10 rows)

| column | type | meaning |
|---|---|---|
| `store_id` | integer | store number |
| `store_name` | text | the name used inside the company (city and street) |
| `city`, `state` | text | location; state is `TX`, `LA` or `AR` |
| `ad_zone` | text | the advertising zone. All stores in a zone carry the same weekly ad |
| `sq_ft` | integer | sales floor area in square feet |
| `opened` | integer | the year the store opened |

## products.csv: one row per tracked product (1,200 rows)

| column | type | meaning |
|---|---|---|
| `item_id` | integer | the chain's item number |
| `upc` | text | 12-digit UPC barcode (some begin with 0) |
| `product_name` | text | the description printed on the shelf tag |
| `brand` | text | the brand. `Piney Woods` is the chain's own store brand |
| `brand_tier` | text | `national` or `store` |
| `department` | text | Produce, Dairy, Meat & Seafood, Bakery, Frozen, Beverages, Grocery, Household, Health & Beauty |
| `category` | text | the aisle-level group inside the department, for example Milk, Cereal or Paper |
| `size` | text | package size as printed on the label |
| `case_pack` | integer | units in one case from the distribution center |
| `unit_cost` | number | what the chain pays for one unit, in US dollars. This is the current standard cost, the same for every week and store |

Products that are sold by weight at the register (loose produce, service-counter meat and deli) are not tracked.

## calendar.csv: one row per day (730 rows)

| column | type | meaning |
|---|---|---|
| `date` | text | `YYYY-MM-DD` |
| `ad_week` | integer | the ad week: four-digit year, then the week number (`202401` is the first ad week of 2024). An ad week runs Wednesday to Tuesday |
| `weekday` | text | `Monday` ... `Sunday` |
| `wday` | integer | day of the ad week: 1 = Wednesday ... 7 = Tuesday |
| `month`, `year` | integer | |
| `d` | text | day number, `d_1` ... `d_730` |
| `event_name_1`, `event_type_1` | text | a holiday or event on that day (blank if none). Types are Sporting, Cultural, National and Religious |
| `event_name_2`, `event_type_2` | text | a second event on the same day (almost always blank) |
| `snap_TX`, `snap_LA`, `snap_AR` | 0/1 | 1 if SNAP food benefits are issued in that state on that day |

All stores are closed on Christmas Day.

## prices.csv: weekly shelf price (1,260,000 rows)

| column | type | meaning |
|---|---|---|
| `store_id` | integer | store number |
| `item_id` | integer | item number |
| `ad_week` | integer | ad week (see `calendar.csv`) |
| `sell_price` | number | the shelf price of one unit in US dollars, the same on all seven days of the ad week. A multi-buy offer such as "3 for $5" is written as the price of one unit (1.67) |

## sales_train.csv.gz: daily unit sales from the registers (about 8.8 million rows)

| column | type | meaning |
|---|---|---|
| `date` | text | calendar day, `YYYY-MM-DD` |
| `store_id` | integer | store number |
| `item_id` | integer | item number |
| `unit_sales` | integer | units scanned at the registers that day, minus units returned |

There is a row for every store, product and day, including days with no sales. A return is entered as a negative scan.

## inventory.csv.gz: daily stock count (8,760,000 rows)

| column | type | meaning |
|---|---|---|
| `date` | text | calendar day, `YYYY-MM-DD` |
| `store_id` | integer | store number |
| `item_id` | integer | item number |
| `on_hand` | integer | units of the product in the store (sales floor and back room) when the store opens that morning |
| `received` | integer | units delivered from the distribution center overnight. They are already included in `on_hand` |
| `shrink` | integer | units written off at the end of that day (spoiled, damaged or out of date) |

For one product in one store, the counts link from day to day: this morning's `on_hand` = yesterday's `on_hand` - yesterday's units sold - yesterday's `shrink` + this morning's `received`. Returned units are not put back on the shelf.

## orders.csv.gz: a random 5% of all orders (about 344,000 rows)

| column | type | meaning |
|---|---|---|
| `order_id` | integer | receipt number |
| `store_id` | integer | store number |
| `date` | text | `YYYY-MM-DD` |
| `order_time` | text | time the receipt was printed, `HH:MM` (stores are open 6:00 to 23:00) |
| `customer_id` | integer | loyalty-card number, blank if no card was scanned |
| `tender_type` | text | how the order was paid: `CREDIT`, `DEBIT`, `CASH` or `EBT` (SNAP benefits) |

## order_lines.csv.gz: the tracked products on those orders (about 1.5 million rows)

| column | type | meaning |
|---|---|---|
| `order_id` | integer | receipt number (joins `orders.csv.gz`) |
| `item_id` | integer | product bought |
| `add_to_cart_order` | integer | position of the product on the receipt |
| `units` | integer | units of that product on the order |

Every order at every store had the same 5% chance of being kept, so 20 times the number of sampled orders estimates the orders a store had on a day. A kept order comes with all of its lines for the 1,200 tracked products. Lines for other products are left out, so an order can have fewer lines than the shopper's full basket, and an order that bought no tracked products does not appear. The order files have no prices; join `prices.csv`.

## What is NOT recorded

- There is no promotion flag and no copy of the weekly ad. A promotion shows up only as a lower `sell_price`.
- There is no count of people entering the stores. Orders (receipts) are the only measure of traffic, and they come as a 5% sample.
- Only completed sales are recorded. There is no record of what shoppers asked for or looked for.
- Stock is counted once a day, at opening. There is no count during the day.
- There is no record of delivery schedules, truck loads, store labour hours, advertising spend, competitor prices or weather.
- `unit_cost` is a single standard cost. Supplier allowances and promotion funding are not recorded.

## Units and allowed values (as the systems are supposed to write them)

- Money is in US dollars with two decimals. Units are whole numbers.
- Dates are `YYYY-MM-DD`.
- Codes have no leading or trailing spaces.
- `unit_sales` is a small whole number. A store sells at most a few hundred units of one product in a day, and a negative number means returns.
- `on_hand`, `received` and `shrink` are 0 or more. `received` is a whole number of cases.

The exports were not checked before release. Please check them yourself and report what you find, with the number of rows each problem affects.
