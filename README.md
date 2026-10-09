🔎 Multi-Agent AI Research & Report Generator

An intelligent, multi-agent research system built with LangGraph, LangChain, and LLMs that automates web research, information analysis, and structured report generation using real-time search results.

The system orchestrates specialized AI agents to research a given topic, analyze collected information, generate a Markdown report, and review the output through a supervisor-controlled workflow.

🚀 Overview

Traditional research often requires manually searching multiple sources, extracting relevant information, analyzing findings, and organizing everything into a coherent report.

This project streamlines that process using a multi-agent architecture in which each agent performs a specific task within a coordinated workflow.

The current implementation demonstrates market research through a practical example: analyzing the football player market in Egypt.

✨ Key Features
Multi-Agent Architecture: Separate agents handle orchestration, research, analysis, writing, and review.
Live Web Research: Retrieves current search results using Tavily Search.
LLM-Powered Analysis: Summarizes research findings and produces structured analysis using compatible chat models.
Automated Report Generation: Creates Markdown reports with an executive summary, analysis, and source links.
Supervisor-Based Orchestration: Uses LangGraph to manage workflow execution and agent transitions.
Quality Review Stage: Includes a critic agent to review and approve generated reports.
Execution Tracing: Records agent activities, execution times, and workflow summaries.
Configurable Settings: Supports environment-based model selection, temperature, and revision limits.
Source Tracking: Collects URLs and titles from retrieved web results for reference.
🏗️ System Architecture

The workflow consists of five specialized agents coordinated by a supervisor.

flowchart TD
    A([Start]) --> B[Supervisor]
    B --> C[Researcher]
    C --> B
    B --> D[Analyst]
    D --> B
    B --> E[Writer]
    E --> B
    B --> F[Critic]
    F --> B
    B --> G{Approved or Revision Limit Reached?}
    G -->|No| E
    G -->|Yes| H[Final Report]
    H --> I([End])

Agent Responsibilities
Agent	Responsibility
Supervisor	Determines the next agent based on the current workflow state.
Researcher	Searches the web, summarizes findings, and collects source references.
Analyst	Synthesizes research notes into a detailed analysis.
Writer	Generates a structured Markdown report.
Critic	Reviews the draft, records feedback, and determines approval status.

Workflow orchestration: LangGraph manages the shared state and transitions between agents. Each agent returns its results to the supervisor, which decides the next step.

⚙️ Tech Stack
Python — Core programming language
LangGraph — Stateful agent orchestration and workflow management
LangChain — LLM integration and tool execution
ChatOpenAI — Chat model interface
OpenRouter — Access to compatible LLMs through an OpenAI-compatible API
Tavily Search — Web search and research retrieval
python-dotenv — Environment variable management
IPython — Markdown report rendering in notebook environments
📁 Project Structure

A suggested repository structure for organizing the current implementation:

multi-agent-research/
├── research_agent.py       # Main agent workflow
├── .env.example            # Environment variable template
├── .gitignore              # Files excluded from version control
├── requirements.txt        # Python dependencies
└── README.md               # Project documentation


If the implementation currently lives in a Jupyter notebook, you can keep it as a notebook or move the reusable logic into research_agent.py.

🛠️ Installation
1. Clone the Repository
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY


Replace the repository URL with your actual GitHub repository.

2. Create a Virtual Environment
python -m venv .venv


Activate it:

Windows

.venv\Scripts\activate


macOS / Linux

source .venv/bin/activate

3. Install Dependencies
pip install langgraph langchain-core langchain-openai langchain-community tavily-python python-dotenv ipython


Alternatively, create a requirements.txt file containing:

langgraph
langchain-core
langchain-openai
langchain-community
tavily-python
python-dotenv
ipython


Then install the dependencies:

pip install -r requirements.txt

🔑 Configuration

The project requires API credentials for the language model provider and web search service.

1. Obtain API Keys
OpenRouter: https://openrouter.ai/keys
Tavily: https://app.tavily.com/
2. Configure Environment Variables

Create a .env file in the project root:

OPENROUTER_API_KEY=your_openrouter_api_key
TAVILY_API_KEY=your_tavily_api_key

MODEL_NAME=openai/gpt-4o-mini
TEMPERATURE=0.2
MAX_REVISIONS=2


The application reads these settings from the environment. The default model is openai/gpt-4o-mini, accessed through OpenRouter.

Security note: Never commit your .env file or expose API keys in source code, notebooks, screenshots, or logs.

