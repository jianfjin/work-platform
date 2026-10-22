# Data model and retention design
**Version:** 0.3 · **Updated:** 22 October 2026 · **Status:** logical schema, not executable migrations

UUID identifiers, UTC timestamptz, integer EUR gross cents. Company fees have a separate lifecycle from job content. There is no prepaid wallet/lot model in the current MVP.

Database-access tooling is confirmed as Drizzle ORM with node-postgres (pg), TypeScript schema declarations and reviewed generated/custom SQL migrations. The logical entities and critical database constraints below remain authoritative; qualify Better Auth schema generation/adapter atomicity, one-connection transactions and job claims through Hyperdrive before producing executable migrations.

## 1. Entities
| Table | Fields / constraints |
|---|---|
| principals/accounts | Company owners and staff in MVP; auth subject/email ownership state, locale/status and scoped roles; client accounts/linking deferred until after MVP, with guest ownership stored separately |
| guest_job_owners | Job-scoped email/ownership reference, purpose and delete_at; no public client identity |
| guest_tokens/sessions | Token hash UNIQUE, job, purpose, one-use consumed_at, expires_at, revocation; session bound to that job; all expire/revoke with deletion |
| companies | Owner UNIQUE for MVP, declared legal/display name, own contact/email/phone, optional website/KVK, categories, coarse base area, radius_km, status; no quality-verified flag |
| alert_preferences | Company, explicit subscription status, categories/radius/frequency/locale, confirmed recipient, consent/time, unsubscribe version; frequency defaults to DAILY_DIGEST with INSTANT optional, while subscription stays off without opt-in |
| jobs | Task-scoped guest-owner reference in MVP, category, state, current material revision, monotonic publication_generation, city/PC4, first_published_at, immutable delete_at, closed_at, version; region validation result |
| job_content/revisions | Short description, optional budget/timing/measurements, source revision, privacy-safe projection; all task-scoped/deletable |
| job_private_location_contact | Full postcode and optional selected client email/phone; encrypted/restricted; derived task coordinates and source/licensing metadata; no street address required |
| contact_preferences | Job UNIQUE, CLIENT_INITIATED or ALLOW_RESPONDERS, allowed fields, consent version/time, revoked_at; default private |
| responses | UNIQUE(job_id, company_id), state, draft/current version, fee fulfillment reference, published/withdrawn times; no competitor visibility relation |
| response_versions | Introduction, rough range/site-visit-needed, price basis/assumptions, own contact/website snapshot, job revision; task-scoped content deleted at deadline |
| photo_assets | Job, uploader owner, storage original/variant/version keys, real type/bytes/pixels, scan/privacy state, delete progress; no public URL |
| payment_attempts | Response opaque reference, company, price/policy/material-revision/publication-generation snapshots, EUR gross=100 under R1, stable attempt key/request hash/create time, provider=Mollie, environment/merchant profile, Mollie payment ID, checkout URL, non-secret credential-version reference and state |
| response_fulfillments | response_id UNIQUE, successful payment/effect UNIQUE, fulfilled_at, fee amount; retain minimum finance-safe projection after content erasure |
| refunds/disputes | Payment, stable request/provider reference UNIQUE, reason, amount/reservation/state, timestamps; refund obligations retain original verification/approval time, submission deadline, attempt/acceptance/completion times and escalation state; bounded to captured unreturned principal |
| journal_transactions/entries | Business effect type+ID UNIQUE; balanced debit/credit rows, immutable correction linkage; no full task body/client email |
| billing_documents | Company billing snapshot, amount/tax/policy/time, original/corrective document link; statutory retention separate |
| reports/cases | Reporter contact/evidence in restricted store, category/target, reasoned decision, hold/review expiry, appeal; minimize task copies |
| provider_notifications | Internal UUID, provider/environment/merchant profile, known Mollie payment reference, received/processed/retry state; classic callbacks have no provider event ID/signature envelope; no permanent raw webhook/customer payloads |
| outbox/inbox/deliveries | Minimal IDs/type/lease/attempt data; UNIQUE(company,job,revision,kind) for alerts independent of daily/instant frequency, with shared delivery history across mode changes; no message/photo/guest-token payload |
| audit_events/deletion_tombstones | Reason/actor/opaque IDs/action/time/progress; purpose-limited retention; do not hide deleted content in audit JSON |

## 2. Constraints and concurrency
Material task edits create a new scope revision under the confirmed [material-edit policy](../docs/adr/0005-version-material-task-scope-changes.md). Preserve the revision associated with each offer version and payment attempt: obsolete unpublished captures refund, while existing published offers expose a scope-change label until reviewed/edited against the current revision. Revision snapshots do not extend task retention.

The confirmed [reopening policy](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md) preserves fees/offers/expiry, resets contact permission and keeps canceled attempts invalid. The proposed generation snapshot makes close/reopen cancellation durable: closure advances the job generation under lock; fulfillment requires the attempt's generation to match, independent of current OPEN state. Consent version advances on reset, so an old permission write cannot restore sharing after reopening.

Confirmed WITHDRAWN is terminal for that company/task in MVP. Preserve the unique response and fulfilled-fee identity through ordinary retention instead of deleting/recreating it to obtain another purchase. Client queries include withdrawn entries only as company-name/status markers; omit offer body, website and company contacts at the query/DTO boundary. Task reopening, edits and old payment events cannot restore PUBLISHED.

