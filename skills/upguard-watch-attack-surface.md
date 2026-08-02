---
name: Watch your own attack surface
description: >-
  Review your organisation's domains, IPs, active risks, risk changes, and
  potential vulnerabilities in UpGuard CyberRisk.
api: openapi/upguard-cyberrisk-openapi-original.json
operations: [getOrganisationV1, domains, ips, risks, org_risks_diff, org_vulnerabilities, domain_details]
generated: '2026-07-21'
method: generated
---

# Watch your own attack surface

Authenticate with the `Authorization` API-key header; base URL
`https://cyber-risk.upguard.com/api/public`.

1. **Confirm scope** — `getOrganisationV1` (`GET /organisation`) returns the
   organisation your API key is bound to.
2. **Inventory assets** — `domains` (`GET /domains`) and `ips` (`GET /ips`)
   list monitored assets; `add_custom_domains` / `add_custom_ips` (`PUT`)
   extend the inventory, and `ranges` (`GET /ranges`) shows IP ranges.
   Filter with `labels` and trim payloads with the `omit_*` parameters.
3. **Read active risks** — `risks` (`GET /risks`) for the account's current
   risk findings; `available_risks_v2` (`GET /available_risks/v2`) is the
   catalog of all risk types the platform can raise (the v1 `available_risks`
   operation is deprecated in-spec).
4. **Diff over time** — `org_risks_diff` (`GET /risks/diff`) reports risk
   changes between two dates — the right primitive for scheduled agents.
5. **Check vulnerabilities** — `org_vulnerabilities` (`GET /vulnerabilities`)
   lists potential CVE exposure across your domains and IPs.
6. **Drill in** — `domain_details` (`GET /domain`) / `ip_details`
   (`GET /ip`) for per-asset scan detail.

All list endpoints paginate with `page_token`/`page_size` and signal
throttling with `429` (`conventions/upguard-conventions.yml`). Wire ongoing
changes to a webhook instead of polling where possible
(`asyncapi/upguard-webhooks.yml`).
