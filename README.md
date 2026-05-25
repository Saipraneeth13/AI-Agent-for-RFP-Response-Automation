# 🤖 AI Agent for RFP Response Automation

> Intelligent automation for Request for Proposal (RFP) responses — powered by AI to save time, improve accuracy, and win more contracts.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [How It Works](#how-it-works)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**AI-Agent-for-RFP-Response-Automation** is an intelligent pipeline that automatically parses incoming RFP documents, extracts key requirements, matches them against a company knowledge base, and drafts tailored, high-quality responses — dramatically reducing the manual effort involved in the RFP lifecycle.

Whether you're a sales team drowning in RFQs or a procurement consultant managing dozens of bids at once, this agent handles the heavy lifting so you can focus on strategy and relationships.

---

## ✨ Features

- 📄 **RFP Parsing** — Extracts requirements, criteria, deadlines, and evaluation metrics from PDF, DOCX, and HTML RFP documents
- 🧠 **AI-Powered Response Generation** — Uses large language models to draft contextually accurate, professional responses
- 🗂️ **Knowledge Base Integration** — Pulls from your internal content library (past proposals, case studies, certifications, team bios) to ground responses in real company data
- 🔍 **Requirement Matching** — Automatically maps RFP sections to the most relevant existing content
- ✅ **Compliance Checking** — Flags missing mandatory sections and potential disqualifiers
- 📊 **Scoring & Prioritization** — Estimates win probability and effort level to help you decide which RFPs to pursue
- 📝 **Export-Ready Output** — Generates formatted DOCX/PDF responses ready for submission
- 🔄 **Human-in-the-Loop Review** — Built-in review interface for editors to approve, revise, or regenerate sections

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    RFP Input Layer                       │
│         (PDF / DOCX / HTML / Email attachment)          │
└───────────────────────┬─────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────┐
│                  Document Parser                         │
│     Extracts sections, tables, deadlines, criteria      │
└───────────────────────┬─────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────┐
│               Requirement Analyzer (AI)                  │
│   Classifies, prioritizes, and maps to response schema  │
└───────────┬───────────────────────────┬─────────────────┘
            │                           │
            ▼                           ▼
┌───────────────────┐       ┌───────────────────────────┐
│  Knowledge Base   │       │    LLM Response Engine     │
│  (Vector Store)   │──────▶│  Drafts section responses  │
└───────────────────┘       └────────────┬──────────────┘
                                         │
                                         ▼
                            ┌───────────────────────────┐
                            │   Review & Edit Interface  │
                            │  (approve / revise / regen)│
                            └────────────┬──────────────┘
                                         │
                                         ▼
                            ┌───────────────────────────┐
                            │      Output Generator      │
                            │    DOCX / PDF / Markdown   │
                            └───────────────────────────┘
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- Node.js 18+ (for the review UI, if applicable)
- An API key for your LLM provider (OpenAI, Anthropic, etc.)
- A vector database (Pinecone, Weaviate, or local ChromaDB)

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/AI-Agent-for-RFP-Response-Automation.git
cd AI-Agent-for-RFP-Response-Automation

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Configuration

Copy the example environment file and fill in your credentials:

```bash
cp .env.example .env
```

```env
# .env

# LLM Provider
LLM_PROVIDER=anthropic           # or openai, azure
ANTHROPIC_API_KEY=your_key_here
LLM_MODEL=claude-sonnet-4-20250514

# Vector Store
VECTOR_DB=chroma                 # or pinecone, weaviate
CHROMA_PERSIST_DIR=./data/chroma

# Knowledge Base
KB_SOURCE_DIR=./knowledge_base

# Output
OUTPUT_DIR=./output
DEFAULT_OUTPUT_FORMAT=docx       # or pdf, markdown
```

---

## 🛠️ Usage

### 1. Ingest your knowledge base

```bash
python ingest.py --source ./knowledge_base
```

This chunks and embeds your company's proposals, case studies, product sheets, and bios into the vector store.

### 2. Run the agent on an RFP

```bash
python main.py --rfp ./rfps/sample_rfp.pdf
```

### 3. Review and export

```bash
# Launch the review interface (optional)
python review_ui.py

# Export directly without review
python main.py --rfp ./rfps/sample_rfp.pdf --auto-export --format docx
```

### CLI Options

| Flag | Description | Default |
|------|-------------|---------|
| `--rfp` | Path to the RFP file | Required |
| `--format` | Output format: `docx`, `pdf`, `md` | `docx` |
| `--auto-export` | Skip the review step | `False` |
| `--kb-dir` | Override knowledge base directory | `.env` value |
| `--verbose` | Enable detailed logging | `False` |

---

## 📁 Project Structure

```
AI-Agent-for-RFP-Response-Automation/
│
├── agents/
│   ├── parser_agent.py          # Document parsing & section extraction
│   ├── requirement_agent.py     # Requirement classification & mapping
│   ├── response_agent.py        # LLM-powered response drafting
│   └── compliance_agent.py      # Mandatory section & compliance checks
│
├── knowledge_base/              # Your company's source content
│   ├── case_studies/
│   ├── certifications/
│   ├── team_bios/
│   └── past_proposals/
│
├── data/
│   └── chroma/                  # Local vector store (auto-generated)
│
├── output/                      # Generated RFP responses
│
├── rfps/                        # Incoming RFP files
│
├── ui/                          # Review interface (if applicable)
│
├── ingest.py                    # Knowledge base ingestion script
├── main.py                      # Main entry point
├── review_ui.py                 # Human-in-the-loop review tool
├── requirements.txt
├── .env.example
└── README.md
```

---

## ⚙️ How It Works

1. **Parse** — The parser agent reads the RFP file, splits it into logical sections (executive summary, technical requirements, pricing, past performance, etc.), and extracts metadata like submission deadlines and evaluation criteria.

2. **Analyze** — The requirement agent classifies each section, determines what content type is needed (narrative, table, reference list, etc.), and scores complexity.

3. **Retrieve** — Relevant chunks from the knowledge base are retrieved via semantic search for each requirement.

4. **Generate** — The response agent sends a structured prompt to the LLM with the RFP context + retrieved knowledge, producing a draft response per section.

5. **Validate** — The compliance agent checks that all mandatory sections are addressed and flags any potential issues.

6. **Review** — A reviewer can approve, edit, or trigger regeneration of any section via the review interface.

7. **Export** — The final document is assembled and exported in the desired format.

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m 'Add some feature'`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for our code of conduct and detailed contribution guidelines.

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙋 FAQ

**Q: What file formats does the RFP parser support?**  
A: PDF, DOCX, and HTML are supported out of the box. CSV-based requirement sheets can also be ingested.

**Q: Can I use a locally hosted LLM?**  
A: Yes — set `LLM_PROVIDER=local` and configure the `LOCAL_LLM_ENDPOINT` in your `.env`.

**Q: How do I keep proprietary knowledge base content secure?**  
A: The vector store runs locally by default (ChromaDB). For cloud deployments, use your provider's access controls and encryption features.

**Q: Can this handle multi-lot or multi-volume RFPs?**  
A: Yes — the parser detects document structure and processes each volume/lot independently.

---

<p align="center">Built with ❤️ to help teams respond smarter, faster, and with more confidence.</p>
