# Invoice Dispute Auto-Resolver - Build Plan

## THE PROBLEM

Freelancers lose $2-5K/year on clients who:
- Ghost on invoices
- Pay late repeatedly
- Dispute charges without basis
- Drag out payment for months

**Current solutions:** FreshBooks, Wave, QuickBooks send reminders but don't escalate or help recover.

**What freelancers want:** A system that automatically escalates from polite to firm to legal threats.

---

## MVP FEATURES (Version 1.0)

### Core Flow:
1. **Import invoice** (manual entry OR connect to existing tool)
2. **Set due date + client details**
3. **Automatic escalation sequence:**
   - Day 1 past due: Polite reminder
   - Day 7: Firm follow-up
   - Day 14: Final notice
   - Day 21: Collections warning
   - Day 30: Small claims template generated

### What's NOT in MVP:
- Bank integrations
- Payment processing
- Legal representation
- Multiple currencies
- Team features

---

## TECHNICAL ARCHITECTURE

### Option A: Standalone Web App (Recommended for MVP)

```
Frontend: Next.js or React
Backend: Node.js + Express OR Python + FastAPI
Database: PostgreSQL or Supabase
Auth: Clerk or Auth0
Email: Resend or SendGrid
Hosting: Vercel or Railway
```

**Why this stack:**
- Fast to build
- Cheap to host
- Easy to scale later

---

### Option B: Browser Extension (Alternative)

Add escalation features directly to FreshBooks/Wave/QuickBooks.

**Pros:**
- Users already in their invoicing tool
- Lower friction

**Cons:**
- Harder to build (multiple integrations)
- Less control
- Dependent on platform APIs

**Recommendation:** Build standalone first, add integrations later.

---

## DATABASE SCHEMA (Simplified)

```sql
users
- id
- email
- name
- created_at

clients
- id
- user_id
- name
- email
- company_name
- phone (optional)

invoices
- id
- user_id
- client_id
- amount
- currency
- due_date
- status (pending, overdue, paid, disputed)
- original_invoice_url (optional)
- created_at

escalation_logs
- id
- invoice_id
- escalation_level (1-5)
- sent_at
- email_content
- opened (boolean)
- clicked (boolean)

templates
- id
- escalation_level
- subject
- body
- is_custom (boolean)
```

---

## EMAIL TEMPLATES (The Core Value)

### Level 1: Polite Reminder (Day 1)
```
Subject: Invoice #[NUMBER] - Quick Reminder

Hi [CLIENT_NAME],

Hope you're doing well! Just wanted to bump this invoice to the top of your inbox.

Invoice #[NUMBER]: $[AMOUNT]
Due: [DATE]

Link to pay: [LINK]

Thanks!
[USER_NAME]
```

---

### Level 2: Firm Follow-Up (Day 7)
```
Subject: Invoice #[NUMBER] - Payment Overdue

Hi [CLIENT_NAME],

Following up on invoice #[NUMBER] for $[AMOUNT], which was due on [DATE].

I understand things get busy, but timely payment helps me keep doing great work for you.

Could you let me know if there's an issue with the invoice or when I can expect payment?

Link: [LINK]

Thanks,
[USER_NAME]
```

---

### Level 3: Final Notice (Day 14)
```
Subject: FINAL NOTICE - Invoice #[NUMBER] - [DAYS] Days Overdue

Hi [CLIENT_NAME],

This is my final notice for invoice #[NUMBER] ($[AMOUNT]), now [DAYS] days overdue.

I've reached out multiple times without response. I value our relationship, but I need this resolved.

Please confirm payment by [DATE + 7 DAYS] to avoid further action.

[USER_NAME]
```

---

### Level 4: Collections Warning (Day 21)
```
Subject: NOTICE: Account Being Sent to Collections

[CLIENT_NAME],

Invoice #[NUMBER] for $[AMOUNT] is now 21 days overdue.

If payment is not received within 7 days, I will:
1. Transfer this debt to a collections agency
2. Report this to credit bureaus
3. Pursue legal action in small claims court

You will be responsible for all collection fees and legal costs.

Last chance to resolve this amicably.

[USER_NAME]
```

---

### Level 5: Small Claims Template (Day 30)
```
Generates PDF with:
- Court filing instructions
- Pre-filled small claims form
- Evidence checklist
- Step-by-step guide
```

