# Pakistan Freelancer Savings Guide — Tool Spec
**Date:** Feb 25, 2026
**Status:** Design phase

---

## Concept

One-page interactive tool that:
1. **HOOKS** with a personalized savings calculator ("You're losing $X/year")
2. **EDUCATES** by walking through the compliance journey in plain language
3. **TRACKS** progress with a visual savings bar as they complete steps
4. **CONVERTS** by naturally positioning Cenoa as the solution at key friction points

---

## Target User

Pakistani freelancer, 20-35, earning $500-$5,000/month from international clients via Upwork/Fiverr/direct work. Uses Payoneer. Probably not registered with PSEB. Probably a non-filer. Doesn't know about the 80% rule. Doesn't know how much money they're leaving on the table.

---

## Page Structure

### SECTION 1: The Hook — Savings Calculator

**Headline:** "How Much Money Are You Leaving on the Table?"
**Subtext:** "Most Pakistani freelancers lose $500-$2,000 every year to hidden fees, wrong tax rates, and missed registrations. Let's find your number."

**Inputs (4 fields):**

1. **Monthly income in USD** — Slider + text input, range $100 - $20,000, default $1,500
2. **How do you receive payments?** — Dropdown:
   - Payoneer (default)
   - Wise
   - Direct bank wire
   - Other / Mixed
3. **Are you registered with PSEB (Tech Destination)?** — Yes / No / What's that? (default: "What's that?")
4. **Do you file income tax returns?** — Yes / No / Not sure (default: "Not sure")

**Calculate button** → reveals results below

**Results Panel — "Your Annual Savings Report"**

Shows 3 columns:

| | Your Current Situation | After Completing the Journey | You Save |
|---|---|---|---|
| Platform fees | -$X | -$Y (with Cenoa) | $Z |
| Tax rate | X% ($X) | 0.25% ($Y) | $Z |
| Non-filer costs | ~$X est. | $0 | $Z |
| 80% rule risk | ⚠️ Unknown | ✅ Protected | Peace of mind |
| **TOTAL** | **-$X/year** | **-$Y/year** | **💰 $Z/year** |

### Calculation Logic

**Platform fee calculation:**
```
if paymentMethod == "payoneer":
  receivingFee = annualIncome * 0.01  // 1% receiving
  fxMarkup = annualIncome * 0.025     // ~2.5% FX (conservative estimate)
  totalPlatformFee = receivingFee + fxMarkup  // ~3.5%

if paymentMethod == "wise":
  transferFee = annualIncome * 0.01   // ~1% transfer fee
  fxMarkup = annualIncome * 0.005     // ~0.5% FX (Wise is better here)
  totalPlatformFee = transferFee + fxMarkup  // ~1.5%

if paymentMethod == "wire":
  // Assume 2 transfers/month, $25 average combined fee
  totalPlatformFee = 24 * 25  // $600/year fixed

cenoaFee = annualIncome * 0.0099  // 0.99%
platformSavings = totalPlatformFee - cenoaFee
```

**Tax calculation:**
```
if psebRegistered == true:
  currentTaxRate = 0.0025  // 0.25%
else:
  currentTaxRate = 0.01    // 1%

optimizedTaxRate = 0.0025  // 0.25% (after PSEB registration)
taxSavings = (currentTaxRate - optimizedTaxRate) * annualIncome
// Only show if not already registered
```

**Non-filer cost estimation:**
```
if isFiler == false:
  // Conservative estimate based on typical freelancer life
  // Property: won't calculate (too variable)
  // Vehicle: won't calculate (too variable)
  // Bank withdrawal tax: 0.6% vs 0.3% on cash withdrawals
  // Bank profit tax: 35% vs 15% on savings
  // Airport tax: PKR 30K vs 15K per trip (assume 1 trip/year)
  // We'll show a range estimate
  nonFilerCost = "PKR 30,000 - 150,000/year" // ($100 - $525)
  // Display: "You pay 2-3x more on property, vehicles, bank fees, and travel"
```

**Total savings:**
```
totalSavings = platformSavings + taxSavings + nonFilerEstimate
```

### Example Output for $2,000/month freelancer, Payoneer, no PSEB, non-filer:

