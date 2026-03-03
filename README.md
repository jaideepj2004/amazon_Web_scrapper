# Amazon Web Scraper

A Flask web application that scrapes **Amazon India** product listings for a user-provided search term, extracts product details and customer reviews, and stores everything in a **MySQL database**.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Scraping Logic](#scraping-logic)
- [Database Schema](#database-schema)
- [Setup & Running](#setup--running)
- [Important Notes](#important-notes)

---

## Overview

Enter a product name in the web UI, and the scraper fetches the first 8 product results from Amazon India (`amazon.in`), visits each product page, and extracts:

- Product name
- Price
- Description
- Customer reviews (name, rating, comment)

All scraped data is stored in a MySQL table and confirmation is sent to the browser.

---

## Features

- Scrapes up to 8 products per search query
- Rotates User-Agent headers to avoid simple bot detection
- Proxy support (plug in your proxy list)
- Retry logic for HTTP 503 errors (up to 3 retries with exponential back-off)
- MySQL storage — creates a fresh table per search and inserts scraped rows
- Simple HTML form frontend

---

## Tech Stack

| Component | Technology |
|---|---|
| Backend | Python, Flask |
| Web Scraping | `requests`, `BeautifulSoup4` |
| Database | MySQL (`mysql-connector-python`) |
| Frontend | HTML (`index.html`), CSS (`styles.css`) |

---

## Project Structure

```
amazon_Web_scrapper/
├── app.py          # Flask app + scraping logic + MySQL operations
├── index.html      # Frontend (search form)
├── styles.css      # Stylesheet
├── mysql.sql       # SQL snippet (table definition preview)
└── README.md
```

---

## Scraping Logic

1. Build search URL: `https://www.amazon.in/s?k=<product>`
2. Fetch search results page with a random User-Agent header.
3. Find all product links (`<a class="a-text-normal">`).
4. For each link (up to 8 products):
   - Visit the product page.
   - Extract: **product name** (`span.a-size-large.product-title-word-break`), **price** (`span.a-price-whole`), **description** (`span.a-list-item`).
   - Extract all customer reviews: **name**, **rating**, **comment**.
5. Aggregate review tuples: `(product_name, price, description, customer_name, rating, comment)`.

---

## Database Schema

A new MySQL table named `prod` is created dynamically per request:

```sql
CREATE TABLE prod (
    id                  INT AUTO_INCREMENT PRIMARY KEY,
    product_description VARCHAR(500),
    price               VARCHAR(25),
    category            VARCHAR(500),
    custumer_name       VARCHAR(200),
    rating              VARCHAR(500),
    comment             VARCHAR(5000)
);
```

---

## Setup & Running

### Prerequisites

- Python 3.8+
- MySQL server running locally
- A `product` database created in MySQL

```sql
CREATE DATABASE product;
```

### Install dependencies

```bash
git clone https://github.com/jaideepj2004/amazon_Web_scrapper.git
cd amazon_Web_scrapper
pip install flask requests beautifulsoup4 mysql-connector-python
```

### Configure MySQL credentials

Open `app.py` and update the connection block:

```python
cnx = mysql.connector.connect(
    user='root',
    password='Your password',   # ← change this
    host='localhost',
    database='product'
)
```

### Run

```bash
python app.py
```

Open `http://127.0.0.1:5000`, enter a product name, and click **Search**.

---

## Important Notes

- Amazon actively blocks scrapers. The User-Agent rotation helps, but you may still receive `503` or CAPTCHA responses.
- The proxy list in `app.py` contains placeholder URLs — replace with real proxies for reliable scraping.
- This project is for **educational purposes only**. Always comply with Amazon's Terms of Service.
