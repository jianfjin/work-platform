# Security, privacy and marketplace trust

**Status:** proposed controls and launch gates · **Accountable roles:** tech lead, trust lead, privacy/legal and finance

## 1. Sensitive assets and trust boundaries

Protect client address/contact details, home-interior photos, private messages/offers, business verification evidence, account/session credentials and access-fund records. A paid entitlement is not blanket authority to see every field or participant.

Threat actors include an unpaid scraper, a paid competing business, a fraudulent job poster, an account thief, an abusive participant, a malicious upload author and an overprivileged staff account. Model these actors in authorization tests, not just anonymous versus logged-in users.

## 2. Access matrix

| Data/action | Anonymous | Approved carpenter, unpaid | Entitled firm | Owning client | Support | Finance |
|---|---|---|---|---|---|---|
| Marketing / opted-in public portfolio | Read | Read | Read | Read | Read | Read |
| Safe live-job preview | No | Read | Read | Own | Assigned review | No default |
| Full brief / job media | No | No | Permitted active/read-only history | Own | Case-scoped break-glass only | No |
| Shared scope Q&A | No | No | Own entitled jobs; writes per state | Own | Case-scoped | No |
| Private channel / offer | No | No | Own channel only | All their own job channels | Case-scoped break-glass only | No |
| Client contact/address | No | No | Only individually granted fields | Own | Case-scoped exceptional access | Billing data only where necessary |
| Balance / receipt | No | Own business | Own business | Not other users' finances | Masked case summary | Scoped financial records |
| Financial restoration/cash refund | No | Request own only | Request own only | No | Recommend/case decision as granted | Execute controlled workflow |
| Modify posted ledger history | No | No | No | No | No | No; append compensating entry only |

Suspension, job safety holds, deleted content and revoked grants further restrict access. Staff MFA does not itself grant permission to read private messages.

## 3. Threat/control/test matrix

| Threat | Control | Proof obligation |
|---|---|---|
| Unpaid preview leaks | Separate safe projection/DTO, server-only DAL, no full text/photos in HTML/JSON/RSC | Inspect raw network payloads and caches with unauthenticated/unpaid principals |
| Paid competitor reads another firm's offer | Channel/offer row ownership checked for every request and cursor | Cross-business IDOR test matrix including attachments and exports |
| Double spend / fourth participant | One database transaction, consistent job/wallet locks and unique constraints | Real PostgreSQL concurrent test with 50 contenders |
| Fake/replayed payment callback | Stored intent lookup + authenticated PSP fetch + unique grant effect | Replayed/unknown/changed amount/wrong environment callbacks never credit |
| Refund double payout | Source-lot cap and reservation, stable PSP attempt and verified outcome | Race spend versus refund; lost refund response; reconciliation test |
| Account takeover / staff misuse | Maintained auth, rate limits, secure cookies, CSRF checks, staff MFA, scoped roles and break-glass audit | Session/role expiry and support privilege escalation tests |
| Malicious upload / hidden contact in photo | Real MIME checks, quarantine/scan, safe transforms, EXIF removal, moderation | Executable/SVG/polyglot/oversized-pixel/PDF fixtures rejected or quarantined |
| Script injection / hostile links | Plain text or sanitized subset, output escaping, safe link handling and CSP | XSS fixtures in every text field and message/quote preview |
| Bulk scraping / phantom paid jobs | Verified contacts, publication review, business checks, rate/spend limits and duplicate detection | Abuse simulation; no fabricated jobs sold during seed recruitment |
| Unsafe notification / telemetry | Generic authenticated links, redacted metadata, no message replay analytics | Email/log/analytics payload inspection |
| Shared-board pricing leakage | Distinct composers, explicit audience, no automatic private-to-shared copy, report/moderation | UX audience test and shared endpoint contract tests |

## 4. Privacy by design

The proposed privacy baseline uses data minimization, purpose limitation, access controls, rights workflows and security appropriate to sensitive home/job data. The governing source is the [GDPR text](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679), particularly Articles 5, 6, 12–22, 25 and 32–35. Specific lawful bases, notices, retention and incident decisions must be recorded for this service before launch, not inferred from a generic checkbox.

Proposed processing register:

| Purpose/data | Proposed basis to review | Minimization/control |
|---|---|---|
| Account, job and access fulfillment | Contract/steps toward contract as applicable | Required fields only; coarse city for discovery |
| Financial records and billing | Contract and applicable legal obligations | Separate finance access/retention; no full chat copy |
| Abuse/fraud prevention and moderation | Legitimate-interest assessment or applicable obligation | Scoped risk signals, reviewed decisions, appeals |
| Public portfolio and optional contact disclosure | Documented user choice/appropriate basis | Default private, per-field/per-business grant, revocable future access |
| Marketing mail/optional analytics | Separate consent where required | Off by default; core service works without it |

