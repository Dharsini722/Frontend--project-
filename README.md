<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Real-Time Stock Ticker</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #0e1117;
      color: #ffffff;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }

    h1 {
      color: #00c3ff;
      margin-bottom: 20px;
    }

    #ticker {
      background-color: #1a1d24;
      border-radius: 10px;
      padding: 15px;
      width: 90%;
      overflow: hidden;
      white-space: nowrap;
      box-shadow: 0 0 15px rgba(0, 195, 255, 0.3);
    }

    .ticker-content {
      display: inline-block;
      animation: scroll 15s linear infinite;
    }

    @keyframes scroll {
      from { transform: translateX(100%); }
      to { transform: translateX(-100%); }
    }

    .stock {
      margin: 0 30px;
      font-size: 1.2em;
    }

    .up {
      color: #00ff80;
    }

    .down {
      color: #ff4040;
    }
  </style>
</head>
<body>
  <h1>📈 Real-Time Stock Ticker</h1>
  <div id="ticker">
    <div class="ticker-content" id="stocks"></div>
  </div>

  <script>
    const API_KEY = "demo"; // Replace with your Alpha Vantage API key
    const symbols = ["AAPL", "TSLA", "MSFT", "GOOGL", "AMZN"];

    async function fetchStock(symbol) {
      try {
        const res = await fetch(`https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol=${symbol}&apikey=${API_KEY}`);
        const data = await res.json();
        const quote = data["Global Quote"];
        return {
          symbol: symbol,
          price: parseFloat(quote["05. price"]).toFixed(2),
          change: parseFloat(quote["09. change"]).toFixed(2),
        };
      } catch (err) {
        console.error("Error fetching stock:", symbol, err);
        return null;
      }
    }

    async function updateTicker() {
      const stocksEl = document.getElementById("stocks");
      stocksEl.innerHTML = "Loading...";

      const results = await Promise.all(symbols.map(fetchStock));
      const validStocks = results.filter(s => s);

      stocksEl.innerHTML = validStocks.map(stock => `
        <span class="stock ${stock.change >= 0 ? "up" : "down"}">
          ${stock.symbol}: $${stock.price} (${stock.change >= 0 ? "+" : ""}${stock.change})
        </span>
      `).join("");
    }

    updateTicker();
    setInterval(updateTicker, 20000); // refresh every 20s
  </script>
</body>
</html># Frontend--project-
