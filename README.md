# CaptureR By Ratin Motion — Web Store Listing Notes
_Updated for manifest v0.5.0 (activeTab, scripting, downloads, storage, host_permissions)_

## Permission Justifications (paste into "Privacy practices" tab)

Chrome's reviewers reject a listing fastest when a permission is requested but
not clearly justified, or when the justification doesn't match what the code
actually does. Paste these as-is — each one matches the current `manifest.json`.

**Host permission: `<all_urls>`**
> CaptureR lets the user capture the currently open webpage (visible area, full page, or a custom snip/selection) as an image or asset. To do this on any site the user visits, the extension needs to read the page's DOM and inject a content script for the capture UI. It does not run automatically on pages the user hasn't opened the popup on, and no page data is transmitted anywhere except the local bridge described below or the user's own Downloads folder.
>
> Note: `<all_urls>` is the single permission reviewers scrutinize hardest, because it's broad. If the listing gets flagged for it, the fallback is to narrow this to `activeTab`-only capture (drop `<all_urls>` from `host_permissions` entirely) and rely purely on `activeTab`, which only grants access to the tab the user is actively looking at when they click the extension icon — no code changes needed for that beyond removing the `<all_urls>` entry, since `capturePackage()` already only runs against the active tab passed in from the popup.

**Host permission: `http://127.0.0.1:43821/*`**
> This extension is a companion to a desktop Adobe After Effects panel (RatinSpecial) made by the same developer. When that panel is open, it runs a local bridge server on 127.0.0.1:43821 purely for inter-process communication on the user's own machine. The extension sends captured screenshots to this local address so they can be automatically imported into the user's After Effects project. No data leaves the user's computer; this is not a remote server.

**Permission: `activeTab`**
> Needed to access and capture the content of the tab the user is currently viewing when they click the extension icon.

**Permission: `scripting`**
> Needed to inject the capture/snip UI (selection overlay) into the active page on demand, and to read image-load state while assembling a full-page capture.

**Permission: `downloads`**
> Needed as a fallback: if After Effects/RatinSpecial isn't reachable at capture time (or the user picks "Download ZIP only"), the capture is saved as a .zip into the user's Downloads/CaptureR folder instead.

**Permission: `storage`**
> Needed to remember the user's last-used destination choice ("Send to After Effects" vs "Download ZIP only") in `chrome.storage` so the popup doesn't reset to the default every time it's reopened. This is a small local preference flag only — no page content, browsing history, or personal data is stored.

## Single Purpose description (paste into the listing field)
> Capture webpages (full page, visible area, or a custom snip) and either save them as a ZIP, or send them into an After Effects project via a local bridge running on the user's own machine.

## Privacy Policy (draft — host this on a webpage and link it in the listing)

CaptureR By Ratin Motion does not collect, store, or transmit any personal data
to external servers. All webpage captures are processed locally in the
user's browser and are either:

1. sent to a local bridge (http://127.0.0.1:43821) running on the user's own
   computer as part of the RatinSpecial After Effects panel, or
2. saved locally to the user's Downloads folder as a .zip file.

The extension also stores one small local preference (which of the two
destinations above was last selected) using `chrome.storage`, so the popup
remembers the user's choice between sessions. This preference never leaves
the user's browser.

No data is sent to any third party or remote server operated by the
developer. The extension does not use analytics, tracking, or advertising
scripts, and does not load or execute any remotely-hosted code — all
JavaScript ships inside the extension package.

Contact: [your email/support link here]

---

## Before you submit — checklist
- [ ] Upload the latest repacked extension zip (manifest at root — the file I've been sending you after each fix, e.g. `CaptureR_Extension_webstore_v4.zip`)
- [ ] Bump `"version"` in `manifest.json` before each new upload — the Web Store rejects a re-upload with the same version number as what's already live/in review
- [ ] Host the privacy policy text above on a real URL (a GitHub Pages page or simple site works) — Chrome Web Store requires a live link, not just pasted text
- [ ] Add 1+ screenshot (1280x800 or 640x400) showing the popup/capture UI
- [ ] Write a longer store description (2-3 short paragraphs) explaining it's a companion tool to RatinSpecial/After Effects — this helps reviewers understand the localhost permission isn't random
- [ ] Fill in the "Single purpose" field with the text above
- [ ] Pick a category (likely "Productivity" or "Developer Tools")
- [ ] Pay the one-time $5 developer registration fee if you haven't already
- [ ] If the listing gets rejected specifically over `<all_urls>`, see the fallback note under that permission above before resubmitting
