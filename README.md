# AI Use Case Risk Assessment

A rules-based intake and risk-tiering tool for AI use cases. Someone describes how they want to use AI, answers five short sections of questions, and gets:

- An **EU AI Act classification**: prohibited, high-risk, limited risk (transparency) or minimal risk, with the article that drives it
- An **inherent risk score (0–100)** built from ten NIST AI RMF–aligned factors
- A **final risk tier**: Low, Moderate, High, Critical or Prohibited
- The **controls required before launch**, tagged to NIST AI RMF functions and AI Act articles
- **US and privacy flags** (GDPR Art. 22, NYC Local Law 144, Colorado AI Act, COPPA, GLBA/ECOA)
- The **approval path** and **reassessment cadence**
- A **copyable assessment summary** for the AI inventory or ticket

**[Try it live →](https://cmp2627.github.io/ai-use-case-risk-assessment/)**

Four built-in examples show each outcome: a résumé screener (High), a customer support chatbot (Moderate), an internal coding assistant (Low) and a call-center mood monitor (Prohibited).

## Why I built it

AI governance platforms like OneTrust, LogicGate and ServiceNow give you an intake workflow, but each organization still has to design the logic inside it: how to score risk, where to set tiers, which controls each tier triggers. Free EU AI Act checkers stop at the legal classification. This project is that missing design layer, built so every result shows its reasoning.

## How the logic works

### 1. Capability screen: no self-incrimination required
Nobody checks a box that says "uses manipulative techniques that cause significant harm." So the intake never asks whether a system is prohibited. It asks neutral questions about what the system *does* (detects emotions, identifies people from biometrics, tailors offers by financial situation, scores trustworthiness, predicts crime) with follow-ups that only appear when relevant (what it's for, who it's used on, how the output is used). The engine combines those answers with the rest of the intake to decide:

- **Likely prohibited (Art. 5).** Example: emotion detection + used on employees + not for medical or safety reasons → Art. 5(1)(f).
- **High-risk biometric use (Annex III §1).** Example: remote identification, emotion recognition outside work or school.
- **Open question.** A checked capability with an unanswered follow-up holds the tier at Critical until it's answered.
- **Red flags** that aren't Art. 5 but raise the tier to at least High. Example: automated decisions about people with no human review (GDPR Art. 22, California ADMT rules, Colorado AI Act), or sensitive data sent to a vendor that may train on it.

### 2. Regulatory classification (EU AI Act), in order
1. **Prohibited practices (Art. 5),** derived from the capability screen. Any match stops the assessment.
2. **High-risk (Art. 6).** Safety components of regulated products (Annex I), or use in a listed area such as employment, education, credit, insurance, essential services, biometrics, critical infrastructure or law enforcement (Annex III).
3. **Art. 6(3) exception.** An Annex III use can avoid high-risk status if it only performs a narrow procedural or preparatory task. The exception is blocked whenever the system profiles individuals.
4. **Transparency (Art. 50).** Chatbots people interact with directly, and AI-generated content shared externally.
5. Otherwise **minimal risk**.

### 3. Inherent risk score
Ten factors, normalized to 0–100:

| Factor | Max points | NIST AI RMF |
|---|---|---|
| Decision impact (informational → automated decision) | 20 | MAP |
| Data sensitivity (public → children's data) | 18 | MAP |
| Human oversight (every output reviewed → none) | 16 | MANAGE |
| Affected people (employees → vulnerable groups) | 14 | MAP |
| Domain is high-risk or a consequential decision | 12 | MAP |
| Harm if wrong (easy to reverse → irreversible) | 12 | MEASURE |
| Supply chain (source + vendor training on your data) | 11 | GOVERN |
| Scale (people affected per year) | 10 | MAP |
| Profiles individuals | 8 | MEASURE |
| Exposure (internal → general public) | 8 | GOVERN |

Score bands: **Low** < 25 · **Moderate** 25–49 · **High** 50–74 · **Critical** ≥ 75.

### 4. Final tier: the law sets a floor
The final tier is the highest of the score band, the regulatory floor, and High if any red flag is present:

| AI Act classification | Minimum tier |
|---|---|
| Prohibited | Prohibited (blocked) |
| Possible prohibited (open follow-up) | Critical |
| High-risk | High |
| Limited risk / Art. 6(3) exception claimed | Moderate |
| Minimal risk | Low |

A use case can score low on the organization's own factors and still be High because the law says so. The panel says when that happens.

### 5. Controls and governance
Controls AI-01 to AI-20 are triggered by tier and by specific answers. Examples: a vendor assessment whenever a third-party model is used, a DPIA when personal data is involved, bias testing for profiling or consequential decisions, and separate provider (Art. 9–11, 43, 49) vs. deployer (Art. 26) obligations for high-risk systems.

| Tier | Approval | Reassess |
|---|---|---|
| Low | Business owner self-attests | 24 months |
| Moderate | AI governance lead (+ Privacy if personal data) | 12 months |
| High | AI governance committee with Legal and Privacy | 6 months |
| Critical | Executive risk owner + committee | Quarterly |
| Prohibited | Blocked; escalate to Legal | — |

## Regulatory dates

EU AI Act dates reflect the 2026 Digital AI Omnibus amendments:
- Prohibitions (Art. 5): applying since 2 Feb 2025
- Transparency duties (Art. 50): 2 Aug 2026
- Annex III high-risk obligations: 2 Dec 2027
- Annex I (product-embedded) high-risk obligations: 2 Aug 2028

## Limitations

- Decision support only. It isn't legal advice and doesn't replace a legal review.
- Prohibited practices reflect Art. 5 as originally enacted; practices added by the 2026 Omnibus aren't encoded yet.
- Scoring weights are a reasoned starting point, meant to be calibrated to an organization's risk appetite.
- US state AI laws change quickly; flags point to laws to check rather than determining applicability.

## What I'd add next

- Export to a JSON schema compatible with GRC platform intake forms (OneTrust, ServiceNow)
- An AI vendor addendum linked to a third-party risk management (TPRM) workflow
- A saved AI inventory with review dates
- A small-practice edition in plain language, with HIPAA, FTC Safeguards and ABA Opinion 512 rule packs

## Tech

One self-contained HTML file: vanilla JavaScript, no build step, no data leaves the browser. Open `index.html` locally or serve it with GitHub Pages.

## Author

**Christine Portieles**, CISA, CISM · Juris Master, Cybersecurity, Privacy & Technology Risk Management (FSU College of Law)
