# WebToolbox Chrome Extension Privacy Policy

Last updated: October 14, 2026.

This policy describes the WebToolbox Chrome extension, including the preview,
copy/reveal, click-to-fill, Login capture and confirmed Save/Update functionality
prepared for version 0.3.0. The extension connects to the WebToolbox server you
configure. It does not include a publisher-operated hosted vault service. This
policy does not replace the practices of your chosen server operator, Google
Chrome, the Chrome Web Store or GitHub.

## Purpose and information handled

Information is used to find, preview, copy and fill Login records, and to offer
confirmed creation or credential updates after a supported login submission.

- **Settings.** The configured server origin, global Login capture preference
  and temporary pending settings needed to recover an interrupted Chrome
  permission prompt are saved in local extension storage. Capture is enabled by
  default, subject to Chrome website access and a compatible signed-in server.
- **Website addresses and titles.** Opening the popup uses the current tab's
  origin (scheme, host and effective port) for server-side matching. Capture
  observers also use permitted top-level website origins to check server access.
  On supported submission, the source page title becomes the proposed record
  name, and its original URL (origin plus path, without query string or fragment)
  becomes the proposed website address. The title and URL remain local during
  automatic comparison; they are sent to your server only for a confirmed new
  record. A title or path can itself contain personal information. Review them
  before saving; the proposed name can be edited.
- **Website login fields.** Packaged scripts inspect supported ordinary
  top-level inputs: types, labels, autocomplete hints, visibility, form ownership,
  destinations and constraints. This metadata and page HTML stay local. When
  capture is enabled and Chrome allows the website, an observer reads supported
  username/password values after deliberate login submission. It does not read
  website one-time codes for capture. It respects supported exclusion markers,
  including `data-wtb-ignore`. Capture does not establish that sign-in succeeded.
- **Automatic credential comparison.** Before showing a badge or capture offer,
  the extension sends the original origin, exact username when available, a fresh
  random nonce and a nonce/session/origin-bound password digest to your chosen
  server. The raw submitted password is not sent for this automatic comparison.
  The server compares with eligible saved records; an unchanged username/password
  pair produces no offer. A digest is still sensitive: the public nonce does not
  prevent password guessing if the request is exposed. HTTPS remains essential.
  Comparison requests do not extend the server session or capture deadline.
- **Login summaries and selected previews.** The server returns matching record
  names, identifiers, template metadata and usernames where permitted. A selected
  preview can include usernames, passwords, URLs, email/phone/address/text/secret
  fields, notes, tags and location. Generated OTP codes include timing data;
  OTP setup secrets, attachment contents and version history are not returned.
  Masking is visual protection; masked values still reach the popup temporarily.
- **Browser-managed authentication.** Requests include WebToolbox session cookies
  managed by Chrome. A normal sign-in with MFA completed in the same Chrome
  profile is required. The extension does not request cookie-reading permission
  or read cookie values. Captured website credentials are not used to sign in
  to your WebToolbox server.
- **Explicit clipboard output.** A field's copy action rechecks access and writes
  that value to the system clipboard. The extension does not read the clipboard.
  Revealing a masked field is separate and does not copy it.
- **Explicit Fill output.** Clicking Fill requests fresh mapped username/password
  values and generated OTP codes for supported fields, bound to the selected
  record/template revision and origin. The server receives identifiers, revision,
  origin and role names, not DOM selectors or page contents. Values go temporarily
  to the isolated page script and into the inputs, with normal input/change
  events. Fill does not submit the form, run on page load, or continue filling
  another page automatically. Each later sign-in step needs another Fill click.
- **Confirmed Save/Update.** Opening captured review refreshes server comparison
  and template/target metadata. Only clicking Save or Update sends the raw
  captured username/password to your server for preparation and confirmation.
  New item uses your chosen eligible template, name and original URL. Update
  requires an explicitly selected record and changes only supplied mapped
  credentials, preserving unrelated values, OTP configuration, URLs and files.
  No OTP setup secret is captured or created. The website cannot approve a write.

## Destinations and use

API requests go only to the WebToolbox server you select and approve. They can
include website origins, usernames and comparison digests, selected record and
template identifiers/revisions, requested roles, pagination and confirmed write
information described above. Chrome and the server handle ordinary connection
metadata, including your client IP and session authentication.

There is no separate publisher analytics service, advertising, telemetry, data
sale, AI-training upload or remote executable code. Background scripts observe
supported login submissions on Chrome-permitted websites while capture is enabled;
this is not a browsing-history recording service. The extension has no history
permission and does not retain or upload a browsing history. Turning capture off
disables that observation. Preview OTP renewal never fills a website.

Your server retains its own encrypted vault, record history, sessions, logs and
backups. Browser API audit metadata includes event time, user identifier when
available, event/outcome, API route, response status, client IP and request ID.
Browser API audit records do not include request bodies, submitted credentials,
password digests/nonces, generated OTP values or full website URLs. Confirmed
write bodies are processed transiently; operation/outcome metadata supports
idempotency and recovery. Server/proxy administrators control separate logging,
backup and monitoring systems; contact your operator about those practices.

