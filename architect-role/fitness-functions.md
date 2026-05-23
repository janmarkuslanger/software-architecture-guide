# Fitness Functions

## Overview
A fitness function is an automated check that verifies whether the system still meets a defined architectural property. The term comes from evolutionary computation, where a fitness function scores how close a candidate solution is to a goal.

In software architecture, fitness functions act as a continuous guard: they run in CI/CD pipelines and fail the build when an architectural rule is violated. Where ADRs *document* a decision, fitness functions *enforce* it.

## Characteristics

| Property | Description |
|---|---|
| Automated | Run without human intervention — typically in CI |
| Objective | Produce a clear pass/fail result |
| Targeted | Each function checks one specific architectural concern |
| Living | Evolve alongside the system as requirements change |

## When to use them
- A rule was violated once and the team wants to prevent recurrence.
- An architectural constraint must survive team turnover (new developers cannot accidentally break it).
- A quality attribute has a measurable threshold (response time, test coverage, coupling).
- An ADR contains a constraint that is easy to check programmatically.

## Types

| Type | What it checks | Example |
|---|---|---|
| Structural | Module boundaries, dependency direction, layering rules | No UI module may import from the DB layer |
| Performance | Response time, throughput, memory usage | P95 latency below 200 ms |
| Security | Dependency vulnerabilities, open ports, secret exposure | No known CVEs in production dependencies |
| Process | Test coverage, code review approval count | Coverage must not fall below 80 % |

---

## Example 1 — Module boundary enforcement (Node.js / Jest)

This fitness function prevents modules from crossing architectural boundaries. The rule: nothing inside `src/orders/` may import directly from `src/payments/`. The two domains must communicate only through a defined interface.

```js
// fitness/module-boundaries.test.js
import { execSync } from 'node:child_process';
import path from 'node:path';

test('orders module does not import directly from payments', () => {
  const srcDir = path.resolve('src/orders');

  // grep -r returns exit code 1 when nothing matches (desired outcome)
  let output = '';
  try {
    output = execSync(
      `grep -r "from '.*payments" ${srcDir} --include="*.ts"`,
      { encoding: 'utf8' }
    );
  } catch {
    // non-zero exit means no matches found — constraint satisfied
    return;
  }

  // if grep succeeded, forbidden imports were found
  throw new Error(
    `Forbidden cross-module imports detected:\n${output}`
  );
});
```

Run it in CI alongside unit tests:

```yaml
# .github/workflows/ci.yml
- name: Fitness functions
  run: npx jest fitness/
```

If a developer adds `import { calculateRefund } from '../../payments/refunds'` inside the orders module, this test fails immediately with a clear message listing the offending lines.

---

## Example 2 — Response-time threshold (Python / pytest)

This fitness function runs against a deployed staging environment and fails if the 95th-percentile latency of a critical endpoint exceeds 200 ms.

```python
# fitness/test_response_time.py
import statistics
import time
import httpx
import pytest

ENDPOINT = "https://staging.example.com/api/orders"
SAMPLE_SIZE = 20
P95_LIMIT_MS = 200


@pytest.fixture(scope="module")
def latencies():
    results = []
    with httpx.Client() as client:
        for _ in range(SAMPLE_SIZE):
            start = time.perf_counter()
            response = client.get(ENDPOINT)
            elapsed_ms = (time.perf_counter() - start) * 1000
            assert response.status_code == 200
            results.append(elapsed_ms)
    return sorted(results)


def test_p95_latency_below_threshold(latencies):
    index = int(len(latencies) * 0.95)
    p95 = latencies[index]
    assert p95 < P95_LIMIT_MS, (
        f"P95 latency {p95:.1f} ms exceeds threshold of {P95_LIMIT_MS} ms"
    )
```

Run it as a post-deployment step:

```yaml
# .github/workflows/deploy.yml
- name: Run performance fitness functions
  run: pytest fitness/test_response_time.py -v
```

This makes a regression visible the moment it is deployed to staging, long before it reaches production.

---

## Relation to other practices

- **ADRs**: Write an ADR to record *why* a constraint exists; write a fitness function to *enforce* it automatically.
- **Quality attributes**: Fitness functions are the operational expression of quality attribute requirements — they translate "the system must be fast" into a concrete, measurable check.
- **Evolutionary architecture**: Fitness functions are the core mechanism that allows architecture to evolve safely. Teams can refactor freely as long as all fitness functions stay green.

## Decision considerations / trade-offs
- Fitness functions add maintenance cost: they must be updated when thresholds or rules change intentionally.
- A failing fitness function should block the build, not just warn — a warning that no one acts on provides no value.
- Start with the most critical constraints (security, core module boundaries) and add more as violations occur.
