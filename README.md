voXRas Trust Mark : Open Algorithm



Simple. Transparent. Honest.

voXRas collects anonymous feedback from verified people inside and outside an organization, turns it into a public Trust Mark badge, and gives the organization a private improvement report.

No hidden rules. No secret weights. The same math applies to every organization.

This document describes the open Trust Mark algorithm implemented in the voXRas platform. The application code in this repository is proprietary; only this algorithm specification is published under CC BY-NC 4.0.

Insights Hub (AI models, prompts, templates) is documented separately in docs/hub/. Production model IDs for each calendar quarter are in the quarterly model log.





How it works

flowchart LR
    A[Verified members share honest ratings] --> B[Scores are collected]
    B --> C[Simple average is calculated]
    C --> D[Trust Mark badge is shown]
    D --> E[Organization gets a report]
    E --> F[Organization improves]
    F --> A

The loop: Many honest voices → one clear public signal → helpful private feedback → improvement → repeat.





1. Collect feedback (Trust Mark)

flowchart TB
    subgraph WHO["Who gives feedback"]
        E[Onboarded team members<br/>Culture perspective]
        C[Onboarded clients<br/>Client perspective]
    end

    subgraph WHAT["What they answer"]
        Q1[Standard questions<br/>1 to 5 stars]
        Q2[Private written comments<br/>one per question]
    end

    E --> Q1
    C --> Q1
    E --> Q2
    C --> Q2

    Q1 --> STORE[Ratings are saved<br/>anonymously]
    Q2 --> INSIGHT[Comments shape private<br/>AI insights — not public]



Rules







Rule



Detail





Who can submit



Verified onboarded members only — employees (Culture) and clients (Client)





Rating scale



1 to 5 stars per question





Perspectives



Culture (team inside) and Client (people outside)





Questions



9 standard (3 Client + 3 Culture + 3 Common). On Onboard / Enterprise, up to 3 custom questions and rewording of one standard question per section (wording only — question IDs unchanged)





Per submission



Each person answers their perspective questions + common questions + any custom questions (Free: standard 9 only)





Cadence



One submission per person per feedback period — Free: every six months; Onboard / Enterprise: quarterly (weekly if platform admin enables it)





Ratings



Saved anonymously and used for the public badge





Comments



Required for every rated question — used only for private AI insights, never shown publicly

Related product channels that do not feed the Trust Mark badge: see §5 Related channels.





2. Badge algorithm (fully open)

This is the entire badge logic. Nothing is hidden.

flowchart TD
    START[All saved star ratings] --> ADD[Add them up]
    ADD --> COUNT[Count how many ratings]
    COUNT --> AVG["Overall score = total ÷ count"]

    AVG --> CHECK{What is the average?}

    CHECK -->|4.0 or higher| GREEN["🟢 Strong Trust Mark"]
    CHECK -->|2.5 to 3.9| BLUE["🔵 Neutral Trust Mark"]
    CHECK -->|Below 2.5| RED["🔴 Needs Attention"]
    CHECK -->|No ratings yet| BLUE2["🔵 Neutral (starting point)"]



Thresholds







Average score



Badge



Label





4.0 – 5.0



🟢 Green



Strong — consistently positive signal





2.5 – 3.9



🔵 Blue



Neutral — mixed or average; room to grow





Below 2.5



🔴 Red



Needs Attention — time to listen and act





No reviews



🔵 Blue



Neutral — default until enough signal exists



Pseudocode

overall = sum(all_ratings) / count(all_ratings)

if overall >= 4.0  →  STRONG          (green)
if overall >= 2.5  →  NEUTRAL         (blue)
if overall <  2.5  →  NEEDS ATTENTION (red)
if no ratings yet  →  NEUTRAL         (blue)

Every stored 1–5 star rating from verified Trust Mark submissions counts — across perspective, common, and custom questions that were actually rated.

Public QR visitor ratings and Google Maps reviews are never included in this average.

Reviews vs. ratings







Term



Meaning





Review



One verified submission by one person in one feedback period





Rating



One 1–5 star answer to one question (typically 6–9 ratings per submission)





Overall score



Average of all ratings — not an average of per-person submission scores





Review count (public)



Number of verified submissions shown on the entity page

The red badge label is Needs Attention in this specification. The product UI may display Not Strong for the same threshold — the math is identical.

Three ways the badge appears







View



What it measures



When it updates





Live Trust Mark



All-time average of every stored Trust Mark rating



After every new verified submission





Signal Story



Badge per feedback period (six-month, quarter, or week)



Shown as history over time





Blockchain proof (optional)



