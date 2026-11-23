# Black-box test design

## Equivalence partitioning

### Input 1: answer-PDF file

| Partition | Representative input | Expected behavior |
|---|---|---|
| Valid | .pdf, MIME application/pdf, %PDF- signature, within 10 MiB | Continue extraction and submission processing |
| Wrong extension | .txt with PDF MIME | Reject in upload middleware |
| Wrong MIME | .pdf with non-PDF MIME | Reject in upload middleware |
| Spoofed PDF | Extension and MIME look valid, signature is not %PDF- | Reject in controller |
| Oversized | More than 10 MiB | Reject at Multer file-size limit |
| Empty/no file | No upload or zero-byte file | Reject as missing/invalid PDF |

### Input 2: similarity evaluation score and threshold

| Partition | Representative input | Expected behavior |
|---|---|---|
| Valid low score | 0–69, threshold 70 | Not predicted for review |
| Valid boundary score | 70, threshold 70 | Predicted for review (inclusive comparison) |
| Valid high score | 71–100, threshold 70 | Predicted for review |
| Invalid score | Below 0, above 100, non-number or non-finite | TypeError before metrics are returned |
| Invalid threshold | Below 0, above 100, non-number or non-finite | TypeError before threshold evaluation |

### Input 3: answer labels

| Partition | Representative input | Expected behavior |
|---|---|---|
| Valid main label | Q1, Q2) | Map to matching main question |
| Valid nested label | Q1(a)(i) or sequential a) then i) | Map to the matching verified leaf question |
| Unrecognized label | Appendix with no known question marker | Do not map as a question; report no matched answers if none map |
| Numeric answer point | 1., 2) inside an answer | Preserve as answer text; do not start a new main question |

## Boundary value analysis

### Upload size (10 MiB = 10,485,760 bytes)

| Case | Size | Expected result |
|---|---:|---|
| Below lower boundary | 0 bytes | Rejected: empty content is not a valid PDF |
| Small valid candidate | 1 byte | Rejected on PDF signature, not size |
| Just below maximum | 10,485,759 bytes | Pass size check; remaining PDF validation applies |
| At maximum | 10,485,760 bytes | Pass size check; remaining PDF validation applies |
| Just above maximum | 10,485,761 bytes | Rejected by Multer size limit |

The route uses Multer memory storage with a 10 MiB file-size limit. The manual over-limit check in the controller is secondary; an integration case should confirm the route-level middleware response.

### Assignment deadline

The submission controller treats dueAt <= Date.now() as closed. For a controlled test clock T, use due times T + 1 ms, T, and T - 1 ms.

| Case | Due time | Expected result |
|---|---|---|
| Just before close | T + 1 ms | Accept if other preconditions pass |
| At exact close | T | Reject with HTTP 410 |
| Just after close | T - 1 ms | Reject with HTTP 410 |

Capture the chosen test clock and timezone in the actual-result notes. Avoid relying on a human clicking at an exact wall-clock second.

## Decision table: publish a lecturer assignment

Y = yes; N = no; — = irrelevant after an earlier condition blocks publishing.

| Rule | Lecturer authenticated and ready | Assignment belongs to lecturer and is in review | At least one active non-empty question | All active non-empty questions verified | Result |
|---|---|---|---|---|---|
| P1 | N | — | — | — | Reject by auth/role/password middleware |
| P2 | Y | N | — | — | HTTP 404 reviewable assignment not found |
| P3 | Y | Y | N | — | HTTP 400 add at least one question |
| P4 | Y | Y | Y | N | HTTP 409; report count of questions remaining |
| P5 | Y | Y | Y | Y | Success; assignment becomes published and similarity status pending |

Each row is a candidate test. The table reflects the publishAssignment implementation and surrounding route middleware; the group should execute P1–P5 against a local test database before marking them Pass.
