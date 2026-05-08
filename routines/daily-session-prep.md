# Routine: Daily Session Prep

## Routine Name
NCP-AAI Daily Session Prep

## Schedule
Weekdays at 8:00 PM IST (14:30 UTC)

## Instructions (paste this as the Routine prompt)

```
You are preparing a study session for Tushar Sarang who is studying for the 
NVIDIA-Certified Professional: Agentic AI (NCP-AAI) exam on July 5, 2026.

Steps:
1. Get today's date in YYYY-MM-DD format
2. Read study-plan.md to find the topic scheduled for today based on current date and phase
3. Read progress.md to see what has already been covered — skip completed topics
4. Read wrong-answers.md to identify any recurring weak areas

5. Create the folder: sessions/[today]/

6. Write sessions/[today]/session-study.md with this exact structure:

---
# Study Session — [date]
## Topic: [topic name]
## Exam Section: [section name] ([weight]% of exam)
## Estimated Time: 1 hour

## Concept Brief
[300 word max explanation of the concept, written at practitioner level.
Reference Tushar's actual work where possible:
- PACE = 4-agent compliance loop (Planner→Author→Coder→Evaluator)
- CodeIndex = MCP tool layer, ast-grep + SQLite
- RAG Pipeline = patient document retrieval, vector search
- AWS Bedrock = Claude clinical assistant
- de-id.org = browser-native PHI de-identification]

## Key Points to Remember
[3-5 bullet points — the things most likely to appear on exam]

## Study Brain References
[List any study-brain/*.md files relevant to this topic for Claude to load]

## Exam Questions
Q1: [multiple choice question]
A) ...
B) ...
C) ...
D) ...

Q2: [multiple choice question]
A) ...
B) ...
C) ...
D) ...

Q3: [multiple choice question]
A) ...
B) ...
C) ...
D) ...

## Answers
Q1: [correct answer] — [one line explanation]
Q2: [correct answer] — [one line explanation]  
Q3: [correct answer] — [one line explanation]
---

7. Also create sessions/[today]/my-answers.md with just this content:
---
# My Answers — [date]
## Topic: [topic]

Q1: 
Q2: 
Q3: 

## Notes During Session:

---

8. Do not run the actual study session. 
   Just prepare the files so Tushar can run /start-study when he sits down.

9. Print a one-line summary: "Session ready: [topic] — sessions/[date]/"
```

## Setup Steps
1. Go to claude.ai/code/routines
2. Click New Routine
3. Name: "NCP-AAI Daily Session Prep"
4. Paste the instructions above into the prompt field
5. Select repository: your ncp-aai-prep GitHub repo
6. Trigger: Schedule → Weekdays → 8:00 PM your timezone
7. Click Create
