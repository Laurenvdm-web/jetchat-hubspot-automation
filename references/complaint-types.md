# Complaint Types & Fault Assigned — Reference

Use this file to auto-select the most appropriate complaint type and fault assignment based on the complaint text. Do not ask Lauren to confirm — just pick and proceed.

---

## Complaint Type Options

1. Content & Listing Accuracy
2. Cleanliness & Pests
3. Property Condition
4. Check-In & Access
5. Cancellation Policy Discrepancy
6. Billing & Charges
7. Noise & External Disturbance
8. Safety & Health Risk
9. Overbooking / Unit Unavailable
10. Technical / System Failure
11. Partner Initiated Cancellations
12. Other

### HubSpot Enum Mapping

| Selection | HubSpot enum value |
|---|---|
| Content & Listing Accuracy | `content_issue_partial_refund` |
| Cleanliness & Pests | `cleaning_issue_up_to_cleaning_fee` |
| Property Condition | `maintenance_issue_partial_full_refund` |
| Check-In & Access | `checkin_issue_partial_refund` |
| Cancellation Policy Discrepancy | `Cancellation Policy Descrepency` |
| Billing & Charges | `Disputes/Charge Backs` |
| Noise & External Disturbance | `noise_issue_partial_refund` |
| Safety & Health Risk | `safety_issue_full_refund_if_severe` |
| Overbooking / Unit Unavailable | `overbooking_full_refund` |
| Technical / System Failure | `Technical / System Failure` |
| Partner Initiated Cancellations | `Partner Initiated Cancellations` |
| Other | `Other` |

---

## Fault Assignment Options

1. Partner at fault
2. Jetstream at fault
3. Unclear / Under Investigation
4. Not Applicable

### HubSpot Field

- Field key: `fault_assignment`

| Selection | HubSpot enum value |
|---|---|
| Partner at fault | `partner_at_fault` |
| Jetstream at fault | `jetstream_at_fault` |
| Unclear / Under Investigation | `unsure_lack_of_info` |
| Not Applicable | `Not Applicable` |
