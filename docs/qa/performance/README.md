# JMeter health endpoint check

This plan makes 100 small `GET /` requests to `http://localhost:5000` with 10 virtual users, a 10-second ramp-up, and 10 requests per user. It checks only the backend health route. It does not measure PDF processing, MongoDB workloads, similarity evaluation, or production capacity.

## Prerequisites and run

1. Install Java and Apache JMeter.
2. Configure `backend/.env` with a valid `MONGO_URI` and required backend settings. Never commit this file.
3. From one terminal, start the backend with `npm start` in `backend`; confirm `http://localhost:5000/` responds with HTTP 200.
4. From the repository root, run:

   ```powershell
   jmeter -n -t docs/qa/performance/unicheck-health.jmx -l results/health.jtl -e -o results/health-report
   ```

5. Keep generated `results/` files local unless the lecturer specifically requests them. Capture the HTML dashboard and Summary Report.

Record JMeter version, date, machine, number of users, samples, error percentage, median latency, and 90th-percentile latency. If the backend or database is unavailable, mark the run **Not run** and report the blocker; do not make up measurements.

## Basic acceptance criteria

- 100 samples complete and 0 HTTP errors.
- The response assertion passes for HTTP 200.
- Record median and p90 response time as observations for this machine only; do not describe them as an SLA or production capacity result.
- Investigate any non-2xx response, connection failure, or increasing latency before reporting the run as passed.