Calendar quarter snapshot (SHA-256)



Locked at quarter end (Mar, Jun, Sep, Dec)

The live public badge is recalculated after every new verified submission. Signal Story shows how the Trust Mark changed period by period. Blockchain verification is separate, optional, and does not change the badge math — it locks one cryptographic proof per organization per calendar quarter when enabled. The public verify API returns the stored proof record for auditing; it does not recompute the hash from live badge data. An optional Dogecoin memo anchor may also be written when live anchoring is enabled — not required for the proof to exist.

Badge visuals (not math): Free-plan organizations use a distinct Free Trust Mark asset (trust-free-green/blue/red). Onboard, Enterprise, and active Early Access use the standard Trust Mark asset (trust-green/blue/red). Thresholds and labels are the same — only the visual tier differs.





3. Public vs. private (Trust Mark)

flowchart LR
    subgraph PUBLIC["Public / open view"]
        P1[Trust Mark badge]
        P2[Overall score]
        P3[Number of reviews]
        P4[Per-question Trust Mark labels]
        P5[Signal Story — badge history]
    end

    subgraph PRIVATE["Organization only"]
        O1[AI insights from comments]
        O2[Full improvement report]
        O3[Export PDF / CSV]
        O4[Culture vs Client analytics]
    end

    RATINGS[(Verified ratings)] --> PUBLIC
    RATINGS --> PRIVATE
    COMMENTS[(Written comments)] --> PRIVATE







Audience



What they see





Everyone



Live badge, submission count (“reviews”), overall score (rating average), per-question Trust Mark labels, Signal Story (period history and streak)





Organization



AI insights, improvement reports, culture vs. client breakdown; PDF/CSV export on Enterprise (or platform-admin trial grant)

Trust principle: The badge is public and math-based. Personal comments stay private and only help the organization improve.





4. How Trust Mark reports are built

flowchart TB
    A[All verified ratings] --> C[Report]
    B[Collected insights from comments] --> C
    D[Badge + scores + culture vs client split] --> C

    C --> R1[Summary of strengths]
    C --> R2[Areas to improve]
    C --> R3[Action suggestions]

    R1 --> OUT[Organization reads report and acts]
    R2 --> OUT
    R3 --> OUT
    OUT --> NEW[Next feedback cycle]
    NEW --> A



Report ingredients





Average scores across all questions



Culture vs. Client breakdown



Patterns from past AI insights (derived from private comments)



Current badge status

Reports are generated on demand by the organization, are evidence-based, and use only what the data supports — no invented claims.





5. Related channels (not Trust Mark)

These product channels collect useful visitor or product signal. They do not change the public Trust Mark badge math above.

5a. Public QR (visitor feedback)

For shops, venues, and public-facing locations. Anyone who scans the organization QR can leave feedback without an account.







Rule



Detail





Who can submit



Anyone with the public QR / page link — no login





What they enter



Exactly one 1–5 star rating and one comment (both required)





Comment length



Min 10, max 500 characters





Plan



Onboard / Enterprise (org admin must enable Public QR)





Affects Trust Mark?



No





Visibility



Org admin sees submissions and averages; not part of the public badge algorithm



5b. Google Maps place (locked link)







Rule



Detail





Who links



Org admin sets a Google Maps / Business Profile URL once; changes only via platform admin





What is fetched



Place rating, rating count, and a small Places API review sample





Window for reports



Reviews with publishTime in the last 90 days (quarterly window)





API note



Places API returns a limited review sample (typically up to ~5), not full history





Affects Trust Mark?



No



5c. Quarterly public visitor report (AI)

Onboard+ org admins can generate one Public AI document from Public QR + Google only (never Trust Mark culture/client data).

flowchart LR
    Q[Public QR ratings + comments<br/>last 90 days] --> R[Public visitor report]
    G[Google place rating +<br/>in-window review sample] --> R
    R --> T1[Executive summary]
    R --> T2[Themes and findings]
    R --> T3[Suggested actions as prose only]







Rule



Detail





Inputs



Public QR entries and/or linked Google place (last 90 days)





Output



Single text: summary + themes + suggested-actions narrative





Action items



Text suggestions only — does not create structured Trust Mark action records





Quota



Counts against the plan’s monthly AI generation allowance





Affects Trust Mark?



No



5d. R&D feedback

Separate allowlisted product/service feedback (/{slug}/rnd). Transparent attributed comments for participants; does not drive the Trust Mark badge.

5e. Plans (feature gates)

Platform admin may grant complimentary trials (Early Access offer, feature overrides). Those affect feature access, not badge math.







Plan



Trust Mark



Custom questions



