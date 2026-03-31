---
name: contract-drafter
description: |
  Draft, review, and explain contracts, NDAs, service agreements, terms of service,
  employment contracts, and other legal documents. Use when the user needs legal document
  creation or review. Always append legal disclaimer.
  Triggers:
  - "contract"
  - "NDA"
  - "non-disclosure agreement"
  - "service agreement"
  - "terms of service"
  - "privacy policy"
  - "legal document"
  - "employment agreement"
  - "freelance contract"
  - "MOU"
  - "memorandum of understanding"
  - "合同"
  - "保密协议"
  - "服务协议"
  - "隐私政策"
  - "用户协议"
---

# Contract Drafter — SKILL.md

> Draft and review legal documents in plain, clear language that protects all parties.

> ⚠️ **Always include this disclaimer**: "This document is a draft for review purposes only and does not constitute legal advice. Consult a qualified attorney before signing or relying on any legal document."

---

## Overview

You work from proven templates and fill them with specifics. Never draft critical legal clauses from imagination — use the frameworks below. Always flag risks and missing protections clearly.

---

## Phase 1 — Understand the Request

Gather before drafting:
- **Document type**: NDA, service agreement, employment, ToS, etc.
- **Parties**: Who signs? Individuals or companies?
- **Relationship**: Client/vendor, employer/employee, licensor/licensee?
- **Key terms**: Duration, payment, scope, IP ownership, termination, jurisdiction
- **Concerns**: What does the user most want to protect?

For **reviews**: ask for the full document text, then identify risks.

---

## Phase 2 — NDA Templates

### Mutual NDA

```markdown
MUTUAL NON-DISCLOSURE AGREEMENT

This Mutual Non-Disclosure Agreement ("Agreement") is entered into as of [DATE] by and between:

Party A: [FULL LEGAL NAME], a [company type] organized under the laws of [JURISDICTION] ("Party A")
Party B: [FULL LEGAL NAME], a [company type] organized under the laws of [JURISDICTION] ("Party B")

(each a "Party," collectively the "Parties")

1. PURPOSE
The Parties wish to explore a potential business relationship (the "Purpose") and may disclose confidential information to each other in connection with this Purpose.

2. DEFINITION OF CONFIDENTIAL INFORMATION
"Confidential Information" means any non-public information disclosed by either Party to the other, either directly or indirectly, in writing, orally, or by inspection of tangible objects, that is designated as confidential or that reasonably should be understood to be confidential given the nature of the information and circumstances of disclosure.

Confidential Information does not include information that:
(a) is or becomes publicly known through no breach of this Agreement;
(b) was rightfully known before receipt from the disclosing Party;
(c) is rightfully received from a third party without restriction;
(d) is independently developed without use of Confidential Information; or
(e) is required to be disclosed by law or court order (with prompt prior notice to the disclosing Party).

3. OBLIGATIONS
Each Party agrees to:
(a) hold the other Party's Confidential Information in strict confidence;
(b) not disclose it to third parties without prior written consent;
(c) use it solely for the Purpose;
(d) protect it with at least the same degree of care it uses to protect its own confidential information, but no less than reasonable care;
(e) limit access to those employees or contractors with a need to know.

4. TERM
This Agreement commences on the date first written above and continues for [DURATION, e.g., 2 years], unless earlier terminated by either Party upon [30] days written notice. Obligations of confidentiality survive termination for [3] years.

5. RETURN OF INFORMATION
Upon request, each Party will promptly return or destroy the other Party's Confidential Information, and certify in writing that it has done so.

6. NO LICENSE
Nothing in this Agreement grants any rights to patents, copyrights, trade secrets, or other intellectual property of either Party.

7. REMEDIES
Each Party acknowledges that breach of this Agreement may cause irreparable harm for which monetary damages would be inadequate, and agrees that injunctive relief may be sought without posting bond.

8. GOVERNING LAW
This Agreement shall be governed by the laws of [JURISDICTION], without regard to conflict of law provisions.

9. ENTIRE AGREEMENT
This Agreement constitutes the entire agreement between the Parties regarding its subject matter and supersedes all prior discussions and agreements.

PARTY A: _______________________ Date: _______
[Name], [Title]

PARTY B: _______________________ Date: _______
[Name], [Title]
```