```
📊 Your Annual Savings Report

Annual income: $24,000/year

WHAT YOU'RE LOSING NOW:
├── Payoneer fees (1% receiving + 2.5% FX)       -$840/year
├── Tax at 1% (no PSEB registration)              -$240/year  
├── Non-filer penalties on daily life              -$100 to $525/year (est.)
├── 80% rule compliance                           ⚠️ Not tracking — risk of 45% tax
│
│   Total losses: ~$1,180 - $1,605/year

AFTER COMPLETING THE JOURNEY:
├── Cenoa fees (0.99%)                             -$238/year
├── Tax at 0.25% (PSEB registered)                 -$60/year
├── Filer status (reduced rates everywhere)         $0 extra
├── 80% rule tracking                              ✅ Protected
│
│   Total costs: ~$298/year

💰 YOU SAVE: $882 - $1,307/year
   That's $73 - $109 extra in your pocket every month.
```

---

### SECTION 2: The Journey — 6 Steps

Below the calculator. Each step:
- Has a clear title and "what you unlock"
- Shows savings contribution (ties back to the number above)
- Has a collapsible detailed guide
- Has a checkbox (tracked in localStorage)
- Updates the progress bar when checked

#### Progress Bar (sticky, follows scroll):
```
Your Journey:  ██████░░░░░░░░░░  Step 3 of 6
Savings unlocked: $580/year ✅  |  Remaining: $720/year 🔓
```

---

#### STEP 1: Become a Tax Filer
**Unlocks:** Reduced rates on everything in life
**Savings contribution:** $100-525/year (non-filer penalties avoided)

**Why (plain language):**
Being a non-filer in Pakistan doesn't just mean you don't file taxes. It means you pay MORE on practically everything:
- Property purchase tax: 10% instead of 3%
- Vehicle registration: 2-3 times higher
- Cash withdrawal tax: double (0.6% vs 0.3%)
- Bank savings tax: 35% instead of 15%
- Airport departure: PKR 30,000 instead of 15,000
- Your SIM card can be blocked
- You can't buy property above PKR 5 million
- You can't buy vehicles above PKR 7 million

**How (step-by-step):**
1. Get your NTN (National Tax Number):
   - Go to FBR IRIS portal: https://iris.fbr.gov.pk
   - Your NTN = your CNIC number (national ID number)
   - Register with your CNIC + mobile number
   - Complete registration form with income details
   
2. File your income tax return:
   - Log into IRIS portal
   - Select "Income Tax Return" for current tax year
   - Declare all income (export + local)
   - File Wealth Statement (required even if income is low)
   - Deadline: September 30 each year (subject to extensions)

3. Verify your Active Taxpayer status:
   - Check ATL: https://e.fbr.gov.pk/esbn/Loging.aspx
   - Enter your CNIC → should show "Active"
   - ATL is updated on October 1 each year

**Time required:** ~1-2 hours online
**Cost:** Free

---

#### STEP 2: Open a Freelancer Bank Account
**Unlocks:** Proper payment channel + USD retention + correct bank certificates
**Savings contribution:** Foundation for everything else

**Why:**
Most Pakistani freelancers use their regular bank account (or JazzCash/NayaPay). A "Freelancer Account" is a special account type that:
- Comes with a linked USD account (ESFCA) where you can keep dollars
- Automatically processes foreign remittances correctly
- Issues PRCs (bank certificates) that you need for tax benefits
- Lets you pay for tools and subscriptions directly in USD

**What is ESFCA?**
Exporters' Special Foreign Currency Account. The government lets you keep up to **$5,000/month or 50% of your earnings** (whichever is higher) in USD inside Pakistan. You get a debit card that works for international online payments. No more blocked Canva or ChatGPT subscriptions.

**How:**
1. Choose a bank. Best options for freelancers:
   - **Meezan Bank** — Payoneer partnership, auto e-PRC, Islamic banking
   - **UBL** — Online account opening, good freelancer support
   - **Allied Bank** — Online/app opening, straightforward process
   - **Faysal Bank** — Competitive FX rates
   
2. Open the account:
   - Online: UBL, Allied Bank allow digital opening via app
   - Branch: Meezan requires branch visit but has best Payoneer integration
   - You'll get TWO accounts: PKR account + linked ESFCA (USD/EUR/GBP)
   
3. Documents needed:
   - CNIC
   - Proof of freelance income (Payoneer/Upwork statements)
   - PSEB certificate (helpful but usually not required just to open)

4. Set your USD retention percentage:
   - Tell the bank how much of each remittance to keep in USD
   - Rest is auto-converted to PKR at bank's rate
   - You can change this by visiting the branch

**Time required:** 1-3 days
**Cost:** Free (most banks)

---

#### STEP 3: Fix Your Payment Method
**Unlocks:** $600+/year in fee savings
**Savings contribution:** Platform fee reduction

