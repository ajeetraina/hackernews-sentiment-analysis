# 🐳 Hacker News Sentiment Analysis for Docker

A multi-agent AI system powered by **Docker cagent** that fetches Docker-related discussions from Hacker News and classifies comments into positive and negative sentiment.

![Docker](https://img.shields.io/badge/Docker-cagent-blue?logo=docker)
![MCP](https://img.shields.io/badge/MCP-Hacker%20News-orange)
![License](https://img.shields.io/badge/License-MIT-green)

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      Docker cagent                               │
│                  (Multi-Agent Orchestrator)                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐    ┌───────────────────┐    ┌──────────────┐  │
│  │  HN Fetcher  │───▶│ Sentiment Analyzer │───▶│   Report     │  │
│  │    Agent     │    │      Agent         │    │  Generator   │  │
│  └──────┬───────┘    └───────────────────┘    └──────┬───────┘  │
│         │                                            │          │
│         ▼                                            ▼          │
│  ┌──────────────┐                           ┌──────────────┐    │
│  │mcp-hackernews│                           │   Output     │    │
│  │  MCP Server  │                           │    JSON      │    │
│  └──────────────┘                           └──────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 🚀 Quick Start

### Prerequisites

- Docker Desktop 4.49+ (includes cagent)
- One of: OpenAI API Key, GitHub Token, or Docker Model Runner

### Choose Your Model Provider

| Provider | Config File | Cost | Setup |
|----------|-------------|------|-------|
| **OpenAI** | `cagent.yaml` | Pay per token | `export OPENAI_API_KEY=sk-...` |
| **GitHub Models** | `cagent-github-models.yaml` | Free tier | `export GITHUB_TOKEN=ghp_...` |
| **Local (DMR)** | `cagent-local.yaml` | **$0** | Just Docker! |
| **Multi-Agent** | `cagent-multi-agent.yaml` | Varies | Based on models used |

### Run the Agent

```bash
# Clone the repo
git clone https://github.com/ajeetraina/hackernews-sentiment-analysis.git
cd hackernews-sentiment-analysis

# Option 1: OpenAI (best quality)
export OPENAI_API_KEY=your_key_here
cagent run cagent.yaml

# Option 2: GitHub Models (free tier)
export GITHUB_TOKEN=your_github_pat
cagent run cagent-github-models.yaml

# Option 3: Local models (free, private)
docker model pull ai/llama3.3:70B-Q4_0
cagent run cagent-local.yaml

# Option 4: Multi-agent workflow
cagent run cagent-multi-agent.yaml
```

## 📊 What It Does

1. **Fetches** Docker-related stories from Hacker News
2. **Collects** all comments from high-engagement discussions
3. **Analyzes** sentiment (Positive/Negative/Neutral)
4. **Extracts** key themes from the community
5. **Generates** a structured JSON report

## 🎯 Sentiment Classification

### Positive Themes
- Ease of use / Developer experience
- Portability / Cross-platform
- DevOps workflow improvement
- Community and ecosystem
- Reproducibility

### Negative Themes
- Performance overhead
- Resource consumption
- Docker Desktop pricing
- Complexity for simple use cases
- Security concerns
- macOS/Windows issues

## 📁 Project Structure

```
.
├── cagent.yaml                 # OpenAI version
├── cagent-github-models.yaml   # GitHub Models version (free)
├── cagent-local.yaml           # Docker Model Runner (local, free)
├── cagent-multi-agent.yaml     # Multi-agent with sub-agents
├── docker-compose.yaml         # Run with web dashboard
├── templates/
│   └── index.html              # Web dashboard UI
└── output/
    ├── dashboard.json          # Dashboard data
    ├── positive_comments.json  # Positive comments
    └── negative_comments.json  # Negative comments
```

## 🔧 Configuration Options

### Change Search Queries

Edit the `instruction` in any cagent YAML file:

```yaml
instruction: |
  Search Hacker News for Docker-related stories using these queries:
    - "Docker"
    - "Docker security"      # Add custom queries
    - "Docker alternatives"
    - "Podman vs Docker"
```

### Switch Models

```bash
# Override model at runtime
cagent run cagent.yaml --model openai/gpt-4o-mini
cagent run cagent.yaml --model anthropic/claude-sonnet-4-5
cagent run cagent.yaml --model dmr/ai/qwen3:8B-Q4_0
```

## 🌐 Web Dashboard (Optional)

Run with Docker Compose to get a web UI:

```bash
docker compose up -d
open http://localhost:8080
```

## 📈 Example Output

```json
{
  "summary": {
    "total": 156,
    "positive": 67,
    "negative": 52,
    "neutral": 37,
    "sentiment_score": 0.18
  },
  "top_positive_themes": [
    {"theme": "Ease of Use", "count": 24},
    {"theme": "Portability", "count": 19}
  ],
  "top_negative_themes": [
    {"theme": "Performance Overhead", "count": 18},
    {"theme": "Docker Desktop Pricing", "count": 11}
  ]
}
```

## 🔗 Related Resources

- [Docker cagent Documentation](https://docs.docker.com/ai/cagent/)
- [Docker MCP Catalog](https://hub.docker.com/u/mcp)
- [GitHub Models](https://github.com/marketplace/models)
- [Configure cagent with GitHub Models](https://www.docker.com/blog/configure-cagent-github-models/)

## 🤝 Contributing

Contributions welcome! Feel free to:
- Add new analysis features
- Improve sentiment classification
- Add more visualization options
- Create integrations (Slack, Discord, etc.)

## 📝 License

MIT License - Feel free to use and modify!

---

Built with ❤️ by [Ajeet Singh Raina](https://github.com/ajeetraina) using Docker cagent and mcp-hackernews
