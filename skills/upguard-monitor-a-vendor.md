---
name: Monitor a vendor and review its risks
description: >-
  Start monitoring a third-party vendor in UpGuard CyberRisk, tier it, and
  review its security rating, risks, and vulnerabilities.
api: openapi/upguard-cyberrisk-openapi-original.json
operations: [monitorvendor, vendor, vendor_update_tier, vendor_risks, vendor_vulnerabilities, unmonitorvendor]
generated: '2026-07-21'
method: generated
---

# Monitor a vendor and review its risks

Authenticate every request by setting the `Authorization` header to an API key
generated in CyberRisk Account Settings (see
`authentication/upguard-authentication.yml`). Base URL:
`https://cyber-risk.upguard.com/api/public`.

1. **Start monitoring** — `monitorvendor` (`POST /vendor/monitor`) with the
   vendor's primary hostname. Vendors are keyed by hostname, not opaque ids.
   A `402` means the action would exceed your contract's licensing limits.
2. **Fetch the vendor profile** — `vendor` (`GET /vendor`) with
   `hostname` to get the security rating and details. Use
   `generate_ad_hoc_report=true` only when you need an unmonitored vendor
   snapshot.
3. **Tier it** — `vendor_update_tier` (`PUT /vendor/tier`) to slot the vendor
   into your tiering scheme; labels can be assigned with
   `vendor_update_labels`.
4. **Review risks** — `vendor_risks` (`GET /risks/vendors`) for active risks;
   `vendor_vulnerabilities` (`GET /vulnerabilities/vendor`) for CVE-based
   vulnerabilities. Both paginate with `page_token`/`page_size`; follow
   `next_page_token` until it is absent (see
   `conventions/upguard-conventions.yml`).
5. **Track changes** — `vendor_risks_diff` (`GET /risks/vendors/diff`) reports
   risk changes over a date window.
6. **Stop monitoring** when offboarded — `unmonitorvendor`
   (`POST /vendor/unmonitor`).

Errors arrive as `{ "error": "..." }` (see
`errors/upguard-problem-types.yml`); every endpoint can return `429` — back
off and retry. There is no idempotency key: do not blindly retry POSTs
without checking state first.
