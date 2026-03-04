# Pakistan Freelancer Guide — V2 Redesign Spec

## Overview
Rebuild the UI of `index.html` from scratch. Keep ALL content and calculation logic, but completely rethink the presentation.

## Files
- **Output:** `/Users/yigithanyigit/.openclaw/workspace/cenoa/tools/pk-freelancer-guide/index.html`
- **V1 backup (content reference):** `/Users/yigithanyigit/.openclaw/workspace/cenoa/tools/pk-freelancer-guide/index-v1.html`
- **Brand CSS:** `/Users/yigithanyigit/.openclaw/workspace/cenoa/tools/shared.css` (MUST be linked as `../shared.css`)
- **Style reference (working Cenoa tool):** `/Users/yigithanyigit/.openclaw/workspace/cenoa/tools/pk-freelancer-guide/style-reference.html`

## Tech Stack (keep same)
- Single HTML file, no backend
- Tailwind CDN (`<script src="https://cdn.tailwindcss.com"></script>`)
- shared.css linked as `../shared.css`
- localStorage for state persistence
- GTM tag: GTM-NDV9WQ7
- Cenoa brand colors: blue #0A3DBF, mint #88DCC4, dark #061E6E

## Brand & Color Rules
- MUST have high contrast — NO text-gray-400 or text-gray-500 on light backgrounds
- Body text minimum: text-gray-800, 16px (text-base)
- Secondary text minimum: text-gray-700
- Metadata/labels minimum: text-gray-600
- Dark backgrounds use full white text (no opacity tricks)

## Architecture: Two-Phase Experience

### PHASE 1: Calculator (The Hook)

**Goal:** Emotional impact. Show them their money loss FAST.

**Key UX Changes:**
1. **Real-time calculation** — NO "Calculate" button. Results update live as user adjusts inputs
2. **Progressive input** — Start with just 2 inputs (monthly income slider + payment method). Show initial results immediately. Then reveal "Refine your estimate" expandable with PSEB/filer questions
3. **Visual output** — Replace the comparison table with:
   - Two side-by-side cards: "Now" (red-tinted) vs "After" (green-tinted) with big numbers
   - One massive savings number in between/below with animated counter
   - Simple donut or horizontal bar chart showing fee breakdown
4. **No jargon** — Don't mention PSEB, PRC, Section 65F in the calculator section. Use plain language: "freelancer registration", "tax filing status"
5. **Mobile-first** — Cards stack vertically on mobile. Large touch targets on sliders.
6. **Trust signal** — Add "Based on SBP official rates & FBR guidelines · Updated Feb 2026 · Your data stays on your device"

**Calculation logic:** Keep EXACTLY the same as v1 (platform fees, tax rates, non-filer costs, 80% rule). Just wire it to real-time updates.

### PHASE 2: The Journey (6 Steps)

**Goal:** Educational, actionable, with gamification that motivates completion.

**Key UX Changes:**
1. **Vertical timeline layout** — NOT accordions. A visual path/timeline going down the page. Each step is a card connected by a line/path. Completed steps "light up" (green border, checkmark icon). Incomplete steps are slightly muted.

2. **Progressive disclosure per step** — Each step card shows:
   - Step number + title + one-line benefit + savings amount
   - "Learn more" expands the full content (or scrolls to reveal)
   - Inside each expanded step: use TABS (Why / How-to / Details) instead of dumping all text at once

3. **Soft sequential unlock** — Steps 2+ have a subtle visual hint that they follow from Step 1, but are NOT locked. Users can open any step. Just visually emphasize the "recommended order."

4. **Micro-celebrations** — When a checkbox is ticked:
   - The step card gets a green glow/border + checkmark
   - The savings counter in the sticky progress bar animates up
   - A brief subtle animation (pulse, confetti particles, whatever feels right — keep it tasteful)

5. **Sticky progress bar** (keep, but improve):
   - Shows: step count, savings unlocked (animated), % complete
   - More prominent — slightly taller, with the savings number being the hero element
   - Green fill animation

6. **Contextual CTAs:**
   - After Step 3 (Fix Payment Method): "Open Free Cenoa Account →"
   - After Step 6 / completion: "Start Saving with Cenoa →"
   - Don't overdo CTAs — max 2-3 on the entire page

7. **80% Repatriation Tracker** — Pull it OUT of Step 6 and make it a standalone mini-tool section below the journey. It's valuable enough to stand on its own.

8. **Completion celebration** — When all 6 steps checked: show a prominent celebration section with confetti animation, summary of total savings, achievement badges for each completed step.

### Content Updates to Apply (while rebuilding)

1. **Section 65F tax credit** — CONFIRMED extended to June 30, 2026 (Finance Act 2025). Update:
   - Calculator: after PSEB registration, effective tax rate can be 0% (not just 0.25%)
   - Step 4: mention "With Section 65F tax credit (valid through June 2026), your effective tax rate drops to 0%"
   - Add note: "The 0.25% withholding is collected but can be claimed back as a full tax credit"

2. **Cenoa is NOT SWIFT** — Update Step 3 comparison table:
   - Cenoa row: change "❓ Check with Cenoa support" to "⚠️ Same issue as Wise — arrives as local transfer, bank may assign wrong code"
   - Update the warning box to be honest: "Cenoa routes payments through local partners, not SWIFT. This means your bank may assign an incorrect purpose code. You'll need to follow the PRC correction process (see Step 4) to fix this."
   - Still position Cenoa as cheapest on fees (0.99% vs 3.5%)

3. **Purpose codes** — Add clarity wherever PRC is mentioned:
   - Correct code: **9186** (Freelance computer and information services)
   - Wrong codes banks assign: **9471** (Home remittance from Pakistanis abroad), **0401** (Personal home remittance)
   - Why it happens: the code comes from the SWIFT header; non-SWIFT transfers don't carry it

## All Step Content (preserve from v1)
Keep ALL the educational content from each step in index-v1.html. Every bullet point, every link, every how-to instruction. Just restructure the presentation (tabs, better typography, visual hierarchy).

### Step 1: Become a Tax Filer
### Step 2: Open a Freelancer Bank Account
### Step 3: Fix Your Payment Method
### Step 4: Register with PSEB (Tech Destination)
### Step 5: Set Up Proper Invoicing
### Step 6: Track Your 80% Repatriation

## Responsive Requirements
- Mobile-first (most Pakistani freelancers use mobile)
- Tables → stacked cards on mobile
- Minimum touch target: 44px
- Body text: 16px minimum
- No horizontal scroll

## State Persistence (localStorage)
Keep same localStorage approach:
- Calculator inputs
- Step completion checkboxes
- Repatriation tracker data
- Whether calculator has been run

## SEO
Keep all meta tags, canonical URL, OG tags from v1.
Target keywords: "Pakistani freelancer tax guide", "PSEB registration", "freelancer savings Pakistan", "Payoneer fees Pakistan"

## DO NOT
- Add any backend or API calls
- Add login/registration
- Remove any educational content
- Use low-contrast colors
- Make the page dependent on external JS libraries (except Tailwind CDN)