Add the following to .gitignore:

.env
.venv/
__pycache__/
*.py[cod]
.ipynb_checkpoints/

▶️ Usage
Run the Research Workflow

The current implementation defines the topic directly in the script:

topic_name = "سوق لاعبي كرة القدم في مصر"

result_state = run_research(topic_name)

final_report_text = result_state.get(
    "final_report",
    "No report generated."
)

print(final_report_text)


For notebook execution, the generated report can be displayed as formatted Markdown:

from IPython.display import display, Markdown

display(Markdown(final_report_text))

Research a Different Topic

Pass a different topic to run_research():

result_state = run_research(
    "The electric vehicle market in the Middle East"
)

print(result_state.get("final_report", "No report generated."))


Other potential research topics include:

Market and competitor analysis
Emerging technology trends
Industry landscape research
Business opportunities and market risks
Sports market analysis
Startup and investment research

The quality and coverage of each report depend on search results, source reliability, model behavior, and the research questions generated for the topic.

📊 Workflow State and Outputs

The system maintains a shared state throughout the workflow.

State Field	Description
topic	Research subject
questions	Questions used to guide research
research_notes	Summarized findings from web searches
sources	Collected source titles and URLs
analysis	Synthesized analytical output
draft_report	Generated Markdown draft
critic	Review score, approval status, and feedback
final_report	Report returned by the review stage
revision_count	Number of review cycles recorded
trace	Agent execution summaries and timing information
messages	Messages recorded by the agents

The workflow returns the final state as a Python dictionary, allowing the report and execution metadata to be inspected programmatically.

📝 Example Output

The generated report follows a structured Markdown format:

# Market Research Report: [Topic]

## 1. Executive Summary

An overview of the research subject and its market context.

## 2. Analysis and Key Findings

A detailed analysis based on the collected web research.

## 3. Sources and References

- [Source Title](https://example.com)
- [Another Source](https://example.org)


This structure makes the output suitable for documentation, notebook display, and further processing.

🔄 How the Workflow Operates
Initialization: Creates the research state and initializes the supervisor.
Research Planning: Generates research questions for the selected topic.
Information Retrieval: Searches the web and summarizes retrieved results.
Analysis: Synthesizes research notes into a detailed analytical response.
Report Writing: Converts the analysis into a structured Markdown report.
Review: Evaluates the draft and records a review decision.
Finalization: Returns the final report and workflow metadata.

The supervisor uses conditional routing to determine which agent should execute next.

⚠️ Current Limitations

The current implementation is a working foundation with several areas that could be improved:

Review logic: The critic currently uses a predefined approval decision rather than an LLM-based evaluation.
Revision handling: The supervisor supports revision routing, but the critic currently approves every draft, so revision cycles are not meaningfully exercised.
Source validation: Search results are collected, but the system does not independently verify source credibility or factual accuracy.
Error handling: Research exceptions are printed, and failed searches may result in incomplete reports.
Search provider: The implementation uses Tavily for web retrieval; it does not directly call Google Search.
Configuration consistency: LLM_PROVIDER is defined but is not currently used to select between different providers.
Report evaluation: No automated factuality, citation coverage, or output-quality benchmark is included.

These limitations should be considered when using the system for business, financial, or other high-impact research.

🛣️ Roadmap

Potential future enhancements include:

Implement an LLM-based critic with explicit evaluation criteria.
Add meaningful iterative revisions based on reviewer feedback.
Introduce structured research questions and topic-specific planning.
Validate citations and associate claims with individual sources.
Add configurable search depth and source limits.
Support additional LLM providers.
Export reports to PDF, HTML, or DOCX.
Add a Streamlit or FastAPI interface.
Implement unit tests and workflow integration tests.
Add observability, token usage tracking, and cost estimation.
🤝 Contributing

Contributions, suggestions, and bug reports are welcome.

Fork the repository.
Create a feature branch.
Implement your changes and add tests where appropriate.
Submit a pull request describing the changes.
🔒 Responsible Use

This project generates research summaries from third-party web content and language models. Outputs may contain inaccuracies, incomplete information, or unsupported conclusions.

Always review important findings, verify source material, and avoid treating generated reports as a substitute for professional due diligence.

📄 License

No license has been specified yet. If you plan to distribute this project publicly, consider adding a LICENSE file with the license that matches your intended use.

Built with Python, LangGraph, LangChain, and Tavily.
