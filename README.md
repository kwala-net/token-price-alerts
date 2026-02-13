<p align="center">
  <img src="https://img.shields.io/badge/Kwala-Network-6366f1?style=for-the-badge&logo=ethereum" alt="Kwala" />
  <img src="https://img.shields.io/badge/Telegram-Alerts-26a5e4?style=for-the-badge&logo=telegram" alt="Telegram" />
  <img src="https://img.shields.io/badge/Polygon-SOL%20%7C%20ETH-8247e5?style=for-the-badge&logo=polygon" alt="Polygon" />
</p>

<h1 align="center">🔔 Token Price Alerts</h1>
<p align="center">
  <strong>Real-time crypto price monitoring with instant Telegram notifications</strong>
</p>

<p align="center">
  <em>A blockchain automation project built on Kwala that monitors token prices (SOL, ETH) and sends Telegram alerts when they fall below your configured threshold.</em>
</p>

<p align="center">
  <a href="#-overview">Overview</a> •
  <a href="#-features">Features</a> •
  <a href="#-architecture">Architecture</a> •
  <a href="#-installation">Installation</a> •
  <a href="#-configuration">Configuration</a> •
  <a href="#-usage">Usage</a> •
  <a href="#-roadmap">Roadmap</a>
</p>

---

## 📋 Overview

Never miss a price drop again. This project combines **Kwala Network oracles** with automated workflows to monitor token prices in real time and alert you via Telegram the moment they fall below your threshold. Perfect for traders, DeFi users, or anyone who wants to stay informed without constantly checking charts.

**Configure triggers and thresholds via simple YAML—no custom code required.**

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **📊 Oracle-Powered** | Uses Kwala price oracles for accurate, real-time token prices |
| **⚡ Instant Alerts** | Telegram notifications sent immediately when threshold is breached |
| **🎯 Configurable Thresholds** | Set your own price targets (e.g., alert when SOL drops below $195) |
| **🔄 Recurring Checks** | Continuous monitoring with oracle-based price polling |
| **📱 Zero Manual Checking** | Fully automated—set it and forget it |
| **🛡️ Reliable Delivery** | Built-in retry mechanism (up to 5 attempts) for notification delivery |
| **📝 YAML Configuration** | No coding required—configure everything via YAML files |

---

## 🏗️ Architecture

```
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│   Kwala Oracle      │────▶│   Price Workflow     │────▶│   Telegram Bot       │
│   (Price Feed)      │     │   (YAML Config)      │     │   (Your Phone)       │
└─────────────────────┘     └─────────────────────┘     └─────────────────────┘
         │                            │
         │  Price < Threshold         │  Trigger
         └────────────────────────────┘
```

### Components

| Component | Purpose |
|-----------|---------|
| **Price Oracle** | Kwala fetches live token prices from trusted sources |
| **Workflow (YAML)** | Defines trigger conditions, threshold, and notification actions |
| **Telegram** | Delivers instant alerts to your device |

---

## 🛠️ Prerequisites

Before you begin, ensure you have:

