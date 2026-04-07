# Ugandan Law MCP Server

**The Uganda Legal Information Institute (ULII) alternative for the AI age.**

[![npm version](https://badge.fury.io/js/@ansvar%2Fugandan-law-mcp.svg)](https://www.npmjs.com/package/@ansvar/ugandan-law-mcp)
[![MCP Registry](https://img.shields.io/badge/MCP-Registry-blue)](https://registry.modelcontextprotocol.io)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![GitHub stars](https://img.shields.io/github/stars/Ansvar-Systems/Ugandan-law-mcp?style=social)](https://github.com/Ansvar-Systems/Ugandan-law-mcp)
[![CI](https://github.com/Ansvar-Systems/Ugandan-law-mcp/actions/workflows/ci.yml/badge.svg)](https://github.com/Ansvar-Systems/Ugandan-law-mcp/actions/workflows/ci.yml)
[![Provisions](https://img.shields.io/badge/provisions-17%2C024-blue)](https://github.com/Ansvar-Systems/Ugandan-law-mcp)

Query **622 Ugandan laws** -- from the Data Protection and Privacy Act 2019 and the Penal Code Act to the Companies Act 2012, Employment Act, and more -- directly from Claude, Cursor, or any MCP-compatible client.

If you're building legal tech, compliance tools, or doing Ugandan legal research, this is your verified reference database.

Built by [Ansvar Systems](https://ansvar.eu) -- Stockholm, Sweden

---

## Why This Exists

Ugandan legal research means navigating ulii.org and parliament.go.ug across a body of legislation that spans independence-era Acts, post-1995 Constitution reforms, and recent data protection statutes. Whether you're:
- A **lawyer** validating citations in a brief or contract
- A **compliance officer** checking obligations under the Data Protection and Privacy Act 2019
- A **legal tech developer** building tools on Ugandan law
- A **researcher** tracing legislative history across 622 Acts

...you shouldn't need dozens of browser tabs and manual cross-referencing. Ask Claude. Get the exact provision. With context.

This MCP server makes Ugandan law **searchable, cross-referenceable, and AI-readable**.

---

## Quick Start

### Use Remotely (No Install Needed)

> Connect directly to the hosted version -- zero dependencies, nothing to install.

**Endpoint:** `https://mcp.ansvar.eu/law-ug/mcp`

| Client | How to Connect |
|--------|---------------|
| **Claude.ai** | Settings > Connectors > Add Integration > paste URL |
| **Claude Code** | `claude mcp add ugandan-law --transport http https://mcp.ansvar.eu/law-ug/mcp` |
| **Claude Desktop** | Add to config (see below) |
| **GitHub Copilot** | Add to VS Code settings (see below) |

**Claude Desktop** -- add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "ugandan-law": {
      "type": "url",
      "url": "https://mcp.ansvar.eu/law-ug/mcp"
    }
  }
}
```

**GitHub Copilot** -- add to VS Code `settings.json`:

```json
{
  "github.copilot.chat.mcp.servers": {
    "ugandan-law": {
      "type": "http",
      "url": "https://mcp.ansvar.eu/law-ug/mcp"
    }
  }
}
```

### Use Locally (npm)

```bash
npx @ansvar/ugandan-law-mcp
```

**Claude Desktop** -- add to `claude_desktop_config.json`:

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "ugandan-law": {
      "command": "npx",
      "args": ["-y", "@ansvar/ugandan-law-mcp"]
    }
  }
}
```

**Cursor / VS Code:**

```json
{
  "mcp.servers": {
    "ugandan-law": {
      "command": "npx",
      "args": ["-y", "@ansvar/ugandan-law-mcp"]
    }
  }
}
```

---

## Example Queries

Once connected, just ask naturally:

- *"What does the Data Protection and Privacy Act 2019 say about personal data processing?"*
- *"Find provisions in the Penal Code Act about cybercrime offences"*
- *"Search for company law under the Companies Act 2012"*
- *"Is the Employment Act still in force?"*
- *"What does the Constitution say about the right to privacy?"*
- *"Find provisions about environmental protection under the National Environment Act"*
- *"Validate the citation 'Section 13 Data Protection and Privacy Act 2019'"*
- *"Build a legal stance on data breach notification requirements in Uganda"*

---

## What's Included

| Category | Count | Details |
|----------|-------|---------|
| **Statutes** | 622 laws | Comprehensive Ugandan legislation from ulii.org |
| **Provisions** | 17,024 sections | Full-text searchable with FTS5 |
| **Database Size** | ~30 MB | Optimized SQLite, portable |
| **Legal Definitions** | Table reserved | Extraction planned for upcoming release |
| **Freshness Checks** | Automated | Drift detection against official sources |

**Verified data only** -- every citation is validated against official sources (ULII, Parliament of Uganda). Zero LLM-generated content.

---

## Why This Works

**Verbatim Source Text (No LLM Processing):**
- All statute text is ingested from [ulii.org](https://ulii.org) (Uganda Legal Information Institute) and [parliament.go.ug](https://parliament.go.ug)
- Provisions are returned **unchanged** from SQLite FTS5 database rows
- Zero LLM summarization or paraphrasing -- the database contains statute text, not AI interpretations

**Smart Context Management:**
- Search returns ranked provisions with BM25 scoring (safe for context)
- Provision retrieval gives exact text by Act identifier + section number
- Cross-references help navigate without loading everything at once

**Technical Architecture:**
```
ulii.org / parliament.go.ug --> Parse --> SQLite --> FTS5 snippet() --> MCP response
                                  ^                        ^
                           Provision parser         Verbatim database query
```

### Traditional Research vs. This MCP

| Traditional Approach | This MCP Server |
|---------------------|-----------------|
| Search ulii.org by Act name | Search by plain English: *"data protection consent lawful basis"* |
| Navigate multi-section statutes manually | Get the exact provision with context |
| Manual cross-referencing between Acts | `build_legal_stance` aggregates across sources |
| "Is this Act still in force?" -- check manually | `check_currency` tool -- answer in seconds |
| Find EAC/AU alignment -- search manually | `get_eu_basis` -- linked frameworks instantly |
| No API, no integration | MCP protocol -- AI-native |

**Traditional:** Search ULII -> Navigate HTML -> Ctrl+F -> Cross-reference between Acts -> Repeat

**This MCP:** *"What are the data controller obligations under the Data Protection and Privacy Act 2019?"* -> Done.

---

## Available Tools (13)

### Core Legal Research Tools (8)

| Tool | Description |
|------|-------------|
| `search_legislation` | FTS5 full-text search across 17,024 provisions with BM25 ranking. Supports quoted phrases, boolean operators, prefix wildcards |
| `get_provision` | Retrieve specific provision by Act identifier + section number |
| `check_currency` | Check if a statute is in force, amended, or repealed |
| `validate_citation` | Validate citation against database -- zero-hallucination check |
| `build_legal_stance` | Aggregate citations from multiple statutes for a legal topic |
| `format_citation` | Format citations per Ugandan legal conventions |
| `list_sources` | List all available statutes with metadata, coverage scope, and data provenance |
| `about` | Server info, capabilities, dataset statistics, and coverage summary |

### International Law Integration Tools (5)

| Tool | Description |
|------|-------------|
| `get_eu_basis` | Get EU directives/regulations that a Ugandan statute aligns with (e.g., DPPA 2019 and GDPR principles) |
| `get_ugandan_implementations` | Find Ugandan laws aligning with a specific international framework |
| `search_eu_implementations` | Search EU documents with Ugandan alignment counts |
| `get_provision_eu_basis` | Get international law references for a specific provision |
| `validate_eu_compliance` | Check alignment status of Ugandan statutes against EU/EAC frameworks |

---

## International Law Alignment

Uganda is not an EU member state. The international alignment tools cover the frameworks that matter for Ugandan law practice:

- **EAC frameworks** -- East African Community treaty obligations and harmonisation instruments
- **African Union** -- AU conventions including the Malabo Convention on Data Protection
- **Commonwealth** -- Commonwealth legal frameworks and model laws
- **Data Protection and Privacy Act 2019** aligns with international data protection principles; the `get_eu_basis` tool maps these to GDPR-equivalent provisions for cross-reference
- **Employment Act** aligns with ILO conventions and EAC labour protocols

The international bridge tools allow you to explore alignment relationships -- checking which Ugandan provisions correspond to EAC or international requirements, and vice versa.

> **Note:** International cross-references reflect alignment and treaty obligations, not formal transposition. Uganda adopts its own legislative approach, and these tools help identify where Ugandan and international law address similar domains.

---

## Data Sources & Freshness

All content is sourced from authoritative Ugandan legal databases:

- **[Uganda Legal Information Institute (ULII)](https://ulii.org/)** -- Primary source for Ugandan legislation and case law
- **[Parliament of Uganda](https://parliament.go.ug/)** -- Official parliamentary records and Bills

### Data Provenance

| Field | Value |
|-------|-------|
| **Authority** | ULII, Parliament of Uganda |
| **Primary language** | English |
| **License** | Public domain (government publications) |
| **Coverage** | 622 Ugandan Acts and statutory instruments |
| **Last ingested** | 2026-02-28 |

### Automated Freshness Checks

A [GitHub Actions workflow](.github/workflows/check-updates.yml) monitors Ugandan legal sources for changes:

| Check | Method |
|-------|--------|
| **Statute amendments** | Drift detection against known provision anchors |
| **New statutes** | Comparison against ULII index |
| **Repealed statutes** | Status change detection |

**Verified data only** -- every citation is validated against official sources. Zero LLM-generated content.

---

## Security

This project uses multiple layers of automated security scanning:

| Scanner | What It Does | Schedule |
|---------|-------------|----------|
| **CodeQL** | Static analysis for security vulnerabilities | Weekly + PRs |
| **Semgrep** | SAST scanning (OWASP top 10, secrets, TypeScript) | Every push |
| **Gitleaks** | Secret detection across git history | Every push |
| **Trivy** | CVE scanning on filesystem and npm dependencies | Daily |
| **Socket.dev** | Supply chain attack detection | PRs |
| **Dependabot** | Automated dependency updates | Weekly |

See [SECURITY.md](SECURITY.md) for the full policy and vulnerability reporting.

---

## Important Disclaimers

### Legal Advice

> **THIS TOOL IS NOT LEGAL ADVICE**
>
> Statute text is sourced from ULII and Parliament of Uganda official sources. However:
> - This is a **research tool**, not a substitute for professional legal counsel
> - **Court case coverage is not included** -- do not rely solely on this for case law research
> - **Verify critical citations** against primary sources before court filings
> - **International cross-references** reflect alignment relationships, not formal transposition
> - **Customary law and regional ordinances** may not be fully captured in this dataset

**Before using professionally, read:** [DISCLAIMER.md](DISCLAIMER.md) | [SECURITY.md](SECURITY.md)

### Client Confidentiality

Queries go through the Claude API. For privileged or confidential matters, use on-premise deployment.

### Bar Association Reference

For professional use, consult the **Uganda Law Society** guidelines on AI-assisted legal research.

---

## Development

### Setup

```bash
git clone https://github.com/Ansvar-Systems/Ugandan-law-mcp
cd Ugandan-law-mcp
npm install
npm run build
npm test
```

### Running Locally

```bash
npm run dev                                       # Start MCP server
npx @anthropic/mcp-inspector node dist/index.js   # Test with MCP Inspector
```

### Data Management

```bash
npm run ingest              # Ingest statutes from ulii.org
npm run build:db            # Rebuild SQLite database
npm run drift:detect        # Run drift detection against anchors
npm run check-updates       # Check for source updates
npm run census              # Generate coverage census
```

### Performance

- **Search Speed:** <100ms for most FTS5 queries
- **Database Size:** ~30 MB (efficient, portable)
- **Reliability:** 100% ingestion success rate across 622 laws

---

## Related Projects: Complete Compliance Suite

This server is part of **Ansvar's Compliance Suite** -- MCP servers that work together for end-to-end compliance coverage:

### [@ansvar/eu-regulations-mcp](https://github.com/Ansvar-Systems/EU_compliance_MCP)
**Query 49 EU regulations directly from Claude** -- GDPR, AI Act, DORA, NIS2, MiFID II, eIDAS, and more. Full regulatory text with article-level search. `npx @ansvar/eu-regulations-mcp`

### [@ansvar/us-regulations-mcp](https://github.com/Ansvar-Systems/US_Compliance_MCP)
**Query US federal and state compliance laws** -- HIPAA, CCPA, SOX, GLBA, FERPA, and more. `npx @ansvar/us-regulations-mcp`

### [@ansvar/security-controls-mcp](https://github.com/Ansvar-Systems/security-controls-mcp)
**Query 261 security frameworks** -- ISO 27001, NIST CSF, SOC 2, CIS Controls, SCF, and more. `npx @ansvar/security-controls-mcp`

**80+ national law MCPs** covering Tanzania, Namibia, Kenya, Nigeria, Ghana, South Africa, Dominican Republic, Sri Lanka, and more.

---

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Priority areas:
- Court case law expansion (Supreme Court, Court of Appeal, High Court from ULII)
- EAC treaty cross-references
- Constitutional Court decisions
- Historical statute versions and amendment tracking

---

## Roadmap

- [x] Core statute database with FTS5 search
- [x] Full corpus ingestion (622 laws, 17,024 provisions)
- [x] International law alignment tools
- [x] Vercel Streamable HTTP deployment
- [x] npm package publication
- [ ] Court case law expansion (ULII case law archive)
- [ ] EAC protocol cross-references
- [ ] Constitutional Court decisions
- [ ] Historical statute versions

---

## Citation

If you use this MCP server in academic research:

```bibtex
@software{ugandan_law_mcp_2026,
  author = {Ansvar Systems AB},
  title = {Ugandan Law MCP Server: AI-Powered Legal Research Tool},
  year = {2026},
  url = {https://github.com/Ansvar-Systems/Ugandan-law-mcp},
  note = {622 Ugandan laws with 17,024 provisions}
}
```

---

## License

Apache License 2.0. See [LICENSE](./LICENSE) for details.

### Data Licenses

- **Statutes & Legislation:** Government of Uganda (public domain)
- **International Metadata:** Public domain

---

## About Ansvar Systems

We build AI-accelerated compliance and legal research tools for the global market. This MCP server makes Ugandan law accessible to legal professionals and compliance teams worldwide.

So we're open-sourcing it. Navigating 622 Acts shouldn't require a law degree.

**[ansvar.eu](https://ansvar.eu)** -- Stockholm, Sweden

---

<p align="center">
  <sub>Built with care in Stockholm, Sweden</sub>
</p>
