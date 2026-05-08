# Routine: Weekly Review

## Routine Name
NCP-AAI Weekly Review

## Schedule
Every Sunday at 9:00 AM IST (03:30 UTC)

## Instructions (paste this as the Routine prompt)

```
You are generating a weekly study review for Tushar Sarang who is studying 
for the NVIDIA-Certified Professional: Agentic AI (NCP-AAI) exam on July 5, 2026.

Steps:
1. Get today's date. Calculate the date 7 days ago.
2. Read progress.md — find all sessions from the past 7 days
3. Read wrong-answers.md — find all wrong answers from the past 7 days
4. Read study-plan.md — check if this week's topics were covered on schedule

5. Create: sessions/weekly-review-[date].md with this structure:

---
# Weekly Review — Week of [date]
## Days Until Exam: [calculate from July 5, 2026]

## Topics Covered This Week
[list each topic with date and score if available]

## On Track?
[Compare what was planned in study-plan.md vs what was actually covered.
Be honest — if behind, say so clearly.]

## Recurring Weak Areas
[Analyze wrong-answers.md — are any concepts appearing more than once?
List them and suggest focused review.]

## Wins This Week
[What landed well? Where did scores improve?]

## Focus for Next Week
[Based on study-plan.md, what are next week's topics?
Based on wrong-answers.md, what needs revisiting first?]

## Adjusted Priority (if needed)
[If significantly behind, suggest what to cut or compress to stay on track for July 5]
---

6. Print summary: "Weekly review written: sessions/weekly-review-[date].md"
```

## Setup Steps
1. Go to claude.ai/code/routines
2. Click New Routine
3. Name: "NCP-AAI Weekly Review"
4. Paste the instructions above into the prompt field
5. Select repository: your ncp-aai-prep GitHub repo
6. Trigger: Schedule → Weekly → Sunday → 9:00 AM your timezone
7. Click Create
