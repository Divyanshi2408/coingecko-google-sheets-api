# 🚀 CoinGecko Google Sheets API Integration

## 🎯 Objective

To build an automated system using **Google Apps Script** that fetches and updates **real-time cryptocurrency market data** from the [CoinGecko public API](https://www.coingecko.com/en/api) into a **structured Google Sheet**.

---

## 📦 Features

- ✅ Fetches the top 10 cryptocurrencies by market cap
- ✅ Populates columns:
  - Name
  - Symbol
  - Current Price (USD)
  - Market Cap
  - 24h % Change
  - Last Updated
- ✅ Handles API rate limits (`429 Too Many Requests`)
- ✅ Can be triggered manually or automatically (time-driven)

---

## 🧠 Approach

1. Used `UrlFetchApp` in Apps Script to call CoinGecko’s REST API endpoint:
https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=10&page=1

2. Parsed JSON response and appended formatted rows to the sheet.

3. Implemented graceful error handling for rate limits:

```javascript
const response = UrlFetchApp.fetch(url, { muteHttpExceptions: true });
if (response.getResponseCode() === 429) {
sheet.appendRow(["Error", "Rate Limit Exceeded. Please try again in 1–2 minutes."]);
return;
}
```

### 🛠 Tech Stack
- Google Apps Script
- Google Sheets
- CoinGecko REST API

### ⏰ Trigger Setup
- Function: fetchCryptoData
- Trigger type: Time-driven
- Frequency: Hourly

⚠️ API Rate Limit Handling
If the sheet shows:

javascript
```
Error | Rate Limit Exceeded. Please try again in 1–2 minutes.
```
This is normal behavior during testing. The script is designed to fail gracefully when CoinGecko's free tier rate limit is hit. It will work again after a short cooldown period.