Public QR / Google AI



R&D



Export





Free



Yes (Free Trust Mark visual; up to 5 members; six-month rounds)



No



No



No



No





Onboard



Yes (standard visual)



Up to 3



Yes (org admin enables Public QR)



No



No





Enterprise



Yes (standard visual)



Up to 3



Yes (when enabled)



Yes



Yes



5f. Fair use (wait times)

voXRas limits how often the same network can repeat certain actions. This protects organizations from spam and keeps the platform fair for everyone.







If this happens…



What to do





You see “Too many requests” or a wait message



Pause and try again later (often after a few minutes, sometimes up to an hour)





Several people share the same office Wi‑Fi



Space out sign-in codes, feedback submits, and Public QR scans





A busy venue uses Public QR



Visitors can leave feedback freely in normal use; very rapid repeats from the same network may need a short wait

Normal use — one person signing in, one Trust Mark submission per round, or visitors scanning a QR at a shop — should not hit these limits. Limits are not part of Trust Mark badge math.





Open algorithm (one-page reference)

VOXRAS TRUST MARK — OPEN ALGORITHM
==================================

INPUT (Trust Mark only)
  • Verified onboarded members only (employees → Culture, clients → Client)
  • Ratings: 1 to 5, per question
  • 9 standard questions (3 Client + 3 Culture + 3 Common)
  • On Onboard / Enterprise: up to 3 custom + one reworded standard question per section
  • Private comment required for every rated question
  • One submission (review) per person per feedback period (six-month on Free; quarterly on paid; weekly if admin-enabled)

NOT INPUT TO THE BADGE
  • Public QR visitor ratings / comments
  • Google Maps ratings / reviews
  • R&D product feedback

STORAGE
  • Save every verified star rating (anonymous)
  • Trust Mark comments are not shown publicly — used only for private AI insights

BADGE — LIVE (recalculated after each new verified submission)
  overall = sum of all stored Trust Mark ratings ÷ count of those ratings
  (review count = submissions; overall score = average of individual star ratings)

  if overall >= 4.0  →  STRONG           (green)
  if overall >= 2.5  →  NEUTRAL          (blue)
  if overall <  2.5  →  NEEDS ATTENTION  (red)  [UI may show "Not Strong"]
  if no ratings yet  →  NEUTRAL          (blue)

VISUALS (same thresholds; plan-tier artwork only)
  • Free     → trust-free-green / blue / red
  • Paid     → trust-green / blue / red

HISTORY
  • Signal Story  → badge per feedback period (six-month, quarter, or week)
  • Blockchain    → optional SHA-256 lock per calendar quarter (if enabled)
                    verify API returns stored record; optional Dogecoin memo anchor

PUBLIC OUTPUT
  • Live badge, submission count, overall score, per-question labels, Signal Story

PRIVATE OUTPUT (organization)
  • Trust Mark AI insights + improvement report
  • PDF/CSV export on Enterprise (or admin trial grant)
  • Optional: Public QR feed + quarterly Public QR + Google AI report (separate)

PRINCIPLE
  • Simple math in public
  • Honest signal over hype
  • Feedback loop: collect → show → report → improve → collect again
  • Visitor / Google channels never rewrite the Trust Mark average
  • Fair-use wait times protect against spam — they do not change badge math





Why we publish this







Value



How voXRas delivers it





Transparent



Badge = one average with fixed, published thresholds





Fair



Same rules for every organization





Honest



A red badge is shown when earned — never hidden





Respectful



Ratings are public; personal Trust Mark comments are private





Verifiable



Anyone can read the algorithm and check the math





Separated channels



Visitor QR and Google reviews inform a separate report — they do not buy a greener badge





Questions this algorithm answers





Is this organization trusted by the people who work there and the people they serve?



Is the signal getting better or worse over time?



Where should leadership focus to improve?



Separately: what are visitors and Google reviewers saying this quarter? (Public QR + Google report — not the badge)





License

Copyright (c) 2026 voxon.org — voXRas is a product of voxon.

This Trust Mark algorithm documentation is licensed under the
Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0).

The Insights Hub documentation (AI models, prompts, and templates) uses the same license.

You are free to:





Share — copy and redistribute the material



Adapt — remix, transform, and build upon the material

Under the following terms:





Attribution — You must give appropriate credit to voXRas



NonCommercial — You may not use this material for commercial purposes

Full license: https://creativecommons.org/licenses/by-nc/4.0/legalcode

Note: This license applies to this algorithm documentation and the docs/hub/ hub documentation.
The voXRas platform, application code, and services are proprietary
and are not included in this license.



voXRas See what is real.
