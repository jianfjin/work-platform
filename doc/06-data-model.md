# Data model and retention design
**Version:** 0.3 · **Updated:** 21 October 2026 · **Status:** logical schema, not executable migrations

UUID identifiers, UTC timestamptz, integer EUR gross cents. Company fees have a separate lifecycle from job content. There is no prepaid wallet/lot model in the current MVP.

## 1. Entities
| Table | Fields / constraints |
|---|---|
| principals/accounts | Company owner or optional client account; auth subject/email ownership state, locale/status; no mandatory client account row |
| guest_job_owners | Job-scoped email/ownership reference, purpose and delete_at; no public client identity |
| guest_tokens/sessions | Token hash UNIQUE, job, purpose, one-use consumed_at, expires_at, revocation; session bound to that job; all expire/revoke with deletion |
| companies | Owner UNIQUE for MVP, declared legal/display name, own contact/email/phone, optional website/KVK, categories, coarse base area, radius_km, status; no quality-verified flag |
| alert_preferences | Company, subscription status, categories/radius/frequency/locale, confirmed recipient, consent/time, unsubscribe version |
| jobs | Owner reference, category, state, current material revision, city/PC4, first_published_at, immutable delete_at, closed_at, version; region validation result |
| job_content/revisions | Short description, optional budget/timing/measurements, source revision, privacy-safe projection; all task-scoped/deletable |
| job_private_location_contact | Full postcode and optional selected client email/phone; encrypted/restricted; derived task coordinates and source/licensing metadata; no street address required |
| contact_preferences | Job UNIQUE, CLIENT_INITIATED or ALLOW_RESPONDERS, allowed fields, consent version/time, revoked_at; default private |
| responses | UNIQUE(job_id, company_id), state, draft/current version, fee fulfillment reference, published/withdrawn times; no competitor visibility relation |
| response_versions | Introduction, rough range/site-visit-needed, price basis/assumptions, own contact/website snapshot, job revision; task-scoped content deleted at deadline |
| photo_assets | Job, uploader owner, storage original/variant/version keys, real type/bytes/pixels, scan/privacy state, delete progress; no public URL |
| payment_attempts | Response opaque reference, company, price/policy/revision snapshots, EUR gross=100 under R1, stable attempt key, provider/environment/merchant/payment ID, state |
| response_fulfillments | response_id UNIQUE, successful payment/effect UNIQUE, fulfilled_at, fee amount; retain minimum finance-safe projection after content erasure |
| refunds/disputes | Payment, stable request/provider reference UNIQUE, reason, amount/reservation/state, timestamps; bounded to captured unreturned principal |
| journal_transactions/entries | Business effect type+ID UNIQUE; balanced debit/credit rows, immutable correction linkage; no full task body/client email |
| billing_documents | Company billing snapshot, amount/tax/policy/time, original/corrective document link; statutory retention separate |
| reports/cases | Reporter contact/evidence in restricted store, category/target, reasoned decision, hold/review expiry, appeal; minimize task copies |
| outbox/inbox/deliveries | Minimal IDs/type/lease/attempt data; UNIQUE(company,job,revision,kind) for alerts; no message/photo/guest-token payload |
| audit_events/deletion_tombstones | Reason/actor/opaque IDs/action/time/progress; purpose-limited retention; do not hide deleted content in audit JSON |

## 2. Constraints and concurrency
```sql
-- Design fragments, not a full migration.
CREATE UNIQUE INDEX one_company_response
  ON responses (job_id, company_id);

CREATE UNIQUE INDEX one_response_fulfillment
  ON response_fulfillments (response_id);

CREATE UNIQUE INDEX one_provider_payment
  ON payment_attempts (provider, environment, provider_payment_id)
  WHERE provider_payment_id IS NOT NULL;

CREATE UNIQUE INDEX one_business_effect
  ON journal_transactions (reference_type, reference_id);
```

One unresolved payable attempt per response is enforced through a partial uniqueness rule on the real state model plus the response lock. A refunded failed attempt can be followed by a new deliberate attempt; the UNIQUE fulfillment and payment/refund logic ensure only one retained successful publication fee. Never pretend a per-row CHECK validates multi-row journal balance; use a reviewed posting procedure/deferred trigger plus tests.

Consistent lock order: job -> response -> payment/refund children. Company owner authority derives from the session; a caller-provided company ID is not sufficient. No commercial participant counter/cap, “remaining slots,” public offer aggregation or wallet balances.

## 3. Read projections
- Company safe task DTO: category, city/PC4, description, safe photo references, approximate distance, open/revision/deletion status.
- Company own response DTO: own submitted content, publication/payment/refund status; client contacts only from a separate current-consent read.
- Client DTO: own task plus private paginated all-response list including each company's self-submitted contacts; no public URL.
- Finance DTO: own company's fee/refund/document metadata, opaque task reference where needed; no client photos/contact or proposal text.
- Public SEO: static regional/category education pages, not live task/person data.

Indexes: open-task category+region+delete_at; responses(job_id,published_at,id) under client ownership and company_id for own list; due-deletion index; deliveries recipient/status; pending payment/refund reconciliation. Verify with realistic plans/data.

## 4. Deletion matrix
| Dataset | Proposed lifecycle |
|---|---|
| Published task description, photos/variants, offers/contact snapshots, full postcode/contact, coordinates and guest grants | First publication + three calendar months; access stops then, live purge target <=24h |
| Unpublished abandoned guest drafts/unverified ownership | Proposed seven days from creation, disclosed; shorter than published-task lifecycle |
| Active company profile and alert preference | While service/opt-in remains active; review inactivity and deletion requests; not reset/copy client tasks |
| Closed job | Same original delete_at, not close + three months; client can request earlier deletion |
| Operational logs | Proposed 30 days, redacted, justified exceptions reviewed |
| Minimal platform-fee accounting/invoices | Accountant-approved legal schedule, potentially years; no task contents retained automatically |
| Complaint/legal evidence | Case-specific minimum, purpose, authorized access, review/deletion date; no blanket permanent archive |
| Backups | Rolling <=30 days, isolated; restore filters/tombstones before access |

Store delete_at once and prohibit ordinary edits/extensions at DB/service boundary. Purge all object versions/thumbs/quarantine and invalidate download capabilities. Keep a minimal non-content deletion marker while needed for backup expiry/recovery, then expire it under the approved schedule. Financial opaque references must not be a route to resurrect erased task data.

## 5. Consent and external copies
Record exact client contact mode, fields, audience explanation version and action time. Grant checks are dynamic; after withdrawal/close/revocation no future field disclosure. Audit that disclosure occurred without retaining the disclosed value indefinitely. Emails only carry generic/safe metadata; downloaded external copies and off-platform contact cannot be remotely deleted.

## 6. Migration note
Version 0.3 replaces earlier wallet, entitlement-slot, chat/award/review and 12-month-content tables. Since this repository contains documents only, there is no live database migration to run. If an implementation has started elsewhere, inventory it and plan a separate reviewed migration; do not deploy these sketches over unknown production data.
