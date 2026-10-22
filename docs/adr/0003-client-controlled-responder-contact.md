---
status: accepted
---

# Share client contacts only through explicit responder permission

**v0.4 applicability:** Client contact consent remains in force for the client-task board; the former offer-publication fee is canceled, and a subscription alone grants no client-contact access. The publication-payment reference below is historical. See [ADR 0013](0013-expand-to-three-boards-and-free-subscription-services.md).

On 21 October 2026 the founder confirmed that client contact details remain private by default, with an explicit option to share selected email/phone fields with every published responder while the task is open. This supports callbacks without individual responder selection: if twelve companies publish offers, all twelve can access the selected fields, while paying the publication fee alone grants no contact access. Revocation or task closure stops future disclosure; details already copied cannot be recalled.

Full postcode and street address remain outside this permission. Implement current permission checks and clear audience wording rather than copying client contacts into offers, notifications or payment records; final legal wording remains part of the existing launch review.
