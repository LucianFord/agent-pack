# Example output — surgeon-reviewer

> Illustrative example on a typical small PR (a discount-code feature in an Express checkout
> service), showing the report shape: tiered findings, concrete triggers, verdict.
> No style comments. No padding.

```
PR: "Add discount code support to checkout" (+182 −14)

Findings:

1. src/checkout/applyDiscount.js:31 — WILL BREAK
   Diagnosis: discount percent read from client payload without server-side lookup.
   Trigger: buyer tampers `{"code":"SAVE10","percent":99}` in the request body.
   Consequence: any buyer sets their own discount; direct revenue loss on every order.
   Fix direction: look up percent by code from the discounts table; ignore client percent.

2. src/checkout/applyDiscount.js:44 — WILL COST
   Diagnosis: discount usage counter incremented before payment confirmation, never decremented on failure.
   Trigger: card declined → code still marked "used"; after N retries the legitimate user is locked out.
   Consequence: support tickets + abandoned carts on every failed payment with a code.
   Fix direction: increment in the payment-success path, or use a reservation/confirm two-step.

3. src/checkout/totals.js:58 — WILL ROT
   Diagnosis: rounding rule `Math.round(x*100)/100` duplicated in 3 places with one variant using toFixed(2).
   Consequence: totals will drift by a cent between cart and charge; the next person fixes one copy.
   Fix direction: single `cents()` helper; compute in integer cents end-to-end.

Verdict: DO NOT SHIP — finding 1 must be fixed; finding 2 should be fixed in this PR
(touches the same code path); finding 3 can be a follow-up.
```

**What you should notice:** two findings, both with a reproducible trigger; the tier-3 item
explicitly does *not* block merge. That restraint is the product.
