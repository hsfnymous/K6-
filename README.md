# K6-
A Comprehensive Performance Testing Study Using k6: Tool Justification, Methodology, Data Analysis, and Recommendations

1. Introduction

Performance testing is a critical component of modern software quality engineering, ensuring that applications remain stable, efficient, and responsive under varying levels of load. This article presents a detailed analysis of performance testing conducted using the k6 tool, including Load Testing, Smoke Testing, and Spike Testing. The goal is to evaluate system performance, identify bottlenecks, and recommend improvements for future scalability and reliability. 

The study follows industry-standard methodologies, includes raw output from k6, and interprets the results using both numerical insights and performance engineering principles.

2. Tool Selection Justification
   
   2.1 Why K6?

K6 is an open-source, developer-centric load testing tool designed for modern microservices, REST APIs, and web applications. Several justifications support the choice of k6 for this study:

2.1.1 Scriptability and Developer-Friendly UX

K6 uses JavaScript as its scripting language, making test scenarios easy to write, maintain, and version-control. For teams already using JavaScript for backend or frontend development, k6 integrates seamlessly into existing workflows.

2.1.2 CLI-Based, Fast, and Resource Efficient

Unlike GUI-based load testing tools, k6 is lightweight and consumes minimal system resources. This makes it suitable for high-volume distributed testing and CI/CD integration.

2.1.3 Rich Metrics and Automatic Aggregation

k6 provides:

- Latency percentiles (p90, p95, p99)

- Request throughput (RPS)

- Error rates

- Data transfer metrics

- Execution statistics (iterations/s, VU lifecycle details)

These metrics allow for deeper performance analysis and bottleneck identification.

2.1.4 High Scalability with k6 Cloud & Extensions

The k6 core engine can scale to tens of thousands of virtual users locally or integrate with k6 Cloud for enterprise-grade distributed testing.

2.1.5 Modern Load Test Types Supported

k6 supports:

- Load tests

- Stress tests

- Spike tests

- Soak tests

- Smoke tests

- Breakpoint tests

This study uses three major types relevant for performance validation:

1. Load Test – Evaluates system behavior under expected user load
2. Smoke Test – Baseline test to confirm system stability
3. Spike Test – Tests system resilience to sudden traffic surges

Thus, k6 is an optimal tool for obtaining robust, repeatable, and developer-friendly performance insights.


3. Test Environment Setup and Methodology

3.1 Test Environment

| Component                  | Description                                       |
| -------------------------- | ------------------------------------------------- |
| **Load Testing Tool**      | k6 (CLI, JavaScript scripts)                      |
| **Target Environment**     | API/Web service endpoint under test               |
| **Hardware**               | Standard workstation/server running the k6 engine |
| **Network**                | Stable internet connection (low jitter)           |
| **Duration Configuration** | Varies per test type                              |
| **Virtual Users (VUs)**    | Scaled according to test methodology              |

3.2 General Test Methodology

Each test scenario follows these standard steps:

1. Script Development:
A JavaScript file was created defining the endpoint and logic of the test.

Parameter Definition:

VUs for simulating real traffic

Duration

Thresholds (optional)

Execution of k6 Script:
Commands such as k6 run script.js were executed.

Raw Output Collection:
Console logs were captured for analysis (shown in the screenshots provided).

Results Aggregation:
Metrics such as average latency, percentiles, throughput, data transfer, and execution rate were analyzed.

Bottleneck Detection:
Outliers, failed requests, and high percentiles were examined to determine system weaknesses.

4. Test Types and Raw Results

The following section documents the results from three executed test types:

Load Test

Smoke Test

Spike Test

Raw k6 outputs (from your screenshots) are summarized and translated into readable tables and explanations.

4.1 Load Test
4.1.1 Objective

To evaluate how the system behaves under a sustained, expected level of traffic.

4.1.2 Key Raw Metrics (from screenshot)

