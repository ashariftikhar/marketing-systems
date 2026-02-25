# Automation Workflow — Lead Capture & Nurture Flow

**Tool:** n8n
**Trigger:** Form submission (Netlify Forms / Typeform webhook)
**Output:** CRM entry + Email sequence + Slack notification

---

## Workflow Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                        TRIGGER                                       │
│              Form Submitted (Webhook Node)                           │
│         { name, email, website, source }                            │
└─────────────────────────┬───────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────────┐
│                      DATA PROCESSING                                 │
│                    Set Node — Format Data                            │
│         • Timestamp added                                            │
│         • Source tagged (UTM parameter)                              │
│         • Lead score initialized (default: 10)                       │
└──────────┬──────────────────────────────────┬───────────────────────┘
           │                                  │
           ▼                                  ▼
┌──────────────────────┐          ┌───────────────────────┐
│   CRM — Create       │          │   Email Node          │
│   Record in Notion   │          │   Send Welcome Email  │
│   or HubSpot         │          │   (< 60 seconds)      │
│   • Name             │          │   • Personalized      │
│   • Email            │          │   • Delivers asset    │
│   • Website          │          │   • Sets expectation  │
│   • Source           │          └───────────┬───────────┘
│   • Score: 10        │                      │
└──────────────────────┘                      │
                                              ▼
                           ┌──────────────────────────────┐
                           │   Slack Notification          │
                           │   → #leads channel            │
                           │   "New lead: {name}           │
                           │    from {source}              │
                           │    website: {website}"        │
                           └──────────────┬───────────────┘
                                          │
                                          ▼
                           ┌──────────────────────────────┐
                           │   Wait Node: 2 Days           │
                           └──────────────┬───────────────┘
                                          │
                                          ▼
                           ┌──────────────────────────────┐
                           │   Email 2 — Follow-up        │
                           │   "Quick tip while you wait" │
                           │   + 1 actionable insight      │
                           └──────────────┬───────────────┘
                                          │
                                          ▼
                           ┌──────────────────────────────┐
                           │   Wait Node: 3 Days           │
                           └──────────────┬───────────────┘
                                          │
                                          ▼
                           ┌──────────────────────────────┐
                           │   Email 3 — Case Study        │
                           │   Relevant to their industry  │
                           │   + soft CTA                  │
                           └──────────────┬───────────────┘
                                          │
                                          ▼
                           ┌──────────────────────────────┐
                           │   Wait Node: 4 Days           │
                           └──────────────┬───────────────┘
                                          │
                                          ▼
                           ┌──────────────────────────────┐
                           │   Email 4 — Call Invite       │
                           │   "20-min audit walkthrough"  │
                           │   + Calendly link             │
                           └──────────────┬───────────────┘
                                          │
                                          ▼
                           ┌──────────────────────────────┐
                           │   Wait Node: 5 Days           │
                           └──────────────┬───────────────┘
                                          │
                                          ▼
                           ┌──────────────────────────────┐
                           │   Email 5 — Final             │
                           │   "Closing your file"         │
                           │   Last chance to engage        │
                           └──────────────┬───────────────┘
                                          │
                                          ▼
                           ┌──────────────────────────────┐
                           │   Tag as "Cold" in CRM        │
                           │   Move to re-engagement       │
                           │   sequence (90 days later)    │
                           └──────────────────────────────┘
```

---

## Lead Scoring Logic

Update lead score automatically based on behavior:

| Action | Score Change |
|--------|-------------|
| Email 1 opened | +5 |
| Email 1 clicked | +10 |
| Email 2 opened | +5 |
| Email 3 clicked (case study) | +15 |
| Email 4 clicked (Calendly) | +25 |
| Call booked | +50 |

**Threshold:** Score ≥ 40 → Slack alert to flag for personal outreach.

---

## n8n Nodes Required

| Node | Purpose |
|------|---------|
| Webhook | Receive form submission |
| Set | Format and structure data |
| HTTP Request | Create CRM record via API |
| Send Email | All email sequence steps |
| Wait | Delays between sequence emails |
| Slack | Lead notification |
| IF | Conditional scoring logic |
| Merge | Combine parallel branches |

---

## Setup Instructions

1. **Create n8n account** — cloud.n8n.io or self-host on a VPS
2. **Set up Webhook node** — copy the webhook URL
3. **Add webhook URL to your form** — Netlify Forms → Notifications → Webhook
4. **Configure email credentials** — Gmail OAuth or SMTP
5. **Connect Notion/HubSpot** — use official n8n integrations
6. **Test with a dummy submission** — verify all nodes fire correctly
7. **Activate workflow** — set to "Active" in n8n dashboard

---

*Part of the [Marketing Systems](https://github.com/ashariftikhar/marketing-systems) repository by [Ashar Iftikhar](https://github.com/ashariftikhar)*
