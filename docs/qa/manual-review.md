# Manual security and usability review

Use this checklist with synthetic accounts and synthetic PDFs only. Record the date, tester, commit SHA, browser, account role, steps, expected result, observed result, severity, and screenshot/evidence filename. Mark checks **Not run** until someone performs them; source inspection is not a substitute for a live test.

## Security review

| ID | Check | Procedure | Expected result | Current evidence / status |
|---|---|---|---|---|
| SEC-M1 | Authentication | Without a bearer token, request protected admin, assignment, submission, and module endpoints. | Requests are rejected; no user or academic data is returned. | Auth middleware is present in the route modules. The current automated tests are service-level tests; HTTP behavior still needs a manual/API check. |
| SEC-M2 | Role and record ownership | As Student A, request Student B's submission; as Lecturer B, request Lecturer A's assignment/report. | Denied or not found, with no PDF, answer text, or report data exposed. | Review the controller ownership filters; confirm with separate synthetic records and accounts. Not yet verified end to end. |
| SEC-M3 | PDF validation and size | Try a non-PDF, a `.pdf` with a non-PDF MIME type, a fake PDF signature, and files just under/over 10 MiB. | Invalid files and over-limit files are rejected with a clear error; no public file URL is exposed. | Middleware checks PDF MIME type, extension, one-file limit, and 10 MiB size; question upload also checks the `%PDF-` signature. Multipart behavior still needs manual confirmation. |
| SEC-M4 | CORS policy | Send requests with a trusted frontend Origin and an unrelated Origin; inspect `Access-Control-Allow-Origin`. | Only the configured frontend origin is permitted. | **Finding SEC-001:** `backend/server.js` currently calls `app.use(cors())` without an origin allowlist. Record the behavior and retest after Member 3's CORS change; do not claim it is fixed in this commit. |
| SEC-M5 | Secrets and private PDFs | Inspect tracked files and Git status for real `.env` files, credentials, test accounts, or uploaded PDFs. Check that runtime uploads stay under the private upload folder. | No secrets or real academic content are committed; PDFs are not publicly served. | `.gitignore` excludes `.env` files and `backend/private_uploads/`. Confirm the actual staged files before pushing. |

For every finding, record evidence and severity. Do not include credentials, real student information, or private PDFs in screenshots or issue text.

## Usability review

Ask one lecturer and one student to try the workflows below. Use a 1–5 rating (1 = difficult, 5 = clear) and write down one observed issue and one suggested improvement for each workflow.

| ID | User and task | Observe |
|---|---|---|
| UX-M1 | Lecturer uploads a question paper, reviews extracted question blocks, and tries to publish while one question is unverified. | Is the question needing review easy to identify? Is the reason publishing is blocked clear? |
| UX-M2 | Student opens a published assignment and uploads a valid answer PDF. | Are file rules, deadline, answer-label guidance, and progress clear? |
| UX-M3 | Student tries an invalid PDF and a late submission. | Are the errors understandable and actionable without exposing implementation details? |
| UX-M4 | Lecturer reviews a similarity report with a short/common answer and a high score. | Does the interface make clear that the score is evidence for human review, not a plagiarism verdict? |
| UX-M5 | Keyboard and narrow viewport review of login, upload, question review, and report screens. | Check focus order, labels, visible focus, contrast, overflow, and readable text. |

Do not present expected behavior or planned checks as measured usability results. Record actual participant feedback only with their permission and without personal data.