| Metric                       | Value           |
| ---------------------------- | --------------- |
| **http_req_duration (avg)**  | 62.86 ms        |
| **min**                      | 44.92 ms        |
| **med**                      | 58.82 ms        |
| **max**                      | 469.45 ms       |
| **p90**                      | 67.21 ms        |
| **p95**                      | 72.29 ms        |
| **Errors (http_req_failed)** | 0.02%           |
| **http_reqs**                | 26,070 requests |
| **Throughput**               | 66.63 req/s     |
| **Data received**            | 231 MB          |
| **Data sent**                | 35 MB           |

4.1.3 Observations

Average Latency:
62 ms is excellent for most API infrastructures.

Percentile Stability:
p90 and p95 values remain under 75 ms, indicating consistent performance for the majority of users.

Low Error Rate (0.02%):
Indicates high reliability during sustained load.

High Max Latency (469 ms):
Longer tail latency suggests occasional slow responses—likely a backend processing delay or network jitter.

Throughput:
~67 requests/sec sustained with stable response times.

4.2 Smoke Test
4.2.1 Objective

To validate whether the system is functionally operational under minimal load before running heavier tests.

4.2.2 Raw Metrics Summary

| Metric                       | Value     |
| ---------------------------- | --------- |
| **http_req_duration (avg)**  | 67.41 ms  |
| **min**                      | 54.98 ms  |
| **med**                      | 64 ms     |
| **max**                      | 197.68 ms |
| **p90**                      | 72.53 ms  |
| **p95**                      | 75.17 ms  |
| **Errors (http_req_failed)** | 0%        |
| **Checks Passed**            | 100%      |
| **http_reqs**                | 170       |
| **Iterations**               | 170       |

4.2.3 Observations

Zero Failed Checks:
The endpoint is reachable, stable, and functioning correctly.

Low Request Count:
Expected for a smoke test—goal is not load simulation but functionality validation.

Latency Range:

Average and percentiles similar to Load Test

Max latency remains below 200 ms

This validates that the environment is ready for heavier testing.

4.3 Spike Test
4.3.1 Objective

To measure system resilience during sudden, extreme traffic spikes, simulating real-world unpredictable load surges.

4.3.2 Raw Metrics Summary

| Metric                       | Value                  |
| ---------------------------- | ---------------------- |
| **http_req_duration (avg)**  | 253.33 ms              |
| **min**                      | 0 ms (likely rounding) |
| **med**                      | 64.66 ms               |
| **max**                      | 12.53 s                |
| **p90**                      | 574 ms                 |
| **p95**                      | 1.56 s                 |
| **Errors (http_req_failed)** | 94.28%                 |
| **http_reqs**                | 21,013                 |
| **vus_max**                  | 2000 VUs               |

4.3.3 Observations

Extremely High Error Rate (94.28%):
The system is unable to handle sudden high traffic spikes.
Likely causes:

Server resource exhaustion

Connection pooling issues

Rate limiting

Backend timeout

Drastic Latency Degradation:

Maximum latency: 12.53 seconds

p95 at 1.56 seconds shows severe slowdown under pressure

Throughput Drop:
Unlike the Load Test, throughput does not scale with increased VUs due to request failures.

Potential Overload or Throttling:
The system may be protected by:

API gateway rate limit

Load balancer timeout

Backend thread pool saturation

Overall, the system does not gracefully handle sudden traffic bursts.

5. Data Visualization (Graphs/Charts)
<img width="2000" height="1200" alt="avg_latency" src="https://github.com/user-attachments/assets/9b93a98a-0947-46f6-baf5-4e67f8693611" />



<img width="2000" height="1200" alt="error_rate" src="https://github.com/user-attachments/assets/e182fd8e-4d7a-486d-a0ce-8f3c02df9037" />
<img width="2000" height="1200" alt="throughput" src="https://github.com/user-attachments/assets/be7dc89c-bed0-404a-8c03-4e3d6bc45d27" />

