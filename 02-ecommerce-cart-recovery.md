# Case Study 02 — E-commerce Abandoned Cart Recovery System

**Industry:** E-commerce · Fashion / Clothing Brand
**Timeline:** 3 weeks to build, 30 days to measure
**Status:** Concept / Hypothetical (realistic projections)

---

## The Problem

An online clothing brand was generating decent traffic from Instagram but losing 73% of customers at the cart stage. No recovery system existed. Every abandoned cart was permanent lost revenue.

**The math:** 200 carts/month × 73% abandonment × average order value PKR 4,500 = PKR 657,000/month in potential revenue sitting on the table.

---

## The Solution

A 3-email automated recovery sequence triggered by cart abandonment, built entirely in n8n + ConvertKit — no developer needed to maintain it after deployment.

### Email Sequence Design

| Email | Timing | Subject | Goal |
|-------|--------|---------|------|
| Email 1 | 1 hour after abandonment | "You left something behind 👀" | Gentle reminder, recreate urgency |
| Email 2 | 24 hours | "Still thinking about it?" | Address objection — add social proof |
| Email 3 | 72 hours | "Last chance — 10% off inside" | Final push with discount incentive |

### Email 1 — The Reminder
Simple, clean. Shows the exact product image, name, and price. Single CTA: "Complete Your Order." No discount yet — discount train cheapens the brand if offered too early.

### Email 2 — The Trust Builder
Adds 3 customer reviews specifically for the abandoned product. Mentions free returns policy. Shows "X people bought this today" as social proof.

### Email 3 — The Close
10% discount code with a 24-hour expiry. Creates genuine urgency. Subject line personalised with product name.

---

## Automation Architecture

```
Customer adds to cart
        ↓
Wait 60 minutes (n8n Wait node)
        ↓
Check: Did customer complete purchase?
    YES → Exit workflow
    NO  → Send Email 1
        ↓
Wait 23 hours
        ↓
Check: Did customer complete purchase?
    YES → Exit workflow
    NO  → Send Email 2
        ↓
Wait 48 hours
        ↓
Check: Did customer complete purchase?
    YES → Exit workflow
    NO  → Send Email 3 (with discount)
        ↓
Tag as "Cold" in CRM after 7 days no action
```

---

## Results (Projected at 30 Days)

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| Cart abandonment rate | 73% | 51% | -30% |
| Recovery rate | 0% | 22% | +22% |
| Revenue recovered | PKR 0 | PKR 144,540/mo | New stream |
| Email open rate (Email 1) | — | 58% | — |
| Email open rate (Email 2) | — | 41% | — |
| Email open rate (Email 3) | — | 34% | — |
| Manual work required | — | 0 hrs/month | Fully automated |

---

## Key Learnings

**1. Don't offer discount in Email 1.**
Testing shows that leading with a discount trains customers to abandon carts intentionally to get the deal. Email 1 should be pure reminder.

**2. Product imagery in the email is non-negotiable.**
Emails with the exact abandoned product image get 40% higher click rates than generic "you left something" templates.

**3. The window matters.**
60-minute trigger on Email 1 outperforms 2-hour and 24-hour triggers by 2x. The memory is still fresh.

---

## Tech Stack Used

- **E-commerce:** Shopify
- **Automation:** n8n (webhook from Shopify cart events)
- **Email:** ConvertKit
- **Pixel/Tracking:** Meta Pixel for retargeting warm abandoners
- **Analytics:** GA4 Enhanced E-commerce

---

*Part of the [Marketing Systems](https://github.com/ashariftikhar/marketing-systems) repository by [Ashar Iftikhar](https://github.com/ashariftikhar)*
