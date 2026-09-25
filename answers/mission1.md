# Mission 1: Python habits that break JavaScript security

## Evidence

Output of `npm run test:m1`, pasted or as a screenshot in `img/`:

```
> cyse411-assignment1-secure-status-portal@1.0.0 test:m1
> node tests/mission1.test.js


normalizeService()
  PASS  valid entry is normalized and the name is trimmed
  PASS  returns a NEW object, not the same reference
  PASS  extra fields such as isAdmin are dropped
  PASS  null is rejected
  PASS  an array is rejected
  PASS  a string is rejected
  PASS  blank name is rejected
  PASS  name longer than 64 chars is rejected
  PASS  name that is not a string is rejected
  PASS  status 'UP' is rejected (case matters)
  PASS  unknown status is rejected
  PASS  online: "false" (string) is rejected
  PASS  online: 0 is rejected
  PASS  online: false (boolean) is accepted
  PASS  latencyMs: "120" (string) is rejected
  PASS  negative latency is rejected
  PASS  Infinity latency is rejected
  PASS  latencyMs: 0 is accepted (0 is falsy but valid!)
  PASS  missing latencyMs is rejected

parseStatusReport()
  PASS  invalid JSON fails safe
  PASS  missing services array fails safe
  PASS  services that is not an array fails safe
  PASS  JSON null fails safe
  PASS  mixed report keeps valid entries and counts rejected ones

24 passed, 0 failed

```

## Connections: Python to JavaScript

For each check you implemented, write how you would do it in Python and how you did it in JavaScript.

| Rule | Python | JavaScript, as in my code |
|---|---|---|
| raw is a dictionary or object, not a list | `isinstance(raw, dict)` | `raw === null || typeof raw !== "object" || Array.isArray(raw) ` |
| name is a non-empty string after trimming | `if(name) //using the boolean "false" value of a null string ` | `  const name = raw.name.trim(); if(name.length === 0 || name.length > MAX_NAME_LENGTH) { return null;  } ` |
| status is one of the allowed values | `if(raw.status in ALLOWED_STATUS)'| if(typeof raw.status !== "string" || !ALLOWED_STATUS.includes(raw.status)) ` |
| online is a real boolean | `if(isinstance(raw.online, bool)`|   `if(typeof raw.online !== "boolean")` |
| latencyMs is a finite number ≥ 0 | `math.isfinite(raw.latencyMs) and latencyMs > 0 ` | if(typeof raw.latencyMs !== "number" || !Number.isFinite(raw.latencyMs) || raw.latencyMs < 0)` |
| invalid JSON does not crash the program | try: temp = json.loads(parsed) print(temp) except json.JSONDecodeError as e: print(f"error {e}") |  try{ parsed = JSON.parse(jsonText); } catch(e){ return { services: [], rejected: 0, error: "invalid report" }; |

## Questions

1. Why is `latencyMs: 0` a trap for code such as `if (!raw.latencyMs) return null;`?

   > Since 0 is considered falsy, the code "if(!raw.latencyMs)" may read as "if(!false)" or "if(true)". This will enter the if command and return null.

2. Your function builds a **new** object and ignores fields like `isAdmin`. Describe in two or three sentences what could go wrong later in an application that copied **every** field it received.

   > A function that builds a new object while ignoring fields could result in overridden Identity and Access within applications. When not checking fields such as "isAdmin", the new object may open backdoors for people with lower privileges. Additionally, copying every field that is received could have avalanche consequences, such as interfering with other applications or cascading into data bloating / corruption.