6. Interpretation of Results and Identified Bottlenecks

Based on the collected data, several key performance insights emerge:

6.1 System Strengths
1. Consistent Performance Under Expected Load

Load Test median latency: 58 ms

p95 under 73 ms

Error rate almost zero

This shows the system performs well under its normal operating range.

2. Clean Baseline Functionality

Smoke Test results confirm:

No request failures

Stable performance

Predictable response times

This validates that the test environment and configurations are sound.

6.2 System Weaknesses / Bottlenecks
1. Severe Failure Under Spike Traffic

The failure rate of 94% and latency spikes beyond 10 seconds indicate:

Backend is not horizontally scalable

Insufficient concurrency handling

Bottlenecks in database or API orchestration

Possible thread pool exhaustion

2. High Tail Latency (Load Test & Spike Test)

Even under normal load:

Max latency occasionally reached 469 ms

Under spikes:

Max latency exceeded 12 s

Tail latency issues often indicate:

Long-running DB queries

Slow cache warm-ups

Non-optimized code paths

Resource locking or contention

3. Throughput Limitation

Load Test throughput plateaued at ~66 req/s.
Increasing VUs did not improve throughput — an indication of a server-side bottleneck, likely CPU, connection pool, or backend queue.

6.3 Possible Root Causes

Based on the metrics, probable bottleneck origins include:

Insufficient backend worker threads

Database performance constraints

Slow external API dependencies

Synchronous blocking operations

Aggressive rate-limiting

CPU or memory saturation on the host

More granular root-cause analysis would require:

System resource monitoring logs

APM traces (New Relic, Datadog, OpenTelemetry)

Server logs during test execution

7. Recommendations and Final Conclusions

Based on the analysis, here are actionable recommendations.

7.1 Scaling and Architecture Improvements
✔ Implement Auto-Scaling

Introduce horizontal auto-scaling policies triggered by:

CPU usage > 70%

Response duration > threshold

Queue size > limit

✔ Introduce Caching Layers

Reduce load on backend services by using:

Redis / Memcached caching

HTTP response caching

Database query caching

✔ Optimize Database Calls

Look into:

Long-running queries

Missing indexes

Slow joins

Inefficient SELECT patterns

✔ Increase Connection Pool Sizes

If the system is throttling due to limited DB or HTTP connection pools, increase the pool limits.

7.2 Code-Level Optimization

Replace blocking calls with async operations

Refactor heavy computation paths

Optimize API response serialization

Reduce payload sizes

7.3 Improve Spike Handling / Rate Limiting
✔ Implement Graceful Degradation

Instead of failing 94% of requests:

Queue overflow fallback

Circuit breakers

Cached fallback responses

✔ Pre-warm Resources

Before spike windows, ensure:

Cache warming

Thread pool ramp-up

DB connection provisioning

7.4 Observability Enhancements

Integrate:

Distributed tracing

Detailed error logging

Per-endpoint performance dashboards

APM tools will help pinpoint internal bottlenecks.

8. Final Conclusion

This performance testing study using k6 demonstrates that the system is stable under normal operating conditions but struggles significantly when exposed to sudden, extreme traffic surges.

| Test Type      | Stability       | Latency   | Errors | Throughput        |
| -------------- | --------------- | --------- | ------ | ----------------- |
| **Smoke Test** | Excellent       | Low       | 0%     | Low (expected)    |
| **Load Test**  | Very Good       | Low       | 0.02%  | Moderate          |
| **Spike Test** | Critical Issues | Very High | 94%    | Severely Degraded |

1. Introduction

Performance testing is a critical component of modern software quality engineering, ensuring that applications remain stable, efficient, and responsive under varying levels of load. This article presents a detailed analysis of performance testing conducted using the k6 tool, including Load Testing, Smoke Testing, and Spike Testing. The goal is to evaluate system performance, identify bottlenecks, and recommend improvements for future scalability and reliability.


