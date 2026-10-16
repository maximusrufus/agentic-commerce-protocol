# Appendix C (non-normative): Card-rail mapping of ATDRP evidence

Status: draft for SEP #303. Co-authored with @Flyingmiata-droid. Every cell is read from the vendor's own public
documentation on the date shown; cells say UNKNOWN when the documentation could not be read.

## Why this appendix exists
ATDRP's dispute record carries protocol artifacts (merchant-signed checkout, payment mandate, receipts). When the
underlying payment ran on a card rail, the chargeback is decided by the issuer from the network reason code and the
evidence the merchant submits through its gateway. This appendix records, per gateway, (a) whether the merchant
receives an agent signal, (b) whether the dispute object links back to it, and (c) whether any documented field can
carry it to the issuer.

Finding at the time of writing: merchants can recover the agent signal in one lookup on two gateways, but **no
gateway documents a structured way to put it in front of the issuer**.

## Mapping table

| Gateway | Agent signal to merchant | Dispute links back to payment | Field that can reach the issuer | ATDRP artifact that could ride there | Read on |
|---|---|---|---|---|---|
| Stripe | `PaymentIntent.agent_details` (`name`, `network_business_profile`), API version 2026-09-30.preview | Yes: `Dispute.payment_intent` (expandable) | None structured; free-text/file evidence only (`evidence.uncategorized_text`, `uncategorized_file`) | mandate + receipt as an uncategorized file | 2026-10-16 |
| Braintree | `transaction.facilitator_details` (`oauth_application_name`, `oauth_application_client_id`), response only | Yes: `dispute.transaction.id` | None structured; evidence comment/file upload only | mandate + receipt as file evidence | 2026-10-16 |
| Adyen | | | | | |
| Worldpay | | | | | |
| Checkout.com | none documented (2025 blog mentions providing issuers "the original consent record"; not in docs) | | | | |
| Cybersource | agent-side API only; nothing merchant-side | | | | |
| Fiserv Commerce Hub | UNKNOWN (docs not readable) | UNKNOWN | UNKNOWN | | |
| Elavon Converge | UNKNOWN (docs not readable) | UNKNOWN | UNKNOWN | | |
| Global Payments | UNKNOWN (docs not readable) | UNKNOWN | UNKNOWN | | |
| (remaining gateways from the 16-gateway scorecard) | | | | | |

## What would close the gap (informative)
1. A structured agent indicator in the network dispute evidence schema (for example alongside existing compelling-evidence
   data elements).
2. Until then, ATDRP recommends attaching the signed mandate and receipts as file evidence and stating the
   agent-initiated nature in the free-text field.
