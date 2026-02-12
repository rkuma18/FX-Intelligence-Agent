# FX Intelligence Agent

An AI-powered multi-currency conversion assistant using **GPT-4 function calling** for intelligent foreign exchange queries. Supports **170+ fiat currencies** and **14 cryptocurrencies** with real-time rates from ExchangeRate-API and CoinGecko. Built with LangChain and deployed on Hugging Face Spaces.

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![LangChain](https://img.shields.io/badge/LangChain-Latest-green.svg)](https://github.com/langchain-ai/langchain)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-purple.svg)](https://openai.com/)
[![Streamlit](https://img.shields.io/badge/Streamlit-Latest-red.svg)](https://streamlit.io/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Project Overview

FX Intelligence Agent transforms currency conversion into an intelligent, conversational experience. Unlike traditional converters, this AI agent understands natural language queries and autonomously selects the right tools to answer your foreign exchange questions.

### Why FX Intelligence Agent?

- **Natural Language Interface**: "How much is 500 euros in dollars?" instead of dropdown menus
- **Multi-Asset Support**: Fiat currencies (USD, EUR, GBP) + Cryptocurrencies (BTC, ETH, SOL)
- **Intelligent Tool Selection**: GPT-4 autonomously chooses conversion functions via function calling
- **Real-Time Rates**: Live exchange rates updated every request
- **Production Deployment**: Dockerized and deployed on Hugging Face Spaces

---

## Key Features

✅ **AI-Powered Conversations**: GPT-4 understands context and handles follow-up questions
✅ **170+ Fiat Currencies**: USD, EUR, GBP, INR, JPY, and 165+ more via ExchangeRate-API
✅ **14 Cryptocurrencies**: BTC, ETH, LTC, BCH, ADA, DOT, LINK, XRP, USDT, USDC, BNB, SOL, MATIC, AVAX
✅ **4 LLM Tools**: Conversion, exchange rates, crypto prices, fiat-to-Bitcoin
✅ **Function Calling**: LLM autonomously decides which tool(s) to call
✅ **Streamlit Chat UI**: Clean, modern interface with custom styling
✅ **Lazy Initialization**: Efficient API key management and LLM initialization
✅ **Docker Deployment**: Container-ready with Hugging Face Spaces support

---

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│           FX Intelligence Agent Architecture            │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  User Query: "Convert 500 USD to EUR and BTC"          │
│         │                                                │
│         ▼                                                │
│  ┌──────────────────┐                                   │
│  │   GPT-4 (LLM)    │                                   │
│  │  Function Calling│                                   │
│  └────────┬─────────┘                                   │
│           │                                              │
│           │ Analyzes query, decides:                    │
│           │ • Need convert(500, USD, EUR)               │
│           │ • Need convert_fiat_to_btc(500, USD)        │
│           │                                              │
│           ▼                                              │
│  ┌─────────────────────────────────────┐               │
│  │         Tool Execution              │               │
│  ├─────────────────────────────────────┤               │
│  │                                     │               │
│  │  Tool 1: convert()                 │               │
│  │    ├─> ExchangeRate-API            │               │
│  │    └─> Returns: 500 USD = 462.50 EUR               │
│  │                                     │               │
│  │  Tool 2: convert_fiat_to_btc()     │               │
│  │    ├─> ExchangeRate-API (USD→USD)  │               │
│  │    ├─> CoinGecko API (BTC price)   │               │
│  │    └─> Returns: 500 USD = 0.01234 BTC              │
│  │                                     │               │
│  └──────────────┬──────────────────────┘               │
│                 │                                        │
│                 ▼                                        │
│  ┌─────────────────────────────────────┐               │
│  │   GPT-4 Synthesizes Response        │               │
│  │   "500 USD equals 462.50 EUR        │               │
│  │    and 0.01234 BTC"                 │               │
│  └─────────────────────────────────────┘               │
│                                                          │
└─────────────────────────────────────────────────────────┘

Tool Descriptions:
1. convert(amount, from_currency, to_currency)
2. get_conversion_factor(from_currency, to_currency)
3. get_crypto_rate(base_currency, target_currency)
4. convert_fiat_to_btc(amount, fiat_currency)
```

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| **LLM** | OpenAI GPT-4 with function calling |
| **Agent Framework** | LangChain (tool binding) |
| **Fiat Currency API** | ExchangeRate-API (v6) |
| **Crypto API** | CoinGecko (free tier, no key required) |
| **Frontend** | Streamlit with custom CSS |
| **Deployment** | Docker + Hugging Face Spaces |
| **Language** | Python 3.9+ |

---

## Installation

### Prerequisites

- Python 3.9 or higher
- OpenAI API key (for GPT-4)
- ExchangeRate-API key (free tier: 1,500 requests/month)

### Setup Steps

```bash
# Clone the repository
git clone https://github.com/rkuma18/FX-Intelligence-Agent.git
cd FX-Intelligence-Agent

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
cp .env.example .env
# Edit .env and add your API keys
```

### `requirements.txt`

```
langchain-openai
langchain-core
langchain-community
langchain
requests
python-dotenv
streamlit
typing
```

### Environment Variables

Create a `.env` file:

```env
# Required
OPENAI_API_KEY=your_openai_api_key_here
EXCHANGE_RATE_API_KEY=your_exchangerate_api_key_here

# Optional - for enhanced crypto features (if needed in future)
# COINGECKO_API_KEY=your_coingecko_key_here
```

**Get API Keys:**
- OpenAI: https://platform.openai.com/api-keys
- ExchangeRate-API: https://www.exchangerate-api.com/ (free tier available)

---

## Usage

### 1. Run the Streamlit App

```bash
streamlit run app.py

# App opens at: http://localhost:8501
```

### 2. Enter API Keys in UI

The app includes a secure sidebar for entering API keys:
- OpenAI API Key (for GPT-4)
- ExchangeRate-API Key (for currency data)

**Note**: Keys are stored in session state only (not saved to disk)

### 3. Start Chatting!

**Example Queries:**

```
You: How much is 500 USD in EUR?
Agent: 500 USD equals 462.50 EUR at the current exchange rate.

You: What's the Bitcoin price in dollars?
Agent: 1 BTC = 43,521.30 USD

You: Convert 1000 GBP to USD, EUR, and JPY
Agent: 1000 GBP converts to:
       - 1,270.50 USD
       - 1,175.30 EUR
       - 189,432 JPY

You: How much Bitcoin can I buy with 5000 euros?
Agent: 5000 EUR equals 0.12345 BTC at current rates.
```

---

## Agent Tools

The GPT-4 agent has access to 4 specialized tools:

### 1. `convert(amount, from_currency, to_currency)`

**Purpose**: Convert a specific amount between two currencies

**Example Usage**:
```python
# User: "Convert 500 USD to EUR"
# Agent calls: convert(500, "USD", "EUR")
# Returns: "500 USD = 462.50 EUR"
```

**Supported**: 170+ fiat currencies (USD, EUR, GBP, INR, JPY, AUD, CAD, etc.)

---

### 2. `get_conversion_factor(from_currency, to_currency)`

**Purpose**: Get just the exchange rate (no amount conversion)

**Example Usage**:
```python
# User: "What's the exchange rate from USD to GBP?"
# Agent calls: get_conversion_factor("USD", "GBP")
# Returns: "1 USD = 0.7850 GBP"
```

---

### 3. `get_crypto_rate(base_currency, target_currency)`

**Purpose**: Get cryptocurrency exchange rates

**Example Usage**:
```python
# User: "What's the Ethereum price in dollars?"
# Agent calls: get_crypto_rate("ETH", "USD")
# Returns: "1 ETH = 2,345.67 USD"
```

**Supported Cryptocurrencies** (14 total):
- Bitcoin (BTC)
- Ethereum (ETH)
- Litecoin (LTC)
- Bitcoin Cash (BCH)
- Cardano (ADA)
- Polkadot (DOT)
- Chainlink (LINK)
- Ripple (XRP)
- Tether (USDT)
- USD Coin (USDC)
- Binance Coin (BNB)
- Solana (SOL)
- Polygon (MATIC)
- Avalanche (AVAX)

---

### 4. `convert_fiat_to_btc(amount, fiat_currency)`

**Purpose**: Convert any fiat currency directly to Bitcoin

**Example Usage**:
```python
# User: "How much BTC can I buy with 10,000 Indian rupees?"
# Agent calls: convert_fiat_to_btc(10000, "INR")
# Process: INR → USD → BTC
# Returns: "10000 INR = 0.00283 BTC"
```

**How It Works**:
1. Convert fiat to USD (if not already USD)
2. Get current BTC price from CoinGecko
3. Calculate BTC amount with 8 decimal precision

---

## Example Conversations

### Travel Planning
```
You: I'm traveling to Japan. How much is 500 USD in yen?
Agent: 500 USD equals 74,910 JPY at the current exchange rate (1 USD = 149.82 JPY).

You: What about in euros?
Agent: 500 USD equals 462.50 EUR.
```

### Crypto Trading
```
You: What's the Bitcoin price?
Agent: 1 BTC = 43,521.30 USD

You: How much Bitcoin can I get for 5000 dollars?
Agent: 5000 USD equals 0.11489 BTC.

You: What about Ethereum?
Agent: 1 ETH = 2,345.67 USD. For 5000 USD, you could get 2.1317 ETH.
```

### Multi-Currency Comparison
```
You: I have 1000 euros. Show me the value in USD, GBP, and JPY.
Agent: 1000 EUR converts to:
       - 1,081.00 USD (1 EUR = 1.081 USD)
       - 934.50 GBP (1 EUR = 0.9345 GBP)
       - 161,500 JPY (1 EUR = 161.5 JPY)
```

### Business Analytics
```
You: Our product costs 99 USD. What should we charge in euros, pounds, and rupees?
Agent: For a 99 USD product:
       - Europe: 91.58 EUR
       - UK: 77.72 GBP
       - India: 8,267 INR
```

---

## Deployment

### Docker Deployment

```dockerfile
# Dockerfile (already included in repo)
FROM python:3.9-slim

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application
COPY . .

# Expose Streamlit port
EXPOSE 8501

# Run Streamlit
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

```bash
# Build and run locally
docker build -t fx-intelligence-agent .
docker run -p 8501:8501 \
  -e OPENAI_API_KEY=your_key \
  -e EXCHANGE_RATE_API_KEY=your_key \
  fx-intelligence-agent
```

### Hugging Face Spaces Deployment

This project is configured for Hugging Face Spaces deployment:

1. **Create a Space** on Hugging Face (select Streamlit SDK)

2. **Push to Space**:
```bash
git remote add space https://huggingface.co/spaces/YOUR_USERNAME/fx-intelligence-agent
git push space main
```

3. **Set Secrets** in Space Settings:
   - `OPENAI_API_KEY`
   - `EXCHANGE_RATE_API_KEY`

4. **Space automatically deploys** from the Dockerfile

**Live Demo** (if deployed): `https://huggingface.co/spaces/rkuma18/fx-intelligence-agent`

---

## API Rate Limits

### ExchangeRate-API (Free Tier)
- **Requests**: 1,500/month
- **Updates**: Daily
- **Upgrade**: $9/month for 100,000 requests

### CoinGecko (Free Tier)
- **Requests**: 30 calls/minute
- **No API Key Required**
- **Pro**: $129/month for higher limits

### OpenAI GPT-4
- **Cost**: ~$0.03 per query (GPT-4 function calling)
- **Alternative**: Use GPT-3.5-turbo for 10x lower cost (change in main.py)

---

## Advanced Features

### 1. Lazy LLM Initialization

```python
# From main.py
class LazyLLM:
    """Only initializes GPT-4 when API keys are available"""
    def _ensure_llm(self):
        llm = get_openai_client()
        self._llm = llm.bind_tools(tools)
```

**Benefits**:
- No crashes if API keys missing at startup
- Streamlit UI can request keys dynamically
- Efficient resource usage

### 2. Error Handling

All tools include robust error handling:
```python
try:
    # API call
    response = requests.get(url, timeout=10)
    # Process
except Exception as e:
    return f"Error: {str(e)}"
```

### 3. Custom Streamlit UI

Features include:
- API key input sidebar
- Chat message styling (user vs assistant)
- Privacy notices
- Warning boxes for API limits

---

## Performance Optimization

### Reduce Costs

```python
# Switch from GPT-4 to GPT-3.5-turbo (10x cheaper)
# In main.py, line 13:
llm = ChatOpenAI(api_key=api_key, model="gpt-3.5-turbo")
```

### Cache Exchange Rates

```python
# Add caching for frequently requested pairs
from functools import lru_cache

@lru_cache(maxsize=100)
def get_cached_rate(from_curr, to_curr):
    return get_conversion_factor(from_curr, to_curr)
```

### Batch Requests

```python
# For multiple conversions, batch API calls
# Future enhancement: Multi-currency tool
```

---

## Testing

### Manual Testing

```bash
# Test individual tools
python -c "from main import convert; print(convert(100, 'USD', 'EUR'))"

# Test crypto rates
python -c "from main import get_crypto_rate; print(get_crypto_rate('BTC', 'USD'))"
```

### Unit Tests (Coming Soon)

```python
# tests/test_tools.py
import pytest
from main import convert, get_conversion_factor

def test_convert():
    result = convert(100, "USD", "EUR")
    assert "EUR" in result
    assert "USD" in result
```

---

## Future Enhancements

- [ ] **Historical Rates**: Query exchange rates for specific dates
- [ ] **Rate Alerts**: Notify when target rate is reached
- [ ] **Trend Analysis**: Show 7-day/30-day rate charts
- [ ] **More Cryptos**: Add top 50 cryptocurrencies
- [ ] **Portfolio Tracker**: Track multi-currency holdings
- [ ] **Batch Conversions**: Upload CSV for bulk conversions
- [ ] **Voice Interface**: Speech-to-text for mobile users
- [ ] **WhatsApp/Telegram Bot**: Deploy as messaging bot
- [ ] **Rate Prediction**: ML model for FX forecasting
- [ ] **Multi-Language**: Support queries in Spanish, French, Hindi

---

## Security & Privacy

### API Key Handling
- Keys entered via UI are stored in session state only
- Not saved to disk or logged
- Cleared when browser session ends

### Best Practices
- Never commit `.env` file to Git
- Use environment variables in production
- Rotate API keys regularly
- Monitor API usage for anomalies

---

## Project Structure

```
FX-Intelligence-Agent/
│
├── main.py                   # Core agent logic & tools
├── app.py                    # Streamlit chat interface
│
├── requirements.txt          # Python dependencies
├── Dockerfile               # Container configuration
├── .huggingface.yaml        # HF Spaces metadata
├── .env                     # API keys (not in git)
│
└── README.md                # This file
```

---

## Contributing

Contributions are welcome! Areas for improvement:

- Additional cryptocurrencies (Doge, Shiba, etc.)
- Historical rate queries
- Rate prediction features
- Alternative LLM support (Claude, Gemini)
- Mobile app version

**How to Contribute:**
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-crypto`)
3. Commit your changes (`git commit -m 'Add Dogecoin support'`)
4. Push to the branch (`git push origin feature/new-crypto`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Author

**Roushan Kumar**

- GitHub: [@rkuma18](https://github.com/rkuma18)
- LinkedIn: [Connect with me](https://www.linkedin.com/in/rk0718/)
- Email: kumarroushan.18@gmail.com

---

## Acknowledgments

- [LangChain](https://github.com/langchain-ai/langchain) for the agent framework
- [OpenAI](https://openai.com/) for GPT-4 function calling
- [ExchangeRate-API](https://www.exchangerate-api.com/) for fiat currency data
- [CoinGecko](https://www.coingecko.com/) for cryptocurrency data
- [Streamlit](https://streamlit.io/) for the UI framework

---

## References

1. [OpenAI Function Calling Documentation](https://platform.openai.com/docs/guides/function-calling)
2. [LangChain Tool Use Guide](https://python.langchain.com/docs/modules/agents/tools/)
3. [ExchangeRate-API Documentation](https://www.exchangerate-api.com/docs/)
4. [CoinGecko API Documentation](https://www.coingecko.com/en/api/documentation)

---

## Support

For questions or issues:
- Open an issue on [GitHub Issues](https://github.com/rkuma18/FX-Intelligence-Agent/issues)
- Email: kumarroushan.18@gmail.com

---

## Use Cases

### For Travelers
- Quick currency conversions before trips
- Compare costs across destinations
- Budget planning in foreign currencies

### For Traders
- Real-time crypto prices
- Multi-currency portfolio valuations
- Quick arbitrage calculations

### For Businesses
- International pricing strategies
- Invoice conversion for clients
- Multi-currency revenue tracking

### For Developers
- Example of LLM function calling
- Template for building AI agents
- LangChain tool integration reference

---

**⭐ If FX Intelligence Agent helps your currency conversion needs, please give it a star on GitHub!**
