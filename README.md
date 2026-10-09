Multi-Agent AI Research System

A stateful, LLM-powered research workflow that automates web information retrieval, research synthesis, analytical report generation, and report review using LangGraph, LangChain, and Tavily Search.

The system coordinates specialized AI agents through a supervisor-controlled workflow, transforming a research topic into a structured Markdown report supported by links to retrieved web sources.

Overview

Conducting research requires gathering information from multiple sources, synthesizing findings, identifying relevant insights, and presenting conclusions in a structured format.

This project explores how a multi-agent architecture can automate these stages through specialized agents operating over a shared state.

Built with LangGraph, the workflow uses conditional routing to coordinate research, analysis, writing, and review. Language models handle information synthesis and analytical generation, while Tavily provides web search results.

The implementation currently demonstrates this workflow using the Egyptian football player market as its example research topic.

Architecture

The system consists of five specialized agents coordinated through a stateful LangGraph workflow.

flowchart TD
    A([Research Topic]) --> B[Supervisor]
    B --> C[Researcher]
    C --> D[Supervisor]
    D --> E[Analyst]
    E --> F[Supervisor]
    F --> G[Writer]
    G --> H[Supervisor]
    H --> I[Critic]
    I --> J[Supervisor]
    J --> K{Approval or Revision Limit}
    K -->|Approved / Limit Reached| L[Final Report]
    K -->|Revision Required| G

Agent Responsibilities

Supervisor

Controls workflow progression by evaluating the current state and selecting the next agent. It coordinates the execution sequence and determines when the workflow should terminate.

Researcher

Generates research questions, retrieves web results through Tavily Search, summarizes retrieved content using an LLM, and collects source titles and URLs.

Analyst

Synthesizes the research notes into a detailed analysis of the selected topic using the configured language model.

Writer

Transforms the analysis into a structured Markdown report containing an executive summary, analytical findings, and source references.

Critic

Represents the report review stage. The current implementation records a predefined score and approval decision; LLM-based evaluation and feedback-driven revision are potential extensions.

Workflow
Initialization: Creates the shared research state, including the topic, language, audience, and workflow metadata.
Research planning: Generates questions covering market conditions, key participants, and relevant risks.
Web retrieval: Searches for relevant information and collects source metadata.
Research synthesis: Uses the language model to summarize retrieved content.
Analysis: Produces a consolidated analytical response from the research notes.
Report generation: Formats the analysis and available references into Markdown.
Review and termination: Executes the critic stage and returns the final report according to the supervisor's routing logic.

The workflow is compiled as a LangGraph StateGraph, with agent outputs stored in a shared state and transitions managed through conditional edges.

Technology Stack
Technology	Purpose
Python	Core implementation
LangGraph	Stateful workflow orchestration and conditional routing
LangChain Core	Message abstractions and shared workflow integration
LangChain Community	Tavily search tool integration
LangChain OpenAI	Chat model interface
OpenRouter	LLM API access through an OpenAI-compatible endpoint
Tavily Search	Web research and search result retrieval
python-dotenv	Environment variable loading
IPython	Markdown report rendering in notebooks
Key Capabilities
Multi-agent orchestration: Separates research responsibilities into specialized workflow nodes.
Web-grounded research: Uses retrieved web content as input to LLM-generated summaries and analysis.
Structured reporting: Produces readable Markdown reports with source links.
Shared state management: Tracks research notes, analysis, draft reports, review results, messages, and execution traces.
Conditional workflow routing: Uses supervisor decisions to control agent transitions.
Execution timing: Records the duration of individual agent steps.
Environment-based configuration: Supports configurable model names, temperature, and revision limits.
Project Structure

A recommended repository layout for the implementation:

multi-agent-ai-research/
├── research_agent.py
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md


The code can also remain in a Jupyter notebook if notebook-based experimentation and Markdown rendering are central to the project.

Installation
Prerequisites
Python 3.10 or later
An OpenRouter API key
A Tavily API key
1. Clone the Repository
git clone https://github.com/Abdel-Alim/REPOSITORY_NAME.git
cd REPOSITORY_NAME


Replace REPOSITORY_NAME with the actual repository name.

2. Create a Virtual Environment
python -m venv .venv


