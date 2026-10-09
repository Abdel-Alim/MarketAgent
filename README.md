Multi-Agent AI Research System

<p align="center"> <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" /> <img src="https://img.shields.io/badge/LangGraph-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white" alt="LangGraph" /> <img src="https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white" alt="LangChain" /> <img src="https://img.shields.io/badge/OpenRouter-6467F2?style=for-the-badge&logo=openrouter&logoColor=white" alt="OpenRouter" /> <img src="https://img.shields.io/badge/Tavily-Web%20Search-0F766E?style=for-the-badge" alt="Tavily Search" /> </p>

<p align="center"> <strong>Autonomous Research Workflows · Multi-Agent Orchestration · LLM-Powered Analysis</strong> </p>

<p align="center"> A multi-agent AI system that transforms research topics into structured reports through web search, information synthesis, analytical generation, and a supervisor-controlled workflow. </p>

<p align="center"> <a href="https://github.com/Abdel-Alim">GitHub Profile</a> </p>

Overview

Multi-Agent AI Research System is a Python-based research automation project built with LangGraph and LangChain.

It coordinates specialized AI agents through a shared state, using Tavily to retrieve web search results and an OpenRouter-compatible language model to summarize information, synthesize findings, and generate structured Markdown reports.

The workflow demonstrates how agent specialization and conditional graph routing can organize a multi-stage research process into a single executable pipeline.

System Architecture
flowchart TD
    A(["Research Topic"]) --> B["Supervisor"]
    B --> C["Researcher"]
    C --> D["Web Search · Tavily"]
    D --> E["LLM Summarization"]
    E --> B

    B --> F["Analyst"]
    F --> G["Research Synthesis"]
    G --> B

    B --> H["Writer"]
    H --> I["Markdown Report"]
    I --> B

    B --> J["Critic"]
    J --> K{"Review Decision"}
    K -->|Approved| L(["Final Report"])
    K -->|Revision Required| H

    style A fill:#172554,color:#ffffff,stroke:#60a5fa
    style B fill:#312e81,color:#ffffff,stroke:#a5b4fc
    style C fill:#164e63,color:#ffffff,stroke:#67e8f9
    style F fill:#14532d,color:#ffffff,stroke:#86efac
    style H fill:#713f12,color:#ffffff,stroke:#fcd34d
    style J fill:#581c87,color:#ffffff,stroke:#d8b4fe
    style L fill:#166534,color:#ffffff,stroke:#86efac


The supervisor controls transitions between workflow nodes. Each agent updates the shared state, allowing subsequent agents to consume previously collected research and generated content.

Implementation note: The current critic returns a predefined approval decision, so the revision path is supported by the graph but is not meaningfully exercised during normal execution.
Agent Components

<table> <tr> <th>Component</th> <th>Role</th> </tr> <tr> <td>🎯 Supervisor</td> <td>Coordinates execution and selects the next agent through conditional routing.</td> </tr> <tr> <td>🔎 Researcher</td> <td>Generates research questions, retrieves web results, summarizes content, and collects source links.</td> </tr> <tr> <td>📊 Analyst</td> <td>Synthesizes collected research into a detailed analytical response.</td> </tr> <tr> <td>✍️ Writer</td> <td>Formats the analysis into a structured Markdown report.</td> </tr> <tr> <td>🛡️ Critic</td> <td>Represents the review stage and records the approval decision and review metadata.</td> </tr> </table>

Features

<p align="center"> <img src="https://img.shields.io/badge/01-Multi--Agent%20Workflow-2563EB?style=flat-square" alt="Multi-agent workflow" /> <img src="https://img.shields.io/badge/02-Live%20Web%20Search-0F766E?style=flat-square" alt="Web search" /> <img src="https://img.shields.io/badge/03-Structured%20Reports-7C3AED?style=flat-square" alt="Structured reports" /> <img src="https://img.shields.io/badge/04-State%20Management-B45309?style=flat-square" alt="State management" /> </p>

Multi-agent orchestration: Specialized nodes handle distinct research tasks.
Web-grounded synthesis: LLM processing uses retrieved search-result content.
Structured report generation: Produces Markdown reports with research findings and source links.
Conditional graph routing: LangGraph manages agent transitions and workflow termination.
Execution tracing: Records agent summaries and execution durations.
Configurable model settings: Supports environment-based model selection and generation temperature.
Shared workflow state: Tracks questions, research notes, sources, analysis, drafts, messages, and review metadata.
Technology Stack

<p align="center"> <a href="https://www.python.org/"> <img src="https://skillicons.dev/icons?i=python" width="52" alt="Python" /> </a> &nbsp; <a href="https://www.langchain.com/"> <img src="https://img.shields.io/badge/LangChain-Framework-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white" alt="LangChain" /> </a> &nbsp; <a href="https://langchain-ai.github.io/langgraph/"> <img src="https://img.shields.io/badge/LangGraph-Orchestration-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white" alt="LangGraph" /> </a> </p>

