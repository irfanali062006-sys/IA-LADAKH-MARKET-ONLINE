IA LADAKH MARKET ONLINE — AI UPGRADE KIT
========================================

WHAT THIS ADDS
- Real AI backend endpoint (Gemini API) for customer/seller questions and general safe questions.
- Owner-only coding assistant mode. Owner pastes the relevant page source and describes the requested upgrade; AI returns suggested code. It does NOT edit or publish the live site automatically.
- Firebase ID token verification on every AI request.
- Owner mode checks the server-verified Firebase custom claim `owner: true` or `admin: true`.
- Gemini API key is stored as a Firebase Functions secret, never in index.html.

IMPORTANT
- This is not a trillion lines; it's a maintainable starting implementation.
- AI cannot guarantee every answer is correct. It must not invent live order/payment facts.
- Existing Firebase website and rules must be backed up before replacing/deploying.
- Deploying Firebase Cloud Functions may require the Firebase Blaze billing plan and a linked billing account, even if usage is within free quotas. Check current Firebase/Google AI billing before enabling.
- Gemini API access, quotas and model availability depend on your Google AI account/project.
- Never paste passwords, OTPs, API keys, payment secrets, or private customer records into the Owner Coding Studio.

FILES
- index.html: existing IA Ladakh Market Online page updated to call /api/ai.
- functions/index.js: secure AI endpoint.
- functions/package.json: backend dependencies.
- firebase.json: hosting rewrite /api/ai -> aiAssistant function in asia-south1.
- .firebaserc: project alias ia-ladakh-market-online.
- scripts/set-owner-claim.js: trusted admin script to grant owner claim to one Firebase UID.

SETUP (PC RECOMMENDED)
1. Back up your current website folder. Do not delete the original.
2. Copy the updated index.html into the website's hosting folder only after reviewing it.
3. Install Node.js and Firebase CLI, then open terminal in this project folder:
   npm install -g firebase-tools
   firebase login
   firebase use ia-ladakh-market-online
4. Store Gemini API key as a server secret (never put it in HTML):
   firebase functions:secrets:set GEMINI_API_KEY
   Paste the key into the secure terminal prompt when asked.
5. Deploy backend first:
   cd functions
   npm install
   cd ..
   firebase deploy --only functions:aiAssistant
6. Deploy hosting after checking that this folder's firebase.json public folder is correct for your actual site structure:
   firebase deploy --only hosting
   CAUTION: This firebase.json serves the current folder as public. If your real website has other files, merge the hosting rewrite into your existing firebase.json instead of overwriting your project config.
7. To make YOUR account the owner, find your Firebase Authentication UID. Create a service-account credential only in a trusted local environment from Firebase Console > Project settings > Service accounts. Keep the JSON outside the website folder and never upload it to GitHub/hosting.
   Set GOOGLE_APPLICATION_CREDENTIALS to that private JSON path, then run:
   node scripts/set-owner-claim.js YOUR_FIREBASE_AUTH_UID
   Sign out and sign in again to refresh the token. This script grants owner claim to the exact UID supplied; double-check UID before running.
8. Test customer mode with a normal account. Test Owner AI with the claimed owner account. Try a short question and then a small test HTML page. Review generated code before copying it into your site.

SECURITY AND LIMITS
- The endpoint requires Firebase Authentication. Owner mode requires an Admin SDK custom claim.
- This kit does not create a custom claim from the browser. It must be assigned from a trusted admin environment.
- This kit does not give the AI filesystem access, direct GitHub write access, or live deployment powers. Generated code is returned as text for review.
- The current endpoint includes public product names/prices/descriptions as context, not private orders or customer personal data.
- Apply/verify Firestore rules separately; this kit does not replace your existing rules.
- If AI returns an error, check Functions logs, secret configuration, Google AI API access/quota, authentication and hosting rewrite.
