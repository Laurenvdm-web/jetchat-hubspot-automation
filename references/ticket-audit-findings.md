# Ticket Audit Findings — Closed Win Stage
*Compiled from review of ~120 real complaint tickets in HubSpot win stage (pipeline 811252326, stage 1369528238). June 2026.*

---

## What's already in the template but frequently ignored

These rules exist but the audit shows agents regularly break them:

- Section 3 still sometimes includes "Ticket created" or a recap of the complaint — both explicitly banned.
- Section 4 is sometimes left blank or says "Pending" on a ticket that is already closed.
- Compensation amount, whether the guest accepted, and who paid are often missing from Section 4.
- Section 3 entries sometimes have no dates.

---

## Patterns worth adding to the skill rules

### 1. Section 4 must state who absorbed the cost

The best tickets are explicit: "Partner refunded our VCC, we refunded the guest — no loss to Jetstream" or "Jetstream absorbed €75, recovery pending." Several closed tickets just say "Guest offered 20% discount" with no indication of whether Jetstream paid, the partner paid, or it was a future-only offer that costs nothing now.

The rule to add: Section 4 must state one of these:
- Partner reimbursed our VCC — Jetstream net $0
- Jetstream absorbed the cost — recovery [pending / confirmed / written off]
- OTA (Airbnb/VRBO) handled the refund directly — Jetstream net $0
- No compensation issued

---

### 2. Closing due to guest non-response

Several tickets are legitimately closed because the guest stopped responding, not because the case was resolved. The template doesn't cover this scenario at all. Agents currently write things like "Guest has not responded in 3 days — closing ticket" with no guidance on format.

Suggested closing note for this scenario:
> Closed — no response from guest after [X] follow-up attempts over [X] days. Case documented. No compensation issued. Ticket can be reopened if guest returns.

---

### 3. B2C Director Note format

The best-run tickets include a "B2C DIRECTOR NOTE" block at the very top (above Section 1) when a TL or Director has reviewed and closed the ticket. It always includes:
- Date and reviewer name
- What was audited or corrected
- Final status decision (e.g., "Closed Jetstream Win, net $0")

Example from a real ticket:
> B2C DIRECTOR NOTE — 09 Jun 2026 (Kirsten Collins): Partner reimbursed €335 in full. Jetstream net €0. Fault corrected to partner_at_fault. Recovery confirmed. Closed Jetstream Win.

This is not in the template at all. If the skill is only for the initial ticket creation, this is fine — but the template should acknowledge this block may be added later and agents should leave the top of the ticket clean so it can be prepended.

---

### 4. "Status Note" closing stamp

Several Director-reviewed tickets also add a "STATUS NOTE — [date]:" block at the very end, separate from Section 4, summarising the final outcome in one or two sentences. This functions as a closing stamp so anyone opening the ticket later sees the outcome immediately at both the top and bottom.

This is a good practice worth formalising, especially for complex cases.

---

### 5. Compensation threshold / escalation

One ticket explicitly documents the decision logic: "If the guest accepts this resolution, close the ticket as the compensation is within margin. If the guest declines and requests a higher amount or cancellation, escalate to the Team Lead." This implies there is a compensation threshold above which an agent cannot decide alone. The template doesn't mention this at all — agents should know to flag if compensation exceeds the threshold rather than documenting a pending decision and closing.

---

### 6. OTA-handled refunds (AirCover, flexible cancellation)

Several tickets were resolved entirely by the OTA — Airbnb AirCover paid the guest, clawed the partner payout, and Jetstream had zero exposure. The template doesn't cover this scenario. Section 4 should note:
- Who initiated the refund (Airbnb, VRBO, etc.)
- Whether Jetstream was involved in the payment at all
- Confirm Cirrus comp fields reflect $0 Jetstream cost

Example from a real ticket: "Closed Jetstream Win, net zero via AirCover. Recovery marked confirmed to reflect zero Jetstream exposure, not a cash reimbursement."

---

### 7. Force majeure / third-party fault

One ticket involved a major power outage caused by the utility network (Enedis), not the partner. The partner coordinated a generator and approved a goodwill refund anyway. The template's fault categories (partner / Jetstream / unsure) don't include "external / force majeure." Agents seem unsure how to frame these — add guidance.

---

### 8. Content update ticket cross-reference in Section 3

When a complaint triggers a content update ticket, several agents logged the HubSpot ticket number in Section 3. This is good practice and should be formalised: if a content update ticket is created as part of handling this case, log it in Section 3 with the ticket ID.

Example: `22 Jun 2026 — Content update ticket created to correct check-in time on listing (ticket #45768038693).`

---

### 9. Resolution label issues to watch for

These appear in real closed tickets and indicate the case was not properly finalised before being closed:

- "Pending — awaiting further update from property" in Section 4 of a closed ticket
- "Awaiting property response regarding exception request or cancellation options" in Resolution
- "Waiting on reply from Partner" in Resolution
- Resolution field truncated mid-sentence (content field may have a character limit — check ticket is complete before closing)

---

## Non-complaint ticket types that also land in the win stage

These use completely different formats and the template does not apply to them. Agents should be aware they exist:

- **Captured Funds Win** — financial surplus from cancelled bookings; auto-generated format
- **Jetstream Win (Director audit)** — cases where Director has audited and confirmed $0 exposure
- **Guest property damage claims** — guest damaged something; involves insurance/Airbnb Resolution Center
- **Policy cancellations** — group bookings, business stays, worker restrictions cancelled per listing terms
- **Partner-initiated cancellations** — partner cancels confirmed bookings; logged for AM follow-up
- **Cancellation policy disputes** — partner demands more payout than platform policy allows; financial/AM issue
- **Stripe/chargeback disputes** — guest files credit card dispute; resolution involves winning the dispute back

---

*Created: 25 Jun 2026*
