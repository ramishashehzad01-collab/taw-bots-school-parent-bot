# Test Checklist

## Parent chatbot

- [ ] WhatsApp webhook verification succeeds.
- [ ] Incoming message event reaches the n8n workflow.
- [ ] A registered parent is found using the sender's phone number.
- [ ] An unregistered parent receives the registration guidance response.
- [ ] The bot returns only student records linked to the verified parent.
- [ ] Fee questions use Supabase fee records only.
- [ ] Timetable questions match the exact class, section, and weekday.
- [ ] Notice questions return active notices only.
- [ ] Out-of-scope questions receive a polite school-support-only response.
- [ ] No real tokens, student records, or private numbers appear in screenshots or repository files.

## Notice broadcast

- [ ] Manual Trigger starts the workflow.
- [ ] The notice query selects the newest active notice with `is_broadcasted = false`.
- [ ] The parents query returns the expected eligible recipients.
- [ ] The Code node creates one item per parent with a valid phone number.
- [ ] `school_notice_broadcast` template is approved and enabled in the correct WhatsApp Business Account.
- [ ] Recipient permission/opt-in is confirmed for every recipient.
- [ ] A single test recipient receives the correctly parameterized notice.
- [ ] All-parent broadcast is run only after explicit authorization and final recipient review.
- [ ] Delivery failures are reviewed before the notice is manually marked broadcasted.

## Release checks

- [ ] Both workflow exports are inactive and credential-free.
- [ ] `.env.example` contains placeholders only.
- [ ] `.gitignore` excludes `.env` and local secrets.
- [ ] Video and screenshots are checked for personal information.
- [ ] README describes known limitations accurately.
