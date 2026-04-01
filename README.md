# amazon-price-tracker-python
Python web scraper that tracks Amazon product prices daily,  logs data into CSV &amp; sends email alerts when price drops  below target — built with BeautifulSoup &amp; Requests.


# 🛒 Amazon Price Tracker | Python Web Scraping Project

## About This Project

I built this because I wanted to learn how web scraping actually 
works in a real scenario — not just pulling data once and moving on, 
but setting something up that runs on its own every single day and 
tells you when something is worth buying.

The idea is simple. Pick a product on Amazon, track its price 
daily, store everything in a CSV, and get an email the moment 
the price drops below a level you set. No manual checking required.

The product I used to test this was a Data Analyst T-Shirt 
priced at $16.99 — nothing fancy, just something with a stable 
Amazon listing to build and test against.

---

## What This Project Does

- Connects to an Amazon product page using Requests
- Pulls the product title and price using BeautifulSoup
- Cleans the raw scraped text (strips whitespace, removes $ sign)
- Stamps each record with today's date using datetime
- Writes the data into a CSV file — and appends new rows 
  each time it runs so you build a price history over time
- Runs automatically every 24 hours using a while loop 
  with time.sleep(86400)
- Sends you an email alert via smtplib when the price 
  falls below your target threshold

---

## How It Works — Step by Step

**Step 1 — Scrape the page**
Used requests.get() with custom headers to mimic a real 
browser request. Without headers Amazon blocks the request 
immediately — this was the first thing I had to figure out.

**Step 2 — Parse the HTML**
Passed the page content through BeautifulSoup twice — 
once to parse, once after prettify() to normalize the HTML 
structure before extracting specific elements by their IDs.

**Step 3 — Extract and clean**
Grabbed title using id='productTitle' and price using 
id='priceblock_ourprice', then stripped the whitespace 
and removed the dollar sign to get a clean numeric value.

**Step 4 — Log to CSV**
Created a CSV with headers Title, Price, Date and set it 
to append mode so every daily run adds a new row rather 
than overwriting the previous data.

**Step 5 — Automate it**
Wrapped everything into a check_price() function and 
called it inside a while True loop with a 24-hour sleep 
interval so it runs indefinitely without any manual input.

**Step 6 — Email alert**
Added a send_mail() function using smtplib that fires 
an email to yourself when the price crosses below 
a set threshold — so you don't have to open the CSV 
to know when to act.

---

## What I Learned From This

Honestly the scraping part was not the hardest bit. The 
trickier things were:

- Headers matter a lot. Amazon detects and blocks bot-like 
  requests without a proper User-Agent string.
- Amazon changes its HTML structure and element IDs 
  over time — id='priceblock_ourprice' may not work on 
  all listings today. This is something to watch out for 
  if you're running it on a different product.
- Running something on a 24-hour loop that appends to 
  a file taught me how persistent data collection actually 
  works in practice — much more useful than a one-time script.

---

## Libraries Used

- **BeautifulSoup (bs4)** — HTML parsing and data extraction
- **Requests** — HTTP requests to fetch the webpage
- **datetime** — timestamping each scraped record
- **csv** — writing and appending data to CSV file
- **pandas** — reading and viewing the collected CSV data
- **smtplib** — sending automated email price alerts
- **time** — controlling the 24-hour loop interval

---

## Tools

- **Python 3** — Jupyter Notebook
- **Gmail SMTP** — for email alerts

---

## Note

This was built as a learning project for Python web scraping and learned by youtube channel "Alex the analyst". 
Amazon's page structure changes frequently so some element 
IDs in the code may need updating depending on the product 
listing. Always check the current HTML structure before running 
it on a new product URL.
