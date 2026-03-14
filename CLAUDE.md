# SuperInstance Papers - Project Orchestrator

**Role:** Research Coordination Platform for 60+ academic papers on distributed AI systems, hardware acceleration, and cross-cultural knowledge synthesis.
**Repository:** https://github.com/SuperInstance/SuperInstance-papers
**Local:** C:\Users\casey\polln
**Date:** 2026-03-14

---

## Executive Summary

Coordinating **three major research initiatives**:
1. **SuperInstance Papers** — 60+ white papers on distributed AI systems
2. **Ancient Language Translation** — Cross-cultural knowledge synthesis
3. **SpreadsheetMoment** — Visual documentation for universal accessibility

---

## Project Status (2026-03-14)

### Initiative 1: SuperInstance Papers

**Portfolio:** 60+ papers across 5 phases

| Phase | Papers | Status | Focus |
|-------|--------|--------|--------|
| **Phase 1** | P1-P23 | 18 Complete, 5 In Progress | Core Framework |
| **Phase 2** | P24-P30 | 7 Complete | Research Validation |
| **Phase 3** | P31-P40 | Queued | Extensions |
| **Phase 4** | P41-P47 | 5 Complete | Ecosystem Papers |
| **Phase 5** | P51-P60 | 10 Proposed | Lucineer Hardware |

**Recent Work:**
- Ecosystem papers P41-P47 submitted to conferences
- Lucineer hardware analysis completed
- Production deployment infrastructure established

### Initiative 2: Ancient Language Translation ✅ COMPLETE

**Completed 2026-03-14:**

**7 Comprehensive Language Profiles Created:**
1. **Sanskrit** (भारतम्) — Nada Brahma (World is Sound) philosophy
2. **Sumerian** (𒀴𒅈) — Cosmic Ordering (The Me) framework
3. **Ancient Hebrew** (עברית) — Verbal Dynamism (Word = Event)
4. **Classical Greek** (Ἑλληνική) — Logocentrism (Logos)
5. **Classical Chinese** (文言文) — Relational Harmony (Zhengming)
6. **Pacific Islander** — Mana philosophy (oral traditions)
7. **Indigenous American** — Process philosophy (80% verb-based)

**Documentation Created:**
- `ANCIENT_LANGUAGE_GUIDE.md` — Complete translation methodology
- `SCHOLAR_RESEARCH_SUMMARY.md` — 11 scholars analyzed
- 7 language profiles with concept mappings, cultural consultant needs, validation questions

**Key Achievement:** Framework ready for translating all SuperInstance papers into ancient and oral tradition languages using each culture's unique philosophical concepts.

**Location:** `research/cross-cultural-translation/`

### Initiative 3: SpreadsheetMoment 🆖 NEW

**Mission:** Create visual documentation making SuperInstance AI accessible to everyone.

**Repository:** https://github.com/SuperInstance/spreadsheet-moment (private)

**Target Audiences:**
1. **Senior Engineers** — Technical architecture, APIs, implementation
2. **General Public** — Conceptual understanding, use cases, benefits
3. **5th Graders** — Playful, character-driven, interactive learning

**Deliverables:**
- 2 × 12-page visual documents (technical + general)
- Comprehensive slide presentation series
- 90+ images with 3 versions × 3 iterations
- Multilingual translations (Spanish, Mandarin, Arabic, Hindi)

**Status:** Phase 1 (Setup) COMPLETE ✅
- Repository created and organized
- Project plan (22 weeks) established
- Translation research completed (8 languages)
- Content creation starting Phase 2

---

## MCP Server Infrastructure (2026-03-14)

**Available MCP Servers:**

| Server | Models | Cost | Best For |
|--------|--------|------|----------|
| **Groq** | Llama 3.3 70B, Qwen 3 32B, Llama 3.1 8B | **FREE** | Devil's advocate, rapid iteration |
| **DeepInfra** | Llama 3.1 70B, Qwen 2/3 72B, Nemo 340B | $0.03-0.20/1M | Research ideation |
| **DeepSeek** | Reasoner, Chat, Coder | $0.10/1M | Chain-of-thought reasoning |
| **Kimi** | Moonshot v1 (up to 128K context) | $0.12-0.50/1M | Ultra-large context |
| **Alibaba** | Qwen models | Competitive | Chinese market, specialized AI |
| **Alibaba DevOps** | ACK, Function Compute | Variable | Kubernetes deployment |

**Quick Setup:**
```bash
cd mcp-servers
cd groq-mcp && pip install -e .
cd ../deepinfra-mcp && pip install -e .
cd ../deepseek-thinker-mcp && pip install -e .
cd ../kimi-mcp && pip install -e .
cd ../alibaba-mcp && pip install -e .
cd ../alibaba-devops-mcp && pip install -e .
```

