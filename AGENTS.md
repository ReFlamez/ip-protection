# AGENTS.md

## Code Review Rules

### 1. IP privacy and proxy separation
- Flag changes that allow a destination or second-hop proxy to learn the client's original IP address, or allow the first-hop proxy to learn the destination hostname, contrary to the intended two-hop privacy boundary.
- Flag changes that make it possible to correlate a user's original IP address, account identity, and destination traffic across the proxy system.
- Verify the actual data flow and trust boundaries before reporting a finding.

### 2. Tunnel security
- Flag changes that weaken the required encryption or bypass the intended CONNECT or CONNECT-UDP (MASQUE) proxy path for traffic that should be protected.
- Flag insecure fallback behavior that sends qualifying traffic directly without an explicit, documented reason.
- Distinguish intentional fail-open behavior from an accidental privacy bypass.

### 3. Domain-list and context enforcement
- Flag changes that cause Masked Domain List (MDL) domains to bypass protection in third-party contexts when IP Protection should apply.
- Verify first-party, third-party, subdomain, and Related Website Set handling before reporting incorrect classification.
- Do not assume that every request to an MDL domain must be proxied: first-party context and documented exceptions matter.

### 4. Authentication and token privacy
- Flag changes that expose credentials, unblinded authentication tokens, or sensitive user identifiers in logs, errors, telemetry, or network messages.
- Flag changes that bypass token quotas, expiration checks, rate limits, or other proxy-abuse protections without a documented security rationale.
- Verify the actual token lifecycle and authentication protocol before reporting a vulnerability.

### 5. Geolocation and reveal-token boundaries
- Flag changes that disclose more precise location information than intended or weaken the separation between a user's identity and proxy traffic.
- Verify that any Probabilistic Reveal Token (PRT) handling follows its documented eligibility, disclosure, and timing requirements.
- Do not treat an explicitly designed, controlled fraud-prevention mechanism as a vulnerability without evidence of a violated privacy or security boundary.

### 6. Tests and evidence
- For changes affecting privacy boundaries, proxy routing, domain classification, or token handling, check whether relevant tests cover both the unsafe case and a valid safe case.
- Report actionable findings with file paths, line numbers, impact, and evidence from the changed code.
- Distinguish verified defects from hypothetical risks. Do not report speculative issues as confirmed vulnerabilities.
- Keep formatting and other deterministic checks in automated tooling rather than raising them as code-review findings.