- [ ] A **Kwala account**
- [ ] A **Telegram bot** (create via [@BotFather](https://t.me/BotFather))
- [ ] Your **Telegram Chat ID** (use [@userinfobot](https://t.me/userinfobot))
- [ ] The **contract address** for the price oracle (provided by Kwala)
- [ ] A **target price threshold** (e.g., $195 for SOL)

---

## 📦 Installation

### 1. Clone the repository

```bash
git clone https://github.com/kwala-net/token-price-alerts.git
cd token-price-alerts
```

### 2. Configure your Telegram bot

1. Create a bot using [@BotFather](https://t.me/BotFather)
2. Copy your **bot token**
3. Get your **Chat ID** using [@userinfobot](https://t.me/userinfobot)

### 3. Deploy to Kwala

1. Log in to your [Kwala Dashboard](https://kwala.network)
2. Create a new workflow
3. Upload `solana-price-tracker.yaml` (or your configured YAML)
4. Activate the workflow

---

## ⚙️ Configuration

### Workflow YAML Structure

Edit `solana-price-tracker.yaml` with your values:

```yaml
Name: sol_pricedrop1
Trigger:
  TriggerSourceContract: YOUR_ORACLE_CONTRACT_ADDRESS
  TriggerChainID: 1
  TriggerPrice: 195.0                    # Alert when price drops below this
  RecurringSourceContract: YOUR_ORACLE_CONTRACT_ADDRESS
  RecurringChainID: 1
  RecurringPrice: 195.0
  RepeatEvery: oracle_price              # Oracle-driven price checks
  ExecuteAfter: oracle_price
  ExpiresIn: 1758618000                  # Unix timestamp for expiry

Actions:
  - Name: telegram_notifier1
    Type: post
    APIEndpoint: https://api.telegram.org/bot<YOUR_BOT_TOKEN>/sendMessage
    APIPayload:
      chat_id: "<YOUR_CHAT_ID>"
      text: "Sol price has dropped to $195.0"
    RetriesUntilSuccess: 5
```

### Configuration Checklist

| Field | Description | Example |
|-------|-------------|---------|
| `TriggerSourceContract` | Kwala oracle/contract address | `0xD31a59c85aE9D8edEFeC411D448f90841571b89c` |
| `TriggerPrice` / `RecurringPrice` | Price threshold (USD) | `195.0` |
| `APIEndpoint` | Telegram API with your bot token | `https://api.telegram.org/bot123:ABC/sendMessage` |
| `chat_id` | Your Telegram Chat ID | `123456789` |
| `text` | Custom alert message | `"Sol price has dropped to $195.0"` |

---

## 🚀 Usage

Once deployed and configured, the system runs **automatically**:

1. **Continuous Monitoring** — Kwala oracle checks token prices in real time
2. **Threshold Detection** — When price falls below your configured level, the workflow triggers
3. **Instant Notification** — You receive a Telegram message: *"Sol price has dropped to $195.0"*

### Supported Tokens

- **SOL** (Solana)
- **ETH** (Ethereum)
- *Extendable to other tokens supported by Kwala oracles*

---

## 📁 Project Structure

```
token-price-alerts/
├── solana-price-tracker.yaml    # SOL price alert workflow config
├── README.md                    # This file
└── .gitignore
```

---

## 🎨 Customization

### Change the Price Threshold

Edit the YAML file:

```yaml
TriggerPrice: 180.0        # Alert when SOL drops below $180
RecurringPrice: 180.0
```

### Modify the Notification Message

```yaml
APIPayload:
  chat_id: "<YOUR_CHAT_ID>"
  text: "⚠️ SOL dipped below $195! Consider buying the dip."
```

### Adjust Expiry

Set `ExpiresIn` to a Unix timestamp for when the workflow should stop:

```bash
# Get Unix timestamp (e.g., 30 days from now)
date -v+30d +%s
```

---

## 🛡️ Security Considerations

- **Never commit** bot tokens or Chat IDs to version control
- Use **environment variables** or Kwala's secrets management for production
- Add `*.yaml` with sensitive data to `.gitignore` if storing tokens locally
- Monitor Kwala workflow execution logs for anomalies

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 🙏 Acknowledgments

- Built with [Kwala Network](https://kwala.network)
- Price data via Kwala oracles
- Notifications via [Telegram Bot API](https://core.telegram.org/bots/api)

---

## 📚 Resources

| Resource | Link |
|----------|------|
| Kwala Documentation | [kwala.network/docs](https://kwala.network/docs) |
| Kwala Use Case Guide | [Oracle Price Alert Notifications](https://kwala.network/docs/use-cases/oracle-price-alert-notifications) |
| Telegram Bot API | [core.telegram.org/bots](https://core.telegram.org/bots/api) |

---

## 💬 Support

- **Issues?** [Open an issue](https://github.com/kwala-net/token-price-alerts/issues) on GitHub
- **Docs** — Check [Kwala Documentation](https://kwala.network/docs)
- **Community** — Join the Kwala community

---

## 🗺️ Roadmap

- [ ] Support for multiple token pairs (SOL, ETH, BTC, etc.)
- [ ] Customizable threshold per token
- [ ] Price *rise* alerts (notify when price goes *above* threshold)
- [ ] Discord / Slack notification options
- [ ] Web dashboard for monitoring multiple alerts
- [ ] Email notification support
- [ ] Multi-chain support (Ethereum, Polygon, Arbitrum, etc.)

---

<p align="center">
  <strong>Made with ❤️ using Kwala Network</strong>
</p>

<p align="center">
  <em>⭐ Star this repo if you find it useful!</em>
</p>