### One-Way NDA (Discloser → Recipient)

Same structure as mutual, but Section 3 obligations apply only to the Recipient, and Section 1 identifies the Disclosing and Receiving Party explicitly.

---

## Phase 3 — Service Agreement Template

```markdown
SERVICE AGREEMENT

This Service Agreement ("Agreement") is entered into as of [DATE] between:

Client: [CLIENT NAME] ("Client")
Service Provider: [PROVIDER NAME] ("Provider")

1. SERVICES
Provider agrees to perform the following services ("Services"):
[Detailed description of deliverables, scope, and specifications]

2. TIMELINE
- Project Start: [DATE]
- Project End / Delivery: [DATE]
- Milestones: [List key milestones with dates if applicable]

3. COMPENSATION
3.1 Fee: Client agrees to pay Provider [AMOUNT] for the Services.
3.2 Payment Schedule: [e.g., 50% upon signing, 50% upon delivery]
3.3 Late Payment: Invoices unpaid after [30] days accrue interest at [1.5%] per month.
3.4 Expenses: [Included / Client reimburses pre-approved expenses]

4. INTELLECTUAL PROPERTY
[Option A — Work for hire]: All work product created under this Agreement is work-for-hire and shall be the sole property of Client upon full payment.
[Option B — License]: Provider retains ownership of all work product and grants Client a [exclusive/non-exclusive] license to use it for [purpose].
[Option C — Shared]: [Specify what Client owns vs. what Provider retains]

5. CONFIDENTIALITY
Each Party agrees to maintain the confidentiality of the other's non-public business information and not to disclose it to third parties during and for [2] years after this Agreement.

6. INDEPENDENT CONTRACTOR
Provider is an independent contractor, not an employee of Client. Provider is responsible for all taxes on compensation received.

7. TERMINATION
Either Party may terminate this Agreement with [30] days written notice. Upon termination, Client shall pay for all Services completed to date of termination.

8. LIMITATION OF LIABILITY
Provider's total liability shall not exceed the total fees paid under this Agreement. Neither Party shall be liable for indirect, incidental, or consequential damages.

9. WARRANTIES
Provider warrants that: (a) it has the right to enter this Agreement; (b) the Services will be performed professionally; (c) deliverables will not infringe third-party IP rights.

10. DISPUTE RESOLUTION
Disputes shall be resolved by [arbitration / mediation / courts] in [JURISDICTION] under the laws of [JURISDICTION].

CLIENT: _______________________ Date: _______
PROVIDER: ______________________ Date: _______
```

---

## Phase 4 — Freelance / Contractor Agreement

Key clauses beyond the Service Agreement:

```markdown
INTELLECTUAL PROPERTY ASSIGNMENT
Contractor irrevocably assigns to Client all right, title, and interest in all work product, including inventions, developments, and materials created under this Agreement, upon full payment of fees.

NON-SOLICITATION
During this Agreement and for [12] months thereafter, Contractor shall not solicit Client's employees or clients for competitive purposes.

REPRESENTATIONS
Contractor represents that: (a) no prior obligations conflict with this Agreement; (b) no third-party IP will be incorporated without written consent; (c) Contractor has all necessary skills to perform the Services.
```

---

## Phase 5 — Terms of Service / Privacy Policy

### Terms of Service Key Sections

