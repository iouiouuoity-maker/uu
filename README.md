# ErrorMap Classroom — V6

**New server version:** separate student and teacher accounts, editable teacher questions, class assignments, shared SQLite results, and optional queued email reports. Read [START_HERE.md](START_HERE.md) for setup and deployment. Start with `python3 server/start.py`, then open `http://localhost:8000`. GitHub Pages serves only the static independent-practice version described below.

---

# ErrorMap — English learning workspace

An eight-page Grade 7 learning tool with English, Kazakh and Russian interfaces by Assel Amanzholova, School No. 22.

## Run locally

No build or package installation is needed:

```bash
python3 -m http.server 8000 --directory dist
```

Open `http://localhost:8000`. Use an HTTP server rather than opening files directly, so all pages share one storage origin.

## Publish with GitHub Pages

1. Upload the contents of this folder to a GitHub repository, including `dist`, `tests` and `.github`.
2. Under **Settings → Pages → Source**, select **GitHub Actions**.
3. Run **Publish ErrorMap** in **Actions**. The workflow tests the learning logic before deployment.

The project uses relative links and works under a repository subpath. No repository has been created or published automatically.

## The learning cycle

1. **Baseline:** 24 questions, one for each rule. Each skill area has four rules. First-cycle difficulty bands are balanced across the complete test. A missed answer flags its specific rule; it is only an initial signal.
2. **Lesson:** the learner reads the rule and completes four separate practice examples. Each first answer is recorded. A hint makes that item assisted. A correction never changes the recorded first answer. Wrong answers must be corrected before moving on.
3. **Reflection:** the learner writes a rule explanation and a new sentence. Only completeness is checked automatically; teachers review the actual language.
4. **Rule check:** a lesson with at least 3/4 independent first answers and a reflection unlocks two assessment questions. These are separate from practice and exclude the question reserved for the final recheck. A 2/2 result completes the rule check. A failed check returns the rule to review. Repeated check examples are labelled and recorded.
5. **Recheck:** opens after every assigned rule has a completed check. The learner receives one reserved B question for each baseline rule, matched to its A question’s difficulty band. Results are compared rule by rule.
6. **Next cycle:** preserves completed results and selects less-used baseline pairs. Previously seen assessment examples are disclosed. A finite bank cannot provide unlimited unseen items.

If the baseline flags no rules, the recheck is available without compulsory remediation. Optional practice and writing remain available. Teacher assignments can add or remove extra practice without changing test scores.

Thresholds are classroom workflow choices, not validated mastery cut-offs. The A/B pairs are aligned by topic and assigned difficulty, but have not been statistically equated. The site does not claim measured classroom effectiveness.

## Content model

- 6 skill areas, 24 specific rules.
- 240 questions in total.
- Each rule has 3 A/B assessment pairs (6 assessment items) and 4 dedicated practice items.
- Total: 144 assessment questions and 96 practice questions.
- Every item has a unique ID, rule ID, difficulty band, answer and English explanation.
- Options are shuffled for each attempt.

`dist/assets/question-bank.json` is the portable content database. `question-bank.js` provides the same object to the browser without a fetch dependency. Update both together. The tests verify that they match.

The Library defaults to practice. Assessment answer keys can be opened as a teacher resource; viewing them can affect later results. This is not a protected exam system.

## Pages

| Page | Purpose |
| --- | --- |
| `index.html` | Next action, baseline result and learning progress |
| `diagnostic.html` | Baseline, resumable assessment and gated recheck |
| `error-map.html` | Rule-level evidence and baseline/recheck comparison |
| `learning-path.html` | Assigned rules, lessons, corrections and rule checks |
| `writing.html` | Local draft, selected pattern checks and saved revisions |
| `library.html` | Searchable question bank with skill, level and type filters |
| `teacher.html` | Assignments, notes, reflections, cycle history and exports |
| `about.html` | Method, content and limitations |

All pages are in `dist/`.

## Saved work and profiles

The version 4 model uses browser `localStorage`. Each learner code has separate cycles, unfinished assessments, lessons, hints, first answers, corrections, reflections, writing and notes. A refresh resumes unfinished work. Completed work is not copied between profiles.

The teacher page can export answer-level CSV, download a full JSON backup, restore a validated version 4 backup and clear only the active learner. Import replaces the local learner collection after confirmation. Original earlier-version profiles are retained in a legacy archive when found; their different tests are not compared with the new results.

If storage fails, the page offers a recovery download and does not navigate away from an unfinished assessment. Multiple tabs receive local storage updates. Avoid editing the same learner simultaneously in multiple tabs.

There is no server database, account authentication, private teacher role or cross-device sync. Anyone using the same browser can open its local profiles. Use pseudonymous learner codes, not personal information. Browser clearing removes local records unless backed up.

## Writing checker

Ten pattern checks cover selected endings, auxiliaries, collocations, prepositions, spelling and capitals. Suggestions require human judgement and never change the diagnostic map. Editing a draft invalidates old suggestions. The checker does not automatically grade complete texts or claim that no findings means no errors.

## Verification

Run:

```bash
node tests/check.cjs
```

The checks cover bank integrity, all eight English pages, matching A/B pairs, balanced first-cycle difficulty, review before submission, unfinished work recovery, scoring, hint accounting, mandatory correction, targeted assignments, failed rule checks, disclosed repeated examples, reserved recheck items, cycle history, profile isolation, writing false-positive guards, escaping, CSV formula protection, backup validation, storage failure and local links.

A browser walkthrough also completed a 23/24 baseline, targeted lesson with 3/4 independent answers, a 2/2 rule check and a 24/24 recheck. These are QA inputs, not student research results; no QA learner data is included in the project.

## Dependencies

Static HTML, CSS and JavaScript. Google Fonts is optional; a system-font fallback is configured. No external JavaScript service, API key or language model is connected. The layout includes responsive breakpoints, keyboard focus styles, reduced-motion support and print styles.

## Interface languages

Use the language selector on any page: English / Қазақша / Русский. The choice persists on the device and does not change learner records, selected answers, drafts or scoring. Navigation, instructions, rule cards, explanations and messages are localised. English question sentences, answer options and learner-authored work remain in their original language. No translation API is used.

Locale text is maintained in `dist/assets/translations.json` and the matching dictionary in `i18n.js`. Question explanations in Kazakh and Russian show the expected English answer with the corresponding translated rule.
