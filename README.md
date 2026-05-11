<p align="center">
  <img src="assets/banner.png" alt="ResearchMind Banner" width="100%"/>
</p>

<h1 align="center">🔬 ResearchMind</h1>

<p align="center">
  <strong>A Multi-Agent AI Research System Powered by LangChain</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/LangChain-0.2+-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white" alt="LangChain"/>
  <img src="https://img.shields.io/badge/OpenAI-GPT--4o--mini-412991?style=for-the-badge&logo=openai&logoColor=white" alt="OpenAI"/>
  <img src="https://img.shields.io/badge/Streamlit-1.30+-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white" alt="Streamlit"/>
  <img src="https://img.shields.io/badge/Tavily-Search_API-FF8C32?style=for-the-badge" alt="Tavily"/>
</p>

<p align="center">
  Four specialized AI agents collaborate in a pipeline — <b>searching</b>, <b>scraping</b>, <b>writing</b>, and <b>critiquing</b> — to deliver polished research reports on any topic in minutes.
</p>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Agent Pipeline](#-agent-pipeline)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Usage](#-usage)
- [Example Output](#-example-output)
- [Key Design Decisions](#-key-design-decisions)
- [Future Enhancements](#-future-enhancements)
- [License](#-license)

---

## 🧠 Overview

**ResearchMind** is a multi-agent AI system that automates the research report generation process. Instead of relying on a single monolithic LLM call, the system decomposes the research task into four distinct stages, each handled by a specialized agent or chain. This separation-of-concerns approach produces higher-quality outputs, mirrors a real research workflow, and demonstrates core concepts in **agentic AI design**.

### Key Highlights

- 🔗 **Multi-Agent Orchestration** — Four agents collaborate in a sequential pipeline, each with a clear responsibility.
- 🛠️ **Tool-Augmented Agents** — Search and Reader agents are equipped with external tools (web search, URL scraping) for real-time data gathering.
- ✍️ **Prompt-Engineered Chains** — Writer and Critic use carefully crafted prompt templates for structured, professional output.
- 🎨 **Premium UI** — A custom-styled Streamlit interface with real-time pipeline progress tracking, dark theme, and downloadable reports.

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      User Input (Topic)                     │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────────┐
│  AGENT 1 — Search Agent                                      │
│  ┌──────────────┐                                            │
│  │  Tavily API  │  Searches the web for 5 relevant results   │
│  └──────────────┘  Returns titles, URLs, and snippets        │
└──────────────────────────┬───────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────────┐
│  AGENT 2 — Reader Agent                                      │
│  ┌──────────────────┐                                        │
│  │  BeautifulSoup4  │  Scrapes the most relevant URL         │
│  └──────────────────┘  Extracts clean text content (3000ch)  │
└──────────────────────────┬───────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────────┐
│  CHAIN 3 — Writer Chain                                      │
│  Prompt Template → GPT-4o-mini → StrOutputParser             │
│  Produces a structured report: Intro, Findings, Conclusion   │
└──────────────────────────┬───────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────────┐
│  CHAIN 4 — Critic Chain                                      │
│  Prompt Template → GPT-4o-mini → StrOutputParser             │
│  Scores the report (X/10), lists strengths and improvements  │
└──────────────────────────┬───────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────────┐
│              📝 Final Report + 🧐 Critic Feedback            │
│              (Displayed in UI & downloadable as .md)         │
└──────────────────────────────────────────────────────────────┘
```

---

## 🤖 Agent Pipeline

| Stage | Component | Type | Tool / Technique | Purpose |
|:-----:|-----------|------|-----------------|---------|
| 01 | **Search Agent** | LangChain Agent | Tavily Web Search API | Queries the web and retrieves the top 5 relevant results with titles, URLs, and snippets |
| 02 | **Reader Agent** | LangChain Agent | BeautifulSoup4 URL Scraper | Selects the best URL from search results and scrapes clean, readable text content |
| 03 | **Writer Chain** | LangChain LCEL Chain | Prompt → LLM → Parser | Synthesizes search results + scraped content into a structured research report |
| 04 | **Critic Chain** | LangChain LCEL Chain | Prompt → LLM → Parser | Reviews the report and provides a score (X/10), strengths, weaknesses, and a verdict |

---

## 🛠 Tech Stack

| Category | Technology | Purpose |
|----------|-----------|---------|
| **LLM Framework** | [LangChain](https://python.langchain.com/) | Agent creation, prompt templates, LCEL chains, tool integration |
| **Language Model** | [OpenAI GPT-4o-mini](https://platform.openai.com/) | Core reasoning engine for all agents and chains |
| **Web Search** | [Tavily API](https://tavily.com/) | Real-time web search optimized for AI agents |
| **Web Scraping** | [BeautifulSoup4](https://www.crummy.com/software/BeautifulSoup/) + Requests | HTML parsing and clean text extraction from URLs |
| **Frontend** | [Streamlit](https://streamlit.io/) | Interactive web UI with custom CSS theming |
| **Environment** | python-dotenv | Secure API key management via `.env` file |
| **Language** | Python 3.10+ | Core programming language |

---

## 📁 Project Structure

```
Multi-agent-research-system/
│
├── app.py              # Streamlit web UI — custom-styled frontend with
│                       #   real-time pipeline progress, result display,
│                       #   and .md report download
│
├── agents.py           # Agent & chain definitions
│                       #   - build_search_agent() → LangChain agent w/ Tavily tool
│                       #   - build_reader_agent() → LangChain agent w/ scraper tool
│                       #   - writer_chain → LCEL prompt | LLM | parser
│                       #   - critic_chain → LCEL prompt | LLM | parser
│
├── tools.py            # Custom LangChain tools
│                       #   - web_search() → Tavily API wrapper
│                       #   - scrape_url() → BeautifulSoup4 URL scraper
│
├── pipeline.py         # CLI pipeline runner — runs all 4 stages sequentially
│                       #   and prints results to the terminal
│
├── requirements.txt    # Python dependencies
├── .env                # API keys (OPENAI_API_KEY, TAVILY_API_KEY)
└── .gitignore          # Excludes .env from version control
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10 or higher
- An [OpenAI API key](https://platform.openai.com/api-keys)
- A [Tavily API key](https://tavily.com/) (free tier available)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/Multi-agent-research-system.git
cd Multi-agent-research-system

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # macOS / Linux
venv\Scripts\activate           # Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Set up environment variables
#    Create a .env file in the project root:
echo "OPENAI_API_KEY=your_openai_api_key_here" > .env
echo "TAVILY_API_KEY=your_tavily_api_key_here" >> .env
```

---

## 💡 Usage

### Option 1 — Web UI (Streamlit)

```bash
streamlit run app.py
```

This launches a polished dark-themed web interface where you can:

1. Enter any research topic
2. Watch the 4-stage pipeline execute in real-time with status indicators
3. View the final report rendered as formatted Markdown
4. Read the critic's feedback and score
5. Download the complete report as a `.md` file

### Option 2 — CLI Pipeline

```bash
python pipeline.py
```

This runs the same 4-agent pipeline directly in the terminal with step-by-step output logging.

---

## 📝 Example Output

**Input Topic:** `"LLM agents 2025"`

### 🔍 Search Agent
> Finds the top 5 web results with titles, URLs, and content snippets from Tavily.

### 📄 Reader Agent
> Automatically selects the most relevant URL and extracts up to 3,000 characters of clean text.

### ✍️ Writer Chain — Research Report

```markdown
# Research Report: LLM Agents in 2025

## Introduction
Large Language Model (LLM) agents have emerged as one of the most
transformative developments in artificial intelligence...

## Key Findings
1. **Agentic Frameworks Are Maturing** — LangChain, CrewAI, and AutoGen...
2. **Tool-Use Is Becoming Standard** — Modern agents integrate search, code...
3. **Multi-Agent Systems Show Promise** — Collaborative agent architectures...

## Conclusion
The trajectory of LLM agents in 2025 suggests a shift from...

## Sources
- https://example.com/llm-agents-2025
- https://example.com/ai-research-trends
```

### 🧐 Critic Chain — Feedback

```
Score: 8/10

Strengths:
- Well-structured with clear section divisions
- Cites specific technologies and frameworks
- Professional and balanced tone

Areas to Improve:
- Could include more quantitative data
- Missing discussion of limitations and risks

One line verdict:
A solid, well-researched report that would benefit from deeper statistical analysis.
```

---

## 🧩 Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| **Agent vs. Chain separation** | Search and Reader need *tool access* (web search, scraping), so they use full LangChain agents. Writer and Critic are pure text-in → text-out tasks, so lightweight LCEL chains are more efficient. |
| **Sequential pipeline** | Each stage depends on the previous stage's output (search → scrape → write → critique), so a linear pipeline is the natural fit. |
| **GPT-4o-mini** | Balances quality and cost — strong enough for structured research writing while keeping API costs low. |
| **Tavily over SerpAPI** | Tavily is purpose-built for AI agent search with cleaner, more relevant results and a generous free tier. |
| **Custom Streamlit CSS** | The default Streamlit theme doesn't convey the right feel for a research tool; custom CSS with dark mode, accent colors, and typography creates a premium experience. |

---

## 🔮 Future Enhancements

- [ ] **Iterative refinement loop** — Feed critic feedback back to the writer for automatic report improvement
- [ ] **Multi-source scraping** — Scrape multiple URLs in parallel for richer research material
- [ ] **PDF export** — Generate downloadable PDF reports alongside Markdown
- [ ] **Citation formatting** — Auto-format references in APA / IEEE style
- [ ] **Memory / conversation** — Allow follow-up questions on generated reports
- [ ] **Support additional LLMs** — Add support for Anthropic Claude, Google Gemini, and open-source models

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

<p align="center">
  Built with ❤️ using LangChain, OpenAI, and Streamlit
</p>
