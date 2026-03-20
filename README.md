# μ MicroTrade — AI Trading Terminal

A Bloomberg Terminal-inspired personal trading dashboard built for small trades under $100. Real money. Real trades. Real-time data.

![MicroTrade](https://img.shields.io/badge/MicroTrade-v1.0-FFB000?style=flat-square) ![Alpaca](https://img.shields.io/badge/Broker-Alpaca-00C805?style=flat-square) ![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

---

## Features

- **Real Trading** — Buy and sell fractional shares with real money through Alpaca
- **$100 Safety Cap** — Built-in limit prevents any single order over $100
- **Live Price Streaming** — WebSocket connection for real-time price updates
- **Interactive Charts** — Canvas-rendered price charts with hover tooltips, multiple timeframes
- **News Feed** — Live financial news from Alpaca's news API, filterable by stock
- **Portfolio Tracking** — Real-time P&L, positions, and account overview
- **Order Management** — View active orders, order history, and cancel pending orders
- **Paper Trading** — Test everything risk-free with Alpaca's sandbox before going live
- **Bloomberg Aesthetic** — Amber-on-black terminal UI with IBM Plex Mono typography

---

## Quick Start

### 1. Prerequisites

- **Node.js** 18+ — [Download](https://nodejs.org)
- **Alpaca Account** — [Sign up free](https://alpaca.markets)

### 2. Clone & Install

```bash
git clone <your-repo-url> microtrade
cd microtrade
npm install
```

### 3. Configure

```bash
cp .env.example .env
```

Edit `.env` with your Alpaca API keys:

```env
ALPACA_API_KEY=PK1234567890        # From Alpaca dashboard
ALPACA_SECRET_KEY=abcdef1234567890  # From Alpaca dashboard
ALPACA_PAPER=true                   # Start with paper trading!
```

**Where to get keys:**
- Paper trading: [app.alpaca.markets/paper/dashboard](https://app.alpaca.markets/paper/dashboard/overview) → API Keys
- Live trading: [app.alpaca.markets/dashboard](https://app.alpaca.markets/dashboard/overview) → API Keys

### 4. Run

```bash
npm run dev
```

Open **http://localhost:5173** in your browser.

---

## Architecture

```
microtrade/
├── server/
│   └── index.js          # Express API + WebSocket server
├── src/
│   ├── main.jsx          # React entry point
│   ├── App.jsx           # Main layout with routing
│   ├── index.css         # Global Bloomberg-style CSS
│   ├── store.js          # Zustand state management
│   ├── hooks/
│   │   └── useWebSocket.js  # Live price streaming hook
│   └── components/
│       ├── TopBar.jsx       # Logo, balance, fund button
│       ├── Watchlist.jsx    # Stock watchlist with search
│       ├── Chart.jsx        # Canvas price chart
│       ├── TradePanel.jsx   # Buy/sell execution bar
│       ├── Portfolio.jsx    # Positions & account summary
│       ├── OrderBook.jsx    # Order history & management
│       ├── NewsFeed.jsx     # News sidebar
│       ├── StatusBar.jsx    # Market status bar
│       ├── Notification.jsx # Toast notifications
│       └── SetupScreen.jsx  # Onboarding / setup guide
├── .env.example
├── package.json
└── vite.config.js
```

### API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/status` | GET | Server & market status |
| `/api/account` | GET | Account balance & info |
| `/api/positions` | GET | Open positions |
| `/api/orders` | GET | Order history |
| `/api/orders` | POST | Place a new order |
| `/api/orders/:id` | DELETE | Cancel an order |
| `/api/quote/:symbol` | GET | Latest quote for a symbol |
| `/api/bars/:symbol` | GET | Price history (OHLCV) |
| `/api/snapshots` | GET | Multi-symbol snapshots |
| `/api/news` | GET | Financial news feed |
| `/api/assets/search` | GET | Search tradeable stocks |
| `/ws` | WebSocket | Live price streaming |

---

## Deployment

### Option A: Railway (Recommended — easiest)

1. Push your code to GitHub
2. Go to [railway.app](https://railway.app)
3. New Project → Deploy from GitHub repo
4. Add environment variables (your `.env` values)
5. Railway auto-detects Node.js and deploys

```bash
# Build command (auto-detected):
npm run build

# Start command:
npm start
```

### Option B: Render

1. Push to GitHub
2. Go to [render.com](https://render.com) → New Web Service
3. Connect your repo
4. Build Command: `npm install && npm run build`
5. Start Command: `npm start`
6. Add environment variables

### Option C: VPS (DigitalOcean, Linode, etc.)

```bash
# On your server:
git clone <your-repo> microtrade
cd microtrade
npm install
npm run build

# Run with PM2 for production:
npm install -g pm2
pm2 start server/index.js --name microtrade
```

### Option D: Docker

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build
ENV NODE_ENV=production
EXPOSE 3001
CMD ["node", "server/index.js"]
```

```bash
docker build -t microtrade .
docker run -p 3001:3001 --env-file .env microtrade
```

---

## Going Live (Real Money)

> ⚠️ **Start with paper trading first.** Make sure everything works before risking real money.

1. Open [app.alpaca.markets](https://app.alpaca.markets) (not the paper dashboard)
2. Complete identity verification (required by law)
3. Generate live API keys
4. Update your `.env`:
   ```env
   ALPACA_API_KEY=your_live_key
   ALPACA_SECRET_KEY=your_live_secret
   ALPACA_PAPER=false
   ```
5. Link your bank account on the Alpaca dashboard for deposits/withdrawals
6. Restart the server

### Deposits & Withdrawals

MicroTrade uses Alpaca's ACH transfer system for moving money:

- **Deposit**: Link your bank on Alpaca dashboard → Transfer funds in
- **Withdraw**: Alpaca dashboard → Transfer funds to your bank
- Transfers typically take 1-3 business days
- The `± FUND` button in MicroTrade links to the Alpaca dashboard

---

## Safety Features

- **$100 order cap** — Server-side enforcement, cannot be bypassed from the frontend
- **Paper trading default** — Starts in sandbox mode, requires explicit switch to live
- **PDT warning** — Alerts when approaching pattern day trader limits (3 day trades / 5 days)
- **Market status** — Shows when market is open/closed to prevent confusion

---

## Tech Stack

- **Frontend**: React 18, Vite, Zustand (state), Canvas API (charts)
- **Backend**: Express.js, WebSocket (ws)
- **Broker**: Alpaca Trade API (commission-free, fractional shares)
- **Data**: Alpaca Market Data API (quotes, bars, news)
- **Styling**: Custom CSS with Bloomberg-inspired design system

---

## License

MIT — Use it, modify it, trade with it. Not financial advice.