---

## DEVELOPMENT PHASES

### Phase 1: MVP (2-3 weeks)

**Week 1: Foundation**
- [ ] Set up project (Next.js + Supabase)
- [ ] User auth (Clerk)
- [ ] Database schema
- [ ] Basic UI (dashboard, invoice list)

**Week 2: Core Features**
- [ ] Add invoice manually
- [ ] Client management
- [ ] Email template system
- [ ] Send test emails

**Week 3: Automation**
- [ ] Cron job for checking overdue invoices
- [ ] Automatic email sending
- [ ] Escalation logic
- [ ] Email tracking (opened/clicked)

---

### Phase 2: Polish (1-2 weeks)

- [ ] Better email templates (markdown editor)
- [ ] Invoice import (CSV upload)
- [ ] Dashboard analytics
- [ ] Mobile responsive
- [ ] Error handling
- [ ] Onboarding flow

---

### Phase 3: Growth Features

- [ ] Integrate with FreshBooks API
- [ ] Integrate with Wave API
- [ ] Integrate with QuickBooks API
- [ ] Payment processing (Stripe)
- [ ] SMS reminders (Twilio)
- [ ] Automated small claims filing

---

## MONETIZATION

### Option 1: Subscription
- **Free:** 3 invoices, basic templates
- **Pro ($15/month):** Unlimited invoices, custom templates, analytics
- **Business ($29/month):** Team features, integrations, priority support

### Option 2: Success Fee
- Free to use
- 5% of recovered invoices over 60 days late
- Only pay if it works

### Option 3: Hybrid
- Free for invoices under $500
- $9.99/month for unlimited
- $49 one-time for small claims template pack

**Recommendation:** Start with subscription (predictable revenue)

---

## COSTS TO RUN

| Service | Cost |
|---------|------|
| Hosting (Vercel) | Free - $20/mo |
| Database (Supabase) | Free - $25/mo |
| Auth (Clerk) | Free - $25/mo |
| Email (Resend) | Free - $20/mo |
| Domain | $12/year |

**First 100 users:** ~$0-50/month
**At scale (1000+ users):** ~$100-200/month

---

## GO-TO-MARKET

### Week 1-2: Beta
1. Post in r/freelance, r/upwork, r/forhire
2. "I built a tool to help freelancers get paid - looking for 10 beta testers"
3. Offer free lifetime Pro for feedback
4. Collect testimonials

### Week 3-4: Launch
1. Product Hunt launch
2. r/SideProject post
3. Cold email freelancers from Twitter
4. Partner with freelance communities

### Month 2-3: Growth
1. Content marketing (SEO)
   - "How to collect unpaid invoices"
   - "Freelancer guide to late payments"
   - "Small claims court for freelancers"
2. Twitter/X threads
3. Guest posts on freelance blogs
4. Affiliate program (20% commission)

---

## LEGAL CONSIDERATIONS

**Important:** You are NOT providing legal advice.

1. **Terms of Service:**
   - "This is not legal advice"
   - "Templates are starting points"
   - "Consult a lawyer for legal matters"

2. **Email compliance:**
   - Include unsubscribe link
   - CAN-SPAM compliance
   - GDPR for EU users

3. **Small claims templates:**
   - Must be clearly "for information only"
   - Different rules by jurisdiction
   - Recommend legal consultation

---

## COMPETITIVE ADVANTAGE

**Why you win:**
1. **Focus:** Only for freelancers (not enterprise)
2. **Escalation:** No one does the automatic escalation well
3. **Price:** $15/mo vs $300/hr lawyer
4. **Templates:** Pre-written by someone who knows what works

**Why competitors haven't done this:**
- FreshBooks/QuickBooks don't want to be aggressive (bad for their brand)
- Lawyers want to keep billing hours
- Collections agencies only want big debts

---

## MVP CODE SNIPPETS

### Cron Job (check overdue invoices daily)

