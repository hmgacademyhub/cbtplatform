# Prompt Template — HMG Academy CBT Pro Maintenance & Enhancement

Use this template when asking an AI assistant/developer to review or improve the CBT platform.

---

## Context

You are working on **HMG Academy CBT Pro**, a free browser-based CBT platform by **HMG Concepts / HMG Academy**, founded by **Adewale Samson Adeagbo**.

Brand details:

- Founder: Adewale Samson Adeagbo
- WhatsApp: +234 810 086 6322
- Phone: +234 907 790 7677
- Email: hismarvellousgrace@gmail.com
- Tech/Partnerships: buildingmyictcareer@gmail.com
- HMG Academy: https://hmgacademy.pages.dev/
- HMG Concepts: https://hmgconcepts.pages.dev/
- Motto: Learning Deliberately. Teaching Authentically.

The system uses:

- static HTML/CSS/JavaScript;
- Supabase free tier;
- no paid AI API;
- no build process;
- no exposed service-role key.

---

## Files to inspect

Inspect all project files, especially:

```text
index.html
teacher.html
student.html
admin.html
link_checker.html
deployment_validator.html
feature_guide.html
offline.html
sw.js
manifest.webmanifest
COMPLETE_SQL_SETUP.sql
README.md
DEPLOYMENT.md
FEATURES.md
SECURITY.md
CHANGELOG.md
further_maths_sample.csv
```

---

## Required behaviour

When improving the platform:

1. Preserve existing features.
2. Do not introduce paid APIs.
3. Do not add AI API calls.
4. Do not expose Supabase `service_role` key.
5. Keep the app deployable as static files.
6. Ensure Supabase SQL is idempotent and correctly ordered.
7. Ensure RLS policies do not expose other teachers’ data.
8. Use RPCs for anonymous student flows instead of broad table reads.
9. Keep HMG brand details embedded.
10. Update documentation after code changes.

---

## Security checklist for generated changes

Ask these questions before accepting a change:

- Does it require a secret in the frontend? If yes, reject or redesign.
- Does anonymous access read entire tables? If yes, reject or use RPC.
- Does an admin RPC verify admin status inside PostgreSQL? If no, fix.
- Does a policy reference a function before that function is created? If yes, reorder SQL.
- Does scoring use decimal values for partial credit? If no, fix.
- Does the feature work without paid AI API? If no, reject or provide free alternative.

---

## Example request

```text
Take the role of a senior CBT platform engineer and Supabase security auditor.
Review this HMG Academy CBT Pro repository.
Fix bugs, security issues, SQL ordering issues, documentation gaps, and deployment problems.
Preserve all existing features.
Use only free tools.
Do not add paid AI APIs.
Ensure teacher/student/admin workflows are correct.
Update README, DEPLOYMENT, FEATURES, SECURITY, CHANGELOG, DIAGNOSIS_REPORT, and EXPERT_ENHANCEMENT_REPORT.
Create a ready-to-upload folder and zip.
```

---

## Acceptance criteria

A completed enhancement should include:

- JavaScript syntax check passes.
- SQL setup is correctly ordered.
- RLS policies are secure.
- Teacher can create an exam.
- Student can load via code/link.
- Registered student can verify ID.
- Attempt limit works.
- Result saves.
- Teacher sees result.
- Admin sees platform data.
- Documentation is updated.
- Ready-to-upload folder and zip are produced.

## Current enterprise expectations added 2026-06-23

When maintaining the platform, confirm these features remain working:

- Per-exam granular anti-cheat controls are available in create/edit exam screens.
- Camera proctoring is optional and must not run when the teacher disables it.
- Math/Science keyboard inserts symbols into short, numeric, cloze, essay, and multi-part numeric answer fields.
- Student result submission tries `submit_student_result(jsonb)` first, then safe REST fallbacks, and tells the student to download backup only if all saves fail.
- `certificate.html` verifies certificates through `verify_certificate(text)`.
- SQL adds `anti_cheat_config`, certificate validity columns, `submit_student_result`, and `verify_certificate` idempotently.
- Documentation, deployment validator, feature guide, README, CHANGELOG, and expert report must be updated after any behaviour change.


---

## CBT v2 question-type requirements

The platform must preserve and support all existing question types plus the CBT v2 additions. Do not remove or downgrade any of them.

### Supported question types

```text
mcq              — Multiple Choice, one correct option
mrq              — Multiple Response, multiple correct options with partial/all-or-nothing scoring
tf               — True/False
short            — Short typed answer with alternate accepted answers
numeric          — Numeric answer with optional tolerance/unit
matching         — Left/right pair matching with optional distractors
ordering         — Sequence/reorder items
cloze            — Multi-blank/fill-in-the-gap
essay            — Keyword/minimum-word rule-based essay scoring, no AI API
categorization   — Classify items into categories
multi_numeric    — Multi-part numeric answer with partial credit
assertion_reason — Assertion–Reason logic question, usually A–E options
case_study       — Passage/scenario-based question
image_mcq        — Image/diagram-based MCQ
matrix           — Grid/matrix rows with shared choices, e.g. True/False or Yes/No
hot_text         — Student selects correct words/phrases from chunks
code             — Code, SQL, pseudocode, or algorithm response scored by keywords/tests and teacher review
```

### Required handling for CBT v2 question types

When editing or auditing the code, verify that:

1. `student.html` can render each type listed above.
2. `student.html` can score each type without AI APIs.
3. `student.html` stores the student answer in `answers_data` with `qtype`, `answer`, `time_sec`, and any needed schema/metadata.
4. `teacher.html` CSV and XLSX import can parse advanced types using the existing extended columns, especially `Type`, `Accept`, and `Items`.
5. Manual question entry supports advanced types through the advanced JSON/schema field.
6. Result review and teacher answer review do not crash on old or new question types.
7. The Math/Science keyboard remains visible during every active exam, including older exams whose database rows do not have `math_keyboard` set.
8. Advanced text/code/essay scoring remains transparent rule-based scoring and must not call paid AI APIs.

### Advanced question JSON examples

Use the `Items` column for structured data:

```json
{"passage":"A beaker of warm water was covered with a cold lid..."}
```

```json
[{"statement":"Water boils at 100°C at sea level.","answer":"True"},{"statement":"CO₂ is oxygen gas.","answer":"False"}]
```

```json
[{"text":"2","correct":true},{"text":"4","correct":false},{"text":"5","correct":true}]
```

```json
{"language":"JavaScript","keywords":["function","return","Math.max"]}
```

### Acceptance criteria for question-type work

A valid CBT v2 enhancement must pass these checks:

- Create/import one exam containing at least one `assertion_reason`, `case_study`, `image_mcq`, `matrix`, `hot_text`, and `code` question.
- Student can answer and submit without JavaScript errors.
- Teacher can see the submitted results.
- Result export still works.
- No paid AI/API calls are introduced.
- Legacy MCQ/short/numeric/matching/ordering/cloze/essay/categorization/multi_numeric questions still work.

## CBT v2 maintenance checks

- Confirm Math/Science keyboard is visible during all exams, including old exams without `math_keyboard` in their stored row.
- Confirm question types include assertion_reason, case_study, image_mcq, matrix, hot_text, and code.
- Confirm admin-supervised teacher control mode works and shows an ADMIN CONTROL MODE banner.
- Confirm admin can open/lock exams and clear exam results.
- Preserve no-AI/no-paid-API policy.

