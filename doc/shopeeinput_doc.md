

# Shopee Input

## Description

<!-- include::../../snippets/pipeline/source-always.adoc[] -->

The Shopee Input transform calls the Shopee API based on your environment, retrieves records, and introduces them into a pipeline.

Two Environment types are available:
- **Sandbox** — Uses https://openplatform.sandbox.test-stable.shopee.sg url as API end points. Only work on test shop made in Open Shopee Platform console.
- **Production** — Uses https://partner.shopeemobile.com url as API end points. Work on real shop page on Shopee.

Two API types are available:

- **Product** — walks every page of `get_item_list` to collect item ids, then fetches each item's base info with `get_item_base_info`.
- **Order** — walks every order status over a configurable time range to collect `order_sn`s, then fetches each order's detail with `get_order_detail`.

The output schema is dynamic: the transform emits one column per top-level key of the returned JSON. Scalar values are typed; nested objects and arrays are returned as a JSON string in a single column.

The transform talks to the Shopee host. Every field supports Hop variables (`${variable}`).

The transform requires a Shopee account with an access token.When Order is requested a time from and time to field will also need to be filled in timestamp format.

> **Warning:** Warning: Querying a large date range may cause the operation to run longer than access token's lifespan, leading to execution failures. To prevent this, refresh access token before running broad queries or reduce the time frame.

![alt text](images/Screenshot%202026-10-09%20113422.png)

## How the data is retrieved

Both API types follow the same **list → detail** cycle:

1. **List** — collect the identifiers (item ids or `order_sn`s) by paginating the list endpoint.
2. **Detail** — fetch the full record for each identifier in chunks of 50.
3. **Output** — emit one row per record, with a column per top-level JSON key.

### Product

- Lists items with `get_item_list`, paginated by `page_size` until `has_next_page` is `false`.
- Fetches each item's base info with `get_item_base_info`, using a comma-separated `item_id_list` of up to 50 ids per request.

### Order

- Lists orders with `get_order_list`, once per order status (`UNPAID`, `READY_TO_SHIP`, `PROCESSED`, `SHIPPED`, `COMPLETED`, `IN_CANCEL`, `CANCELLED`).
- Shopee caps a list query at a 15-day window, so the configured time range is split into 15-day windows and each window is paginated by a string `cursor` until `more` is `false`.
- Fetches each order's detail with `get_order_detail`, using a comma-separated `order_sn_list` of up to 50 ids per request.

## Output columns

The output is built dynamically from the response:

- **One row per record** (item or order).
- **One column per top-level key** of the record's JSON.
- **Scalars are typed** (Integer, Number, Boolean, String).
- **Nested objects and arrays are kept as a JSON string** in a single column. For example, an order with multiple line items returns `item_list` as one JSON string column rather than exploding it into extra rows.


![alt text](images/Screenshot%202026-10-09%20103121.png)
![alt text](images/Screenshot%202026-10-09%20103025.png)

