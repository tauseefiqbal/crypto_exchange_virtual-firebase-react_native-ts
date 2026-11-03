# VCrypto — Virtual Cryptocurrency Exchange & Portfolio Simulator

**VCrypto** is a cross-platform mobile and web virtual cryptocurrency exchange application built with **React Native (Expo)**, **TypeScript**, and **Firebase**. Every new trader starts with a **$100,000 USD** virtual balance to buy and sell real-time market cryptocurrencies (synced live from the CoinGecko API), track portfolio performance with interactive price charts, and compete on a real-time global leaderboard.

---

## Table of Contents

- [1. Features](#1-features)
  - [1.1 Core Trading & Portfolio Features](#11-core-trading--portfolio-features)
  - [1.2 Live Market & Analytics Features](#12-live-market--analytics-features)
  - [1.3 Authentication, Leaderboard & Security Features](#13-authentication-leaderboard--security-features)
- [2. Tech Stack](#2-tech-stack)
  - [2.1 Frontend & Mobile Framework](#21-frontend--mobile-framework)
  - [2.2 Backend, Database & Cloud Services](#22-backend-database--cloud-services)
  - [2.3 DevOps, Build & Tooling](#23-devops-build--tooling)
- [3. Deployment](#3-deployment)
  - [3.1 Live Application URLs (APP URL)](#31-live-application-urls-app-url)
  - [3.2 Android APK Build via Expo EAS](#32-android-apk-build-via-expo-eas)
  - [3.3 Web Build & Firebase Hosting Deployment](#33-web-build--firebase-hosting-deployment)
- [4. Test and Admin Users Credentials](#4-test-and-admin-users-credentials)
  - [4.1 Regular Test User Credentials](#41-regular-test-user-credentials)
  - [4.2 Instant Guest / Demo Access](#42-instant-guest--demo-access)
  - [4.3 Seeded Leaderboard Demo Accounts](#43-seeded-leaderboard-demo-accounts)
  - [4.4 Admin & Firebase Console Credentials](#44-admin--firebase-console-credentials)
- [5. How to Use App for Regular User](#5-how-to-use-app-for-regular-user)
  - [5.1 Signing Up or Logging In](#51-signing-up-or-logging-in)
  - [5.2 Viewing Your Portfolio & $100,000 Starter Balance](#52-viewing-your-portfolio--100000-starter-balance)
  - [5.3 Exploring Live Crypto Markets & Charts](#53-exploring-live-crypto-markets--charts)
  - [5.4 Buying and Selling Cryptocurrencies](#54-buying-and-selling-cryptocurrencies)
  - [5.5 Competing on the Global Rankings Leaderboard](#55-competing-on-the-global-rankings-leaderboard)
  - [5.6 Managing Your User Profile](#56-managing-your-user-profile)
- [6. How to Use App for Admin User](#6-how-to-use-app-for-admin-user)
  - [6.1 Initializing & Seeding the Firestore Database](#61-initializing--seeding-the-firestore-database)
  - [6.2 Syncing Live CoinGecko Market Prices](#62-syncing-live-coingecko-market-prices)
  - [6.3 Auditing Firestore Coin Prices](#63-auditing-firestore-coin-prices)
  - [6.4 Managing Cloud Functions & Scheduled Jobs](#64-managing-cloud-functions--scheduled-jobs)
  - [6.5 Managing Users & Security Rules in Firebase Console](#65-managing-users--security-rules-in-firebase-console)
- [7. Project Architecture & Directory Structure](#7-project-architecture--directory-structure)
  - [7.1 Application Folder Layout](#71-application-folder-layout)
  - [7.2 Cloud Firestore Data Model](#72-cloud-firestore-data-model)
- [8. Local Development Setup & Installation](#8-local-development-setup--installation)
  - [8.1 Prerequisites](#81-prerequisites)
  - [8.2 Environment Variables Configuration](#82-environment-variables-configuration)
  - [8.3 Running the Application Locally](#83-running-the-application-locally)
- [9. Troubleshooting & FAQ](#9-troubleshooting--faq)
  - [9.1 Common Build & Runtime Issues](#91-common-build--runtime-issues)
- [10. License](#10-license)

---

## 1. Features

### 1.1 Core Trading & Portfolio Features
- ✅ **$100,000 USD Virtual Starter Balance:** Every newly registered user or guest account is automatically provisioned with `$100,000.00` in virtual USD to begin trading risk-free.
- ✅ **Real-Time Portfolio Valuation:** Live Firestore listeners (`onSnapshot`) dynamically compute total portfolio net worth and individual asset holdings as market prices fluctuate.
- ✅ **Atomic Buy & Sell Order Execution:** Supports both server-side Cloud Functions (`exchangeCoins`) and client-side atomic Firestore transactions (`runTransaction`) with strict USD/coin balance validation.
- ✅ **Instant USD ↔ Crypto Conversion Calculator:** Bidirectional input fields on the exchange screen automatically calculate equivalent token quantities or USD values in real time.
- ✅ **One-Tap "Buy Max" & "Sell All" Shortcuts:** Quickly allocate 100% of available USD balance on buy orders or liquidate an entire token position on sell orders.

### 1.2 Live Market & Analytics Features
- ✅ **Live CoinGecko Market Sync:** Fetches top 50+ cryptocurrencies by market cap directly from the CoinGecko REST API v3, including current prices and 7-day sparkline histories.
- ✅ **Multi-Timeframe Percentage Change Indicators:** Displays color-coded (`green` / `red`) price movement percentages across **1-Hour**, **24-Hour (1 Day)**, and **7-Day** windows.
- ✅ **Interactive 7-Day Price History Graphs:** Renders smooth bezier line charts on the Coin Details screen using `react-native-chart-kit` and `react-native-svg`.
- ✅ **Pull-to-Refresh Market Feeds:** Swipe down on the Market or Rankings screens to trigger an immediate live price sync and leaderboard recalculation.

### 1.3 Authentication, Leaderboard & Security Features
- ✅ **Multi-Method Firebase Authentication:** Supports **Email & Password Registration/Login**, **One-Tap Guest / Demo Sign-In** (Anonymous Auth), and OAuth entry points.
- ✅ **Persistent Cross-Session Auth State:** Uses `@react-native-async-storage/async-storage` to keep users signed in across app restarts on Android, iOS, and Web.
- ✅ **Global Real-Time Trader Leaderboard:** Ranks up to the top 100 traders ordered by net worth (`orderBy('networth', 'desc')`) with instant updates.
- ✅ **Granular Firestore Security Rules:** Protects market coin documents from unauthorized client tampering, prevents direct client manipulation of the `networth` leaderboard field, and isolates portfolio subcollections (`users/{userId}/portfolioCoins/{coinId}`) to their respective owners.

---

## 2. Tech Stack

### 2.1 Frontend & Mobile Framework
| Technology | Version | Purpose |
| :--- | :--- | :--- |
| **React Native** | `0.74.5` | Cross-platform native mobile UI for Android & iOS |
| **Expo SDK** | `~51.0.38` | Managed app workflow, native APIs, fonts, assets & web support |
| **TypeScript** | `~5.3.3` | Static type checking across screens, services, and Cloud Functions |
| **React Navigation v6** | `^6.1.18` | Native stack (`@react-navigation/stack`) and bottom tabs (`@react-navigation/bottom-tabs`) |
| **React Native Chart Kit** | `^6.12.0` | Bezier line charts for 7-day cryptocurrency price sparklines |
| **React Native SVG** | `15.2.0` | Vector graphics rendering engine for charts and icons |
| **React Native Web** | `~0.19.10` | Browser compatibility layer for running VCrypto on the web |

### 2.2 Backend, Database & Cloud Services
| Technology | Version | Purpose |
| :--- | :--- | :--- |
| **Firebase Authentication** | `^10.14.1` | Email/Password and Anonymous Guest authentication |
| **Cloud Firestore** | `^10.14.1` | NoSQL real-time document database for `coins`, `users`, and `portfolioCoins` |
| **Firebase Cloud Functions (v2)** | `Node / TS` | Serverless triggers (`handleUserSignup`), callable trades (`exchangeCoins`), and scheduled jobs (`syncCoinGeckoPrices`) |
| **CoinGecko Public API v3** | `REST API` | Real-time cryptocurrency market prices, icons, percentage changes, and 7-day sparklines |

### 2.3 DevOps, Build & Tooling
| Tool | Purpose |
| :--- | :--- |
| **Expo Application Services (EAS)** | Cloud Android `.apk` / `.aab` and iOS builds (`eas.json`) |
| **Firebase CLI & Emulator Suite** | Local Auth (`:9099`), Firestore (`:8080`), Functions (`:5001`), and Emulator UI (`:4000`) |
| **Node.js Admin Scripts** | Standalone database initialization, live price syncing, and Firestore auditing (`scripts/`) |

---

## 3. Deployment

### 3.1 Live Application URLs (APP URL)
| Environment / Platform | Live URL / Build Link | Status |
| :--- | :--- | :--- |
| **Web App (APP URL)** | [https://react-native-crypto-exchange.web.app](https://react-native-crypto-exchange.web.app) *(or replace with your custom hosted URL)* | 🟢 Ready / Configurable |
| **Firebase Alternate URL** | [https://react-native-crypto-exchange.firebaseapp.com](https://react-native-crypto-exchange.firebaseapp.com) | 🟢 Ready / Configurable |
| **Expo EAS Android Build** | [https://expo.dev/accounts/tauseefiqbal/projects/VCrypto/builds/1bcbff82-2ba7-4f78-9e65-5cd272095290](https://expo.dev/accounts/tauseefiqbal/projects/VCrypto/builds/1bcbff82-2ba7-4f78-9e65-5cd272095290) | 🟢 APK Preview Build |
| **Expo Project Dashboard** | [https://expo.dev/accounts/tauseefiqbal/projects/VCrypto](https://expo.dev/accounts/tauseefiqbal/projects/VCrypto) | 🟢 Active |

### 3.2 Android APK Build via Expo EAS
The project is pre-configured with `eas.json` to generate an installable Android `.apk` under the `preview` profile:
```bash
# Install EAS CLI globally if not installed
npm install -g eas-cli

# Log in to your Expo account
eas login

# Trigger an installable Android APK build in the cloud
eas build -p android --profile preview

# Check build status or download link anytime
eas build:list
```

### 3.3 Web Build & Firebase Hosting Deployment
To export the static web bundle into the `dist/` directory and deploy it live:
```bash
# Export production web bundle via Expo
npx expo export -p web

# Deploy Firestore rules, indexes, andFunctions/Hosting to Firebase
firebase deploy --only firestore:rules,firestore:indexes
```

---

## 4. Test and Admin Users Credentials

### 4.1 Regular Test User Credentials
You can sign in using the pre-configured test trader account or register a brand-new account directly from the **Welcome Screen** (`Sign In or Register with Email`):

| Role | Email | Password | Starting Balance |
| :--- | :--- | :--- | :--- |
| **Regular Test User 1** | `trader@vcrypto.com` | `Test@1234` | `$100,000.00 USD` |
| **Regular Test User 2** | `demo@vcrypto.com` | `Demo@1234` | `$100,000.00 USD` |

*(Note: If these accounts have not yet been created in your Firebase Auth instance, open the app, tap **Sign In or Register with Email** → **Don't have an account? Register**, and sign up with the credentials above to immediately receive `$100,000`.)*

### 4.2 Instant Guest / Demo Access
- **Button:** Tap **`Continue as Guest / Demo`** (or the Google/Apple quick-demo buttons on the Welcome screen).
- **Credentials Required:** None (uses Firebase Anonymous Authentication).
- **Provisioning:** Automatically seeds sample coins if missing and creates a `Crypto Trader` profile with `$100,000.00 USD`.

### 4.3 Seeded Leaderboard Demo Accounts
Running `node scripts/initFirestore.js` seeds the following benchmark traders into the global `users` collection for leaderboard competition:

| Trader Name | Email | Seeded Net Worth | Document ID |
| :--- | :--- | :--- | :--- |
| **Satoshi Nakamoto** | `satoshi@bitcoin.org` | `$1,452,000.00` | `demo_user_satoshi` |
| **Vitalik Buterin** | `vitalik@ethereum.org` | `$980,500.00` | `demo_user_vitalik` |
| **Alex Crypto** | `alex@vcrupto.com` | `$215,400.00` | `demo_user_alex` |
| **Sarah Jenkins** | `sarah.j@invest.io` | `$112,000.00` | `demo_user_sarah` |

### 4.4 Admin & Firebase Console Credentials
 Administrative operations in VCrypto (market price synchronization, database seeding, security rule updates, and user management) are performed via the **Firebase Console** and **Admin CLI Scripts** rather than an in-app UI login:

| Admin Portal / Resource | Access URL / Identifier | Authentication Method |
| :--- | :--- | :--- |
| **Firebase Project ID** | `react-native-crypto-exchange` | Google Cloud / Firebase Owner Account |
| **Firebase Console** | [https://console.firebase.google.com/project/react-native-crypto-exchange](https://console.firebase.google.com/project/react-native-crypto-exchange) | Project Owner Email & Password |
| **Expo EAS Dashboard** | `tauseefiqbal` ([Expo Dashboard](https://expo.dev/accounts/tauseefiqbal/projects/VCrypto)) | Expo CLI / Account Credentials |

---

## 5. How to Use App for Regular User

### 5.1 Signing Up or Logging In
1. Launch **VCrypto** on your Android device, iOS simulator, or Web browser.
2. On the **Welcome Screen**, choose one of the following:
   - Tap **`Continue as Guest / Demo`** for instant 1-click access.
   - Tap **`Sign In or Register with Email`** to open the authentication modal:
     - **New Users:** Tap *"Don't have an account? Register"*, enter your **Display Name**, **Email**, and **Password**, and tap **`Sign Up & Get $100,000`**.
     - **Returning Users:** Enter your **Email** and **Password**, then tap **`Sign In`**.

### 5.2 Viewing Your Portfolio & $100,000 Starter Balance
1. Navigate to the **Portfolio** tab from the bottom navigation bar.
2. View your total **Portfolio balance** at the top (initialized at `$100,000.00`).
3. Below the header, inspect all cryptocurrencies you currently hold, including token quantity and current USD value.
4. Tap any held coin in your portfolio to jump directly to its **Coin Details** screen.

### 5.3 Exploring Live Crypto Markets & Charts
1. Tap the **Market** tab in the bottom navigation bar.
2. Browse the live list of cryptocurrencies displaying:
   - Coin logo, name, and ticker symbol (`BTC`, `ETH`, `SOL`, etc.).
   - Current market price in USD.
   - 24-hour percentage change badge (`+` green or `-` red).
3. **Pull down** on the list at any time to sync fresh prices directly from CoinGecko.
4. Tap any cryptocurrency to open the **Coin Details Screen**, where you can inspect:
   - **1 Hour**, **1 Day**, and **7 Days** percentage changes.
   - The interactive **7-Day Price History Graph**.
   - Your current position in that coin (`Amount` and `$ USD Value`).

### 5.4 Buying and Selling Cryptocurrencies
1. From the **Coin Details Screen**, tap the green **`Buy`** button or red **`Sell`** button to open the **Coin Exchange Screen**.
2. Enter the amount you wish to trade:
   - Type in the **Coin amount** box (left) to automatically see the USD cost (right), **OR**
   - Type in the **USD** box (right) to automatically calculate the exact fractional coin amount (up to 6 decimal places).
3. Use quick shortcuts if desired:
   - Tap **`Buy max ($...)`** when buying to spend your entire available USD balance.
   - Tap **`Sell all (... COIN)`** when selling to liquidate your full holding.
4. Tap **`Place Order`** at the bottom.
5. Once confirmed, an **`Order Completed`** alert appears and redirects you back to your updated **Portfolio**.

### 5.5 Competing on the Global Rankings Leaderboard
1. Tap the **Rankings** tab in the bottom navigation bar.
2. View the live leaderboard of top traders sorted by total **Net Worth** in descending order.
3. Pull down on the list to refresh rankings at any time.

### 5.6 Managing Your User Profile
1. Tap the **Profile** tab in the bottom navigation bar.
2. View your display name, registered email address, avatar, and current net worth.
3. Tap **`Sign out`** at the bottom of the screen to securely log out and return to the Welcome Screen.

---

## 6. How to Use App for Admin User

### 6.1 Initializing & Seeding the Firestore Database
To initialize a fresh Firestore database with base cryptocurrencies (`USD`, `BTC`, `ETH`, `SOL`, `BNB`, `ADA`, `DOGE`, `XRP`) and demo leaderboard traders:
```bash
node scripts/initFirestore.js
```
**What this script does:**
1. Authenticates with the Firebase project (`react-native-crypto-exchange`).
2. Populates the `coins` collection with baseline market data and 7-day price arrays.
3. Populates the `users` collection and `portfolioCoins` subcollections with demo leaderboard accounts (`Satoshi Nakamoto`, `Vitalik Buterin`, `Alex Crypto`, `Sarah Jenkins`).

### 6.2 Syncing Live CoinGecko Market Prices
To manually push real-time prices and 7-day sparklines for the top 50 cryptocurrencies from CoinGecko into Cloud Firestore:
```bash
node scripts/syncRealPrices.js
```
**What this script does:**
1. Queries `https://api.coingecko.com/api/v3/coins/markets` for the top 50 coins in USD.
2. Executes an atomic Firestore batch write (`writeBatch`) updating `currentPrice`, `valueChange24H`, `valueChange1D`, `valueChange7D`, and `priceHistory` for every coin.

### 6.3 Auditing Firestore Coin Prices
To verify that all coins in Cloud Firestore have valid numeric prices and inspect the current state of the `coins` collection from the terminal:
```bash
node scripts/checkPrices.js
```

### 6.4 Managing Cloud Functions & Scheduled Jobs
The `functions/src/index.ts` module provides four backend functions for production Firebase Blaze environments:
- `handleUserSignup`: Auth `onCreate` trigger that automatically initializes `users/{uid}` and grants `$100,000 USD`.
- `exchangeCoins`: HTTPS Callable function (`onCall`) that validates and executes atomic buy/sell trades.
- `syncCoinGeckoPrices`: Scheduled cron function (`every 5 minutes`) that syncs CoinGecko prices and recalculates all users' net worths.
- `triggerPriceSync`: On-demand HTTPS Callable wrapper to manually trigger a market sync and leaderboard recalculation.

To build and deploy Cloud Functions:
```bash
cd functions
npm install
npm run build
firebase deploy --only functions
```

### 6.5 Managing Users & Security Rules in Firebase Console
1. **Authentication Providers:**
   - Open **Firebase Console → Authentication → Sign-in method**.
   - Ensure **Email/Password** and **Anonymous** providers are **Enabled**.
2. **Firestore Security Rules (`firestore.rules`):**
   - Deploy updated security rules via CLI:
     ```bash
     firebase deploy --only firestore:rules
     ```
   - To allow client-side CoinGecko price syncing during development, ensure authenticated writes to `/coins/{coinId}` are permitted when Cloud Functions are not used.

---

## 7. Project Architecture & Directory Structure

### 7.1 Application Folder Layout
```text
react-native-crypto_exchange_virtual/
├── assets/                     # App icons, splash screen, fonts, and 3D Saly illustrations
├── config/                     # Secondary Firebase configuration helpers (native & web)
├── functions/                  # Firebase Cloud Functions (TypeScript backend)
│   └── src/index.ts            # Signup trigger, atomic trade callable & 5-min CoinGecko cron
├── scripts/                    # Admin CLI maintenance & database seeding scripts
│   ├── initFirestore.js        # Seeds initial coins & demo leaderboard users
│   ├── syncRealPrices.js       # Fetches & batch-writes live CoinGecko prices to Firestore
│   └── checkPrices.js          # Audits Firestore coins collection from terminal
├── src/
│   ├── components/             # Reusable UI components
│   │   ├── CoinPriceGraph/     # 7-day Bezier line chart component
│   │   ├── MarketCoin/         # Market list item row
│   │   ├── PercentageChange/   # Color-coded +/- percentage text component
│   │   ├── PortfolioCoin/      # User holding row item
│   │   └── UserRankingItem/    # Leaderboard row item
│   ├── constants/              # App-wide colors and layout dimensions
│   ├── firebase/               # Core Firebase client services
│   │   ├── auth.ts             # Login, registration, guest auth & $100k profile initializer
│   │   ├── config.ts           # Firebase SDK initialization & Emulator toggle
│   │   ├── seed.ts             # Fallback sample coin seeder
│   │   └── services.ts         # Firestore realtime listeners, CoinGecko sync & trade engine
│   ├── hooks/                  # Custom hooks (cached resources, color scheme)
│   ├── navigation/             # React Navigation Stack & Bottom Tab navigators
│   ├── screens/                # Application screens
│   │   ├── WelcomeScreen/      # Onboarding, Guest login & Email/Password Auth modal
│   │   ├── HomeScreen/         # Landing overview tab
│   │   ├── PortfolioScreen/    # Realtime net worth & active crypto holdings
│   │   ├── MarketScreen/       # Live cryptocurrency prices & pull-to-refresh
│   │   ├── CoinDetailsScreen/  # Historical price chart, stats & Buy/Sell actions
│   │   ├── CoinExchangeScreen/ # Order execution screen with USD/Coin converter
│   │   ├── RankingsScreen/     # Global top-100 trader leaderboard
│   │   └── ProfileScreen/      # User profile summary & Sign Out
│   └── utils/                  # AppContext and currency formatting utilities
├── .env.example                # Environment variable template
├── app.json                    # Expo configuration (app name, slug, icons, package IDs)
├── eas.json                    # Expo Application Services (EAS) build profiles
├── firebase.json               # Firebase Firestore, Functions & Emulator ports config
├── firestore.rules             # Cloud Firestore security rules
└── package.json                # Dependencies and npm scripts
```

### 7.2 Cloud Firestore Data Model
- **`coins/{coinId}`**
  - Fields: `id`, `cgId`, `name`, `symbol`, `image`, `currentPrice` (number), `valueChange24H` (number), `valueChange1D` (number), `valueChange7D` (number), `priceHistory` (number[]), `priceHistoryString` (JSON string), `updatedAt`.
- **`users/{userId}`**
  - Fields: `id`, `name`, `email`, `image`, `networth` (number), `createdAt`, `updatedAt`.
- **`users/{userId}/portfolioCoins/{coinId}`** *(Subcollection)*
  - Fields: `id`, `coinId`, `userId`, `amount` (number), `updatedAt`.

---

## 8. Local Development Setup & Installation

### 8.1 Prerequisites
- **Node.js** `>= 18.x` and **npm**
- **Expo Go** app installed on your physical iOS/Android device (or Android Studio / Xcode simulator)
- A **Firebase Project** with **Authentication** (Email/Password + Anonymous) and **Cloud Firestore** enabled

### 8.2 Environment Variables Configuration
1. Copy the template environment file at the root of the project:
   ```bash
   cp .env.example .env
   ```
2. Open `.env` and populate your Firebase project credentials from **Firebase Console → Project Settings → General → Your apps**:
   ```env
   EXPO_PUBLIC_FIREBASE_API_KEY=your_api_key
   EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN=your-project-id.firebaseapp.com
   EXPO_PUBLIC_FIREBASE_PROJECT_ID=your-project-id
   EXPO_PUBLIC_FIREBASE_STORAGE_BUCKET=your-project-id.appspot.com
   EXPO_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
   EXPO_PUBLIC_FIREBASE_APP_ID=your_app_id
   EXPO_PUBLIC_USE_FIREBASE_EMULATOR=false
   ```

### 8.3 Running the Application Locally
```bash
# 1. Install dependencies
npm install

# 2. (Optional) Seed Firestore with initial coins and demo leaderboard users
node scripts/initFirestore.js

# 3. Start the Expo development server
npm start

# Run directly on Android, iOS, or Web:
npm run android
npm run ios
npm run web
```

---

## 9. Troubleshooting & FAQ

### 9.1 Common Build & Runtime Issues
- **`auth/admin-restricted-operation` when tapping "Continue as Guest / Demo":**
  - **Cause:** Anonymous Authentication is disabled in your Firebase project.
  - **Fix:** Go to **Firebase Console → Authentication → Sign-in method**, select **Anonymous**, toggle **Enable**, and click **Save** (or use **Sign In or Register with Email**).
- **EAS Build stays in `Build queued...` for 20+ minutes:**
  - **Cause:** Normal shared queue wait time on the Expo EAS Free Tier.
  - **Fix:** You can safely press `Ctrl+C` in the terminal without canceling the build and monitor progress via your Expo dashboard or `eas build:list`.
- **Market screen shows 6 sample coins instead of 50 live coins:**
  - **Cause:** CoinGecko public API rate-limiting (HTTP `429`) or restrictive Firestore write rules on `/coins/{coinId}`.
  - **Fix:** Pull down to refresh on the Market screen after 60 seconds, or run `node scripts/syncRealPrices.js`.

---

## 10. License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