**The problem with Payoneer:**
When you withdraw $1,000 from Payoneer to your Pakistani bank, you don't receive $1,000 worth of PKR. Payoneer takes:
- ~1% receiving fee when your client pays you: -$10
- ~2.5% hidden in the exchange rate when you withdraw: -$25
- **You receive ~$965 worth of PKR out of $1,000**

Over a year at $2,000/month: **that's $840 gone.**

**How Payoneer's "hidden fee" works:**
Payoneer shows you an exchange rate when you withdraw. But this rate is NOT the real market rate (what you'd see on Google). It's typically 2-3% worse. This difference is their profit — and they don't show it as a "fee."

**Your options (comparison at $24,000/year):**

| Method | Annual Cost | Bank code (PRC) correct? | Counts for 80% rule? |
|---|---|---|---|
| Payoneer → Regular bank | ~$840 | ❌ Usually wrong code — you must visit bank to fix | ⚠️ Counts, but proving it is harder |
| Payoneer → Meezan Freelancer | ~$840 | ✅ Auto e-PRC (Meezan-Payoneer partnership) | ✅ Counts |
| Wise → Pakistani bank | ~$360 | ❌ Shows as local transfer — bank can't easily fix | ❌ May not count |
| Direct wire | ~$600 | ✅ Correct automatically (comes as foreign remittance) | ✅ Counts |
| **Cenoa** | **~$238** | **❓ Check with Cenoa support** | **❓ Check with Cenoa support** |

**Note on Cenoa PRC status:** We're confirming how Cenoa routes payments to Pakistan. If it arrives through international banking channels (SWIFT), your bank will automatically recognize it as foreign income and issue a correct PRC. If it routes through a local partner (like Wise does), the same code problem may apply. Ask Cenoa support before switching. ⚠️

**Recommendation:** If you're on Payoneer, at minimum switch to a Meezan Freelancer Account for the auto e-PRC. For lower fees, consider Cenoa (0.99% vs ~3.5%).

*[CTA: Open Free Cenoa Account →]*

---

#### STEP 4: Register with PSEB (Tech Destination)
**Unlocks:** Tax drops from 1% → 0.25%
**Savings contribution:** $180/year on $24K income

**What it is:**
PSEB (Pakistan Software Export Board), now called Tech Destination, is the government's official registry for IT freelancers and exporters. Think of it as getting your "freelancer license."

**What it gets you:**
- Tax rate drops from 1% to 0.25% (that's 4x less)
- Government recognition of your work
- Access to training, grants, and international programs
- Professional credibility

**Important: You do NOT need PRCs (bank certificates) to register for the first time.** You only need basic documents. This is a common misconception that stops people from starting. The correct order is: register first → get your PSEB certificate → THEN show it to your bank → bank starts giving you correct PRCs going forward.

**What you need to register (first time):**
- ✅ NTN / tax registration (Step 1)
- ✅ Bank account (Step 2)
- ✅ Account Maintenance Certificate (download from bank app)
- ✅ CNIC (you already have this)
- ✅ Links to your freelance profiles (Upwork, Fiverr, LinkedIn)
- ❌ PRCs — NOT needed for first-time registration

**How to register:**
1. Go to portal.techdestination.com
2. Click "Freelancer" → Create account
3. Fill in your details: skills, platforms, income info
4. Upload documents:
   - CNIC (both sides)
   - NTN certificate (download from FBR IRIS)
   - Account Maintenance Certificate (from bank app)
5. Submit → PSEB reviews (1-2 business days)
6. Once pre-approved, pay fee via 1Bill on your banking app
7. Certificate issued digitally (1-2 days)

**After you get the certificate — take it to your bank immediately:**
   - Email bank support + CC your branch manager
   - Attach your PSEB certificate
   - Ask them to update your profile for 0.25% withholding under code 9186
   - From this point on, your bank should issue correct PRCs automatically (or at least upon request with no argument)

**Annual renewal:**
   - PSEB registration is valid for 1 year
   - Renewal previously required PRCs with correct code 9186 (which you'll now have, since your bank was updated after initial registration)
   - As of late 2025, PSEB may have simplified renewal requirements — confirm on the portal
   - If you somehow can't get correct PRCs for first renewal, you can submit an affidavit (accepted only once)
   - Set a calendar reminder 30 days before expiry

**Time required:** 1-2 hours to apply, 2-5 days for certificate
**Cost:** ~PKR 1,000 (~$3.50) first time | ~PKR 5,000 (~$17.50) renewal

---

#### STEP 5: Set Up Proper Invoicing
**Unlocks:** Audit protection + easier PRC issuance
**Savings contribution:** Prevents future problems (insurance, not savings)

**Why this matters:**
If FBR audits you, they'll ask for invoices. If your bank asks why you're receiving $3,000 from abroad, they'll ask for invoices. If you want your PRC to have the correct code (9186), having proper invoices makes it much easier.

**What your invoices need:**

*For your international client:*
- Invoice number (sequential: INV-001, INV-002...)
- Your full name and address
- Client's name, company, country
- Description of services (be specific: "Web development services" not "work")
- Amount in USD
- Payment terms (Net 15, Net 30, etc.)
- Your payment details (Payoneer email, bank IBAN, or Cenoa details)

*For your Pakistani bank (helps get correct PRC):*
- Service type clearly indicating IT/ITeS work
- Mention that this is "export of IT services" somewhere on the invoice

*For FBR (required if audited):*
- PKR equivalent amount (at the exchange rate on invoice date)
- Your NTN number
- Date of service delivery

**Pro tip:** Create one invoice template with all three requirements built in. Use it for every payment. Keep copies organized by month.

*[Interactive: Simple invoice template they can fill and download as PDF]*

---

#### STEP 6: Track Your 80% Repatriation
**Unlocks:** Protection from 45% tax reclassification
**Savings contribution:** Prevents catastrophic tax reassessment

**The rule:** At least 80% of your foreign export earnings must enter Pakistan through official banking channels each fiscal year (July 1 - June 30).

**What counts as "repatriated":**
- ✅ Payoneer withdrawal to Pakistani bank
- ✅ Wire transfer to Pakistani bank
- ✅ Money in your ESFCA (foreign currency account in Pakistan)
- ❌ Money sitting in Payoneer balance
- ❌ Money spent directly from Payoneer card (outside Pakistan)
- ❌ Money kept in Wise balance
- ⚠️ Wise transfer to Pakistani bank (may not be classified correctly)

**What happens if you fail:**
Your entire income gets reclassified from "IT export" to "regular business income." Tax goes from 0.25% to progressive rates up to **45%**. The government can apply this retroactively.

**Interactive tracker:**

Fiscal year: July 1, [YEAR] — June 30, [YEAR+1]

| | Amount |
|---|---|
| Total earned from foreign clients this fiscal year: | $[____] |
| Total withdrawn/transferred to Pakistani bank: | $[____] |
| Currently in ESFCA: | $[____] |
| **Total repatriated:** | **$[calculated]** |
| **Repatriation percentage:** | **[X]%** |

[Visual gauge: Green (>85%) / Yellow (80-85%) / Red (<80%)]

If below 80%: "You need to repatriate $[X] more before June 30 to stay compliant."

*Data stored in localStorage — private, stays on your device.*

---

### SECTION 3: Footer / Summary

**After completing all 6 steps, show:**

```
🎉 Journey Complete!

You've gone from losing ~$1,500/year to saving ~$1,200/year.
You're now:
✅ A tax filer — lower costs on everything
✅ Properly banked — with USD retention
✅ Paying less in fees
✅ PSEB registered — 0.25% tax rate
✅ Invoicing properly — audit-ready
✅ Tracking your 80% — safe from reclassification

Want to save even more? [Learn how Cenoa works →]
```

---

## Technical Notes

- **Single HTML file** (like our other tools) — no backend needed
- **localStorage** for: step completion checkboxes, 80% tracker data, last calculator inputs
- **No login required** — privacy-first, everything runs locally
- **Responsive** — mobile-first (most Pakistani freelancers use mobile)
- **SEO target keywords:** "Pakistani freelancer tax guide," "PSEB registration," "freelancer savings Pakistan," "Payoneer fees Pakistan"
- **Styling:** Match existing Cenoa tools (dark header, lime/green accent, clean typography)
- **Language:** English (Pakistan's freelancing community communicates in English)

---

## Open Questions (for Yigit/Team)

1. ⚠️ **Section 65F tax credit** — has it been extended beyond June 2025? Using 0.25% as optimal rate for now. If the 100% tax credit is still active, effective rate could be 0%. **MUST VERIFY BEFORE LAUNCH.**
2. ✅ **Cenoa fee** — using 0.99% flat. Confirmed by Yigit.
3. ⚠️ **Does Cenoa send payments through international banking channels (SWIFT) or local partner?** — determines whether Cenoa solves the bank code (PRC) problem. Currently shown as "check with Cenoa support" in the tool. If confirmed: update Step 3 comparison table.
4. ✅ **CTA URL** — https://onelink.ext.cenoa.com/K9pU/cenoacomstart ("Open Free Account")
5. **Should we include Urdu option?** — might increase reach but adds complexity.