Data is used and transferred only for these features, consistent with the
[Chrome Web Store Limited Use requirements](https://developer.chrome.com/docs/webstore/program-policies/limited-use).
It is not used for advertising, unrelated profiling, resale or unrelated human
review.

## Storage and retention

Local extension storage contains non-secret connection settings and the global
capture preference, not vault credentials. Chrome sync storage is not used.
Chrome separately manages website permissions and session cookies.

Matching results and selected previews live in popup memory. Closing the popup
clears that view; errors, configuration changes and inactivity also clear affected
views. The supporting server's preview inactivity timeout defaults to 60 seconds,
configurable from 1 to 3600. Explicit successful data requests restart it; focus
changes and automatic OTP refresh do not. Clear preview on focus loss defaults
to off; enabling it clears the view when switching apps. An unfinished operation
can cancel on focus loss independently of completed-view retention.

Captured candidates, including their raw username/password, source title/URL and
bindings, use Chrome's memory-only `storage.session`, restricted to trusted
extension contexts. They can survive popup closure and service-worker suspension
so you can return to review; they are not written to local/sync storage. Capture
expires after an absolute server-configured lifetime: 120 seconds by default,
configurable from 1 to 3600. Alarms and access checks enforce expiry; reopening,
reviewing and redirecting never extend it. Discard, disabling capture, tab removal,
unsupported navigation, changed server/account/session or permission loss also
clear affected candidates when detected. There is no immediate sign-out push;
subsequent requests recheck access. Supported same-registered-domain redirects
can retain the original source URL/title and deadline in the same tab. Unknown
suffixes and local/IP addresses retain only exact-origin behavior.

Automatic comparison digests/nonces are transient and are not saved or logged.
Before confirmed write dispatch, captured credentials are discarded; an interrupted
response may leave a restricted memory-only receipt containing operation/binding
identifiers and a confirmation token, without username/password. Recovery rechecks
session, website and permission bindings and can only resolve the original
operation. It cannot silently create a second write. Receipt validity is limited
to seven days and rechecked on access/startup; closing the browser clears session
storage. This is distinct from the short credential-capture deadline.

Expired preview OTPs are cleared locally. New codes are requested only while the
selected popup preview is focused and visible, with fresh access checks. Fill
values are released after completion/cancellation/failure; JavaScript reference
cleanup is not a claim of guaranteed physical memory erasure. Values already
filled into a website belong to that page. Clearing the popup does not erase
those values or guarantee their recovery. OTP preview renewal does not change
a code already filled into the page.

Copied values can remain after the popup closes and may be accessible to other
applications or clipboard-history tools. The extension does not automatically
erase the clipboard/history. Removing this client does not delete your server's
records, sessions, history, logs or backups.

## Permissions and connection security

| Permission | Purpose |
| --- | --- |
| `activeTab` | Read the invoked website URL and temporarily access that page for explicit Fill. |
| `scripting` | Execute packaged isolated detection/Fill code and register packaged top-level capture observers on Chrome-permitted websites. |
| `storage` | Save non-secret settings locally and restrict temporary captures/recovery receipts to memory-only trusted session storage. |
| `clipboardWrite` | Copy only after an explicit field copy action. |
| `alarms` | Schedule cleanup of expiring captured credentials; no periodic browsing-history collection or background OTP polling. |
| Optional HTTPS/loopback host access | Connect to your selected server. When capture is enabled, request website access for supported HTTPS sites and loopback development pages. |

There are no mandatory all-site host grants, history, cookie-reading or
clipboard-reading permissions. Enabling capture and saving settings requests
broad optional website access; Chrome may show a read/change website-data prompt.
You control those grants in Chrome. Disabling capture stops observers and clears
captures, but does not automatically remove previously approved website grants.
The configured WebToolbox server is excluded from capture observation. Incognito
use is disabled. All scripts and public-suffix matching data ship in the package;
no remote executable code is downloaded.

Server connections require trusted HTTPS. HTTP is allowed only for explicit
loopback addresses (`localhost`, `127.0.0.1`, `[::1]`) for development. There is
no certificate bypass. Use synthetic data on development fixtures.

## Your choices and support

- Change your server or the global **Enable Login capture** checkbox in extension
  Settings. Save the change to apply it. Discard an individual capture with its X.
- Review the proposed name, website and credentials, then choose New item or an
  explicit Update existing target. Back retains the existing capture deadline;
  neither Back nor closing review saves a record.
- Manage site access, disable or uninstall the extension through Chrome.
- Sign out or use **Profile → Sessions** to end a server session. A completed open
  preview can remain until cleanup, but later data/Fill/capture/write requests
  must pass live session checks. Ask your server administrator about retention
  and preview/capture timeout settings.
- Manage server records, account data, history and backups through your server
  operator; uninstalling this extension does not delete them.

For non-sensitive questions use
[WebToolboxPublic Issues](https://github.com/ssf65org/WebToolboxPublic/issues).
Issues are public: do not include credentials, OTP secrets/codes, cookies, recovery
codes, private addresses, exports or unredacted logs/screenshots. GitHub and the
Chrome Web Store handle anything you submit to them under their own policies;
this is not an automatic extension upload.

This policy will be updated as data handling changes, with its date shown above.
