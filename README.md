# Form IQ

Independent web design studio for Liam. Four color-coded packages with working concept previews.

- Bronze: $125 — business essentials, menus or services, opening hours, and contact links.
- Silver: $350 — up to 3 business pages, lead forms, and appointment requests.
- Gold: $750 — up to 5 pages, structured reservation requests, menus, and subtle motion.
- Diamond: $1,500 — up to 8 pages, guided booking experiences and integration setup.

Prices are USD starting build fees. Hosting, domain, paid services, and out-of-scope features are separate. All example brands are clearly labeled fictional concepts.

## Run

Requires Node.js 22 or later. No package installation is needed.

```sh
node scripts/serve.mjs
node scripts/build.mjs
node --test --test-isolation=none scripts/test.mjs
```

Preview: http://127.0.0.1:4317

## Edit

- `src/index.html`: main site and copy
- `src/style.css`: main visual design and responsive layouts
- `src/main.js`: package data, preview dialog, tier finder, and real inquiry form
- `src/preview.html`, `src/demo.js`, `src/demo.css`: all four interactive concepts
- `src/studio.html`, `src/studio.js`: private inquiry inbox
- `worker/handler.mjs`: server validation, R2 persistence, rate limiting, owner authorization
- `scripts/build.mjs`: builds an ESM Worker with embedded static assets into `dist/server/index.js`

The source is prepared for DJXCLAW/formiq. GitHub upload and public hosting have not completed: this session blocked GitHub writes and could not reach the deployment Git endpoint. The local preview runs independently of deployment.

## Hosting and inbox

The Sites manifest declares the `BUCKET` R2 binding. Real inquiries are stored server-side, never in the public GitHub repository or local browser storage. The local preview uses a separate file-backed inbox in ignored `.sites-runtime/preview-inbox`.

Set the server-side `FORMIQ_OWNER_ID` environment variable to the owner's Sites authenticated user ID. Obtain your own ID from `/api/session` while signed in to the deployed Site, using the native Sites environment-variable tool to configure it. Then visit `/studio` to review inquiries. The inbox fails closed until configured and always checks the trusted server identity. Hosting must protect the `oai-authenticated-user-id` header; do not expose the raw Worker behind a proxy that accepts spoofed auth headers.

Email notifications are not configured. Check the private studio inbox for incoming requests. Demo inquiry interactions never submit or store data.

The allocated Site is private and has not been published. Hosted owner access must be configured after successful publication. The local preview grants owner access only within its loopback-only server; this is not a public login system.

## Admin and quotes

Open http://127.0.0.1:4317/admin while the local server is running. You can review requests, search and filter them, save private notes, draft a quote, publish its price and scope, and change project status.

The customer form shows the selected package's starting price and saves a request with a private tracking link. Draft quotes stay private. Published quotes appear on the customer's tracking page and can be downloaded or printed. Customer email, original brief, and internal notes are never returned by the quote-viewing API. Email notifications and automatic email delivery are not configured; quote links are shared manually.

Local requests and quotes survive server restarts in ignored `.sites-runtime/preview-inbox`. Local quote links only work on this computer. The server must remain running; restart it with `node scripts/serve.mjs` if it stops or after reboot.

## Verification

Server tests cover persistence, idempotency, malformed data, same-origin enforcement, consent, limits, missing storage, unauthorized inbox access, quote drafting and publication, and customer-data privacy. The browser flow was verified from a new request through quote publication and customer viewing. Bronze and Silver have animations and transitions disabled. Gold and Diamond respect reduced-motion preferences and the main site's motion switch.


## Business workflow previews

Bronze demonstrates a cafe menu and business information. Silver adds a preferred-date consultation request. Gold includes a restaurant menu and table request with date, time, party size, name, and email. Diamond demonstrates service selection, illustrative time slots (including a closed Sunday), customer details, and a booking selection summary. All demonstrations are explicitly fictional and never submit, save, or confirm a real booking.

Confirmed reservations, live availability, external calendars, payment services, and customer accounts must be scoped and connected to the client business’s actual provider. Outside service fees are separate. The Form IQ quote intake collects business type and required workflows; validated choices are stored in the private admin request.
