# UniCheck test cases

Actual results are intentionally left for the group to fill from its own browser/database environment. Automated results are shown by running the listed commands; do not mark a manual case Pass without observing it.

| ID | Description | Preconditions | Steps | Expected result | Actual result | Status |
|---|---|---|---|---|---|---|
| API-01 | Health endpoint | Backend dependencies installed | Run npm start in backend; request GET / | HTTP 200 and UniCheck Backend is running! | Automated smoke test | Run with npm test |
| API-02 | Admin route without token | Backend app available | Request GET /api/admin/overview without Authorization | HTTP 401 and authentication-required message | Automated smoke test | Run with npm test |
| SIM-01 | Exact match | None | Compare identical non-empty answers | 100% similarity | Unit test | Run with npm test |
| SIM-02 | Clearly unrelated answers | None | Compare unrelated technical text | Score substantially below identical pair | Unit test | Run with npm test |
| SIM-03 | Score at threshold | Labeled evaluation sample | Evaluate score 70 at threshold 70 | Pair is predicted for lecturer review | Unit + Cucumber test | Run with npm test and npm run test:bdd |
| SIM-04 | Score below threshold | Labeled evaluation sample | Evaluate score 69 at threshold 70 | Pair is not predicted for review | Add to group execution sheet if needed | Not run manually |
| SIM-05 | Common short answer | Synthetic answer pair such as 4 | Compare through similarity service | Candidate remains review evidence with low confidence; no misconduct conclusion | Unit test | Run with npm test |
| SIM-06 | Invalid percentage | None | Evaluate score -1/101 or threshold NaN/101 | Clear TypeError; no misleading metric | Regression unit test | Run with npm test |
| PDF-01 | Valid text PDF | Lecturer account and active module | Upload a text PDF with labeled questions and a future deadline | Assignment enters question-review state; extracted blocks are visible | Manual/API | Not run; capture UI/API evidence |
| PDF-02 | Wrong file type | Lecturer account | Upload .txt or .png as question paper | HTTP 400; file is rejected | Manual/API | Not run |
| PDF-03 | PDF MIME with non-PDF signature | Lecturer account | Upload a file with PDF extension/MIME but non-%PDF- bytes | HTTP 400; fake PDF is rejected | Manual/API | Not run |
| PDF-04 | Oversized PDF | Lecturer account | Upload a PDF greater than 10 MiB | HTTP 400 from upload middleware, with 10 MB message | Manual/API | Not run |
| QST-01 | Publish before question review | Lecturer owns review-state assignment | Leave one non-empty question unverified; request publish | HTTP 409; assignment remains in review | Manual/API | Not run |
| QST-02 | Publish verified questions | Lecturer owns review-state assignment; all active non-empty questions verified | Publish assignment | Success; status becomes published | Manual/API | Not run |
| SUB-01 | Student submits before deadline | Student enrolled in module; published assignment; due date in future | Upload valid answer PDF with matching Q labels | HTTP 201; answers mapped and private PDF stored | Manual/API | Not run |
| SUB-02 | Student submits after deadline | Same setup; deadline elapsed | Upload valid answer PDF | HTTP 410; no new submission | Manual/API | Not run |
| SEC-01 | Student requests another student's submission | Two synthetic students and saved submissions | Authenticate as Student B; request Student A record ID | Access denied/not found; no private content disclosed | Manual/API | Not run |
| SEC-02 | Lecturer requests another lecturer's assignment | Two lecturers and assignments | Authenticate as Lecturer B; request Lecturer A report | Access denied/not found | Manual/API | Not run |
| UX-01 | Student upload feedback | Published assignment | Follow upload flow using valid then invalid document | Instructions and error states explain required PDF, deadline and answer labels | Usability checklist | Not run |
