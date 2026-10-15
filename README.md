# WebToolboxPublic

Public information and the [privacy policy](PRIVACY.md) for WebToolbox and its
Chrome extension. This repository does not contain private deployment settings
or vault data.

## Chrome extension

WebToolbox is a companion to your own WebToolbox server. Open it on a website to
find matching Login records, preview a record, click **Fill** for supported
username/password/current one-time-code fields, or explicitly copy a field.
After a supported login submission, Login capture can offer **New item** or
**Update existing**. Saving or updating always requires your confirmation.

### Release status

Version **0.2.4** is published and installed from the Chrome Web Store, as confirmed
by the operator on October 14, 2026. Candidate **0.3.0** adds template participation,
global Login capture, unchanged-credential suppression and confirmed Save/Update.
It is being prepared; this does not claim its submission, approval or publication.
The existing unlisted item is
[WebToolbox in the Chrome Web Store](https://chromewebstore.google.com/detail/webtoolbox/fadbcipjnkmpjpakgfglcljakmmjohka).
Anyone with an unlisted link can install it; unlisted is not private distribution.

### Features

- Match every URL field in eligible records. Each URL supports **Only match exact
  host**; otherwise base-domain/subdomain matching respects scheme, effective port
  and public-suffix boundaries.
- Enable any user-owned template for Login search on a supporting v3 server.
  Map required username/password fields and an optional TOTP field. Multiple
  enabled templates and the system Login share one matching list.
- Preview supported fields with masked passwords and an independent eye toggle.
  Clicking a field, or Enter/Space while focused, explicitly copies its value.
- Display generated one-time codes with a countdown and focused preview refresh.
  OTP setup secrets are not sent to the extension.
- Click **Fill** beside the record name. Fill never submits a website or runs
  automatically on page load. For a separate OTP page, open the extension there
  and click Fill again. Success and routine missing-value/timing outcomes are
  silent; unsupported detected field lengths and other actionable failures show
  red errors. Supported exclusions, including `data-wtb-ignore`, are respected.
- Enable global Login capture in Settings (on by default, with Chrome website
  access). Supported deliberate submissions are compared with your server before
  a badge appears. An exact saved username/password pair creates no offer.
  Automatic comparison sends a fresh nonce-bound password digest, not the raw
  password. A digest is still sensitive and requires HTTPS protection.
- Review the first captured item in the list. Its name comes from the source page
  title; its original URL excludes query/fragment tokens. Supported same-site
  redirects retain that source and the existing deadline.
- Choose **New item**, a template and an editable name, then **Save**. Or choose
  **Update existing**, explicitly select an item by name/username, then **Update**.
  Only this confirmation sends the captured raw credentials. Updates preserve
  unrelated fields, URLs, OTP settings and files. Capture never collects OTP codes.
- Use Back to return without saving, or X to discard. Interrupted write responses
  can be checked using **Check saved Login**, without another write.

Ordinary top-level inputs are supported. Split OTP boxes, iframe/shadow/custom
controls, signup/password-change/recovery flows and automatic website submission
are outside the current contract. Capture detects a submission, not proof that
sign-in succeeded. Review before saving.

### Requirements and setup

1. Install the available Store version using the link above. An unpacked candidate
   is supplied separately; this documentation repository is not an extension ZIP.
2. Open **Settings**, enter your server's origin, such as
   `https://vault.example.com`, and choose the global capture preference.
3. Click **Save and allow access** and approve Chrome's requested server/website
   access. Capture requests supported HTTPS websites and loopback development
   pages. Reopen the popup if the permission prompt closes it.
4. Sign in to your WebToolbox server and complete MFA in the same Chrome profile.
   Remember-me alone is insufficient. Browser integration must be enabled by the
   server administrator. No extension pairing or separate unlock is required.
5. Open the extension on a matching website for preview/copy/Fill. For capture,
   submit a supported form yourself, then open a captured offer if one appears.

A trusted HTTPS server is required; HTTP is restricted to explicit loopback
addresses for development. Use synthetic data on development fixtures. The
current website shown above matches is distinct from your configured server.
No hosted vault service or WebToolbox account is provided by the extension.

Protocol v3 is required for owned-template participation, capture and Save/Update.
An older server retains supported v2 Fill/v1 preview/copy for the built-in Login;
the extension never retries a write through an older protocol. Use the server
build implementing ADR-025-03; server and extension version numbers are separate.
Manage active server sessions through **Profile → Sessions**.

### Capture and cleanup settings

In extension Settings, uncheck **Enable Login capture** and save to stop observers
everywhere and discard pending captures. Existing Chrome website grants remain
approved; manage them separately in Chrome's extension site-access controls.
The configured server is excluded from observation. Incognito is not supported.

In WebToolbox **Config → Advanced → Browser extension**, supporting servers offer:

- **Clear preview on focus loss:** off by default. Closing the popup always clears
  its selected preview. Pending operations can still cancel when focus is lost.
- **Preview inactivity timeout:** 60 seconds by default, configurable from 1 to
  3600. Explicit successful data requests restart it; focus changes and automatic
  OTP refresh do not.
- **Captured Login lifetime:** 120 seconds by default, configurable from 1 to
  3600. It is an absolute deadline; review, reopening and redirects never extend
  it. Changes apply to new submissions, preserving existing deadlines.

### Privacy and support

Only non-secret settings are saved locally. Selected previews live in popup
memory. Pending captured usernames/passwords use trusted memory-only extension
session storage until expiry/discard/cleanup, never local or sync storage.
Confirmed credentials go only to your selected server. No advertising, telemetry,
remote executable code or browsing-history recording service is included.
Read the [privacy policy](PRIVACY.md) for capture, comparison, recovery, permissions
and retention details.

Copied values use the system clipboard and can remain available to other apps or
clipboard-history tools. The extension does not read or automatically clear it.
Uninstalling the client does not delete server records/history/backups.

Use [GitHub Issues](https://github.com/ssf65org/WebToolboxPublic/issues) for
non-sensitive support. Contact your server operator about accounts, vault data
and logs. Do not post credentials, OTP setup secrets/codes, cookies, recovery
codes, private addresses, exports or unredacted screenshots/logs. For sensitive
security reports, request a private contact route before sharing details.
