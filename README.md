# How to Scrape The RealReal Listings in Node.js

This example shows how to scrape luxury resale products in Node.js using [The RealReal Listings Scraper](https://apify.com/piotrv1001/the-realreal-listings-scraper) on Apify. It calls an existing Actor rather than implementing a marketplace scraper.

## What this example does

- Calls `piotrv1001/the-realreal-listings-scraper`
- Passes a The RealReal category URL, global item limit, and detail-mode setting
- Waits for the Actor run to finish
- Fetches the product dataset
- Prints each structured listing

## Prerequisites

- [Node.js](https://nodejs.org) 18 or newer
- An [Apify account](https://console.apify.com/sign-up)
- An Apify API token from **Settings → Integrations**

## Installation

```bash
npm install
```

## Environment setup

```bash
cp .env.example .env
```

Add your token to `.env`:

```env
APIFY_TOKEN=your_apify_token_here
```

## Usage

```bash
npm start
```

## Code example

```js
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

// Initialize the ApifyClient with your Apify API token
// Set APIFY_TOKEN in your .env file (copy .env.example to get started)
const client = new ApifyClient({
    token: process.env.APIFY_TOKEN,
});

// Prepare Actor input
const input = {
    startUrls: [
        { url: 'https://www.therealreal.com/shop/women/handbags' },
    ],
    maxItems: 50,
    scrapeDetails: false,
    proxyConfiguration: {
        useApifyProxy: true,
        apifyProxyGroups: ['RESIDENTIAL'],
        apifyProxyCountryCode: 'US',
    },
};

// Run the Actor and wait for it to finish
const run = await client.actor('piotrv1001/the-realreal-listings-scraper').call(input);

// Fetch and print Actor results from the run's dataset (if any)
console.log('Results from dataset');
console.log(`💾 Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
    console.dir(item);
});

// 📚 Want to learn more 📖? Go to → https://docs.apify.com/api/client/js/docs
```

## Example output

[`sample-output.json`](./sample-output.json) contains two representative handbag listings. Key fields include product and SKU identifiers, designer, current and reference prices, condition, color, material, availability, images, wishlist activity, marketplace badge, and source URL.

## Use cases

- Build a comparable luxury resale price table
- Monitor current prices and availability by designer
- Separate products by condition, material, color, or style
- Track marketplace popularity and merchandising signals
- Select a smaller set for full descriptions and measurements

## Try the Actor on Apify

**[Open The RealReal Listings Scraper on Apify](https://apify.com/piotrv1001/the-realreal-listings-scraper)**

## Related resources

- [How to Scrape The RealReal Listings for Luxury Resale Research](https://www.falconscrape.com/blog/how-to-scrape-the-realreal-listings)
- [Apify JavaScript client documentation](https://docs.apify.com/api/client/js/docs)

## License

MIT
