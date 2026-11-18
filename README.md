# AI Use Case Risk Assessment

A rules-based intake and risk-tiering tool for AI use cases. Someone describes how they want to use AI, answers five short sections of questions, and gets:

- A **risk tier** (Low, Moderate, High, Critical or Prohibited) with the reasons behind it in plain language
- An **inherent risk score (0–100)** built from ten NIST AI RMF–aligned factors
- **Red flags**, such as automated decisions about people with no human review
- The **controls required before launch**, tagged to NIST AI RMF functions
- The **laws and frameworks that apply**, filtered to where the affected people are (EU AI Act, GDPR, California ADMT rules, Colorado AI Act, NYC Local Law 144, Title VII, GLBA/ECOA/FCRA, COPPA)
- The **approval path** and **reassessment cadence**
- A **copyable assessment summary** for the AI inventory or ticket

**[Try it live →](https://cmp2627.github.io/ai-use-case-risk-assessment/)**

Four built-in examples show each outcome: a résumé screener (High), a customer support chatbot (Moderate), an internal coding assistant (Low) and a call-center mood monitor (Prohibited).

## Why I built it

AI governance platforms like OneTrust, LogicGate and ServiceNow give you an intake workflow, but each organization still has to design the logic inside it: how to score risk, where to set tiers, which controls each tier triggers. Free regulatory checkers answer a single legal question for a single jurisdiction. This project is the missing design layer: a general AI risk assessment, built so every result shows its reasoning, that brings in specific laws only where they apply.

## Design principles

1. **Risk comes from the use, not the jurisdiction.** The tier depends on what the system does, whose lives it touches and how much control people keep. A résumé screener is high-impact in Ohio and in Germany; only the legal obligations differ.
2. **Laws are references, not the framework.** The intake asks where the affected people are, and the results list only the laws for those places.
3. **No self-incrimination required.** Nobody checks a box that says "uses manipulative techniques that cause significant harm." The intake asks neutral questions about capabilities and lets the engine decide.
4. **Every result explains itself.** The tier, its drivers, and anything that raised it are shown on screen.

## How the logic works

### 1. Capability screen
Section 02 asks what the system *does* (detects emotions, identifies people from biometrics, tailors offers by financial situation, scores trustworthiness, predicts crime). Follow-ups appear only when relevant: what it's for, who it's used on, how the output is used. The engine combines those answers with the rest of the intake:

- **Unacceptable risk.** Example: emotion detection + used on employees + not for medical or safety reasons. Uses like this are banned in the EU (AI Act Art. 5) and break most AI acceptable-use policies.
- **High-impact biometric use.** Example: identifying people from a crowd or database.
- **Open question.** A checked capability with an unanswered follow-up holds the tier at Critical until it's answered.

### 2. Red flags
Answer combinations that raise the tier to at least High, wherever the system is used:
- Automated decisions about people with no human review
- Sensitive or children's data sent to a vendor that can or may train on it
- Profiling vulnerable people or children with no human oversight

### 3. Risk classification

| Classification | When | Minimum tier |
|---|---|---|
| Unacceptable risk | A banned capability combination | Prohibited (blocked) |
| Possible unacceptable risk | A capability follow-up is unanswered | Critical |
| High-impact use | Output affects jobs, education, credit, insurance, housing, health care, public benefits, biometrics, critical infrastructure, justice or product safety | High |
| Supporting role in a high-impact area | Only a narrow procedural task, and no profiling | Moderate |
| Transparency required | People talk to it directly, or its content leaves the company | Moderate |
| Standard use | None of the above | Low |

### 4. Inherent risk score
Ten factors, normalized to 0–100:

| Factor | Max points | NIST AI RMF |
|---|---|---|
| Decision impact (informational → automated decision) | 20 | MAP |
| Data sensitivity (public → children's data) | 18 | MAP |
| Human oversight (every output reviewed → none) | 16 | MANAGE |
| Affected people (employees → vulnerable groups) | 14 | MAP |
| High-impact domain | 12 | MAP |
| Harm if wrong (easy to reverse → irreversible) | 12 | MEASURE |
| Supply chain (source + vendor training on your data) | 11 | GOVERN |
| Scale (people affected per year) | 10 | MAP |
| Profiles individuals | 8 | MEASURE |
| Exposure (internal → general public) | 8 | GOVERN |

Score bands: **Low** < 25 · **Moderate** 25–49 · **High** 50–74 · **Critical** ≥ 75.

### 5. Final tier
The final tier is the highest of the score band, the classification's minimum tier, and High if any red flag is present. A use case can score low on the factors and still be High because of what it's used for; the panel says when that happens.

### 6. Laws and frameworks
Shown only for the places selected in section 01:

| Where | What it adds |
|---|---|
| EU | EU AI Act category, articles and dates; GDPR (DPIA, Art. 22 automated decisions, Art. 9 sensitive data) |
| UK | UK GDPR |
| California | CCPA automated decision-making (ADMT) rules for significant decisions |
| Colorado | Colorado AI Act duties for consequential decisions |
| New York City | Local Law 144 bias audits for hiring tools |
| United States | Title VII (hiring), GLBA/ECOA/FCRA (financial and credit), COPPA (children), state privacy laws (sensitive data) |
| Always | NIST AI RMF function tags; ISO/IEC 42001 impact assessment record |

Location-specific controls appear only when relevant, for example EU conformity assessment, notice and appeal rights for California and Colorado, and NYC bias audits.

### 7. Governance

| Tier | Approval | Reassess |
|---|---|---|
| Low | Business owner self-attests | 24 months |
| Moderate | AI governance lead (+ Privacy if personal data) | 12 months |
| High | AI governance committee with Legal and Privacy | 6 months |
| Critical | Executive risk owner + committee | Quarterly |
| Prohibited | Blocked; escalate to Legal | — |

## Limitations

- Decision support only. It isn't legal advice and doesn't replace a legal review.
- Laws change quickly. EU AI Act dates reflect the 2026 Digital AI Omnibus; practices it added to Art. 5 aren't encoded yet. US state law references point to laws to check rather than deciding applicability.
- Scoring weights are a reasoned starting point, meant to be calibrated to an organization's risk appetite.

## What I'd add next

- Export to a JSON schema compatible with GRC platform intake forms (OneTrust, ServiceNow)
- An AI vendor addendum linked to a third-party risk management (TPRM) workflow
- A saved AI inventory with review dates
- A small-practice edition in plain language, with HIPAA, FTC Safeguards and ABA Opinion 512 rule packs

## Tech

One self-contained HTML file: vanilla JavaScript, no build step, no data leaves the browser. Open `index.html` locally or serve it with GitHub Pages.

## Author

**Christine Portieles**, CISA, CISM · Juris Master, Cybersecurity, Privacy & Technology Risk Management (FSU College of Law)