```javascript
// api/cron/check-overdue.js
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(process.env.SUPABASE_URL, process.env.SUPABASE_KEY)

export default async function handler(req, res) {
  // Verify cron secret
  if (req.headers.authorization !== `Bearer ${process.env.CRON_SECRET}`) {
    return res.status(401).json({ error: 'Unauthorized' })
  }

  // Get overdue invoices
  const { data: invoices } = await supabase
    .from('invoices')
    .select('*, clients(*), users(*)')
    .eq('status', 'overdue')

  for (const invoice of invoices) {
    const daysOverdue = Math.floor((new Date() - new Date(invoice.due_date)) / (1000 * 60 * 60 * 24))

    // Determine escalation level
    let level = 1
    if (daysOverdue >= 30) level = 5
    else if (daysOverdue >= 21) level = 4
    else if (daysOverdue >= 14) level = 3
    else if (daysOverdue >= 7) level = 2

    // Check if already sent this level
    const { data: existing } = await supabase
      .from('escalation_logs')
      .select()
      .eq('invoice_id', invoice.id)
      .eq('escalation_level', level)

    if (existing.length === 0) {
      // Send email
      await sendEscalationEmail(invoice, level)

      // Log it
      await supabase.from('escalation_logs').insert({
        invoice_id: invoice.id,
        escalation_level: level,
        sent_at: new Date()
      })
    }
  }

  res.json({ success: true, processed: invoices.length })
}
```

---

### Send Email Function

```javascript
import { Resend } from 'resend'

const resend = new Resend(process.env.RESEND_API_KEY)

async function sendEscalationEmail(invoice, level) {
  const template = getEmailTemplate(level, invoice)

  await resend.emails.send({
    from: `${invoice.users.name} via PaidUp <noreply@paidup.app>`,
    to: invoice.clients.email,
    subject: template.subject,
    html: template.body,
    replyTo: invoice.users.email
  })
}

function getEmailTemplate(level, invoice) {
  const templates = {
    1: {
      subject: `Invoice #${invoice.id} - Quick Reminder`,
      body: `Hi ${invoice.clients.name},<br><br>Just a friendly reminder about invoice #${invoice.id} for $${invoice.amount}, due on ${invoice.due_date}.<br><br>Thanks!<br>${invoice.users.name}`
    },
    2: {
      subject: `Invoice #${invoice.id} - Payment Overdue`,
      body: `Hi ${invoice.clients.name},<br><br>Following up on invoice #${invoice.id} for $${invoice.amount}, which was due on ${invoice.due_date}.<br><br>Please let me know if there's an issue.<br><br>Thanks,<br>${invoice.users.name}`
    },
    // ... levels 3-5
  }

  return templates[level]
}
```

---

## FIRST 10 CUSTOMERS STRATEGY

1. **Post on Reddit:**
   - "I built a tool to chase late-paying clients. Looking for 10 beta testers who hate chasing invoices."
   - r/freelance, r/upwork, r/Entrepreneur

2. **Cold DM freelancers on Twitter:**
   - Search "late payment" "unpaid invoice"
   - Reply with "I built something for this"
   - DM them

3. **Indie Hackers:**
   - Post in "Share your startup"
   - Offer free Pro for honest feedback

4. **Your network:**
   - Any freelancer friends?
   - Post on LinkedIn
   - Ask for intros

---

## SUCCESS METRICS

**Week 1-2:**
- 10 beta users
- 50 invoices tracked
- 5 testimonials

**Month 1:**
- 50 users
- 5 paying customers
- First "I got paid!" story

**Month 3:**
- 200 users
- 30 paying customers ($450/month)
- Featured in a freelance newsletter

**Month 6:**
- 1000 users
- 100 paying customers ($1500/month)
- First integration (FreshBooks)

---

## NEXT STEPS

1. **Today:** Set up repo, Next.js project, Supabase
2. **This week:** Build auth + invoice CRUD
3. **Next week:** Email system + templates
4. **Week 3:** Cron jobs + automation
5. **Week 4:** Polish + beta launch

---

## TECH STACK RECOMMENDATIONS

**If you know JavaScript:**
- Next.js + Supabase + Resend

**If you know Python:**
- FastAPI + PostgreSQL + SendGrid

**If you want fastest path:**
- Bubble.io (no-code)
- Ready in 1-2 weeks
- Less flexible but faster

---

Want me to:
1. Set up the project structure?
2. Write more detailed code for specific features?
3. Research competitors more deeply?
4. Create a landing page design?
