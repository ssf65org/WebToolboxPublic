# WebToolbox Chrome Extension Privacy Policy

Last updated: October 7, 2026.

This policy describes the WebToolbox Chrome extension, including its 0.1.2
read-only Login preview/copy implementation. The extension is a client for the
WebToolbox server you configure. It does not include a publisher-operated hosted
vault service. This policy does not replace the policies or practices of your
chosen server operator, Google Chrome, the Chrome Web Store, or GitHub.

## Purpose and information handled

The extension uses information only to find matching Login records, show a
selected preview, and let you explicitly copy or reveal a value.

It handles the following information:

- **Server connection addresses.** Your selected WebToolbox server origin is
  saved in local extension storage. A temporary pending server origin may also
  be saved while Chrome asks for permission, so an interrupted first save can
  finish when you reopen the popup. Replacing, cancelling, or recovering that
  configuration removes the pending marker as appropriate.
- **The current website origin.** After you invoke the extension, Chrome
  provides the current tab URL. The extension extracts its scheme, hostname,
  and effective port for matching. It sends that origin to your configured
  server; the page path, query string, and fragment are not sent. It does not
  inspect page content, forms, other tabs, or browsing history.
- **Matching Login summaries.** The server returns matching record identifiers,
  record names, usernames, and the matched website origin. The extension uses
  them to display a list and request a selected record.
- **A selected Login preview.** On selection, the server returns supported
  fields, which can include usernames, passwords, URLs, email addresses, phone
  numbers, postal addresses, other text/secret fields, notes, tags, and location
  information. Configured OTP fields return generated codes and timing
  information, not OTP setup secrets. Attached file contents and version history
  are not returned by the browser API. Empty fields are omitted from the view.
- **Browser-managed authentication.** Requests to your chosen server can include
  the WebToolbox session cookies managed by Chrome. The extension relies on a
  normal server sign-in with MFA completed. It does not request cookie-reading
  permission, read cookie values, or save cookies, passwords, or MFA codes as
  authentication credentials in extension storage.
- **Explicit clipboard output.** When you activate a field's copy action, the
  extension rechecks the selected preview and writes that field value to the
  system clipboard. It does not read clipboard contents. Revealing a masked
  field is a separate action and does not copy it.

Password masking is visual protection, not a claim that a selected password
never reaches the browser. Selected field values, including masked values, are
received and held temporarily in the popup so the preview/copy features can work.

## Where information goes and how it is used

Extension API requests go only to the WebToolbox server you select and approve.
They send the current website origin and, when needed, the selected record
identifier or pagination parameters. Chrome and the server also handle normal
connection metadata, such as your client IP address and session authentication.

The extension does not send vault contents or browsing information to a separate
publisher analytics service, advertising network, data broker, or AI-training
service. It contains no telemetry, advertising, background browsing monitor,
page-injected script, or remote executable code. It does not automatically fill
or submit forms, copy values, or create/update Login records.

Your chosen server processes these requests and retains its own vault,
authentication, and operational data. WebToolbox's browser API records security
audit metadata such as event time, user identifier when available, event/outcome,
API route, response status, client IP address, and request identifier. Its browser
API audit entries do not include credential values, generated OTP codes, request
bodies, or full page URLs. Server administrators may configure separate server,
proxy, backup, or monitoring systems; their handling and retention are outside
the extension's control. Contact your server operator about those practices.

Data handled by the extension is used and transferred only for the functionality
described in this policy, consistent with the
[Chrome Web Store Limited Use requirements](https://developer.chrome.com/docs/webstore/program-policies/limited-use).
It is not used for advertising,
unrelated profiling, resale, or unrelated human review.

## Storage and retention

Only non-secret server connection addresses are persisted in local extension
storage. The extension does not use Chrome sync or session storage for vault
data or configuration. Chrome separately manages extension permissions and
WebToolbox session cookies.

Matching results and selected previews are held in the popup's memory. Closing
the popup discards them. Errors, refreshes, configuration changes, and the
server-controlled inactivity timeout also discard the affected view.

The default inactivity timeout on a supporting server is **60 seconds**, and the
server administrator can configure a positive value from **1 to 3600 seconds**.
Successful explicit data requests restart this timer; focus changes and automatic
OTP refresh do not. **Clear preview on focus loss** is off by default on a
supporting server. With it off, switching apps can retain a completed view in an
open popup until its timeout. Enabling it clears the view on focus loss. Older
servers without the policy use focus-loss clearing and a 60-second timeout.

Expired OTP codes are cleared locally. New codes are requested only while the
selected popup preview is focused and visible, subject to fresh permission,
website, record, and session checks. There is no background OTP polling.

Clipboard retention is separate. A copied value can remain after the popup is
closed or cleared, and other applications or clipboard-history tools may access
it. The extension does not automatically erase the clipboard or clipboard history.
Use your operating system's controls if you need to replace copied values or
manage clipboard history.

Clearing or removing the extension does not delete records, sessions, logs, or
backups held by your WebToolbox server. Their retention depends on the server
operator's settings and practices.

## Permissions and connection security

| Permission | Purpose |
| --- | --- |
| `activeTab` | Obtain the current tab URL after you invoke the extension, then extract its origin for Login matching. |
| `storage` | Save the configured and temporary pending server origins locally. |
| `clipboardWrite` | Copy a field value only after an explicit copy action. |
| Optional host access | Send authenticated API requests to the server you choose and approve. The extension reconciles saved grants to retain only the selected server. |

The extension does not request browsing-history, cookie-reading,
clipboard-reading, page-scripting, or persistent content-script permissions.

Normal server connections require trusted HTTPS. HTTP is allowed only for
explicit loopback addresses (`localhost`, `127.0.0.1`, or `[::1]`) for local
development. No certificate bypass is provided. Do not use real secrets for
development testing or paste credentials into an untrusted or insecure website.

## Your choices

- Change the server origin in the extension's **Settings**. Chrome may request
  access to the new server, and obsolete server host grants are removed.
- Close the popup to discard its matches and preview. Ask your server
  administrator to change focus-loss cleanup or the inactivity timeout.
- Revoke the extension's server access, disable it, or uninstall it through
  Chrome's extension controls to stop its use. Uninstalling removes its local
  extension configuration, not your server-side data or previously copied values.
- Sign out of WebToolbox to end that server session. A retained open preview may
  remain visible until it is cleared, but future preview, copy, reveal, or OTP
  renewal requests must pass the server's current session checks.
- Manage or delete server-side records and account data through WebToolbox or
  your server operator. Closing or uninstalling this client does not perform
  server-side deletion.

## Support, third-party services, and policy updates

For non-sensitive extension privacy questions, contact the maintainers through
[WebToolboxPublic GitHub Issues](https://github.com/ssf65org/WebToolboxPublic/issues).
Account, vault-data, and server-log requests should go to the operator of your
configured WebToolbox server.

GitHub issues are public. Do not include credentials, OTP secrets/codes, session
cookies, recovery codes, private server addresses, vault exports, or unredacted
logs/screenshots. Information you choose to submit to GitHub or the Chrome Web
Store is handled by those services under their own policies; it is not an
automatic extension telemetry upload.

This policy will be updated when the extension's data-handling practices change.
The current version will remain available in this public repository, with the
update date shown above.
