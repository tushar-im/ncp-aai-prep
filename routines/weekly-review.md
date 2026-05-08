# Routine: Weekly Review

## Routine Name
NCP-AAI Weekly Review

## Schedule
Every Sunday at 9:00 AM IST (03:30 UTC)

## Instructions (paste this as the Routine prompt)

```
You are generating a weekly study review for Tushar Sarang who is studying
for the NVIDIA-Certified Professional: Agentic AI (NCP-AAI) exam on July 5, 2026.

- Prefer study-brain/ files for additional context if needed.
- Be honest and direct — do not soften feedback if Tushar is behind schedule.

Steps:
1. Get today's date in YYYY-MM-DD format. Calculate the date 7 days ago.
2. Read progress.md — find all sessions logged in the past 7 days
3. Read wrong-answers.md — identify wrong answers from the past 7 days
4. Read study-plan.md — check what was planned for this week vs what was actually covered
5. Check if sessions/weekly-review-[today].md already exists — if it does, skip and print "Weekly review already generated for today." else create the file.
6. Write sessions/weekly-review-[today].md with this exact structure:

<weekly-review-structure>
---
# Weekly Review — Week of [date]
## Days Until Exam: [calculate from July 5, 2026]

## Topics Covered This Week
[List each topic with date. Check sessions/[date]/answers.md for the score field. 
If score is "pending", note as incomplete. If no sessions logged, say so explicitly.]

## On Track?
[Compare study-plan.md planned topics vs actually covered.
Be direct — if behind, state how many topics are behind and what the impact is on the 60-day plan.]

## Recurring Weak Areas
[Analyze wrong-answers.md — flag any concept appearing more than once.
Suggest specific study-brain files to revisit for each weak area.]

## Wins This Week
[What landed well? Where did scores improve? If no data, skip this section.]

## Focus for Next Week
[Based on study-plan.md, list next week's scheduled topics in order.
Based on wrong-answers.md, flag which of those need extra time.]

## Adjusted Priority (if needed)
[If 2+ topics behind: suggest what to compress or cut to stay on track for July 5.
Be specific — name the topics to cut, not just "adjust your schedule".]
---
</weekly-review-structure>

7. Print one-line summary: "Weekly review written: sessions/weekly-review-[date].md"
```

## Setup Steps
1. Go to claude.ai/code/routines
2. Click New Routine
3. Name: "NCP-AAI Weekly Review"
4. Paste the instructions above into the prompt field
5. Select repository: your ncp-aai-prep GitHub repo
6. Trigger: Schedule → Weekly → Sunday → 5:00 AM your timezone
7. Click Create
