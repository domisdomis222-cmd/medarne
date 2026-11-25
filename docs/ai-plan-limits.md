# AI plan limits (demonstration)

Free: 5 successful AI messages per day, resetting at midnight in Europe/Vilnius. Each question or follow-up counts as one message. Starting a new conversation does not reset usage. Failed requests refund their reservation.

Premium: no application daily message limit while the existing seven-day browser trial is active. There is no connected paid subscription system. Provider quotas, model context and payload limits still apply.

Implementation: index.html and medarne-homepage.html use localStorage keys scoped by Firebase UID and Lithuanian date. No message content is stored in this counter. Web Locks coordinate reservations across tabs where supported. An unavailable counter blocks Free sends instead of silently granting unlimited usage. Successful requests made under a Free reservation retain that reservation even if the chat was reset while awaiting the response.

Conversation context sends only the latest 20 question/answer pairs plus fresh saved journal and attachment context. This is a model context window, not a daily usage cap; the previous 20-question conversation stop has been removed.

## Production work still required

These client-side counters and the existing local trial flag are a prototype, not billing or abuse protection. Users can clear localStorage, switch browsers, or modify the frontend. Before selling Premium, enforce usage and paid entitlements in an authenticated server endpoint, update them atomically in server-owned storage, and prevent direct provider access that bypasses that endpoint. Do not trust client-written subscription or usage fields. This change deliberately does not modify the Firebase connection, Firestore rules, payment configuration or the provider setup being handled separately.

Validated with mocked provider/database: five-message cap, concurrent reservations, failure refunds, successful-send consumption, account isolation, Lithuania date rollover, persistence across chat reset, Premium bypass, context window, and storage-unavailable behavior. No real-provider quota or billing enforcement is claimed.

## Premium attachments

PDF, TXT and image attachment analysis is now a Premium trial feature. Both file pickers are disabled for Free, the change handler refuses attempts without Premium, and outgoing requests exclude attachments unless Premium is active. Trial expiry clears attached files, related conversation history and pending-response authority, so a subsequent Free request cannot reuse previously attached content. Upgrade links close the chat and navigate to plans. This remains a client-side demonstration entitlement, with the same production limitations stated above.
