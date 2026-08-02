---
name: Send and read a vendor security questionnaire
description: >-
  Send a security questionnaire to a monitored vendor, then retrieve its
  status, answers, comments, and attachments.
api: openapi/upguard-cyberrisk-openapi-original.json
operations: [getquestionnairetypes, sendQuestionnaire, questionnairesV2, questionnaireGet, questionnaireAnswers, questionnaireComments, questionnaireAttachmentsGet]
generated: '2026-07-21'
method: generated
---

# Send and read a vendor security questionnaire

Authenticate with the `Authorization` API-key header; base URL
`https://cyber-risk.upguard.com/api/public`. The vendor must already be
monitored (see the monitor-a-vendor skill).

1. **Pick a questionnaire type** — `getquestionnairetypes`
   (`GET /vendor/questionnaire_types`) lists the available types with ids.
2. **Send it** — `sendQuestionnaire` (`POST /vendor/questionnaire`) with the
   vendor's primary hostname, the questionnaire type, and recipient email(s).
3. **List questionnaires** — `questionnairesV2` (`GET
   /vendor/questionnaires/v2`) is the current listing endpoint; the older
   `questionnaires` operation is flagged deprecated in-spec — do not use it.
4. **Poll one questionnaire** — `questionnaireGet` (`GET /vendor/questionnaire`)
   by `questionnaire_id` for status/metadata.
5. **Read the results** — `questionnaireAnswers`
   (`GET /vendor/questionnaire/answers`) for questions and answers;
   `questionnaireComments` (`GET /vendor/questionnaire/comments`) for the
   comment thread; `questionnaireAttachmentsGet`
   (`GET /vendor/questionnaire/attachment/list`) then `attachment`
   (`GET /vendor/questionnaire/attachment`) for uploaded files.

List endpoints use `page_token`/`page_size` cursor pagination. `403` means
your API key lacks permission for the questionnaire feature; errors use the
`{ "error": "..." }` envelope (`errors/upguard-problem-types.yml`).
