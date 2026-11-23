# QA metrics and evidence capture

## Commands available in this repository

Run backend commands from `backend`:

```powershell
npm ci
npm test
```

Run frontend checks from the repository root:

```powershell
npm ci
npm run lint
npm run build
```

Record command output, date, commit SHA, Node.js version, and any failures. Do not report a coverage percentage unless a coverage tool and report have actually been added and run. Do not report a hosted CI result until the workflow has run on GitHub.

## Metric definitions

| Metric | Evidence source | Reporting guidance |
|---|---|---|
| Test pass/fail | `npm test`, and any later API/BDD test commands | Give exact passed/failed counts and command. |
| Coverage | Coverage report from the configured tool | State the measured code scope and capture date; do not infer coverage from test count. |
| Defect density | Closed defects divided by a stated KLOC basis, if accepted by the lecturer | Give the formula and count; otherwise report findings by severity and state. |
| Code smells and technical debt | Actual SonarQube analysis | Include project key, analysis date, Quality Gate, and reported values. Do not estimate these values. |
| Security findings | Manual review or scan | List severity, evidence, and open/closed state. Separate source observations from verified exploits. |
| Performance | JMeter Summary Report | Report samples, error percentage, median and p90 latency, machine, JMeter version, and date. |
| Usability | Observed lecturer/student sessions | Give participant role, task, rating scale, observed issue, and improvement. Do not fabricate participant results. |

## Screenshot checklist

Take readable full-window screenshots after the relevant tool or workflow has actually run. Hide tokens, passwords, personal data, and real student work. Save originals in your submission evidence folder; do not add them to Git unless requested by the lecturer.

1. **GitHub Actions:** After the CI workflow is added and pushed, open the repository's **Actions** tab and capture the successful run on the final commit. Include job names and green status.
2. **Test output:** Capture the terminal showing `npm test` and the frontend lint/build commands. Include the command and commit SHA in the caption.
3. **Coverage:** After a coverage tool is configured and run, capture its terminal summary or HTML report with the tested scope visible.
4. **SonarQube:** After a real scan, capture the project overview and Quality Gate with Bugs, Vulnerabilities, Code Smells, Coverage, Duplications, and Technical Debt. Never show a token.
5. **JMeter:** Open `results/health-report/index.html`; capture its dashboard and Summary Report with sample count, errors, median, and p90 latency.
6. **Application workflows (recommended):** Using synthetic records, capture lecturer question review/publish and student submission/report screens. Include enough context to show the workflow result, but hide account identifiers and private content.
7. **Defect lifecycle:** Capture the issue/PR or code change linked to each tracked finding, with the verification result and final state visible.

Suggested filenames: `evidence/01-github-actions.png`, `02-test-output.png`, `03-coverage.png`, `04-sonarqube.png`, `05-jmeter-summary.png`, and optional `06-lecturer-workflow.png` / `07-student-workflow.png`. Add the repository commit SHA and capture date to each caption.
