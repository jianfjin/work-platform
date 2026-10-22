# UX and service blueprint
**Version:** 0.3 · **Updated:** 21 October 2026

## 1. Navigation and promises
Visitor: How it works, Regions/trades, For companies, Help, Dutch/English, **Post free**.
Client guest: job-management link -> My task, Offers, Contact preference, Close/delete.
Company: Tasks, My offers/payments, Profile/radius, Email alerts, Help.
Operator: Reports, Fee exceptions, Deletion/alerts health, Accounts and audit.

No client payment screen. Company task cards say “Reading is free; first published offer €1.00 incl. VAT.” Never label a company “verified” merely because an email was confirmed.

## 2. Client posting
1. Choose category and enter a short description. Optional budget/timing/measurements; avoid a long technical questionnaire.
2. Enter postcode and city; show normalized match and pilot eligibility. Full postcode is private; companies see city/PC4 area and approximate distance.
3. Add up to six optional photos, maximum 10 MB each; show the remaining count, per-file upload progress and clear count/size errors. Explain address/face/contact risks; photo selection is optional. Confirmed inputs are JPEG, PNG, WebP and iPhone HEIC, automatically converted to sanitized JPEG/WebP for display; show processing state and clear unsupported/corrupt-file errors. Scan/metadata removal and real iPhone behavior require qualification.
4. Enter email for a one-use management link, **not compulsory registration**. Phone is requested only if the client wants telephone callbacks.
5. Select contact preference. Default “I will contact the companies.” Alternative explains that all published responders may see selected fields; consent is separate, unchecked and editable.
6. Review safe brief, deletion date and intermediary/no-quality-guarantee wording. Confirm email and publish after automated content/media checks. Flagged material goes to exception review; no promise that every post is manually verified.
7. Generic notice tells the client an offer exists. Reopen management securely through the task-scoped email flow. Optional client accounts and multi-task account dashboards are deferred until after MVP; no client signup/linking prompt in this release.

Confirmed guest-ownership rule: keep the task unpublished until email confirmation, use a short-lived email link for task-scoped management, and provide fresh-link recovery without registration. See [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md). The technical design uses one-use link exchange into a session, rather than a permanently privileged management URL; recovery responses remain generic.

## 3. Company response
1. Register profile, confirm email ownership, set trades/base postcode and radius, optionally subscribe to matched alerts.
2. Read safe task/approved photos free. Check actual category, location approximation and current open/deletion status.
3. Draft introduction, rough bid/range or “site visit required,” availability, own website/contact details.
4. Show €1 gross, what publication buys, no guaranteed work/contact/reply, no fixed competitor limit and refund conditions. Client contact is not automatically included.
5. Hosted checkout -> authenticated server status pending/paid. Only verified payment plus an eligible open task publishes the offer.
6. If task closed/expired/materially changed before publication, do not post; show refund pending, not a wallet credit or a second purchase prompt.
7. Company sees own offer/status/receipt only. Active-offer edits cost nothing. Before withdrawal, explain that it is final for this company/task in MVP, ends callback access, retains the original fee without automatic refund, and permits no republishing/new checkout. The client then sees only the company name and withdrawn marker; offer text and company contacts are hidden. Other company participation/count is never shown.

## 4. Client comparison and direct connection
All published offers appear in a paginated private list, default received order, with each firm's self-declared name, introduction, own website/email/phone, rough bid assumptions and availability. Do not rank the lowest amount as objectively best or expose the list through public URLs.

Client may click email/phone/website, arrange a site visit and negotiate final work/payment directly. Such clicks are navigation signals, not work awards or completed sales.

The founder confirmed CLIENT_INITIATED as default and ALLOW_RESPONDERS as explicit selected email/phone access for every eligible published responder while the task is open; see [ADR 0003](../docs/adr/0003-client-controlled-responder-contact.md). Explain the concrete audience: twelve published offers means all twelve companies may access selected fields, without showing that count to companies. Show that details already copied cannot be recalled. Turning it off or closing stops future platform disclosure.

## 5. Wireframes
```text
[Work Platform - working name] [NL | EN]
Local companies for home maintenance
[Post your task free] [I run a company]
Groningen / Friesland-Fryslân / Drachten pilot
No account required to post; secure email link to manage.
```

```text
Company task view
Replace internal doors · Drachten · PC4 area
Description + sanitized photos · approximate distance
Closes when client chooses; data deleted on [date]
Reading: free       [Prepare an offer]
First successfully published offer: EUR 1.00 incl. VAT
No guaranteed work. No competitor list/count.
```

```text
Client-only task page
[Contact preference] [Close task] [Delete task]
Offers - received order - page controls
Firm A: introduction / rough range / own contacts / website
Firm B: introduction / visit required / own contacts / website
These offers are private to you, not visible to other firms.
```

## 6. Required recovery states
| State | UI and operation |
|---|---|
| Guest email pending/link expired | Explain no account is required; resend with rate limits, generic success; preserve only safe temporary draft |
| Wrong postcode/city/outside trial | Ask correction or show waitlist; no silent nationwide publication |
| Payment pending/browser return | Show server-known status; reauthenticate if needed; no redirect-based success |
| Paid but task unavailable | No publication; full €1 refund workflow and case/reference |
| Duplicate click | Reuse operation; recover original offer/receipt; no double fee |
| Material edit before offer publication | Explain scope change; if payment captured, show full refund status and no publication; unpaid attempts cannot publish against old scope; any new attempt needs deliberate review |
| Material edit after offer publication | Keep offer visible with scope-change label; allow company to review current scope and edit free; no automatic refund or extra fee |
| Task closed | No new response/contact grant/alert; client sees prior permitted offers until deadline |
| Reopen voluntary closure | Before original expiry only; preserve offers/fees without re-charge, keep client contact private until fresh opt-in, and recover/refund invalid old payment attempts without reviving them |
| Published offer withdrawn | Client gets company name and withdrawn marker only; no offer text/company contacts or callback access, automatic refund, restoration, republishing or another checkout; task reopening preserves withdrawal |
| Callback disabled | Company contact section no longer returns client fields; direct previously copied contacts cannot be recalled |
| Deletion deadline | Content unavailable even before physical purge completes; no misleading recover button |
| Offline | Public fallback only; no private offers/balance cached, queued payment or automatic message |
| App suspended/update waiting | Hide stale private data until revalidated; save online/discard before reload |
| Misbehavior report | Case reference, safety action/status and correction/appeal; no implied site investigation |

## 7. PWA and localization
Desktop, Android and iPhone share accounts/server records. Installation is optional; no permission gate for ordinary use. Standalone screens need own back/home navigation, safe-area/keyboard handling and file-selection fallback. In-memory unsent drafts may be lost on reload/OS termination; saved server drafts are not offline replicas.

Dutch/English UI/mail follow user's language. Do not auto-translate or modify an offer's meaning silently. Example Dutch labels: “Plaats gratis je klus”, “Offerte indienen: €1,00 incl. btw”, “Ik neem zelf contact op”, “Bedrijven die reageren mogen contact opnemen”, “Sluit mijn klus”, “Gegevens verwijderd op [datum]”. Native-speaker review before release.

## 8. Service handoffs
Automated publication handles confirmed email and safe content/media; exception queue handles suspected abuse/privacy, not trade certification. Payment worker verifies PSP state and atomically publishes or routes refund. Alert worker matches opt-in radius/category and rechecks eligibility at send. Privacy worker denies at deadline and purges live content/objects. Operators reconcile fees and handle notices/reports; they do not negotiate building work.