```markdown
TERMS OF SERVICE — [PRODUCT NAME]

Last Updated: [DATE]

1. ACCEPTANCE
By accessing or using [Product], you agree to these Terms. If you disagree, do not use the Service.

2. DESCRIPTION OF SERVICE
[Brief description of what the product does]

3. USER ACCOUNTS
- You must provide accurate registration information
- You are responsible for maintaining account security
- Notify us immediately of unauthorized access

4. ACCEPTABLE USE
You may not:
- Use the Service for illegal purposes
- Attempt to gain unauthorized access
- Transmit malware or disruptive code
- Violate others' intellectual property rights
- [Product-specific restrictions]

5. INTELLECTUAL PROPERTY
[Company] retains all rights to the Service, trademarks, and content. Users retain rights to their own content and grant [Company] a license to host and display it.

6. USER CONTENT
By submitting content, you grant [Company] a worldwide, non-exclusive license to use, reproduce, and distribute it in connection with the Service.

7. PRIVACY
Your use of the Service is subject to our Privacy Policy [link].

8. PAYMENT TERMS (if applicable)
[Pricing, billing cycle, refund policy]

9. TERMINATION
We may suspend or terminate your account for violations of these Terms. You may cancel at any time.

10. DISCLAIMER OF WARRANTIES
THE SERVICE IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND.

11. LIMITATION OF LIABILITY
[Company]'s liability is limited to [the amount paid in the prior 12 months / $X].

12. CHANGES
We may update these Terms and will notify you of material changes.

13. GOVERNING LAW
[Jurisdiction]
```

---

## Phase 6 — Contract Review Framework

When reviewing an existing contract, assess:

### Red Flags to Flag
- **No limitation of liability clause** → Unlimited exposure
- **Asymmetric IP assignment** → You assign everything, they assign nothing
- **Perpetual, irrevocable licenses** → You can't get your content back
- **Auto-renewal with no exit** → Hard to terminate
- **Unilateral amendment right** → They can change terms without notice
- **Broad non-compete** → Overly restrictive geographic/time scope
- **No payment protection** → No late payment interest, no kill fee
- **Vague scope** → "Such other services as needed" = scope creep
- **No force majeure** → No protection for events outside your control

### Summary Format for Reviews

```markdown
## Contract Review: [Document Name]

**Overall risk level**: 🔴 High / 🟡 Medium / 🟢 Low

### Key Terms
- Parties: [A] and [B]
- Duration: [X]
- Governing law: [Jurisdiction]
- Value: [$ amount]

### ✅ Protections in Place
- [Clause that protects the user]

### ⚠️ Concerns
| Issue | Clause | Risk | Suggested Fix |
|-------|--------|------|---------------|
| [Issue] | Section X | [Risk description] | [Suggested language] |

### ❌ Missing Clauses
- [Missing protection]: Recommend adding [X]

### Plain-Language Summary
[3-5 sentences explaining what this contract does and key obligations]
```

---

## Quality Standards

**Every document must:**
- Have complete party identification (full legal names)
- Define all key terms on first use
- Have a governing law clause
- Have a termination clause
- Include the standard disclaimer

**Signs a draft needs revision:**
- Blank `[PLACEHOLDER]` fields left unfilled
- Jurisdiction not specified
- IP ownership ambiguous
- No termination rights for either party

---

## ⚠️ Standard Disclaimer

Always append to every document or review:

> **Disclaimer**: This document is prepared for informational and drafting purposes only. It does not constitute legal advice and should not be relied upon as such. Laws vary by jurisdiction. Always consult a qualified attorney before signing or relying on any legal document.

---

## Word Document Export

After delivering the final contract Markdown, automatically generate a .docx file. Do not wait for the user to ask.

```bash
python .codebanana/.skills/contract-drafter/export_docx.py contracts/<filename>.md
```

**Output:** `contracts/<filename>.docx`

**Confirm to user:**
```
📄 Contract Exported
━━━━━━━━━━━━━━━━━━━━━━━━━━━
📁 contracts/<filename>.docx
   Ready to sign in Word or Google Docs
━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
