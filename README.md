# RG GAMER BOY FF — Login website

## Deploy
1. Upload the contents of this folder to the root of the GitHub Pages repository for `the GitHub Pages repository where you host this login website`.
2. Firebase Console → Authentication → Sign-in method: enable Google and Anonymous.
3. Firebase Authentication → Settings → Authorized domains: confirm `rggamerboyff.github.io` is present.
4. Realtime Database → Rules: review and publish `database.rules.json`.
5. Test Google sign-in on the deployed HTTPS URL, guest sign-in, refresh persistence, and sign-out.

## Important limitations before production
- Google popup is used first; redirect fallback is used if the browser blocks popups. `getRedirectResult` is processed on page load to avoid getting stuck on the Google screen.
- Google and guest sessions use browser-local Firebase persistence.
- The guest 4-day renewal feature is intentionally not enabled in this starter. A quiz checked only in browser JavaScript is trivially bypassable. Implement renewal with a trusted backend (for example, a Firebase Cloud Function) before enforcing it.
- The Delete Account button appears only for Google-linked sign-ins and opens the supplied Google Form. The form submission is a request, not an automatic deletion.
- Add the real support email, confirm the Privacy Policy and Terms match actual practices, and set governing law before publishing.
- Username/password link is the separate site supplied by the owner and is not changed by this project.
- Do not put Firebase Admin SDK credentials in frontend files.
- The included database rules allow the exact profile fields written by `app.js`. If you change the data model, update and test rules together.
- Firebase Analytics is not initialized in this version; the measurement ID is retained in config only.

## Owner-supplied assets and destinations
- Logo image: `https://i.supaimg.com/cc740f2b-876c-4dd7-bbf2-16d636413b82/85320e1a-9a75-4178-90bb-a7f01267a661.png` (displayed upright with no CSS rotation).
- Account deletion request form: `https://docs.google.com/forms/d/e/1FAIpQLScPdr838eNtKeTrt40nIlzS9w-Sz7Hvsotf1-MZAlXI5JWXEw/viewform?usp=dialog`.
- Successful Google or Guest sign-in Continue destination: the bundled `./main.html` file.
- Username & Password destination: `https://rggamerboyff01.github.io/FREE-FIRE-PRIVATE-SERVER-2023/`.

## Post-login page and login destinations\n- `main.html` is bundled in the site root and opens after the signed-in user presses Continue following Google or Guest authentication.\n- Username & Password opens the separate owner-supplied website in a new tab. Its own username/password process is not handled by this Firebase login page.\n\n## Firebase project\n- This build uses the owner-supplied `rg-gamer-boy-ff-official` Firebase project.\n- Google and Anonymous sign-in must be enabled in that project's Firebase Authentication settings.\n- Publish and test `database.rules.json` in that project's Realtime Database.\n\n## Region and country selection (preserved)
- After the existing Privacy Policy / Terms consent gate, visitors must choose a region and a country before the sign-in options appear.
- The selected region/country is remembered in this browser using Local Storage. The existing policy acceptance key and behavior are unchanged.
- Region choices: Asia (India, Pakistan, Bangladesh, Indonesia, Singapore, Vietnam, Nepal), North America (United States, Canada, Mexico, Central America and Caribbean countries), and South America (Brazil only). Europe is not included.
- Google and Guest Firebase sign-ins save `region` and `country` inside `users/{uid}/profile`. Existing profile fields are read and preserved before updating, rather than replacing the profile with a smaller object.
- `database.rules.json` allows and validates the new `region` and `country` fields while retaining the existing owner-only profile access rules.
- Users can use “Change Region / Country” on the sign-in screen to update their choice. A signed-in Firebase user’s profile is updated when the new choice is saved.
- Username & Password still opens the existing separate website. This project does not change that separate site's authentication or Firebase profile code.
- The region selector uses the existing system theme and simple CSS states; it does not add animation libraries, background video, or continuous effects. The region/country Local Storage key was bumped to v2 and the previous v1 choice is cleared, so everyone must select a region and country again once. Existing policy acceptance and Firebase sign-in sessions are not cleared.


## Country ping indicator (added)
- The top-right `Secure sign-in` label is replaced by a live ping/status label after a country is selected.
- Asia countries measure round-trip response time to this project's Firebase Realtime Database endpoint (`asia-southeast1`, Singapore). The label includes the selected country's flag/name, but the measured target is the same Singapore-hosted database for every Asia country; it is not a separate server in each country.
- All selected countries show the measured round-trip time to the same configured Firebase Realtime Database endpoint in Singapore. This is a real endpoint measurement, not a country-local server measurement; no North America-hosted Firebase endpoint is configured in this project.
- The measurement sends three small unauthenticated REST requests to the database root. If database rules deny the root read (expected with private rules), the HTTP permission response is still timed; no profile data is read. If the endpoint cannot be reached, the label shows `Connection Failed`.
- The measurement runs when a country is selected, when the page loads with a saved selection, every 10 seconds while the page is visible, when the device comes back online, and when the tab/app becomes visible again. It prevents overlapping ping tests.