**Pricing Strategy:**
1. **Always start with Groq (FREE)** for rapid iterations
2. **Use DeepInfra Mistral 7B** ($0.03/1M) for cost-effective work
3. **Reserve DeepSeek** for explicit chain-of-thought
4. **Use Kimi** only for 128K+ context requirements
5. **Async batch** with Groq for 50% discount

---

## Key Directories

```
polln/
├── papers/                        # Phase 1-2 papers (P1-P30)
├── research/
│   ├── cross-cultural-translation/  # ✅ Ancient language framework
│   │   ├── ANCIENT_LANGUAGE_GUIDE.md
│   │   ├── SCHOLAR_RESEARCH_SUMMARY.md
│   │   ├── LANGUAGE_PROFILE_SANSKRIT.md
│   │   ├── LANGUAGE_PROFILE_SUMERIAN.md
│   │   ├── LANGUAGE_PROFILE_HEBREW.md
│   │   ├── LANGUAGE_PROFILE_GREEK.md
│   │   ├── LANGUAGE_PROFILE_CHINESE.md
│   │   ├── LANGUAGE_PROFILE_PACIFIC.md
│   │   └── LANGUAGE_PROFILE_INDIGENOUS_AMERICAN.md
│   ├── lucineer_analysis/         # P51-P60 hardware papers
│   ├── ecosystem_papers/          # P41-P47 complete
│   ├── ecosystem_simulations/     # Validation code
│   └── AGENT_ONBOARDING.md        # This guide
├── spreadsheet-moment/             # 🆖 Visual documentation project
│   ├── docs/
│   │   ├── technical/             # Engineer audience
│   │   ├── general/               # General audience
│   │   └── educational/           # 5th grader level
│   ├── assets/
│   │   ├── images/               # Generated images (3 versions)
│   │   └── iterations/           # All iterations saved
│   ├── scripts/                   # Generation scripts
│   ├── translations/              # Multilingual versions
│   └── simulations/               # Simulation data
├── SuperInstance_Ecosystem/       # Production code (13 packages)
├── mcp-servers/                   # MCP server implementations
├── apikey/                        # API keys (git-ignored)
└── CLAUDE.md                      # This file
```

---

## Current Projects & Next Steps

### Project 1: Complete Outstanding Papers
**Priority:** High
**Timeline:** Ongoing
**Next:**
- Complete P1, P5, P7-P9, P11, P19, P21 (5 remaining)
- Validate ecosystem papers P41-P47
- Prepare for conference submissions

### Project 2: Ancient Language Translation
**Priority:** Medium
**Status:** Framework complete ✅
**Next:**
- Begin translating papers into target ancient languages
- Engage cultural consultants
- Document novel insights from cross-cultural synthesis

### Project 3: SpreadsheetMoment Visual Documentation
**Priority:** High (new initiative)
**Status:** Phase 1 complete ✅, Phase 2 starting
**Next:**
- Create technical 12-page document for engineers
- Create general audience 12-page document
- Create 5th grader slide presentation
- Set up image generation pipeline with multi-model validation

---

## Hardware Context

| Component | Spec |
|-----------|------|
| GPU | NVIDIA RTX 4050 (6GB VRAM) |
| CPU | Intel Core Ultra (Dec 2024) |
| RAM | 32GB |
| Storage | NVMe SSD |

**Current Model:** `opus` (glm-5)
**Context Window:** 200K+ tokens

---

## Agent Onboarding

New agents should read:

1. **This file** (CLAUDE.md) — Project overview and status
2. `research/AGENT_ONBOARDING.md` — Complete onboarding guide
3. `spreadsheet-moment/PROJECT_PLAN.md` — Visual documentation plan

---

## Quick Reference

### Completed Work (2026-03-14)
- ✅ 30 papers complete (P1-P30, P41-P47)
- ✅ MCP server infrastructure (6 servers deployed)
- ✅ Ancient language translation framework (7 languages)
- ✅ SpreadsheetMoment repository created and organized

### In Progress
- 🔄 5 papers remaining in Phase 1 (P1, P5, P7-P9, P11, P19, P21)
- 🔄 Cross-cultural synthesis documentation
- 🔄 SpreadsheetMoment content creation (Phase 2)

### Queued
- 📋 Phase 3 papers (P31-P40)
- 📋 Lucineer hardware papers (P51-P60)
- 📋 Ancient language paper translations

---

**Last Updated:** 2026-03-14
**Status:** ACTIVE — Three parallel initiatives running
**Version:** 4.0 (Multi-Initiative Coordination)