Do not request government identity documents by default for a KVK check. If business ownership needs evidence, collect only the approved minimum through a restricted workflow. Disable session-replay tooling on jobs, messages, billing and authentication pages. Do not use client photos/messages to train an AI model.

Rights operations verify requester identity, provide an export with third-party redactions where needed, resolve unused paid funds and delete content independently from lawful financial retention. Target completion within 30 days as an internal workflow budget, while legal verifies applicable statutory calendar deadlines/extensions. Reapply deletion tombstones after any backup restore. Proposed operational retention periods are in [Data model](06-data-model.md).

## 5. Business trust and moderation

Verify business registration and account control before selling access. Clearly label what was checked, when and any verification expiry; do not market a registration check as a workmanship guarantee. Profiles list declared specialties and service radius. Recheck significant legal identity changes and define periodic review during G0.

Review all pilot jobs before publication. Confirm genuine scope/contact verification, detect duplicate listings, approve a safe preview and check consent for media. Detect exposed addresses/contact in free text/images and request correction. A rules-based duplicate suggestion can assist operators; no opaque fully automated bans.

Provide report routes for illegal content, fraud, harassment, privacy exposure, unsafe scope and review manipulation. Support assigns a case, applies a reversible hold, records a reasoned decision, notifies affected users appropriately and offers appeal. Urgent privacy exposure triggers immediate access restriction; ordinary commercial dissatisfaction is not automatically fraud.

Reviews require a platform-linked awarded job. Labels distinguish client-reported completion from bilateral confirmation; no proof of payment for construction exists in MVP. Do not delete negative reviews for paying businesses; apply the same published content rules. New entrants must not be demoted solely for lacking reviews; no paid ranking.

## 6. Compliance decisions that block paid launch

| Topic | Evidence / question | Required decision |
|---|---|---|
| Prepaid service / payment regulation | [DNB guidance](https://www.dnb.nl/en/sector-information/open-book-supervision/open-book-supervision-sectors/electronic-money-institutions/authorisation-as-an-electronic-money-institution-overview/services-not-requiring-authorisation/) explains that exclusions/exemptions are conditional | Legal classify the access balance, refundability and any notification/authorization duties; PSP approve actual model. Non-transferability alone is not legal clearance |
| VAT / invoicing | Access prepayment, possible voucher classification, restorations and cash refunds | Accountant approve rate, tax point, invoice/credit notes, rounding and retention; implement before accepting funds |
| DSA / trader transparency | [Commission DSA guidance](https://digital-strategy.ec.europa.eu/en/policies/digital-services-act) describes role/size-dependent duties | Determine platform category, applicable small-business rules, notices, contact point, trader verification and any required disclosures |
| P2B / ranking transparency | [Commission P2B page](https://digital-strategy.ec.europa.eu/en/policies/platform-business-trading-practices) reports a proposed repeal, not proof of enacted repeal | Check legislation in force at launch; retain transparent ranking, terms and moderation appeal design regardless |
| DAC7 | [Commission DAC7 guidance](https://taxation-customs.ec.europa.eu/taxation/tax-transparency-cooperation/administrative-co-operation-and-mutual-assistance/dac7_en) includes personal services in covered activities | Obtain a determination for this facilitation model; not handling construction payments does not by itself establish exemption. Scope any required data/reporting only after determination |
| Privacy / cookies / transfers | Home photos, contact grants, processors and telemetry | Approve notices, purposes, DPIA screening, processor contracts, region/subprocessors and consent controls |
| Consumer and business terms | Client posting free; professional access B2B; construction agreement direct | Verify contractual wording and applicable rights; Dutch/English policy review; transparent fees and no guaranteed-award wording |

These are **scope-assessment tasks**, not claims that all listed duties apply or that this draft establishes compliance. Future construction escrow, payouts or cross-border expansion requires a new architecture and legal review.

## 7. Operational response

Incident priorities: P0 private-data exposure, ledger corruption, forged paid fulfillment, or systematic duplicate charging; P1 broad messaging/payment outage; P2 isolated support defect. Name an on-call owner and backup before beta. Suspend affected purchases immediately for P0, preserve minimal evidence, revoke exposed capabilities, assess affected users and legal notification obligations, then reconcile before reopening.

Secrets live in a managed secret store, separated by environment; rotate PSP and storage credentials, restrict database network access, and audit staff changes. Dependencies receive vulnerability scanning and supported-version updates. Restore drills prove backup availability; they do not replace authorization tests. No production personal data in test fixtures or developer laptops by default.