Activate the environment:

Windows

.venv\Scripts\activate


Linux / macOS

source .venv/bin/activate

3. Install Dependencies

Create a requirements.txt file:

langgraph
langchain-core
langchain-openai
langchain-community
tavily-python
python-dotenv
ipython


Install the dependencies:

pip install -r requirements.txt

Configuration

Create a .env file in the project root:

OPENROUTER_API_KEY=your_openrouter_api_key
TAVILY_API_KEY=your_tavily_api_key

MODEL_NAME=openai/gpt-4o-mini
TEMPERATURE=0.2
MAX_REVISIONS=2


Obtain API credentials from:

OpenRouter: https://openrouter.ai/keys
Tavily: https://app.tavily.com/

The application loads the environment variables and uses the configured model through OpenRouter's OpenAI-compatible API endpoint.

Environment Variables
Variable	Purpose	Default
OPENROUTER_API_KEY	LLM API authentication	Required unless OPENAI_API_KEY is supplied
TAVILY_API_KEY	Web search authentication	Required for research
MODEL_NAME	Model identifier	openai/gpt-4o-mini
TEMPERATURE	Model generation temperature	0.2
MAX_REVISIONS	Maximum configured review cycles	2

Security: Keep .env out of version control. Commit an .env.example file containing placeholder values instead of real credentials.

Recommended .gitignore:

.env
.venv/
__pycache__/
*.py[cod]
.ipynb_checkpoints/

Usage

The main entry point is run_research(topic).

from research_agent import run_research

topic = "سوق لاعبي كرة القدم في مصر"

result = run_research(topic)

report = result.get("final_report", "")

print(report)


If the code is executed inside a Jupyter notebook, render the report as Markdown:

from IPython.display import display, Markdown

display(Markdown(report))


To research another topic, pass a different string to run_research().

result = run_research(
    "The electric vehicle market in the Middle East"
)

print(result.get("final_report", "No report generated."))


The current research prompts are written in Arabic, so changing the topic alone does not guarantee that all research questions or outputs will be generated in English.

Output Format

The generated report follows a consistent Markdown structure.

# Market Research Report: [Topic]

## 1. Executive Summary

An overview of the research topic and its context.

## 2. Analysis and Key Findings

A synthesis of the collected research and analytical findings.

## 3. Sources and References

- [Source Title](https://example.com)
- [Another Source](https://example.org)


The returned workflow state also exposes research notes, source metadata, analysis, review information, messages, and execution traces for further inspection.

Implementation Notes and Limitations

This project is an experimental multi-agent research implementation. The following distinctions are important when evaluating its current behavior:

Search provider: The implementation uses Tavily Search, not a direct Google Search integration.
Critic implementation: The critic currently assigns a fixed score of 9.0, approves the draft unconditionally, and copies the draft into final_report.
Revision behavior: Although the graph supports routing back to the writer, the unconditional approval means that normal execution does not exercise feedback-driven revisions.
Source grounding: Retrieved snippets are passed to the language model for synthesis. The system does not independently verify every claim or guarantee that every conclusion is supported by a cited source.
Error handling: Search errors are printed and processing continues, which may lead to incomplete research.
Configuration: LLM_PROVIDER is declared in the settings but is not currently used to switch between providers.
Language configuration: The state contains language and audience fields, but the current prompts and report template do not consistently use these settings.
Revision configuration: The supervisor uses the configured settings value for its revision limit rather than the per-run max_revisions value stored in the state.

These are opportunities for further development rather than capabilities that should be assumed to exist in the current implementation.

Future Improvements
Implement LLM-based report evaluation with explicit quality criteria.
Support revision cycles driven by critic feedback.
Improve citation-to-claim traceability and source validation.
Add configurable research depth, question count, and result limits.
Make language and audience settings influence agent prompts and report formatting.
Add structured logging and more robust error handling.
Add automated tests for agent nodes, routing, and end-to-end workflow execution.
Introduce a command-line interface or web interface.
Support report export to HTML, PDF, and DOCX.
Track token usage, latency, and estimated API costs.
Author

Abdel-Alim

GitHub: @Abdel-Alim

License

No license is currently specified. Add an appropriate LICENSE file before distributing the project under an open-source license.
