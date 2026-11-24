# Medarnė: Firebase AI connection

The homepage initializes Firebase AI Logic with Gemini and App Check. Chat, attachment handling, and failure paths have automated tests with mocked services. A successful real Gemini response has not yet been verified.

## Configure the existing Firebase project

1. Open the Firebase console and select the existing project `medarne`.
2. Complete Firebase AI Logic setup for the Gemini Developer API. Ensure the selected model `gemini-4-hydrogen` is available to the project.
3. Under Security > App Check, register the existing web app with reCAPTCHA Enterprise. The public app ID is already present in firebaseConfig in both HTML files. Configure the actual website domain in the reCAPTCHA key's allowed domains.
4. Copy the public reCAPTCHA Enterprise site key used for that registration. In BOTH `index.html` and `medarne-homepage.html`, set:

```html
<meta name="firebase-app-check-site-key" content="YOUR_PUBLIC_RECAPTCHA_ENTERPRISE_SITE_KEY">
```

Use the public site key, never an API secret, service-account credential, or App Check debug token. Keep App Check enforcement enabled; the frontend supports limited-use tokens. The code initializes App Check before using Firebase services.

5. Serve the updated page on its configured domain. Sign in, open DI analizė, grant the displayed data-sharing consent and send a generic test question without personal records (uncheck the journal option). Confirm that a real model response arrives.
6. Add a small non-sensitive PDF or TXT and ask about its contents. Confirm both the attachment chip's Open link and a grounded answer. Then test journal inclusion with a non-sensitive test entry.

## Diagnose failures

The chat distinguishes journal loading, file reading, AI connection and model failures. Expand “Klaidos kodas pagalbai” for the stage and a sanitized error code/HTTP status; it excludes user content and raw backend error details.

- `ai/config-missing`: the public App Check site key has not been configured in the page.
- App Check or HTTP 403: check app registration, allowed website domain, AI Logic setup, and key restrictions in the console.
- Journal stage: check sign-in and Firestore access rules for the authenticated user's own health-records. For a files-only conversation, explicitly uncheck journal inclusion.
- HTTP 429: check the project's model quotas.
- HTTP 404: check availability of the configured model.

Attachments and conversation history live only in browser memory; they are removed on refresh/account change. Files can be PDF, JPG/JPEG, PNG, WEBP or TXT: maximum three files, 5 MiB each, 10 MiB combined. They are sent only when submitting a message with consent. Clearing consent invalidates pending responses and clears conversation history.

Official documentation:
- https://firebase.google.com/docs/ai-logic/app-check
- https://firebase.google.com/docs/app-check/web/recaptcha-enterprise-provider
