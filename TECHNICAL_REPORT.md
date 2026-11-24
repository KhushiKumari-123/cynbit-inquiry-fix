# Technical Report — Cynbit Website Improvement

**Candidate task:** Full Stack Developer Intern — Improve Cynbit's Website

## 1. Problem Statement

While reviewing cynbit.in on mobile (the "AI Suite Inquiry" form under AI Automation), I found two issues that together hurt both usability and conversion:

1. **Sticky-header overlap.** The page header stays fixed (`position: sticky`) while the form scrolls. On smaller viewports, form labels and inputs — including the "Primary Automation Goal" dropdown — pass partially behind the header and are visually cut off, making the field hard to read or interact with confidently.
2. **No inline validation, single long form.** All fields (name, email, tech stack, goal, brief, consent) sit on one long page with no client-side validation. A user can submit an invalid email or an empty required field with no feedback until (presumably) a server-side rejection, which is a common source of form abandonment on lead-capture pages like this one.

Both issues sit directly on the page whose entire purpose is to capture a qualified lead, so fixing them has a direct line to the business goal of the page.

## 2. Technical Stack

- **HTML/CSS/JavaScript, no framework, no build step.**
- Rationale: the fix is a UI/interaction problem, not a data or architecture problem. A framework would add dependency weight and build tooling without changing the substance of the fix, and a plain static file is the fastest thing to deploy, review, and diff against the original markup.
- No backend: this is a prototype of the front-end behavior. In a real integration it would POST to Cynbit's existing form endpoint; the validation and layout logic are unchanged either way.

## 3. Implementation

**Layout fix:** the content wrapper sets `scroll-margin-top` equal to the header's height plus a small buffer, and the multi-step navigation explicitly scrolls to a "top" anchor below the header on every step change, instead of relying on the browser's default scroll-into-view behavior, which was what exposed the overlap.

**Validation:** each field has a named validator (regex for email, length checks for text fields, a presence check for the select). On "Continue," the current step's fields are validated; invalid fields get a visible outline, an inline error message, and the error count is announced via an `aria-live="polite"` region for screen reader users. Submission is blocked until the consent checkbox is checked, mirroring the original form's requirement.

**Flow restructuring:** the original single long form is split into three steps — contact info, project details, and a review-and-consent step — with a progress bar. Users see exactly what's left and can correct earlier answers from the review step before submitting, rather than scrolling back through a long page.

## 4. Results

- The header-overlap bug is fully resolved: no field is ever rendered partially behind the sticky header at any scroll position, on any of the three steps.
- Every required field now has enforced client-side validation with a specific, actionable error message, rather than silent or server-only failure.
- The review step gives users a final, legible summary of what they're submitting, which both reduces accidental bad submissions and builds trust before the consent checkbox.
- The prototype is a single static file with no dependencies, so it can be deployed to any static host in under a minute and reviewed by reading one file top to bottom.

## 5. Possible next steps (not implemented, out of scope for the prototype)

- Wire the form to Cynbit's actual submission endpoint and handle server-side error responses in the same inline-error UI.
- Persist partially filled steps (e.g. `sessionStorage`) so a refresh doesn't lose progress.
- A/B test the 3-step flow against the original single-page form to confirm the conversion hypothesis with real traffic.
