# WebToolboxPublic

Public documentation for WebToolbox and its Chrome browser extension. This
repository contains product information and the extension's [privacy policy](PRIVACY.md).
It does not contain the private implementation, deployment configuration, or vault data.

## Chrome extension

The WebToolbox extension is a companion to a WebToolbox server you choose. Open
it on a website to find matching Login records, preview one record, and click
**Fill** to insert eligible username/password/current authenticator-code fields.
You can also explicitly copy a field for manual pasting.

Fill checks supported input metadata locally and never submits the form, fills
on page load, saves or modifies Login records, or monitors browsing in the
background. It does not include a hosted
vault service or create a WebToolbox account for you.

### Release status

The previously released Chrome Web Store version is **0.1.3**, with preview and
explicit copy/reveal. Phase 2 release candidate **0.2.4** adds click-to-fill and
uses the existing MFA-complete WebToolbox session. Its Store update is being
prepared; this text does not claim submission or approval of that update.
The existing **unlisted** item is
[WebToolbox in the Chrome Web Store](https://chromewebstore.google.com/detail/webtoolbox/fadbcipjnkmpjpakgfglcljakmmjohka).
Unlisted means anyone with that link can install; it does not mean private distribution.

### Features

- Matching considers all URL fields in an eligible Login record.
- Each URL field supports **Only match exact host**. Otherwise, matching can
  include the base domain and its subdomains; the URL scheme and effective port
  must still match. Local addresses and shared-hosting boundaries are treated
  conservatively.
- Selecting a match opens a preview. Empty fields are omitted. Passwords start
  visually masked and have an independent eye toggle.
- Clicking a field, or focusing it and pressing Enter/Space, explicitly copies
  its value. Copy and reveal recheck the server session and record eligibility.
- Configured one-time passwords are visible with a countdown and refresh while
  the selected preview is focused and visible. OTP setup secrets are not returned
  by the browser API.
- **Fill** sits beside the selected Login's name. A single click fills eligible
  username/password/current OTP fields. Username/password and OTP on separate
  pages are normal; each page needs its own Fill click. Ordinary successful
  fills, missing OTP fields/values and OTP timing skips finish silently. A detected
  OTP field that cannot accept the complete code shows a red error without truncation.
- Supported website exclusions, including `data-wtb-ignore`, are respected.
  Fill supports ordinary top-level inputs; split OTP boxes, iframe/shadow/custom
  controls and signup/password-change fields are outside this release's contract.
- Closing the popup discards its preview. The server controls focus-loss cleanup
  and an always-enabled inactivity timeout.

### Requirements

- Google Chrome and a compatible WebToolbox server. The server-controlled cleanup
  settings described here are available in WebToolbox 1.4.1.
- A trusted HTTPS connection to the server and access to it from your browser.
- Browser integration enabled by the server administrator.
- A normal WebToolbox sign-in with MFA completed in the same Chrome profile.
  Remember-me alone is not enough.
- A server implementing browser protocol v2 is required for Fill. With an older
  server, preview/copy remain available and Fill is disabled. Session management
  appears under **Profile → Sessions** on a supporting server.

HTTP server connections are restricted to loopback addresses for local
development. Do not bypass certificate warnings or use real credentials in
development testing.

### Setup and use

1. Install the extension from the unlisted Chrome Web Store link above. The
   available Store version may remain 0.1.3 until the Phase 2 update is approved.
   An unpacked release candidate must be supplied separately by the maintainer;
   this documentation repository is not an installable package.
2. Open the extension and choose **Settings**.
3. Enter only your server's origin, such as `https://vault.example.com`. Do not
   enter a page path, query string, or credentials.
4. Click **Save and allow access** and approve Chrome's access request for that
   server. If the permission prompt closes the popup, reopen it; the saved
   configuration should be recovered without entering the address again.
5. Sign in to WebToolbox in a normal tab and complete MFA in the same profile.
6. Visit the website associated with a Login record, open the extension, and
   select a match. With the Phase 2 update and supporting server, click **Fill**
   to insert the supported fields, then submit the website form yourself.
   Clicking a preview field still copies it for manual pasting.

The host displayed in the matching/preview screen is the current website, not
your configured WebToolbox server. View or change the server address in Settings.
After signing in or resolving an access problem, reopen the popup or click
**Retry** to recheck access.

### Preview cleanup

In WebToolbox, go to **Settings → Advanced → Browser extension preview**:

- **Clear preview on focus loss:** off by default. Turning it on discards the
  open preview when switching apps. With it off, an open popup can retain a
  completed view until its timeout; closing the popup always discards it.
- **Preview inactivity timeout (seconds):** 60 by default, configurable from
  1 to 3600 seconds. It is always enabled. Explicit successful data requests
  restart it; focus changes and automatic OTP refresh do not.

Changed server settings apply on the next successful extension response. Refresh
the preview to receive them. Older servers without this policy retain legacy
focus-loss clearing and a 60-second timeout.

### Privacy and clipboard

The extension sends only the current website's origin, not its path or page
content, to your chosen server. It stores non-secret server connection addresses
locally; selected Login data is held temporarily in the popup, not saved in
extension storage. There is no extension telemetry or advertising.

Copied values use the system clipboard. They may remain available to other
applications or clipboard-history tools after the popup closes. The extension
does not read or automatically clear the clipboard.

Read the [privacy policy](PRIVACY.md) for data handling, retention, permissions,
and the distinction between the extension and your server operator.

## Support

For non-sensitive questions or bug reports, use
[GitHub Issues](https://github.com/ssf65org/WebToolboxPublic/issues).
Account, vault-data, and server-log requests should go to the operator of your
WebToolbox server.

Do not post passwords, OTP setup secrets or codes, session cookies, API keys,
backup/recovery codes, private server addresses, unredacted logs, or vault exports
in public issues. Use synthetic examples and remove private information from
screenshots. Do not report a security issue with sensitive details in a public
issue; request a private contact route first without disclosing those details.