Technology	Purpose
Python	Application logic and workflow implementation
LangGraph	State graph, conditional edges, and agent orchestration
LangChain Core	Message types and model workflow integration
LangChain OpenAI	Chat model interface
LangChain Community	Search tool integration
OpenRouter	Language model API access
Tavily Search	Web information retrieval
python-dotenv	Environment configuration
IPython	Markdown rendering in notebooks
Workflow Lifecycle
sequenceDiagram
    participant U as User
    participant S as Supervisor
    participant R as Researcher
    participant A as Analyst
    participant W as Writer
    participant C as Critic

    U->>S: Submit research topic
    S->>R: Start research
    R-->>S: Research notes and sources
    S->>A: Analyze findings
    A-->>S: Analytical synthesis
    S->>W: Generate report
    W-->>S: Markdown draft
    S->>C: Review draft
    C-->>S: Review result
    S-->>U: Final report and state

Project Structure
multi-agent-ai-research/
│
├── research_agent.py      # Main workflow implementation
├── requirements.txt       # Python dependencies
├── .env.example           # Environment configuration template
├── .gitignore             # Excluded files
└── README.md              # Project documentation


The implementation can also be maintained in a Jupyter notebook if interactive experimentation and notebook-based report rendering are required.

Getting Started
Prerequisites
Python 3.10 or later
An OpenRouter API key
A Tavily API key
1. Clone the Repository

Replace REPOSITORY_NAME with the actual repository name.

git clone https://github.com/Abdel-Alim/REPOSITORY_NAME.git
cd REPOSITORY_NAME

2. Create a Virtual Environment
python -m venv .venv


Windows

.venv\Scripts\activate


Linux / macOS

source .venv/bin/activate

3. Install Dependencies

Create requirements.txt:

langgraph
langchain-core
langchain-openai
langchain-community
tavily-python
python-dotenv
ipython


Install the required packages:

pip install -r requirements.txt

Configuration

Create a .env file in the project root:

OPENROUTER_API_KEY=your_openrouter_api_key
TAVILY_API_KEY=your_tavily_api_key

MODEL_NAME=openai/gpt-4o-mini
TEMPERATURE=0.2
MAX_REVISIONS=2

Variable	Description
OPENROUTER_API_KEY	API credential for the configured chat model
TAVILY_API_KEY	API credential for web search
MODEL_NAME	Model identifier
TEMPERATURE	Controls model generation variability
MAX_REVISIONS	Configured revision limit

API credentials:

OpenRouter API Keys
Tavily Dashboard

Security: Never commit .env or expose API keys in public notebooks or logs. Use .env.example for placeholder configuration.

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


To render the report in a Jupyter notebook:

from IPython.display import display, Markdown

display(Markdown(report))


You can replace the topic with another research subject:

result = run_research(
    "The electric vehicle market in the Middle East"
)

print(result.get("final_report", "No report generated."))


The current research questions and report template are written primarily in Arabic. Changing the topic does not automatically change the entire workflow language.

Output

The workflow generates a Markdown report with the following sections:

# Market Research Report: [Topic]

## 1. Executive Summary

Overview of the research subject.

## 2. Analysis and Key Findings

Synthesis of retrieved information and analytical observations.

## 3. Sources and References

- [Source Title](https://example.com)


The returned state also contains intermediate research notes, source metadata, analysis, review information, messages, and execution traces.

Current Limitations
The search implementation uses Tavily, not a direct Google Search integration.
The critic currently returns a fixed score of 9.0 and an unconditional approval decision.
Feedback-driven revision cycles are not meaningfully implemented.
Search results and generated claims are not independently fact-checked.
Search failures may result in incomplete research.
The LLM_PROVIDER setting is declared but does not currently switch providers.
Language and audience fields are not consistently applied to prompts.
The per-run revision limit is not used by the supervisor's termination logic.
Roadmap
Implement an LLM-based critic with explicit evaluation criteria.
Enable feedback-driven report revisions.
Improve citation traceability and source validation.
Add structured logging and robust error handling.
Support configurable languages and research audiences.
Add automated unit and integration tests.
Introduce a command-line interface or web dashboard.
Add token usage, latency, and API cost tracking.
Support PDF, HTML, and DOCX exports.
Author

Abdel-Alim

<p> <a href="https://github.com/Abdel-Alim"> <img src="https://img.shields.io/badge/GitHub-Abdel--Alim-181717?style=for-the-badge&logo=github" alt="GitHub Profile" /> </a> </p>

License

No license has been specified yet. Add a LICENSE file before distributing the project under an open-source license.
