# MOXIEPRO - COMPLETE IMPLEMENTATION PLAN

**Last Updated:** January 2025
**Status:** Ready to Build Phase 1
**Budget:** $100
**Timeline:** 4 weeks to MVP

---

## 📋 TABLE OF CONTENTS

1. [Project Overview](#project-overview)
2. [Final Decisions](#final-decisions)
3. [Tech Stack](#tech-stack)
4. [What's Already Built](#whats-already-built)
5. [Phase 1: Backend Foundation](#phase-1-backend-foundation)
6. [Phase 2: Payment Integration](#phase-2-payment-integration)
7. [Phase 3: Frontend Updates](#phase-3-frontend-updates)
8. [Phase 4: Launch Prep](#phase-4-launch-prep)
9. [Cost Breakdown](#cost-breakdown)
10. [Quick Reference](#quick-reference)

---

## 🎯 PROJECT OVERVIEW

**What is MoxiePro?**
A SaaS tool that audits Psychology Today profiles and provides actionable optimization recommendations.

**Revenue Model:**
- Free: Basic audit preview (score + vague issues)
- $14.99: Full PDF audit (11 detailed sections)
- $297: Done-for-you optimization service (future)

**Target Market:**
Therapists with Psychology Today profiles who want more clients

**Unique Value:**
AI-powered analysis that shows exact revenue impact of profile issues

---

## ✅ FINAL DECISIONS

### Business Model
- [x] **Email Collection:** YES - Required before showing results
- [x] **Audit Access:** 30 days, then expires
- [x] **Account System:** None for MVP (email + magic link only)
- [x] **Duplicate Audits:** Cache per URL for 30 days
- [x] **PDF Storage:** 60 days access for paid users
- [x] **PDF Regeneration:** $4.99 after 60 days
- [x] **Profile URL Cache:** 30 days per URL

### Payments
- [x] **Payment Processor:** Lemon Squeezy (works in Pakistan)
- [x] **Migration Plan:** Switch to Stripe later when LLC ready
- [x] **Pricing:** $14.99 for full audit PDF
- [x] **Refund Policy:** 7-day money-back guarantee

### Features
- [x] **Loading UX:** Progress bar + educational tips
- [x] **Email Strategy:** Resend (transactional) + SendFox (marketing)
- [x] **Results Page:** Single page with tabs
- [x] **Rate Limiting:** 3 audits per IP per day
- [x] **Data Retention:** Keep paid audits + emails for marketing

### Referral System
- [x] **Tier 1 (1-5 refs):** $3 credit per referral
- [x] **Tier 2 (6-15 refs):** $5 credit per referral
- [x] **Tier 3 (16+ refs):** $8 credit per referral + Free PDF at 16
- [x] **30-Day Challenge:** Refer 6 friends in 30 days = Free PDF unlock
- [x] **Influencer Commission:** 25% (add later)

---

## 🛠️ TECH STACK

### Frontend
- **Framework:** React 18
- **Build Tool:** Vite
- **Styling:** Tailwind CSS
- **Routing:** React Router v6
- **State Management:** React Hooks (useState, useEffect)

### Backend
- **Hosting:** Vercel (serverless functions)
- **Database:** Vercel KV (Redis)
- **PDF Storage:** Vercel Blob Storage
- **API Framework:** Vercel serverless functions

### Integrations
- **AI Analysis:** Claude 3.5 Sonnet with prompt caching (~$0.12/audit)
- **Scraper:** Puppeteer + Chromium
- **Payments:** Lemon Squeezy (5% + $0.50)
- **Emails (Transactional):** Resend (3,000/month free)
- **Emails (Marketing):** SendFox (lifetime free)
- **PDF Generation:** @react-pdf/renderer
- **Analytics:** Google Analytics + Hotjar
- **Data Backup:** Google Sheets API

### Development Tools
- **Version Control:** Git + GitHub
- **Package Manager:** npm
- **Environment Variables:** .env.local
- **Deployment:** Vercel CLI

---

## ✅ WHAT'S ALREADY BUILT (40% Complete)

### Frontend - 90% Done
- [x] HomePage.jsx - Landing page with hero, features, CTAs
- [x] AuditPage.jsx - Manual form (needs URL input added)
- [x] ResultsPage.jsx - Mock audit display (needs lock/unlock)
- [x] AboutPage.jsx - About MoxiePro
- [x] PricingPage.jsx - Pricing tiers
- [x] ContactPage.jsx - Contact form
- [x] CaseStudiesPage.jsx - Success stories
- [x] Header.jsx - Navigation
- [x] Footer.jsx - Footer links
- [x] App.jsx - Routing setup
- [x] Tailwind config
- [x] Basic scoring system

### Backend - 0% Done
- [ ] No /api folder yet
- [ ] No serverless functions
- [ ] No database connection
- [ ] No payment integration
- [ ] No scraper
- [ ] No Claude AI integration

### Content/Assets
- [x] F4 Master Audit Template (11 sections, ~8000 words)
- [ ] Legal pages (Terms, Privacy, Refund)
- [ ] Email templates
- [ ] PDF templates

---

## 🚀 PHASE 1: BACKEND FOUNDATION
**Timeline:** Week 1 (Days 1-7)
**Priority:** HIGH
**Goal:** Get scraper + Claude + database working

### Week 1, Day 1: Project Setup (2 hours)

**1.1 Install Dependencies**
```bash
cd MoxiePro
npm install @vercel/kv @anthropic-ai/sdk puppeteer-core @sparticuz/chromium stripe @react-pdf/renderer googleapis
npm install -D @types/node
```

**1.2 Create Environment Variables**

Create `.env.local` file:
```
# Claude AI
ANTHROPIC_API_KEY=sk-ant-xxx

# Vercel KV (auto-added when you create KV database)
KV_URL=
KV_REST_API_URL=
KV_REST_API_TOKEN=
KV_REST_API_READ_ONLY_TOKEN=

# Lemon Squeezy
LEMON_SQUEEZY_API_KEY=
LEMON_SQUEEZY_STORE_ID=
LEMON_SQUEEZY_WEBHOOK_SECRET=

# Resend Email
RESEND_API_KEY=

# SendFox
SENDFOX_API_KEY=
SENDFOX_LIST_ID=

# Google Sheets (optional backup)
GOOGLE_SHEETS_CREDENTIALS=
GOOGLE_SHEET_ID=

# App Settings
VITE_APP_URL=http://localhost:5173
```

**1.3 Create API Folder Structure**
```
api/
├── scrape.js           # Scrapes Psychology Today profiles
├── analyze.js          # Claude AI audit generation
├── save-audit.js       # Saves audit to Vercel KV
├── get-audit.js        # Retrieves audit from KV
├── create-payment.js   # Lemon Squeezy checkout
├── webhook.js          # Payment webhook handler
├── generate-pdf.js     # PDF generation
├── send-email.js       # Email sending
└── utils/
    ├── kv.js           # Vercel KV helpers
    ├── claude.js       # Claude API helpers
    └── errors.js       # Error handling
```

**1.4 Deploy to Vercel (First Time)**
```bash
npm install -g vercel
vercel login
vercel
# Follow prompts, select defaults
```

**1.5 Create Vercel KV Database**
1. Go to Vercel Dashboard
2. Select your project
3. Go to "Storage" tab
4. Click "Create Database"
5. Choose "KV"
6. Name it "moxiepro-kv"
7. Click "Create"
8. Environment variables auto-added to project

**✅ Day 1 Complete When:**
- [ ] All dependencies installed
- [ ] .env.local created with placeholders
- [ ] /api folder structure created
- [ ] Project deployed to Vercel
- [ ] Vercel KV database created

---

### Week 1, Day 2-3: Build Scraper (6 hours)

**2.1 Create Psychology Today Scraper**

File: `api/scrape.js`

**What it does:**
- Accepts PT profile URL
- Uses Puppeteer to scrape profile data
- Extracts: headline, about, specialties, fees, location, photo
- Returns structured JSON

**Key Features:**
- Handles private/deleted profiles
- Extracts text from all sections
- Gets therapist name, credentials, location
- Error handling for invalid URLs

**Testing:**
```bash
# Test locally
curl -X POST http://localhost:3000/api/scrape \
  -H "Content-Type: application/json" \
  -d '{"profileUrl": "https://www.psychologytoday.com/us/therapists/..."}'
```

**2.2 Create Scraper Utility Functions**

File: `api/utils/scraper-helpers.js`

**Functions:**
- `validatePTUrl(url)` - Check if valid PT URL
- `extractHeadline(page)` - Get headline text
- `extractAbout(page)` - Get about section
- `extractSpecialties(page)` - Get specialty list
- `extractLocation(page)` - Get city, state
- `extractFees(page)` - Get pricing info

**✅ Day 2-3 Complete When:**
- [ ] Scraper can extract 10+ fields from PT profiles
- [ ] Handles errors gracefully
- [ ] Returns structured JSON
- [ ] Tested with 5+ real PT profiles

---

### Week 1, Day 4-5: Integrate Claude AI (8 hours)

**3.1 Create Claude API Integration**

File: `api/analyze.js`

**What it does:**
- Receives scraped profile data
- Loads F4 master template (prompt caching)
- Sends to Claude API for analysis
- Returns complete 11-section audit

**Prompt Structure:**
```
System: You are a Psychology Today profile optimization expert...

User: Analyze this profile:
[Scraped data]

Use this framework:
[F4 Master Template - cached]

Return JSON with:
- overallScore
- performanceLevel
- topIssuesPreview (3 vague)
- topIssuesDetailed (5 detailed)
- sectionScores (6 sections)
- quickWins (3 fixes)
- revenueAnalysis
- marketAnalysis
- competitorAnalysis
- optimizationPreview
- implementationRoadmap
```

**3.2 Implement Prompt Caching**

File: `api/utils/claude.js`

**Why:** Saves ~60% on Claude API costs

**How:**
- F4 template is 10,400 tokens
- Cache it for 5 minutes
- Only pay for it once
- Subsequent calls only pay for profile data

**Cost per audit:**
- Without caching: ~$0.15
- With caching: ~$0.12
- Savings: 20%

**3.3 Test Claude Integration**
```bash
# Test with sample profile data
node scripts/test-claude.js
```

**✅ Day 4-5 Complete When:**
- [ ] Claude returns complete audit JSON
- [ ] Prompt caching working
- [ ] Response time < 20 seconds
- [ ] Tested with 3+ profiles
- [ ] Cost per audit ~$0.12

---

### Week 1, Day 6: Database Functions (4 hours)

**4.1 Create KV Helper Functions**

File: `api/utils/kv.js`

**Functions:**

```javascript
// Save audit after generation
async saveAudit(auditId, data) {
  // Saves to: audit:{auditId}
  // TTL: 30 days
}

// Get audit for results page
async getAudit(auditId) {
  // Returns full audit data
}

// Update payment status
async markAsPaid(auditId, paymentId) {
  // Sets isPaid: true
}

// Check if URL already audited
async getAuditByProfileUrl(url) {
  // Returns audit if exists within 30 days
}

// Save user email for marketing
async saveUserEmail(email, auditId, profileUrl) {
  // Saves to: user:{email}
  // For SendFox integration
}

// Rate limiting
async checkRateLimit(ipAddress) {
  // Returns audits count for IP today
  // Increments counter
}
```

**4.2 Create Data Models**

**Audit Object:**
```javascript
{
  id: "audit_abc123",
  profileUrl: "https://psychologytoday.com/...",
  userEmail: "therapist@email.com",
  isPaid: false,
  auditData: {
    overallScore: 67,
    performanceLevel: "Below Average",
    // ... 11 sections
  },
  createdAt: 1705334400000,
  expiresAt: 1707926400000, // 30 days later
  pdfGeneratedAt: null,
  pdfUrl: null
}
```

**User Object:**
```javascript
{
  email: "therapist@email.com",
  audits: ["audit_abc123", "audit_def456"],
  referralCode: "JANE-SMITH-XYZ",
  referralCredits: 15,
  referralCount: 3,
  createdAt: 1705334400000
}
```

**✅ Day 6 Complete When:**
- [ ] All KV functions created and tested
- [ ] Can save/retrieve audits
- [ ] Rate limiting works
- [ ] 30-day expiration logic working

---

### Week 1, Day 7: Connect Everything (4 hours)

**5.1 Create Main Audit Flow Endpoint**

File: `api/create-audit.js`

**Flow:**
```
1. Receive: email + profileUrl
2. Validate inputs
3. Check rate limit (3 per IP/day)
4. Check if URL already audited (30-day cache)
   → If yes: Return existing audit
   → If no: Continue
5. Run scraper → get profile data
6. Run Claude AI → get audit
7. Save to KV with 30-day expiry
8. Save email to SendFox
9. Save to Google Sheets (backup)
10. Send email with audit link
11. Return auditId
```

**5.2 Error Handling**

All API endpoints should return:
```javascript
// Success
{ success: true, data: {...} }

// Error
{
  success: false,
  error: "ERROR_CODE",
  message: "User-friendly message",
  details: {...} // For logging
}
```

**Error Codes:**
- `INVALID_URL` - Not a valid PT profile URL
- `PROFILE_NOT_FOUND` - Profile doesn't exist
- `SCRAPER_FAILED` - Can't access profile
- `RATE_LIMIT_EXCEEDED` - Too many requests
- `AUDIT_GENERATION_FAILED` - Claude API error

**5.3 Test Complete Flow**

```bash
# End-to-end test
curl -X POST http://localhost:3000/api/create-audit \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "profileUrl": "https://www.psychologytoday.com/us/therapists/..."
  }'

# Should return:
{
  "success": true,
  "auditId": "audit_abc123",
  "message": "Audit complete! Check your email."
}
```

**✅ Week 1 Complete When:**
- [ ] Can submit email + URL
- [ ] Scraper extracts profile data
- [ ] Claude generates full audit
- [ ] Audit saved to KV
- [ ] Email sent with results link
- [ ] Rate limiting prevents abuse
- [ ] URL caching prevents duplicate audits
- [ ] All errors handled gracefully

---

## 💳 PHASE 2: PAYMENT INTEGRATION
**Timeline:** Week 2 (Days 8-14)
**Priority:** HIGH
**Goal:** Users can pay $14.99 to unlock audit

### Week 2, Day 8-9: Lemon Squeezy Setup (4 hours)

**6.1 Create Lemon Squeezy Account**

1. Go to lemonsqueezy.com
2. Sign up with email
3. Complete business details
4. Add payout method (Payoneer/Wise works from Pakistan)
5. Copy API key from Settings → API

**6.2 Create Product in Lemon Squeezy**

1. Go to Products → New Product
2. Name: "Psychology Today Profile Audit"
3. Price: $14.99 USD
4. Type: Digital product
5. Description: "Complete 11-section audit with PDF download"
6. Save & copy Product ID

**6.3 Set Up Webhook**

1. Go to Settings → Webhooks
2. Click "Add Endpoint"
3. URL: `https://your-app.vercel.app/api/webhook`
4. Events: Select "order_created" and "subscription_payment_success"
5. Copy signing secret

**✅ Day 8-9 Complete When:**
- [ ] Lemon Squeezy account created
- [ ] Product created ($14.99 audit)
- [ ] Webhook configured
- [ ] API keys added to .env

---

### Week 2, Day 10-11: Payment Flow (6 hours)

**7.1 Create Payment Endpoint**

File: `api/create-payment.js`

**What it does:**
- Receives auditId
- Creates Lemon Squeezy checkout session
- Includes audit metadata
- Returns checkout URL

**Flow:**
```javascript
POST /api/create-payment
Body: { auditId: "audit_abc123" }

1. Get audit from KV
2. Verify audit exists and not paid
3. Create Lemon Squeezy checkout:
   - Product: $14.99 audit
   - Custom data: { auditId }
   - Success URL: /results/{auditId}?paid=true
   - Cancel URL: /results/{auditId}
4. Return checkout URL
```

**7.2 Create Webhook Handler**

File: `api/webhook.js`

**What it does:**
- Receives Lemon Squeezy webhook
- Verifies signature
- Updates audit status in KV
- Triggers PDF generation
- Sends receipt email

**Flow:**
```javascript
POST /api/webhook (from Lemon Squeezy)

1. Verify webhook signature
2. Parse event data
3. Extract auditId from custom_data
4. Update KV:
   - isPaid: true
   - paymentId: ls_xxx
   - paidAt: timestamp
5. Generate PDF (async)
6. Send receipt email with PDF link
7. Return 200 OK
```

**7.3 Test Payment Flow**

Use Lemon Squeezy test mode:
```bash
# Create test checkout
curl -X POST http://localhost:3000/api/create-payment \
  -H "Content-Type: application/json" \
  -d '{"auditId": "audit_abc123"}'

# Returns checkout URL, test payment with test card
```

**✅ Day 10-11 Complete When:**
- [ ] Payment endpoint creates checkout
- [ ] Webhook receives payment events
- [ ] Audit marked as paid in KV
- [ ] Receipt email sent
- [ ] Tested with Lemon Squeezy test mode

---

### Week 2, Day 12-13: PDF Generation (8 hours)

**8.1 Create PDF Template**

File: `api/utils/pdf-template.jsx`

**Sections:**
1. Cover page with logo and score
2. Executive summary
3. Top 5 issues (detailed)
4. Section-by-section scores (chart)
5. Revenue opportunity analysis
6. Market analysis
7. Competitor comparison
8. Quick wins (actionable)
9. Optimization preview (before/after)
10. 30-day implementation roadmap
11. Footer with branding

**Design:**
- Professional layout
- Charts for scores (using react-pdf-charts)
- Icons/emojis for visual interest
- Page breaks in right places
- Branded colors (purple/blue)
- Total pages: ~15-20
- File size: 500-700KB

**8.2 Create PDF Generation Endpoint**

File: `api/generate-pdf.js`

**Flow:**
```javascript
POST /api/generate-pdf
Body: { auditId: "audit_abc123" }

1. Get audit from KV
2. Verify isPaid === true
3. Check if PDF already generated (cache)
4. Generate PDF using @react-pdf/renderer
5. Upload to Vercel Blob Storage
6. Save PDF URL to KV
7. Set expiry: 60 days
8. Return PDF download URL
```

**8.3 PDF Storage**

Use Vercel Blob:
```bash
npm install @vercel/blob
```

**Cost:** $0.15/month per 1000 PDFs (1MB each)

**8.4 Test PDF Generation**

```bash
# Generate test PDF
curl -X POST http://localhost:3000/api/generate-pdf \
  -H "Content-Type: application/json" \
  -d '{"auditId": "audit_abc123"}'

# Returns PDF URL, download and verify
```

**✅ Day 12-13 Complete When:**
- [ ] PDF template styled beautifully
- [ ] PDF generated after payment
- [ ] Stored in Vercel Blob
- [ ] Download link works
- [ ] PDF includes all 11 sections
- [ ] File size < 1MB
- [ ] Mobile-viewable

---

### Week 2, Day 14: Email System (4 hours)

**9.1 Set Up Resend**

1. Go to resend.com
2. Sign up and verify email
3. Add domain (or use resend.dev for testing)
4. Copy API key
5. Create email templates

**9.2 Create Email Templates**

File: `api/utils/email-templates.js`

**Templates:**

**1. Audit Complete Email**
```
Subject: Your Psychology Today Audit is Ready! 🎯

Hi [Name],

Your profile audit is complete!

Your Score: [X]/100 - [Performance Level]

We found [5] critical issues that might be costing you clients.

View your free preview:
[Link to Results Page]

Want the full audit with revenue analysis and step-by-step fixes?
Unlock for just $14.99

- MoxiePro Team
```

**2. Payment Receipt Email**
```
Subject: Receipt: Psychology Today Audit ($14.99)

Hi [Name],

Thanks for your purchase!

Receipt #: [Payment ID]
Amount: $14.99
Date: [Date]

Download your full audit PDF:
[PDF Download Link]

Questions? Reply to this email.

- MoxiePro Team
```

**3. Reminder Email (Day 3)**
```
Subject: Don't forget your audit results! ⏰

Hi [Name],

You started an audit 3 days ago but haven't unlocked the full report yet.

Your score: [X]/100

The full audit shows:
✓ Exact revenue impact of each issue
✓ 3 quick wins you can implement today
✓ 30-day optimization roadmap

Unlock now for $14.99

- MoxiePro Team
```

**4. Urgency Email (Day 25)**
```
Subject: Your audit expires in 5 days! ⚠️

Hi [Name],

Your Psychology Today audit expires in 5 days.

After that, you'll lose access to:
- Your detailed score breakdown
- Revenue opportunity analysis
- Step-by-step optimization plan

Unlock now to keep forever: $14.99

- MoxiePro Team
```

**9.3 Set Up SendFox**

1. Log in to SendFox
2. Create list: "MoxiePro Users"
3. Create automation:
   - Day 3: Reminder email
   - Day 7: Urgency email
   - Day 14: Upsell email ($297)
   - Day 25: Expiration warning
4. Copy API key

**9.4 Create Email Sending Function**

File: `api/send-email.js`

```javascript
// Resend for immediate emails
async sendTransactionalEmail(to, template, data)

// SendFox for drip campaigns
async addToMarketingList(email, name, auditId)
```

**✅ Week 2 Complete When:**
- [ ] Resend account set up
- [ ] 4 email templates created
- [ ] SendFox automation configured
- [ ] Emails sent after audit complete
- [ ] Receipt sent after payment
- [ ] Drip campaign working
- [ ] Tested all emails

---

## 🎨 PHASE 3: FRONTEND UPDATES
**Timeline:** Week 3 (Days 15-21)
**Priority:** HIGH
**Goal:** Update UI for real flow + payments

### Week 3, Day 15-16: Audit Page Updates (4 hours)

**10.1 Update AuditPage Component**

File: `src/pages/AuditPage.jsx`

**Changes:**
- Add email input field (required)
- Add Psychology Today URL input
- Add Terms/Privacy checkbox
- Remove old 8-field manual form
- Add loading state with progress + tips
- Connect to /api/create-audit

**New Form:**
```jsx
<form onSubmit={handleSubmit}>
  <input
    type="email"
    placeholder="Your email"
    required
  />

  <input
    type="url"
    placeholder="Psychology Today profile URL"
    pattern=".*psychologytoday\.com.*"
    required
  />

  <label>
    <input type="checkbox" required />
    I agree to Terms of Service and Privacy Policy
  </label>

  <button type="submit">
    Analyze My Profile
  </button>
</form>
```

**10.2 Create Loading Screen**

File: `src/components/LoadingScreen.jsx`

**Features:**
- Progress bar (0-100%)
- Educational tips that rotate
- Estimated time remaining
- Animated

**Tips:**
```javascript
const tips = [
  "70% of therapists have generic headlines...",
  "Profiles with photos get 2x more inquiries...",
  "Client-focused language increases bookings by 40%...",
  "Top therapists mention location 3-5 times...",
  "Your headline is #1 factor in PT search ranking..."
]
```

**10.3 Handle API Response**

```javascript
const handleSubmit = async (e) => {
  e.preventDefault()
  setLoading(true)

  try {
    const response = await fetch('/api/create-audit', {
      method: 'POST',
      body: JSON.stringify({ email, profileUrl })
    })

    const data = await response.json()

    if (data.success) {
      // Redirect to results
      navigate(`/results/${data.auditId}`)
    } else {
      // Show error
      setError(data.message)
    }
  } catch (err) {
    setError('Something went wrong. Please try again.')
  } finally {
    setLoading(false)
  }
}
```

**✅ Day 15-16 Complete When:**
- [ ] Form updated with email + URL
- [ ] Loading screen with tips
- [ ] Connects to API
- [ ] Shows errors gracefully
- [ ] Redirects to results after completion

---

### Week 3, Day 17-19: Results Page Overhaul (12 hours)

**11.1 Update ResultsPage Component**

File: `src/pages/ResultsPage.jsx`

**Structure:**
```jsx
<ResultsPage>
  {/* Always visible */}
  <ScoreHero score={67} level="Below Average" />

  {/* Tab navigation */}
  <Tabs>
    <Tab name="Overview" icon="🎯">
      <FreePreview issues={vagueIssues} />
      {!isPaid && <UnlockCTA price="$14.99" />}
    </Tab>

    <Tab name="Issues" locked={!isPaid}>
      {isPaid ? (
        <DetailedIssues issues={detailedIssues} />
      ) : (
        <LockedSection title="5 Critical Issues" />
      )}
    </Tab>

    <Tab name="Opportunities" locked={!isPaid}>
      {isPaid ? (
        <RevenueAnalysis data={revenueData} />
      ) : (
        <LockedSection title="Revenue Opportunity" />
      )}
    </Tab>

    <Tab name="Action Plan" locked={!isPaid}>
      {isPaid ? (
        <>
          <QuickWins wins={quickWins} />
          <Roadmap plan={roadmap} />
        </>
      ) : (
        <LockedSection title="Implementation Plan" />
      )}
    </Tab>

    <Tab name="Upgrade" highlight>
      <OptimizationUpsell price="$297" />
    </Tab>
  </Tabs>

  {/* Referral section (if unpaid) */}
  {!isPaid && <ReferralChallenge auditId={auditId} />}
</ResultsPage>
```

**11.2 Create LockedSection Component**

File: `src/components/LockedSection.jsx`

**Features:**
- Blurred background
- Lock icon overlay
- Teaser text
- "Unlock" button
- Shows hint of content

```jsx
<div className="locked-section">
  <div className="blur-overlay">
    <LockIcon />
    <h3>{title}</h3>
    <p>{teaser}</p>
    <button onClick={handleUnlock}>
      Unlock Full Audit - $14.99
    </button>
  </div>

  <div className="blurred-content">
    {previewContent}
  </div>
</div>
```

**11.3 Create Payment Button**

File: `src/components/PaymentButton.jsx`

**Flow:**
```javascript
const handlePayment = async () => {
  setLoading(true)

  try {
    // Create checkout session
    const response = await fetch('/api/create-payment', {
      method: 'POST',
      body: JSON.stringify({ auditId })
    })

    const data = await response.json()

    // Redirect to Lemon Squeezy checkout
    window.location.href = data.checkoutUrl

  } catch (err) {
    alert('Payment failed. Please try again.')
  } finally {
    setLoading(false)
  }
}
```

**11.4 Create Referral Challenge Section**

File: `src/components/ReferralChallenge.jsx`

**Features:**
- Progress bar (X/6 referrals)
- Countdown timer (30 days)
- Unique referral link
- Share buttons
- List of referred friends

```jsx
<div className="referral-challenge">
  <h2>🎁 Unlock Your Free PDF!</h2>
  <p>Refer 6 friends in 30 days → Get full audit FREE</p>

  <ProgressBar current={3} target={6} />

  <div className="timer">
    Time left: {daysLeft} days
  </div>

  <input
    value={`moxiepro.com/r/${referralCode}`}
    readOnly
  />
  <button onClick={copyLink}>Copy Link</button>

  <div className="referrals">
    <h3>Your Referrals:</h3>
    <ul>
      <li>✅ John D. - Paid (Jan 15)</li>
      <li>✅ Sarah K. - Paid (Jan 16)</li>
      <li>✅ Mike T. - Paid (Jan 18)</li>
      <li>⏳ 3 more needed</li>
    </ul>
  </div>
</div>
```

**✅ Day 17-19 Complete When:**
- [ ] Results page shows score + vague issues (free)
- [ ] Locked sections blurred
- [ ] Payment button works
- [ ] Redirects to Lemon Squeezy checkout
- [ ] After payment, content unlocked
- [ ] PDF download button appears (if paid)
- [ ] Referral challenge shown (if unpaid)
- [ ] All tabs working
- [ ] Mobile responsive

---

### Week 3, Day 20-21: Polish & Testing (8 hours)

**12.1 Add Analytics**

**Google Analytics:**
```jsx
// src/index.jsx
import ReactGA from 'react-ga4'

ReactGA.initialize('G-XXXXXXXXXX')

// Track page views
useEffect(() => {
  ReactGA.send({ hitType: 'pageview', page: location.pathname })
}, [location])
```

**Track Events:**
```javascript
// Audit started
ReactGA.event({
  category: 'Audit',
  action: 'Started',
  label: profileUrl
})

// Payment initiated
ReactGA.event({
  category: 'Payment',
  action: 'Initiated',
  value: 14.99
})

// PDF downloaded
ReactGA.event({
  category: 'PDF',
  action: 'Downloaded',
  label: auditId
})
```

**12.2 Add Hotjar**

```html
<!-- public/index.html -->
<script>
  (function(h,o,t,j,a,r){
    // Hotjar tracking code
  })(window,document,'https://static.hotjar.com/c/hotjar-','.js?sv=');
</script>
```

**12.3 Error Boundaries**

File: `src/components/ErrorBoundary.jsx`

```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false }

  static getDerivedStateFromError(error) {
    return { hasError: true }
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-page">
          <h1>Something went wrong</h1>
          <button onClick={() => window.location.href = '/'}>
            Go Home
          </button>
        </div>
      )
    }

    return this.props.children
  }
}
```

**12.4 Mobile Testing Checklist**

Test on 3+ devices:
- [ ] iPhone (Safari)
- [ ] Android (Chrome)
- [ ] Tablet (iPad)

Check:
- [ ] Forms work
- [ ] Navigation works
- [ ] Payment flow works
- [ ] PDF downloads work
- [ ] Images load
- [ ] Text readable
- [ ] Buttons tappable
- [ ] Loading states visible

**12.5 Browser Testing**

Test on:
- [ ] Chrome
- [ ] Firefox
- [ ] Safari
- [ ] Edge

**✅ Week 3 Complete When:**
- [ ] Analytics tracking all events
- [ ] Hotjar recording sessions
- [ ] Error boundaries catching errors
- [ ] Tested on 3+ mobile devices
- [ ] Tested on 4 browsers
- [ ] All bugs fixed

---

## 📜 PHASE 4: LAUNCH PREP
**Timeline:** Week 4 (Days 22-30)
**Priority:** MEDIUM
**Goal:** Legal pages, final testing, soft launch

### Week 4, Day 22-23: Legal Pages (4 hours)

**13.1 Generate Terms of Service**

1. Go to termsfeed.com/privacy-policy-generator
2. Fill in:
   - Company: MoxiePro
   - Website: [your-domain].com
   - Service: AI-powered profile audits
   - Payment terms: $14.99 one-time
   - Refund policy: 7-day guarantee
3. Download HTML
4. Create `src/pages/TermsPage.jsx`
5. Paste content

**Key Points to Include:**
- Service description
- Payment terms ($14.99)
- Refund policy (7-day money-back)
- Limitation of liability
- User responsibilities
- Prohibited use

**13.2 Generate Privacy Policy**

1. Go to termsfeed.com/privacy-policy-generator
2. Fill in:
   - Data collected: Email, profile URL, audit data
   - How used: Generate audit, send emails, marketing
   - Third parties: Stripe, Resend, Google Analytics
   - Cookies: Yes (analytics)
   - GDPR compliance: Yes
3. Download HTML
4. Create `src/pages/PrivacyPage.jsx`
5. Paste content

**Key Points to Include:**
- What data we collect
- How we use it
- Who we share it with
- How long we keep it (30 days free, forever paid)
- User rights (GDPR/CCPA)
- Contact for deletion requests

**13.3 Create Refund Policy Page**

File: `src/pages/RefundPage.jsx`

**Content:**
```markdown
# Refund Policy

## 7-Day Money-Back Guarantee

If you're not satisfied with your audit, we'll refund you. No questions asked.

### How to Request a Refund

1. Email support@moxiepro.com within 7 days of purchase
2. Include your order number
3. Tell us why you're unsatisfied (optional, helps us improve)

### Processing Time

Refunds processed within 5-7 business days.

### What Happens

- You keep access to the audit for 7 days
- After refund, audit link expires
- You can't purchase the same audit again

### Non-Refundable

- After 7 days
- If you've already used recommendations and seen results
- If audit was obtained via referral (free)

Questions? Email support@moxiepro.com
```

**13.4 Update Footer Links**

File: `src/components/Footer.jsx`

Add links to:
- Terms of Service
- Privacy Policy
- Refund Policy
- Contact

**✅ Day 22-23 Complete When:**
- [ ] Terms of Service page live
- [ ] Privacy Policy page live
- [ ] Refund Policy page live
- [ ] Footer links working
- [ ] All legal requirements met

---

### Week 4, Day 24-26: Final Testing (12 hours)

**14.1 End-to-End Testing**

**Test Flow 1: Free User**
1. [ ] Go to homepage
2. [ ] Click "Get Free Audit"
3. [ ] Enter email + PT URL
4. [ ] See loading screen with tips
5. [ ] Redirected to results
6. [ ] See score + vague issues
7. [ ] All sections locked/blurred
8. [ ] Email received with link
9. [ ] Can return via email link
10. [ ] Referral challenge shown

**Test Flow 2: Paid User**
1. [ ] On results page, click "Unlock $14.99"
2. [ ] Redirected to Lemon Squeezy
3. [ ] Complete test payment
4. [ ] Redirected back to results
5. [ ] All content unlocked
6. [ ] PDF download button appears
7. [ ] Click download, PDF opens
8. [ ] Receipt email received
9. [ ] $297 upsell shown

**Test Flow 3: Referral User**
1. [ ] Share referral link
2. [ ] Friend uses link
3. [ ] Friend pays $10.49 (30% off)
4. [ ] Referral counted
5. [ ] Progress bar updated
6. [ ] After 6 referrals, audit unlocked free

**14.2 Edge Case Testing**

- [ ] Invalid PT URL → Show error
- [ ] Already audited URL → Show cached audit
- [ ] Rate limit exceeded → Show error
- [ ] Payment fails → Show error, allow retry
- [ ] Webhook fails → Retry 3 times
- [ ] PDF generation fails → Retry, then manual email
- [ ] Email fails → Log error, manual follow-up

**14.3 Performance Testing**

- [ ] Homepage loads < 2 seconds
- [ ] Audit completes < 30 seconds
- [ ] Results page loads < 1 second
- [ ] Payment redirect < 2 seconds
- [ ] PDF downloads < 5 seconds

**14.4 Security Testing**

- [ ] Rate limiting prevents spam (3/day)
- [ ] Webhook signature verified
- [ ] No sensitive data in URLs
- [ ] HTTPS everywhere
- [ ] No SQL injection possible (using KV)
- [ ] API keys not exposed

**✅ Day 24-26 Complete When:**
- [ ] All flows tested 5+ times
- [ ] All edge cases handled
- [ ] Performance acceptable
- [ ] Security verified
- [ ] Bug list created and prioritized
- [ ] Critical bugs fixed

---

### Week 4, Day 27-28: Soft Launch (8 hours)

**15.1 Beta Testing**

**Recruit 10 beta testers:**
- Offer free audit + $297 optimization
- In exchange for:
  - Honest feedback
  - Video or text testimonial
  - Permission to use their name

**How to find:**
- Your 4K therapist group
- Facebook therapist groups
- LinkedIn direct messages
- Reddit r/therapists (carefully)

**15.2 Collect Feedback**

Create Google Form:
- How easy was it to use? (1-10)
- Was the audit helpful? (1-10)
- Would you pay $14.99 for this? (Yes/No)
- What would you improve?
- Can we use your testimonial?

**15.3 Iterate Based on Feedback**

Common feedback to expect:
- "Loading took too long" → Add better progress indicators
- "Too much text" → Add more visuals
- "Wanted specific examples" → Enhance Claude prompt
- "Didn't trust AI" → Add human verification note

**15.4 Create Case Studies**

For each beta tester:
- Before: Score + issues
- After: Implemented recommendations
- Results: X% more inquiries/bookings
- Testimonial quote
- Photo (if allowed)

**✅ Day 27-28 Complete When:**
- [ ] 10 beta testers signed up
- [ ] All completed audits
- [ ] Feedback collected
- [ ] 3+ testimonials received
- [ ] 1-2 case studies documented
- [ ] Priority bugs fixed

---

### Week 4, Day 29-30: Production Deploy (4 hours)

**16.1 Pre-Launch Checklist**

**Environment:**
- [ ] All API keys in Vercel env vars
- [ ] Lemon Squeezy in live mode
- [ ] Resend verified domain (or using resend.dev)
- [ ] Google Analytics tracking ID
- [ ] Hotjar site ID

**Content:**
- [ ] All pages reviewed for typos
- [ ] All links working
- [ ] Legal pages live
- [ ] F4 template finalized
- [ ] Email templates reviewed

**Technical:**
- [ ] Latest code deployed to Vercel
- [ ] Database (KV) created in production
- [ ] Blob storage set up
- [ ] Webhooks configured
- [ ] Error logging enabled
- [ ] Rate limiting active

**Marketing:**
- [ ] Landing page SEO optimized
- [ ] Meta tags added (title, description, image)
- [ ] Favicon uploaded
- [ ] Social media images ready
- [ ] Launch post drafted

**16.2 Deploy to Production**

```bash
# Pull latest code
git pull origin main

# Test locally one final time
npm run dev

# Deploy to Vercel
vercel --prod

# Verify deployment
curl https://your-app.vercel.app/api/health
```

**16.3 Post-Deploy Testing**

- [ ] Homepage loads
- [ ] Can submit audit
- [ ] Audit completes
- [ ] Email received
- [ ] Payment works (test with real card, then refund)
- [ ] Webhook received
- [ ] PDF generates
- [ ] All emails sent

**16.4 Monitor First 24 Hours**

Watch for:
- Error rates (should be < 1%)
- Audit completion rate (should be > 90%)
- Payment success rate (should be > 95%)
- Email delivery rate (should be > 98%)

**Tools:**
- Vercel dashboard (functions, errors)
- Lemon Squeezy dashboard (payments)
- Google Analytics (traffic, conversions)
- Hotjar (user behavior)

**✅ Week 4 Complete When:**
- [ ] Production deployed
- [ ] All systems tested live
- [ ] Monitoring set up
- [ ] Ready for public launch
- [ ] Launch plan ready

---

## 💰 COST BREAKDOWN

### One-Time Setup Costs
- Vercel account: $0
- Lemon Squeezy account: $0
- Claude API credits: $50 (prepaid)
- Domain (optional): $12/year
- **Total Setup: $50-62**

### Monthly Costs (100 audits/month)
- Vercel hosting: $0 (free tier)
- Vercel KV: $0 (free tier)
- Vercel Blob: $0.15 (PDF storage)
- Claude API: $12 (100 audits × $0.12)
- Resend: $0 (free tier)
- SendFox: $0 (lifetime)
- Google Analytics: $0
- Hotjar: $0 (free tier)
- **Total Monthly: ~$12**

### Revenue (100 audits, 30% conversion)
- 30 sales × $14.99 = $449.70
- Lemon Squeezy fees: ~$37.50 (5% + $0.50)
- Net revenue: $412.20
- **Profit: $400/month**

### At Scale (1000 audits/month)
- Costs: $120 (Claude) + $1.50 (storage) = $121.50
- Revenue: 300 × $14.99 = $4,497
- Fees: $375
- Net: $4,122
- **Profit: $4,000/month**

---

## 📚 QUICK REFERENCE

### Important URLs
- **Production:** https://your-app.vercel.app
- **Vercel Dashboard:** https://vercel.com/dashboard
- **Lemon Squeezy:** https://app.lemonsqueezy.com
- **Resend:** https://resend.com/dashboard
- **SendFox:** https://sendfox.com
- **Google Analytics:** https://analytics.google.com

### API Endpoints
```
POST /api/create-audit        - Start new audit
GET  /api/get-audit/:id        - Get audit by ID
POST /api/create-payment       - Create checkout
POST /api/webhook              - Payment webhook
POST /api/generate-pdf         - Generate PDF
POST /api/send-email           - Send email
```

### Environment Variables Needed
```
ANTHROPIC_API_KEY
KV_URL
KV_REST_API_URL
KV_REST_API_TOKEN
LEMON_SQUEEZY_API_KEY
LEMON_SQUEEZY_STORE_ID
LEMON_SQUEEZY_WEBHOOK_SECRET
RESEND_API_KEY
SENDFOX_API_KEY
SENDFOX_LIST_ID
GOOGLE_SHEETS_CREDENTIALS
GOOGLE_SHEET_ID
VITE_APP_URL
```

### Common Commands
```bash
# Install dependencies
npm install

# Run locally
npm run dev

# Deploy to Vercel
vercel --prod

# Test API endpoint
curl -X POST http://localhost:3000/api/create-audit \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","profileUrl":"https://..."}'

# View logs
vercel logs

# Pull env vars
vercel env pull
```

### Support Resources
- **Claude API Docs:** https://docs.anthropic.com
- **Vercel Docs:** https://vercel.com/docs
- **Lemon Squeezy Docs:** https://docs.lemonsqueezy.com
- **React PDF Docs:** https://react-pdf.org

---

## 🎯 SUCCESS METRICS

### Week 1 Goal
- [ ] Scraper working
- [ ] Claude generating audits
- [ ] Audits saved to database

### Week 2 Goal
- [ ] Payments working
- [ ] PDFs generating
- [ ] Emails sending

### Week 3 Goal
- [ ] Frontend updated
- [ ] Payment flow complete
- [ ] Mobile tested

### Week 4 Goal
- [ ] 10 beta users
- [ ] 3+ testimonials
- [ ] Production deployed

### Month 1 Goal (Post-Launch)
- 100 audits submitted
- 30 paid customers
- $450 revenue
- $400 profit

### Month 3 Goal
- 500 audits submitted
- 150 paid customers
- $2,250 revenue
- $2,000 profit

---

## ❓ TROUBLESHOOTING

### Scraper Fails
- Check if PT changed their HTML structure
- Verify Puppeteer is using correct selectors
- Test with incognito/private mode
- Check if profile is actually public

### Claude API Errors
- Verify API key is correct
- Check if you have credits
- Ensure prompt isn't too long (max 200k tokens)
- Try with simpler prompt

### Payment Not Working
- Verify Lemon Squeezy in live mode
- Check webhook URL is correct
- Verify webhook signature
- Test with test mode first

### Emails Not Sending
- Verify Resend API key
- Check domain verification
- Look at Resend logs
- Test with personal email first

### PDF Not Generating
- Check if @react-pdf/renderer installed
- Verify Vercel Blob configured
- Check file size < 50MB
- Test generation locally first

---

## 📞 NEED HELP?

**If stuck for > 2 hours:**
1. Check error logs in Vercel
2. Search error message + "vercel" or "react"
3. Ask in MoxiePro development chat (if available)
4. Post in r/webdev or r/vercel (be specific)

**Email for issues:**
- Technical: dev@moxiepro.com (set this up)
- Business: support@moxiepro.com

---

## 🚀 YOU GOT THIS!

You've done 40% already. The remaining 60% is just connecting pieces.

Remember:
- ✅ I'll write all the code
- ✅ You just copy-paste and test
- ✅ Each phase builds on the previous
- ✅ Take it step by step
- ✅ Ask questions when stuck

**Next Step:** Week 1, Day 1 - Project Setup

Ready? Let's build! 💪
