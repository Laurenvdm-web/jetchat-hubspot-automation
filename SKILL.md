---
name: b2c-ticket-createticket-v1-0-lv-draft-skill
description: >
  Mini-skill: Creates a HubSpot guest case ticket in the B2C Guest Resolution & Revenue
  Protection pipeline. Called by the b2c-hubspot-orchestrator which passes pre-packaged
  context. Expects guest name, booking ID, resort name, account manager owner ID,
  property manager, Cirrus URL, and guest complaint as inputs. Generates a 4-section
  ticket overview, auto-selects Complaint Type and Fault Assignment, and creates the
  ticket in HubSpot immediately — no confirmation steps.
---

# B2C Guest Case — HubSpot Ticket Creator (Mini-Skill)

This skill is called by `b2c-hubspot-orchestrator`. All Cirrus lookups and screenshot extraction are handled upstream — this skill receives pre-packaged context and focuses solely on creating the ticket.

## Inputs received from orchestrator

- Guest first and last name
- Jetstream Booking ID
- Resort name
- Account manager name + HubSpot owner ID
- Property manager name
- Cirrus URL (auto-built as `https://cirrus.leavetown.com/booking-detail-edit.aspx?bookingId=[BOOKING_ID]`)
- Guest complaint (text)
- escalation_flag (true/false)
- stay_status (pre_stay / in_house / past_stay)
- followup_log (dated actions already taken, if any)

---

## Step-by-step workflow

### Step 1 — Read the reference files

Before writing anything, read all four reference files from GitHub:

- `https://raw.githubusercontent.com/Laurenvdm-web/jetchat-hubspot-automation/main/references/section-1-complaint-overview.md` — rules and examples for Section 1
- `https://raw.githubusercontent.com/Laurenvdm-web/jetchat-hubspot-automation/main/references/section-3-actions-followups.md` — rules and examples for Section 3
- `https://raw.githubusercontent.com/Laurenvdm-web/jetchat-hubspot-automation/main/references/section-4-resolution.md` — what to write in Section 4
- `https://raw.githubusercontent.com/Laurenvdm-web/jetchat-hubspot-automation/main/references/complaint-types.md` — complaint type options and HubSpot enum values

Apply the rules from these files across the full ticket. Do not rely on memory.

---

### Step 2 — Write the 4-section ticket body

Using the rules from the reference files, write the ticket body in plain text (no markdown, no asterisks — HubSpot does not render formatting):

**1. Complaint Overview**
Follow `references/section-1-complaint-overview.md` in full.

**2. Verification**
- For regular issues: leave blank, unless photos were explicitly shared — then add: `Photos submitted by guest — reviewed and support the complaint.`
- For content/listing accuracy issues: add three blank fields: `Content issue Verification: / Guest Verification: / Fault Verification:`
- If the partner denied the complaint or declined a refund, log it here. Example: `Partner contacted — confirmed no issues were reported to on-site team during the stay. Partner declined to issue a refund.`

**3. Follow-Ups / Actions Taken**
Follow `references/section-3-actions-followups.md` in full.

**4. Resolution**
Follow `references/section-4-resolution.md` in full.

---

### Step 3 — Auto-select Complaint Type and Fault Assignment

Based on the complaint text, select the most appropriate values from `https://raw.githubusercontent.com/Laurenvdm-web/jetchat-hubspot-automation/main/references/complaint-types.md` and carry them forward to ticket creation.

---

### Step 4 — Create the ticket in HubSpot

Create immediately using the HubSpot MCP `manage_crm_objects` tool:

- `subject`: `Test — [Guest Name] / [Booking ID] — [Resort Name] — [Complaint Title]`
- `hs_pipeline`: `811252326`
- `hs_pipeline_stage`: `1194468363`
- `hs_ticket_priority`: `HIGH` if escalation_flag is true, otherwise `LOW`
- `hubspot_owner_id`: `752197810` ← Lauren van der Merwe — never change
- `resort_name`: resort name from orchestrator
- `property_manager`: property manager from orchestrator
- `cirrus_url`: Cirrus URL from orchestrator
- `complaint_type`: auto-selected HubSpot enum value from `references/complaint-types.md`
- `fault_assignment`: auto-selected HubSpot enum value from `references/complaint-types.md`
- `account_mananger`: HubSpot owner ID for account manager (note: field key has typo — use exactly `account_mananger`)
- `suggested_compensation_amount`: `0` (always send even when blank — HubSpot requires it)
- `currency`: `USD` (always send — HubSpot requires it when compensation amount is present)
- `content`: the 4-section ticket body

---

## Looking up the Account Manager

If the account manager is not provided by the orchestrator, look it up via Cirrus:

1. Use the booking's `resortId` to call `get_resort` in Cirrus.
2. Read the `jetstreamAccountManager` field from the resort record — this is the account manager's name.
3. Once you have the name, look up their HubSpot owner ID using the `search_owners` tool.
4. Use that owner ID as the `account_mananger` field value when creating the ticket.

**Account manager substitution rule:** If the resolved account manager name is "Ben Day", replace with Kirsten instead. Look up Kirsten's owner ID using `search_owners` (search "Kirsten") and use her ID for the `account_mananger` field.

---

## Rules

- Always read the reference files first — do not rely on memory for section rules
- Ticket owner is always Lauren van der Merwe (ID: `752197810`) — never change
- Priority is High if escalation_flag is true, Low otherwise — never default to Low without checking
- Auto-select complaint_type and fault_assignment based on the complaint — do not ask Lauren to confirm
- If any input is missing, flag it clearly and ask Lauren to fill it in manually
