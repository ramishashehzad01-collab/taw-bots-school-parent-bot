# Setup Guide

## 1. Supabase

1. Open the Supabase project.
2. For a brand-new project, review and run `database/schema.sql` in the SQL Editor.
3. For the already-configured project, do not recreate tables blindly. Compare the reference schema with the existing `parents`, `students`, `parent_students`, `fees`, `timetables`, and `notices` tables.
4. Confirm the `notices` table contains `is_active` and `is_broadcasted` Boolean columns. `is_broadcasted` should default to `false`.
5. Create a server-side Supabase credential in n8n using the project URL and a key with the required permissions. Keep the service-role key private.

## 2. Import the n8n workflows

1. In n8n, open **Workflows**.
2. Choose **Import from File**.
3. Import `workflows/school-parent-bot.json`.
4. Repeat for `workflows/notice-broadcast.json`.
5. Both exports are intentionally inactive and have placeholder WhatsApp values.

## 3. Reconnect credentials

### Main chatbot
- Assign your Supabase credential to the nodes named `parents`, `parent-students`, `students`, `fees`, `notices`, and `timetables`.
- Assign your Groq credential to `Groq Chat Model`.
- Open `whatsapp reply` and `Unregistered Parent Reply`. Replace the placeholder phone-number ID in the URL and configure the secure Authorization header.

### Notice broadcast
- Assign the Supabase credential to `notices` and `parents`.
- Open `HTTP Request`. Replace the placeholder phone-number ID and configure the secure Authorization header.
- Do not execute the workflow with real parent records until all production prerequisites are satisfied.

## 4. Configure Meta webhook

1. Use a publicly reachable HTTPS URL for the n8n webhook.
2. In Meta's WhatsApp webhook configuration, enter the Production URL for the imported `Webhook` node.
3. Configure the same verify token in Meta and in the webhook verification flow.
4. Subscribe to the WhatsApp `messages` webhook field.
5. Send a test message from a registered test parent number and confirm the chatbot receives and replies to it.

The imported webhook path is `school-parent-bot-webhook`. n8n generates/assigns an instance-specific production URL after import. Use the actual URL shown by your n8n instance, not a URL copied from somebody else's environment.

## 5. Prepare the broadcast template

Create a WhatsApp message template in the correct WhatsApp Business Account:

- Name: `school_notice_broadcast`
- Language: `English (US)` / `en_US`
- Body parameters, in exact order:
  1. Notice title (`{{1}}`)
  2. Notice message/details (`{{2}}`)
  3. Publish date (`{{3}}`)

The template must be approved/enabled before business-initiated template sends. Ensure the school has the required permission/opt-in for every recipient.

## 6. Test safely

1. Keep the broadcast recipient set restricted to a test recipient while validating the request.
2. Confirm the customer's 24-hour service window is open if testing a free-form message; otherwise use an approved template.
3. Confirm one request succeeds and the message arrives.
4. Before a real broadcast, restore the all-parent code path, confirm the recipient list, confirm opt-in, and confirm template approval.
5. Run the broadcast once, review all per-recipient outcomes, then manually update `is_broadcasted` only after the broadcast is confirmed complete.

## 7. Credentials and `.env.example`

`.env.example` is a reference list of required values only. The workflow exports do not automatically load it. Configure the actual values in n8n's Credentials UI or protected deployment environment. Never commit a real `.env` file.
