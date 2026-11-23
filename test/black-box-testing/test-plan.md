# UniCheck software test plan

## Purpose and objectives

This plan evaluates the UniCheck assignment similarity detection system selected by the group as an existing system. The goal is to verify the highest-risk user flows, demonstrate black-box and white-box methods, and produce reproducible evidence for the EC4206 assignment. A similarity score is a signal for lecturer review and is not a finding of misconduct.

Objectives:

1. Verify valid and invalid input handling in PDF upload, answer mapping and similarity evaluation.
2. Verify the protected API rejects unauthenticated access and the app health endpoint responds.
3. Demonstrate equivalence partitioning, boundary value analysis, decision tables, complexity analysis, and branch-oriented tests.
4. Run automated checks on every push and pull request; collect coverage and static-analysis results without fabricating unavailable metrics.
5. Record security, usability and performance observations with enough context to reproduce them.

## Scope

**In scope:** backend health and access-control smoke checks; PDF upload validation; assignment/question publication rules; answer hierarchy extraction; similarity score evaluation and threshold handling; frontend lint/build; CI workflow; manual OWASP-oriented access-control and upload checks; basic local health-endpoint load check.

**Out of scope:** proving academic misconduct; legal/disciplinary decisions; production load capacity; penetration testing; exhaustive testing of every React page; live email delivery; production MongoDB data; model accuracy claims without a lecturer-labelled, held-out dataset.

## System under test

UniCheck consists of a React/Vite client and an Express/Node.js API with MongoDB persistence. Lecturers create modules and assignments, review extracted questions, publish assignments, and review similarity reports. Students enroll, upload answer PDFs before the deadline, and can view released feedback. The backend extracts text from PDFs, uses OCR when needed, maps answer labels to questions, and evaluates similarity.

## Quality model

The group targets ISO/IEC 25010 characteristics at a practical project scale:

| Characteristic | Target | Evidence |
|---|---|---|
| Functional suitability | Correct validation, ownership and workflow outcomes | Automated/API and documented manual cases |
| Reliability | Stable parsing and report metrics for edge inputs | Unit tests, boundary cases and failure handling |
| Security | Authentication, role/ownership checks, safe file handling and secret hygiene | Manual review plus unauthenticated API test |
| Usability | Clear upload, question-review, deadline and report-feedback states | Lecturer/student checklist and group observations |
| Performance efficiency | Basic response behavior under a small local request burst | JMeter Summary Report; not a production capacity claim |
| Maintainability | Automated checks, readable test mapping and SonarQube review | CI, coverage report and SonarQube scan |

## Verification and validation

Verification checks whether implementation behavior matches the stated rules: unit tests, boundary tests, API smoke checks, lint, build, and static analysis. Validation checks whether lecturer and student workflows are understandable and useful: run the manual usability checklist with representative group members and record observations. Similarity findings must remain subject to lecturer review.

## STLC and responsibilities

| Phase | Work | Owner |
|---|---|---|
| Requirement analysis | Translate the EC4206 task list and UniCheck workflows into testable requirements | All members |
| Test planning | Maintain this plan, risks, environment and evidence list | Member 1 |
| Test design | Maintain test cases, equivalence classes, boundaries and decision table | Member 1 |
| Implementation | Maintain testable API seam, unit/API tests, BDD and CI | Members 2 and 3 |
| Execution | Run scripts, manual checks, JMeter and SonarQube; record versions/results | All members |
| Closure | Resolve/triage defects, attach evidence, review limitations and reflection | Member 4, reviewed by all |

The member labels are placeholders. Replace them with actual names before submission.

## Risks and controls

| Risk | Control |
|---|---|
| Similarity flags common or short answers | Report false positives and confidence; keep lecturer review in control |
| OCR quality varies by scan | Include readable and OCR-damaged samples; mark OCR checks as environment-dependent |
| Database and credentials are unavailable in CI | Keep API smoke tests database-free; never commit secrets; run data-backed checks separately |
| External tools are not configured | Mark results pending and use the screenshot checklist; do not invent metrics |
| Academic records or student PDFs are sensitive | Use synthetic test accounts and documents; keep private uploads out of Git |

## Entry and exit criteria

**Entry:** repository dependencies install from lockfiles; test data is synthetic; local .env is configured only for any database-backed manual run; no real student submissions are used.

**Exit:** backend unit/API and Cucumber tests pass; frontend lint/build pass; the CI workflow is green on the submitted commit; manual/security/performance/SonarQube evidence is either attached or explicitly marked not run with a reason; every recorded defect has an owner, status and reproduction evidence.

## Test environment

| Item | Planned configuration |
|---|---|
| OS | Windows 11 for local workflow; GitHub Actions Ubuntu runner for CI |
| Runtime | Node.js 22 in CI; record node --version and npm --version locally |
| Browser | Current Chrome or Edge; record exact version when capturing UI evidence |
| API | Local backend at http://localhost:5000 |
| Database | Isolated local/test MongoDB only for data-backed workflows |
| PDF/OCR | Synthetic text PDF; scanned PDF only if Tesseract is available |
| Load tool | Apache JMeter 5.x; record exact version and test date |
| Static analysis | SonarQube server/scanner; record server, scanner and project versions |

## Test data

Create synthetic Admin, Lecturer, Student A and Student B accounts; a lecturer-owned module; a test assignment with Q1 and Q1(a); two answer PDFs with identical, paraphrased, common-short and unrelated responses; and a test PDF that is invalid or over the size boundary. Use no real names, grades, or submissions.
