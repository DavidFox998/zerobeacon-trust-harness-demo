# ZeroBeacon Trust Harness — PayPal gate demo

A thin public client for the ZeroBeacon Trust Harness, built for the
PayPal AI Hackathon. An AI agent proposes a purchase; the hosted
harness audits and verifies the proposal **before** any money moves;
only a measured pass creates a PayPal sandbox order; capture records
the executed payment in a chained, tamper-evident receipt ledger.

License: Apache-2.0 — see `LICENSE`.

## Run it

Hosted: open `index.html` from any static host (or this repo's Pages)
— there is no build step and nothing to install. You need:

1. A ZeroBeacon API key (judges: the ENTERPRISE key in the Devpost
   submission's testing field). The key is held in page memory only.
2. A PayPal **sandbox** buyer account to approve the created order
   (free, from developer.paypal.com — every developer account gets
   sandbox buyer/seller personas).

## The two outcomes

- **Clean proposal** — manifest states the true counts (0 sorries).
  The gate passes, a certificate is issued, a PayPal sandbox order
  is created. Approve it, then capture: the payment executes and a
  receipt is chained.
- **Tampered proposal** — check the tamper box: the agent claims
  work its manifest contradicts. The audit re-measures, the gate
  refuses with the failed check named, and **no PayPal order is
  ever created**.

## How it maps to the hosted API

- `POST /api/trust/paypal/propose` — {proposal, purchase} → gate
  verdict; on pass, certificate + PayPal order id + approve URL.
- `POST /api/trust/paypal/capture` — {paypal_order_id} → executed
  payment + receipt (a repeat capture returns the existing record).
- `GET /api/trust/paypal/status` — configuration self-report.

The proposal's `repo` field names a repository the harness server
can read — the audit re-measures real files on the host. The hosted
demo bundles a small sample repository server-side for this path;
self-hosters point it at any local Lean repository and state its
true counts in the manifest.

## What this repo is not

The harness itself (Apathy, Proven, Anchor, Receipts) is hosted on
the ZeroBeacon platform and is not in this repository; the
certificate issuance core is sealed by design — verification is
public, issuance is the moat.
