# ReAct Research Agent

A Web Research Agent built using the **ReAct (Reasoning + Acting)** pattern. It integrates Google Gemini as the LLM for reasoning and Tavily for web search, producing a structured Markdown research report.

---

## How It Works

This agent follows two core design patterns:

- **Planning Pattern** — The agent first uses Gemini 2.5 Flash to reason about the topic and generate 5–6 well-structured research questions before taking any action.
- **Tool-Use Pattern** — The agent then acts by using the Tavily Search API to find relevant, recent web results for each question.

### Flow

```
Start → Generate Research Questions (LLM) → Web Search per Question (Tavily) → Compile Report
```

### Reasoning Step (LLM)

The LLM is prompted as a research assistant and asked to generate diverse, well-structured questions covering different aspects of the topic. The response is returned as a JSON array of strings, ensuring structured output that the agent can iterate over programmatically.

### Acting Step (Web Search)

For each generated question, the agent calls the Tavily Search API with `search_depth: basic` and retrieves up to 5 results. It extracts the `title` and `content` from each result and stores them mapped to the question.

### Report Generation

After all questions are answered, the agent compiles a Markdown report with:
- A title and introduction
- A section per research question with key findings
- A conclusion summarizing the research

---

## Project Structure

```
ReAct/                        # Project root
├── ReAct.py                  # Main agent implementation
├── .env                      # API keys (not committed)
├── .env.example              # Template for environment variables
├── .gitignore
├── LICENSE
├── README.md
└── docs/
    └── research_report_tavily_gemini.md   # Generated report output
```

---

## Setup

1. Clone the repo and navigate to the `ReAct` folder.

2. Install dependencies:
   ```bash
   pip install google-generativeai python-dotenv requests
   ```

3. Copy `.env.example` to `.env` and fill in your API keys:
   ```bash
   cp .env.example .env
   ```

   ```
   GEMINI_API_KEY=your_gemini_api_key
   TAVILY_API_KEY=your_tavily_api_key
   ```

   - Get a Gemini API key: https://aistudio.google.com/app/apikey
   - Get a Tavily API key: https://app.tavily.com

---

## Usage

```bash
python ReAct.py
```

Enter a topic when prompted (e.g., `Climate Change`, `Quantum Computing`).

The agent will:
1. Generate 5–6 research questions via Gemini 2.5 Flash
2. Search the web for each question using Tavily API
3. Save the final report as `research_report_tavily_gemini.md`

---

## Output

A structured Markdown report saved as `docs/research_report_tavily_gemini.md` with sections for each research question and a conclusion. The `docs/` folder is created automatically if it doesn't exist.

---

## License

MIT — see [LICENSE](LICENSE)
