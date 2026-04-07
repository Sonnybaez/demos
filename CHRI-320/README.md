# CHRI-320: Post-Purchase Confirmation Clarity

Visual demo and implementation reference for the first child ticket of the **Leaky Pipe — Stage 3 / Verification** epic.

## What this is

A side-by-side demo showing the current post-purchase verification page versus the proposed CHRI-320 changes, plus an annotated Vue file the front-end dev can use as a reference when implementing.

The point of this folder is to make the changes obvious to anyone — Karen, Dom, the front-end dev, SLT — without needing to imagine what the wording change "looks like" in context.

## Why we're doing this

In Feb 2026, 9.66% of new sales never submitted a single document, up from 4.24% in Aug-Nov 2025. 45% of unrealised cancellations happen within 24 hours of purchase. From Typeform exit data, 33% of the no-documents group cite the verification process as the barrier and 20% say they don't trust the site (up from 11%).

The current post-purchase touchpoints don't tell the customer the most important thing: **we have their money, and the only thing standing between them and their medication is a few minutes of verification**. Reinforcing payment raises the perceived cost of dropping off and signals legitimacy.

## Folder structure

```
CHRI-320/
├── README.md                              ← you are here
├── current/
│   └── post-purchase-mobile.html          ← replica of the live page (mobile)
├── proposed/
│   └── post-purchase-mobile.html          ← with CHRI-320 changes applied
└── vue-diff/
    └── PurchaseCompleted.annotated.vue    ← annotated changes for the dev
```

## How to view

Open the HTML files directly in a browser, or push to GitHub Pages for a shareable preview link.

The proposed file has a small legend banner at the top of the page that highlights what's changed:
- **Yellow** = changed copy
- **Green** = added element

Both highlights and the legend banner are demo-only — they would be removed before launch.

## What's changing

| # | Element | Current | Proposed |
|---|---------|---------|----------|
| 1 | Page header | Almost there | Payment received — let's get your order moving |
| 2 | Subhead | We've received your request, but we need to verify your identity before we proceed with your prescription. | We've received your payment for order CHEQ-XXXX. Complete the steps below and your prescription goes to a clinician for review. |
| 3 | List lead-in | We ask you to provide: | We just need: |
| 4 | Order badge label | Order number | Paid order |
| 5 | Dispatch timing note | *(not present)* | Approved before 14:00 (Mon–Fri)? Your medication ships the same day. |

## Notes for the front-end dev

- The demo HTML uses **Inter** as a substitute for **Kind Sans** because Kind Sans is a paid foundry font and can't be loaded in a static demo. Visual difference is minimal at body sizes — swap for Kind Sans in the real implementation.
- Brand colours match the CheqUp Brand Guidelines V1 (Potent Purple `#683CF5`, Mindful Midnight `#210847`, Tenacious Turquoise `#83EDFF`, etc).
- The QR code in the demo is a placeholder pattern, not a real code. The real component already loads the right asset.
- The demo is mobile-only for now (390px viewport). Desktop demo to follow if needed — most CheqUp traffic is mobile so that's where the polish matters most.

## What's out of scope for this ticket

- Trust badges and Persona credentials (CHRI-321)
- Full copy alignment audit across email/SMS/portal (CHRI-322)
- Explainer videos (CHRI-323)
- SMS/email nudges for stalled users (CHRI-324, CHRI-325)

## Other surfaces in CHRI-320 (still to do)

This first demo covers the post-purchase verification page only. CHRI-320 also includes:
- Patient dashboard verification-pending state (`start.chequp.com/patient`)
- Order confirmation email ("Thank you for your order")

Both will be added to this folder as separate `current/` and `proposed/` files once the post-purchase page is signed off.

The post-purchase SMS has already been updated and is in production:
> Thank you for your payment - Complete your order in 2 mins. Verify your ID & upload your photo here: https://start.chequp.com/patient/login
> Thank you, CheqUp

## Success metrics

- Reduce no-documents-submitted from 9.66% towards <7%
- Reduce 24-hour cancellation share (currently 45% of unrealised)
- Reduce median time from purchase to first document upload
- Increase confirmation email open rate and CTA click-through rate
