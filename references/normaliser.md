# Normaliser — JetChat Field Extraction Reference

The normaliser converts the raw JetChat thread into a clean, structured object. It filters noise, extracts the real complaint, and standardises fields so the skill always receives the same shape of data.

---

## Fields to extract and use

- `booking_id`
- `property_id`
- `resort_id`
- `guest_name`
- `channel / inbox`
- `stay_status`
- `received_at`
- `complaint_messages`
- `complaint_summary_text`
- `followup_log`
- `is_thread`
- `guest_reason`
- `guest_request`
- `request_history`
- `escalation_flag`
- `early_departure`
- `chat_history`

---

## Fields to ignore (noise)

- Thanks / thank you
- Noted / OK / sounds good
- Have a good day
- See you then
- System / automated messages
- Check-in confirmations

---

## How messages are treated by sender

- Guest messages → scanned for complaint content → feed into `complaint_summary_text` (Section 1 of ticket)
- Leavetown staff responses → treated as actions taken → feed into `followup_log` (Section 3 of ticket)
- System / automated messages → ignored entirely (booking confirmations, pre-arrival reminders)

---

## Outputs — two separate fields

- `complaint_summary_text` — the extracted complaint, ready for Section 1
- `followup_log` — actions taken by staff, ready for Section 3
- `is_thread` — true if the complaint spans more than one message

---

## Stay status — normalised to 4 values

- `inquiry`
- `pre_stay`
- `in_house`
- `past_stay`

Inferred from explicit status field first, then from check-in/out dates if missing.

---

## Complaint extraction logic

- Scans all guest messages in the thread — not just the last one
- Flags noise messages and excludes them
- Identifies complaint messages by sentiment, keywords, and context
- Concatenates complaint messages into `complaint_summary_text`
- Sets `is_thread = true` if complaint spans more than one message
- Takes guest name from the reservation object — never from how staff addressed them in messages (staff may spell it incorrectly)
- If a second person sends messages in the thread (e.g. a travel companion), treat their messages as part of the same guest complaint. Do not use their name as the guest name — always use the name from the reservation object.
- Automated messages are excluded from `followup_log` but kept in `chat_history` — the context of what triggered the guest matters

---

## Smart fields — extracted from complaint content

- `guest_reason` — why the guest is complaining (e.g. "car accident less than 24h after booking, unable to travel"). Detected from guest messages, not staff replies.
- `guest_request` — what the guest is actually asking for. May change across the thread — normaliser captures the most recent ask (e.g. "amicable resolution / date change / name transfer").
- `escalation_flag` — true if guest mentions legal action, chargebacks, insurance disputes, reviews, regulatory bodies, or threatens to escalate to the booking channel (e.g. Airbnb, VRBO). Triggers high priority on the ticket. When the trigger is a channel escalation threat, Section 1 of the ticket must include an explicit line: "Guest threatened to escalate to [channel name]."
- `request_history` — ordered list of what the guest asked for across the thread, so the ticket shows how the request evolved (e.g. refund → date change → name transfer → legal threat).
- `early_departure` — true if the guest checked out before the scheduled checkout date. When true, Section 1 must note the departure date and how many nights were cut short (e.g. "Guest departed early on 26 May 2026 — day 3 of a 7-night stay").

---

## What the normaliser does NOT provide

The following fields are not extracted from JetChat — they are looked up separately by the skill:

- Account manager name and HubSpot owner ID (looked up via Cirrus using `resort_id`)
- Property manager name (pulled directly from JetChat Property details > Property manager)
- Resort name (pulled directly from JetChat Property details > Name)
- Cirrus URL (auto-built from `booking_id`)