```sql
-- Design fragments, not a full migration.
CREATE UNIQUE INDEX one_company_response
  ON responses (job_id, company_id);

CREATE UNIQUE INDEX one_response_fulfillment
  ON response_fulfillments (response_id);

CREATE UNIQUE INDEX one_provider_payment
  ON payment_attempts (provider, environment, merchant_id, provider_payment_id)
  WHERE provider_payment_id IS NOT NULL;

CREATE UNIQUE INDEX one_business_effect
  ON journal_transactions (reference_type, reference_id);
```

Mollie payment, refund and chargeback references are distinct and scoped to the configured merchant/profile/environment. Persist minimal known-payment notification work and an outbox verification request atomically before acknowledgment. Classic callbacks repeat a payment ID across status changes; an internal receipt UUID is not a provider event ID, and permanent deduplication by payment ID would lose later changes. Coalescing must preserve requests arriving during a worker lease. Fulfillment/refund/journal uniqueness remains independent of notification delivery. Keep local operation identity and credential-version context beyond Mollie's one-hour credential-scoped idempotency cache, reconciling ambiguous results before any replacement create. Long-lived finance records contain only minimum references, not raw provider payloads. See [Mollie mapping](05-technical-design.md) and [provider ADR](../docs/adr/0007-use-mollie-for-current-payment-integration.md).

One unresolved payable attempt per response is enforced through a partial uniqueness rule on the real state model plus the response lock. A refunded failed attempt can be followed by a new deliberate attempt; the UNIQUE fulfillment and payment/refund logic ensure only one retained successful publication fee. Never pretend a per-row CHECK validates multi-row journal balance; use a reviewed posting procedure/deferred trigger plus tests.

Consistent lock order: job -> response -> payment/refund children. Company owner authority derives from the session; a caller-provided company ID is not sufficient. No commercial participant counter/cap, “remaining slots,” public offer aggregation or wallet balances.

## 3. Read projections
- Company safe task DTO: category, city/PC4, description, safe photo references, approximate distance, open/revision/deletion status.
- Company own response DTO: own submitted content, publication/payment/refund status; client contacts only from a separate current-consent read.
- Client DTO: own task plus private paginated eligible-offer list with self-submitted contacts for active published offers; withdrawn entries contain only company name/withdrawn marker, without offer text/website/contacts; no public URL.
- Finance DTO: own company's fee/refund/document metadata, opaque task reference where needed; no client photos/contact or proposal text.
- Public SEO: static regional/category education pages, not live task/person data.

Indexes: open-task category+region+delete_at; responses(job_id,published_at,id) under client ownership and company_id for own list; due-deletion index; deliveries recipient/status; pending payment/refund reconciliation. Verify with realistic plans/data.

## 4. Deletion matrix
| Dataset | Lifecycle and decision status |
|---|---|
| Published task description, photos/variants, offers/contact snapshots, full postcode/contact, coordinates and guest grants | Confirmed: first task publication + three calendar months; access stops immediately, live purge including all object versions <=24h |
| Unpublished abandoned guest drafts/unverified ownership | Proposed seven days from creation, disclosed; shorter than published-task lifecycle |
| Active company profile and alert preference | Confirmed separate purpose while service/opt-in remains active; exact schedule/inactivity policy reviewed; not reset/copy client tasks |
| Closed job | Same original delete_at, not close + three months; client can request earlier deletion |
| Operational logs | Proposed 30 days, redacted, justified exceptions reviewed |
| Minimal platform-fee accounting/invoices | Confirmed separate retention; accountant determines minimum fields, period and start date; no task contents retained automatically |
| Complaint/legal evidence | Confirmed narrow exception: strictly necessary evidence for a named case, restricted access and review/deletion date; exact lawful purpose/fields/schedule reviewed; no blanket permanent archive |
| Backups | Confirmed: rolling <=30 days, isolated; restore filters/tombstones before access, with deletion records available independently of restored snapshot |

The confirmed clock starts when the task first becomes visible to companies, independently of any offer publication; see [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md). Store delete_at once and prohibit ordinary edits/extensions at DB/service boundary. Purge ordinary task records and all object versions/thumbs/quarantine within the confirmed 24h limit and invalidate download capabilities at expiry. Keep a minimal non-content deletion marker while needed for the confirmed <=30-day backup expiry/recovery window, then expire it under the reviewed schedule. Separate retention is confirmed for minimum billing records, active company/alert settings and strictly necessary case evidence; exact schedules remain professional-review deliverables. Financial opaque references must not be a route to resurrect erased task data; deletion must preserve necessary financial effects without retaining task bodies or contacts.

## 5. Consent and external copies
Record exact client contact mode, fields, audience explanation version and action time. Grant checks are dynamic; after withdrawal/close/revocation no future field disclosure. Audit that disclosure occurred without retaining the disclosed value indefinitely. Emails only carry generic/safe metadata; downloaded external copies and off-platform contact cannot be remotely deleted.

## 6. Migration note
Version 0.3 replaces earlier wallet, entitlement-slot, chat/award/review and 12-month-content tables. Since this repository contains documents only, there is no live database migration to run. If an implementation has started elsewhere, inventory it and plan a separate reviewed migration; do not deploy these sketches over unknown production data.
