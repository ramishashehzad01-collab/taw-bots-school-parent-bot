# Security and Known Limitations

## Security requirements

- The repository is designed to contain workflow structure, setup instructions, and demo screenshots only (the demo video is shared via a link).
- Supabase service-role keys, Groq API keys, WhatsApp access tokens, webhook verify tokens, and any `.env` file must remain private.
- Configure credentials in n8n after import. The workflow exports deliberately use placeholder WhatsApp values and do not embed connected credential objects.
- Do not upload real school records or personal parent/student data to a public repository.
- Only disclose a student's information after verifying the sender's parent record and the parent-student link.
- Keep all production workflows inactive until credentials and webhook settings have been reviewed.

## Current limitations

1. `notice-broadcast.json` does not automatically set `notices.is_broadcasted = true` after all message requests succeed. Manual status update is documented in the main README.
2. The broadcast workflow sends to every parent row returned by Supabase that has a phone number. A production deployment should add an explicit consent/eligibility check, number normalization/validation, and safe handling for partial delivery failures.
3. The demo video shows a single test recipient. It does not prove that all production parent numbers are valid or opted in.
4. WhatsApp template approval is managed by Meta and belongs to the relevant WhatsApp Business Account; a workflow JSON export cannot carry approval with it.
5. `database/schema.sql` is a reference schema based on the project's known workflow fields. Compare it with the existing Supabase schema before running it on an existing project.
