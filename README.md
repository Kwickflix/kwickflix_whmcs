<p align="center">
  <img src="https://kwickflix.shop/templates/twenty-one-custom/img/logov4.png" alt="KwickFlix" width="800">
</p>

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://kwickflix.shop/)
[![WHMCS](https://img.shields.io/badge/WHMCS-8.x-orange.svg)](https://www.whmcs.com/)
[![PHP](https://img.shields.io/badge/PHP-7.4%2B-777bb3.svg)](https://www.php.net/)
[![License](https://img.shields.io/badge/license-Proprietary-red.svg)](#license)
[![Module Type](https://img.shields.io/badge/module-Provisioning-success.svg)](#)

# KwickFlix — A No-BS, Production-Grade WHMCS Style Package

A complete WHMCS visual overhaul: a refreshed front-end template (`kwickflix`),
a fully redesigned cart (`kwick_cart`), and a powerful admin addon
(`kwickflixstyle`) that lets you control just about everything through a clean
tabbed UI — no CSS edits, no custom.css, no FTP roundtrips for normal changes.

> Built for people who want their WHMCS install to look like a real product,
> not a 2014 invoicing portal.

<p align="center">
  <strong>🔴 <a href="https://kwickflix.tv">Live Demo — kwickflix.tv</a></strong>
  <br>
  <em>See it in action before you install</em>
</p>

---

## Table of Contents

1. [What you get](#what-you-get)
2. [The hard rules (read these first)](#the-hard-rules-read-these-first)
3. [Installation — 3 steps](#installation--3-steps)
4. [The addon, tab by tab](#the-addon-tab-by-tab)
   - [General](#general)
   - [Header / Navigation](#header--navigation)
   - [Homepage](#homepage)
   - [Cart / Checkout](#cart--checkout)
   - [Client Area](#client-area)
   - [Footer](#footer)
5. [Cart customization deep-dive](#cart-customization-deep-dive)
6. [When things look broken (cache clearing)](#when-things-look-broken-cache-clearing)
7. [For developers](#for-developers)
8. [Compatibility](#compatibility)
9. [Uninstalling](#uninstalling)

---

## What you get

The zip mirrors your WHMCS install paths so files land in the right place
when extracted into your WHMCS root:

```
templates/
├── kwickflix/                  ← the WHMCS front-end template
│   ├── header.tpl              ← global design system + navbar + logos + bg
│   ├── footer.tpl              ← 3-column footer + dynamic CSS injection
│   ├── homepage.tpl            ← hero, stat tiles, marquees, social bar, info cards
│   ├── clientareahome.tpl      ← client-area dashboard
│   ├── clientregister.tpl      ← register flow
│   ├── login.tpl               ← login card
│   ├── contact.tpl             ← contact form
│   ├── viewannouncement.tpl    ← single announcement
│   ├── announcements.tpl       ← announcements list
│   ├── knowledgebase*.tpl      ← KB index / category / article
│   ├── affiliates.tpl          ← affiliate dashboard
│   ├── clientareaaddfunds.tpl  ← add funds page
│   ├── theme.yaml              ← theme metadata
│   └── … all other .tpl files preserved and visually compatible
│
└── orderforms/
    └── kwick_cart/             ← the order form (cart/checkout)
        ├── viewcart.tpl        ← Review & Checkout step
        ├── checkout.tpl        ← Payment Details step (gateway picker lives here)
        ├── configureproduct.tpl ← Per-product configuration step
        ├── complete.tpl        ← Order confirmation step
        ├── common.tpl          ← shared partial loader
        ├── css/
        │   ├── style.css       ← stock WHMCS cart CSS (kept; we override on top)
        │   └── kwickflix.css   ← bridge stylesheet — maps stock cart tokens to kwickflix design system
        └── img/                ← cart-specific assets (crypto logos, etc)

modules/
└── addons/
    └── kwickflixstyle/         ← the WHMCS admin addon
        ├── kwickflixstyle.php  ← module manifest + tabbed admin UI
        ├── hooks.php           ← ClientAreaPage + ClientAreaHeadOutput hooks
        └── lib/
            ├── Settings.php    ← key/value settings store (auto-JSON-encodes arrays)
            └── Renderer.php    ← dynamic CSS generator (runs on every page)

README.md                       ← you are here
```

---

## The hard rules (read these first)

These aren't suggestions. Breaking these will produce strange caching issues
that are hard to debug.

1. **NEVER create or use `custom.css` in this WHMCS install.** All styles are
   inline in the `.tpl` files (`{literal}<style>…</style>{/literal}` blocks)
   and any dynamic CSS is injected at runtime by the addon. A `custom.css`
   file causes persistent, hard-to-clear caching issues with WHMCS's asset
   pipeline. If you need to change a static style, edit the inline `<style>`
   block in the relevant `.tpl` file directly. If you want dynamic control,
   use the addon.

2. **After every change, clear compiled templates.**
   `Configuration → System Settings → Cleanup → Compiled Templates → Run.`
   WHMCS aggressively caches compiled `.tpl` files. If you upload new files
   and the page still looks old, this is 95% of the reason.

3. **To preview the template without activating it:** visit
   `/index.php?systpl=kwickflix` — and yes, the `?systpl=` parameter only
   works on `/index.php` specifically. It does NOT work appended to arbitrary
   client area URLs like `/cart.php?systpl=kwickflix`.

---

## Installation — 3 steps

The zip is structured to mirror your WHMCS install paths exactly. After
extracting, the files land where they need to be:

```
your-whmcs-install/
├── templates/
│   ├── kwickflix/                  ← front-end template
│   └── orderforms/
│       └── kwick_cart/             ← order form
└── modules/
    └── addons/
        └── kwickflixstyle/         ← admin addon
```

### Step 1: Extract the zip into your WHMCS root

Upload the contents of the zip (the `templates/` and `modules/` folders +
`README.md`) to your WHMCS installation root, **merging** with the existing
folders. Your existing templates and modules will not be touched — only
the three new directories are added.

You can do this however you prefer: FTP/SFTP, cPanel File Manager, SSH +
`rsync`, etc. If your FTP client asks about overwriting `templates/` or
`modules/`, choose **"merge"** or **"add files to existing folder"** — NOT
"replace folder".

### Step 2: Activate the template + cart in WHMCS admin

1. **Template:** Setup → General Settings → General → "Template" → select
   `kwickflix` → Save.
2. **Cart:** Setup → General Settings → Ordering → "Default Order Form
   Template" → select `kwick_cart` → Save.

To preview the template without switching: visit
`https://yourdomain.tld/index.php?systpl=kwickflix` (the `?systpl=`
parameter only works on `/index.php`, not on arbitrary client-area URLs).

### Step 3: Activate the addon

1. Setup → Addon Modules
2. Find **KwickFlix Style** → click **Activate**
3. Click **Configure** → grant access to your admin role group → Save
4. The addon now appears in your top menu: **Addons → KwickFlix Style**

The `mod_kwickflixstyle_settings` table is created automatically on first
activate. Your settings persist across deactivation/reactivation.

---

## The addon, tab by tab

The admin UI is organized into six tabs. Every setting is documented inline
with a help description below it — what follows is the high-level overview
so you know what each tab is for.

**A note on drag-and-drop everywhere:** Any list in the addon that has a
⋮⋮ grip handle uses the same **live-reorder** UX. Grab the grip, drag,
and the row physically slides into its new position as you move — no
phantom drop-indicator lines, no surprises on release. Works on touch
devices too.

### General

The site-wide controls. Multiple sub-sections cover accent color, social
sharing meta, an optional announcement bar, custom code injection, and
settings backup.

**Master Accent Color**

| Setting | What it does |
|---|---|
| Master Accent Color | The hex color that drives buttons, links, focus rings, hover glows, badges, and selected states across every page of your WHMCS install. Default `#22a04a`. Pick this first — most other things inherit from it. |
| Master Glow Color | The default hover-glow halo around navbar pills. Per-item glow colors set in the navbar rows still override this for individual items. |

**Whole-Site Background**

Choose: blank / solid color / image / video. Image and video both accept
local paths or full external URLs. Video must be MP4 (auto-plays muted,
looped). Optional "Make background greyscale" desaturates the media so
colored foreground content doesn't fight it. Optional "Hide light overlay"
removes the soft cream tint over image/video for raw media.

**Layout**

| Setting | What it does |
|---|---|
| Hide left sidebar across all client area pages | When on, the left-hand sidebar is hidden and the main content area expands to full width. Useful if you prefer relying entirely on the top navbar. |

**SEO / Social Sharing (Open Graph)**

When someone shares a link to your site on Twitter/X, Discord, Slack,
Facebook, etc., these values control what shows in the preview card.

| Setting | What it does |
|---|---|
| OG Title | Headline of the preview card. ~60 characters max. Falls back to the active page's `<title>` when empty. |
| OG Description | Short summary under the title. ~155 characters max. Also emitted as `<meta name="description">` for SEO. |
| OG Image URL | Big preview image. 1200×630 JPG or PNG works best on every platform. Triggers Twitter's `summary_large_image` card type automatically. When empty, no `og:image` is emitted (platform shows a generic preview). |

All three are independent — leave any one blank and it just skips that
particular meta tag instead of emitting an empty one. The hook also emits
`og:type=website` always, plus mirroring `twitter:*` variants when the OG
fields are populated.

**Hero Announcement Box**

A styled promo card rendered at the top of the homepage hero (above the
eyebrow line). Visually matches the stats grid and social bar cards but
with a configurable edge glow.

Two glow modes:

- **Static** — single color forms a thin gradient ring around the box,
  plus a soft same-color halo just outside the edge. Calm, professional.
- **Multi-color** — multiple color stops are arranged around the box
  edge as a `conic-gradient`. Optionally rotate them continuously
  around the edge on an 8-second loop for attention-grabbing promos.

| Setting | What it does |
|---|---|
| Show the announcement box on the homepage | Master on/off. When off, no HTML is emitted at all. |
| Title | Short bold heading in Barlow Condensed uppercase. Matches the other hero card titles. |
| Message | Supporting line under the title. HTML allowed (`<a>`, `<strong>`, `<em>`, emojis). |
| Glow Mode | Radio: **Static** (one color) or **Multi-color** (gradient of N colors). |
| Static Color | When Glow Mode = Static: the single color used for the edge ring and outer halo. |
| Multi-color Stops | When Glow Mode = Multi-color: a flat repeater of color stops. Add as many as you want. Drag the ⋮⋮ handle to reorder — stop order determines how the colors are distributed around the edge. |
| Rotate the colors around the edge of the box | When ON in Multi-color mode, the conic-gradient rotates continuously (8-second loop). Honors the visitor's `prefers-reduced-motion` setting. |

The glow visual uses the "dual-background border trick": the card has a
`background:` with both `padding-box` (the solid surface color) and
`border-box` (the gradient ring) layers, with a 2px transparent border
revealing the ring. **Tight to the edge — no halo bleed.** Rotation
animates the `--ann-angle` custom property via CSS `@property` for
smooth GPU-accelerated angle animation.

The box renders only when toggled on AND at least one of title/message
is set.

**Custom Code Injection**

For analytics tags, chat widgets, pixels, schema markup, and any other
third-party snippets you need on every page.

| Setting | What it does |
|---|---|
| Custom HEAD HTML | Rendered inside `<head>`, right after WHMCS's own headoutput. Best for analytics (GA4, GTM, Plausible, Fathom), pre-load hints, `<meta>` tags, and structured data (`<script type="application/ld+json">` blocks). |
| Custom FOOTER HTML | Rendered just before `</body>`. Best for chat widgets (Crisp, Intercom, Tawk), deferred analytics, social-proof popups, anything that should load late. |

Both render **exactly as-is** with no escaping — you can paste raw
`<script>`, `<link>`, `<noscript>`, etc. The admin is trusted; the
addon does not sanitize. Bad HTML or broken JS here affects your whole
site, so test in staging first if you're not sure what you're pasting.

**Settings Export / Import**

Backup, restore, or migrate the entire addon configuration as a single
JSON blob. Use this when copying staging → production, or before making
big config changes.

- **Export** — a pre-filled readonly textarea shows a `JSON_PRETTY_PRINT`
  snapshot of every saved setting (logos, nav rows, marquees, notices,
  payment gateways, the works). Two buttons: 📋 Copy to clipboard, or
  ⬇ Download as JSON (filename `kwickflixstyle-settings-YYYY-MM-DD-HH-MM-SS.json`).
- **Import** — paste a previously-exported JSON blob into the textarea
  and click the red "Replace all settings with this JSON" button. A
  browser `confirm()` prompt asks for confirmation before overwriting.
  After import, you'll see a green success banner with the count of
  keys written, or a red error banner with the parse error if the JSON
  is malformed.

Only the keys present in the pasted JSON are written — keys you don't
include keep their current values. For a clean "factory reset" before
importing, drop the `mod_kwickflixstyle_settings` table manually first.

### Header / Navigation

Logos, the navbar rows, and the whole-site background.

- **Header Logo URL** — replaces the navbar logo. PNG or SVG, ~58px tall.
- **Top Centered Logo URL** — optional logo rendered above the header bar,
  centered. Useful for a wordmark above an icon-only header logo.
- **Favicon URL** — the little icon shown in browser tabs and bookmarks.
  Accepts `.ico`, `.png`, `.svg`, `.jpg`, `.gif`, `.webp`. The MIME type
  is auto-derived from the file extension. Renders both `<link rel="icon">`
  and `<link rel="shortcut icon">` for old-browser compatibility. Leave
  blank to fall back to browser default.
- **Primary Navbar (Row 1)** — drag-and-drop list of navbar links. Each item
  has: label, URL, emoji or FA icon, target, optional glow color, optional
  hide-when-logged-in / hide-when-logged-out toggle.
- **Secondary Navbar (Row 2)** — same controls, renders below row 1 as a
  thinner row with its own background.
- **Strict Navbar Mode** — when on, hides any WHMCS-injected navbar items
  that aren't in your Row 1 list (e.g. removes the stock "Affiliates" link
  if you haven't added it yourself).
- **Whole-Site Background** — choose: blank / solid color / image / video.
  - Video must be MP4. Hosted locally for performance.
  - "Hide light overlay" removes the cream tint over image/video.

### Homepage

The marketing landing page (`homepage.tpl`).

- **Hero Title** — main headline. Supports HTML for `<span class="accent">`
  to highlight words in your accent color.
- **Hero Motto** — subheading text under the title.
- **Hero CTA** — button text + link to whatever you want.
- **Hero Stats Grid** — drag-and-drop list of stat tiles (number + label).
- **Hero Social Bar** — drag-and-drop list of social/contact buttons with
  per-item icon color, external-link auto-detection (so chat widgets like
  Crisp don't open a blank tab).
- **Hero Logo Marquee** — horizontally scrolling row of logos. Use it for
  payment providers, trust badges, accepted currencies, etc. Drag-and-drop
  reorder, per-item visibility, adjustable speed.
- **Reviews / Testimonials** — repeater for customer quotes.
- **Hero Background** — separate from the site-wide background. Image or
  video, with overlay control.

### Cart / Checkout

This is the big one. Three major sub-sections:

#### Cart/Checkout — Marquee

A horizontally scrolling logo row rendered at the top of `cart.php`
(viewcart + checkout steps). Mirror of the Hero Logo Marquee. Typical use:
crypto logos or accepted payment-method icons.

#### Cart/Checkout — Notice Boxes

Three editable alert boxes at fixed positions on the cart flow:

| Slot | Where it renders |
|---|---|
| **CP** | Configure-product step, above the Continue button. |
| **CO Top** | Final checkout page, **between** the credit-balance options and the payment gateway list. (Replaces the old hardcoded Amazon promo box.) |
| **CO Bot** | Final checkout page, right above the Terms of Service checkbox. |

Each notice has:

- Enable/disable toggle
- Severity color (Info / Success / Warning / Danger — drives bg, border, title color)
- Optional title (renders centered as a bold uppercase row)
- Optional Font Awesome icon (renders **only when a title is set**, flanking
  the title text on both sides)
- HTML body — `<a>`, `<br>`, `<strong>` all render properly. `javascript:`
  URLs in `onclick`/`href` attributes get auto-sanitized so trailing
  `return false` becomes `void 0` (prevents chat widgets opening blank tabs).

Rendering rules:
- Title alone (no body) — just the title row centers
- Body alone (no title) — just the body text centers, no icon renders
- Both title and body — title row with icons flanking, body centered below
- Both empty + enabled — nothing renders (no empty box)

#### Cart/Checkout — Payment Gateways

The crown jewel. Replaces the stock WHMCS gateway picker with a fully
admin-controlled, drag-and-drop, per-card-customizable list.

**Section-level controls (top of the section):**

| Setting | What it does |
|---|---|
| Preferred Ribbon Color | Color of the corner ribbon shown on cards marked "Preferred". Override the default gold gradient. |
| Preferred Border / Glow Color | When a Preferred gateway is **selected**, its thin border + inner glow use this color (default rich gold `#d97706`). Overrides any per-row color so the preferred treatment stays visually distinct. |

**Per-gateway controls (one row per gateway, drag the ⋮⋮ handle to reorder):**

| Setting | Description |
|---|---|
| WHMCS Gateway | Dropdown auto-populated from `tblpaymentgateways` — only active/visible gateways shown. |
| Show this gateway | Quick hide without losing the row's settings. |
| Display Title | Bold heading on the card. Overrides WHMCS's stored gateway name. |
| Subtext | Smaller description below the title. HTML allowed (e.g. `<b>US & UK only.</b>`). |
| Icon / Emoji / Image | Auto-detected. Paste an emoji (`💳`), a path or URL (`/path/to/icon.png` — rendered as `<img>`), or raw HTML (`<i class="fab fa-cc-visa"></i>`). |
| Mark as Preferred | Adds the corner ribbon to this card and applies the section-level Preferred Border/Glow color when selected. |
| Ribbon Label | Custom text inside the ribbon (default "PREFERRED"). |
| Border / Glow Color (per row) | When this gateway is selected, its border + inner glow use this color. Overrides the site's master accent. If the gateway is also Preferred, the section-level Preferred color wins. |

**Cascade for the selected-card highlight:**

1. Is this card Preferred? → section-level Preferred Color
2. Else, does this row have its own color override? → row's color
3. Else → site master accent

**The Preferred gateway auto-selects on page load.** The first row marked
Preferred (per drag-and-drop order) becomes the default `checked` radio
button when the customer reaches the checkout page. They can still pick
anything else.

**If you configure zero rows OR none of them match active WHMCS gateways:**
the cart shows a clean centered empty-state notice ("No payment options are
currently available — please contact support") instead of a broken/empty
gateway list.

### Client Area

The post-login dashboard.

- **Top Stat Tiles** — the row of cards at the top of `clientareahome.tpl`.
  Default tiles: Services, Domains, Invoices, Live Chat. Add/remove/reorder
  freely. Stat numbers are pulled live from WHMCS's client stats.
- **Custom Info Cards** — optional banner cards between the stat tiles and
  WHMCS's built-in panels. Title, icon, description, single CTA button,
  accent color. Empty by default.
- **WHMCS Panel Accents** — accent bars on the standard client-area panels
  (Active Services, Recent News, Affiliate Program).
- **Hide Recent News** — removes the recent-news/announcements card from
  the home. The `/announcements.php` page itself stays accessible.
- **Hide Affiliate Program** — removes the affiliate promo card from the
  home. `/affiliates.php` stays accessible.
- **Hide Recent Support Tickets** — removes the recent-tickets card. WHMCS
  still tracks tickets internally.
- **Notifications Popover Styling** — the notification pill in the navbar.
  Popover dropdown is styled to match the hello-button dropdown
  (rounded corners, accent-tied border, Barlow Condensed header).

### Footer

The 3-column footer.

- **Footer Logo URL** — image at the top of the brand column.
- **Short Description** — 2–3 sentences under the logo (e.g. tagline).
- **Three columns**, each with its own title + drag-and-drop link list.
  Per-item: label, URL, target (`_self` / `_blank`).

---

## Cart customization deep-dive

The cart (`kwick_cart/`) is the most complex piece. Here's how it fits together.

**Bridge CSS (`kwick_cart/css/kwickflix.css`):**
A ~900-line stylesheet that maps the stock cart's CSS tokens (the `--vc-*`
variables WHMCS uses internally) onto the kwickflix design system's tokens
(`--ca-*`). This is what lets the cart inherit your master accent color
automatically. It also overrides the stock cart's hardcoded Bootstrap 3
greens (`#5cb85c`, `#3c763d`, etc) with kwickflix accent tokens.

**Common loader:**
All cart `.tpl` files include `common.tpl`, which loads `style.css`
(stock cart) THEN `kwickflix.css` (the bridge). Order matters — the bridge
must load last so its overrides win the cascade.

**Inline `<style>` blocks per page:**
Each cart page (`viewcart.tpl`, `checkout.tpl`, `configureproduct.tpl`,
`complete.tpl`) has its own inline `{literal}<style>…</style>{/literal}`
block with page-specific styles. These are inline (not a separate `.css`
file) because:
1. We never write `custom.css` (cache death).
2. Inline `<style>` always wins ties in the cascade because of source order.
3. It keeps every page self-contained and easy to debug.

**The order summary card:**
Look at `viewcart.tpl` for the reference styling. `configureproduct.tpl`
mirrors that pattern exactly — same `.summary-container` flatten, same
divider lines, same flat `.total-due-today`. If something looks weird on
configureproduct, the answer is almost always "match viewcart's pattern."

**Smarty cart variables (the `$carttpl` thing):**
The cart templates use `{include file="orderforms/$carttpl/..."}` so
includes resolve to whichever cart is currently active. Don't hardcode
`orderforms/standard_cart/` or `orderforms/kwick_cart/` — let WHMCS resolve.

---

## When things look broken (cache clearing)

90% of "I uploaded new files but nothing changed" issues are stale compiled
templates. WHMCS aggressively caches the compiled PHP versions of `.tpl`
files. New `.tpl` source on disk + old compiled `.tpl` in `/templates_c/` =
the old version keeps rendering.

**Always do this after uploading:**

1. **WHMCS:** Configuration → System Settings → Cleanup → check **Compiled
   Templates** → Run.
2. **WHMCS:** Same Cleanup screen, also tick **Module Cache** if you
   changed addon code.
3. **Browser:** Hard refresh the page — Ctrl+Shift+R (Windows/Linux) or
   Cmd+Shift+R (macOS). Or open DevTools, right-click the reload button,
   "Empty Cache and Hard Reload."

If the page is **completely unstyled** (showing bulleted lists for navbar
items, etc.):

- Bootstrap CSS isn't loading on that page. Check the browser DevTools
  Network tab for 404s on `bootstrap.min.css` or similar.
- Verify all three folders (`kwickflix`, `kwick_cart`, `kwickflixstyle`)
  uploaded fully — partial uploads can leave the template referencing
  missing files.

If the addon's admin save doesn't seem to stick:

- Check Setup → System Logs → Activity Log for hook errors. The activity
  log will tell you if `kwickflixstyle` is throwing anything.
- Verify your admin role has access to the addon module (Setup → Administrator
  Roles → your role → Addon Modules → tick KwickFlix Style).

---

## For developers

### Storage

The addon uses a single MySQL table:

```sql
mod_kwickflixstyle_settings (key VARCHAR PRIMARY, value LONGTEXT)
```

Complex values (arrays of nav items, payment gateway rows, etc.) are
JSON-encoded on save and decoded on read by `Settings::all()`. The Settings
class never escapes HTML — what you save is what you get back, so the
hook layer (`hooks.php`) is responsible for any rendering-time sanitization.

### Hooks

```php
add_hook('ClientAreaPage',          1,    $kfFrontendVars);   // exposes Smarty vars
add_hook('ClientAreaPageCart',      1,    $kfFrontendVars);   // same handler, cart pages
add_hook('ClientAreaHeadOutput',    1,    $kfDynamicCssHook); // injects <style> in <head>
add_hook('ClientAreaPrimaryNavbar', 9999, $kfStrictNav);      // strict navbar mode
```

`$kfFrontendVars` exposes ~80 Smarty variables prefixed `kf_` (e.g.
`$kf_logo`, `$kf_payment_gateways`, `$kf_cart_notice_co_top_html`, etc.).
Each one has a sensible default so the template works even if the addon
is disabled or uninstalled.

`Renderer::dynamicCss($s)` returns the inline `<style>` block. It also
emits a defensive rule that hides `.dropdown-menu:not(.show)` and
`.modal:not(.show):not(.in)` so Bootstrap's hide-by-default behavior is
guaranteed independent of any CSS-load timing issues.

### Adding a new setting

1. Add the field to the admin UI in `kwickflixstyle.php` (search for an
   existing section to model it on — `kfSectionOpen()`/`kfSectionClose()`
   patterns are everywhere).
2. Add it to the save handler payload at the top of `kwickflixstyle_output()`.
3. Expose it as a Smarty variable in `hooks.php` (in `$kfFrontendVars`).
4. Consume it in your `.tpl` file as `{$kf_your_setting}`.
5. For sensitive HTML values, render with `{$kf_var|escape:'none' nofilter}`
   to bypass WHMCS's Smarty auto-escape.

### Repeater (drag-and-drop list) pattern

Every list in the addon that lets you add/remove/reorder rows uses the
same `bindRepeater()` helper. The drag-and-drop UX is **live-reorder** —
the dragged row physically moves into its new position as the cursor
crosses each target row's vertical midpoint. No phantom drop-indicator
lines; what you see during the drag IS the final layout. Touch input
is supported (mobile/tablet admins can drag via the grip).

If you want to add a new drag-and-drop list (like Payment Gateways or
Stat Tiles), follow this pattern:

1. Create a `kfRenderXyzRow($idx, $row)` function that returns the row HTML.
   Use the `<details class="kf-card-row">` shell with a `.kf-card-row-grip`
   span for the drag handle.
2. In the admin section, render a list of existing rows + an empty `<template>`
   for new rows + an "Add" button.
3. At the bottom of the admin JS, call:
   ```js
   bindRepeater({
     listId: '#kf-xyz-list',
     addBtnId: '#kf-add-xyz',
     templateId: '#kf-xyz-template',
     prefix: 'xyz_rows',
   });
   ```
4. Process the saved rows in the save handler (loop `$_POST['xyz_rows']`).

`bindRepeater` handles live-reorder drag, the up/down/delete buttons,
the live-title-sync on the summary line, the visible-badge toggle, and
proper renumbering of the form input `name` attributes after every
add/delete/reorder so `$_POST` arrives as a clean contiguous 0..N-1
indexed array. You don't have to write any of that — just match the
pattern.

For very simple value-only lists (a single color picker per row, etc.)
where the collapsible `<details>` shell would feel heavy, write a
flat custom mini-repeater instead — the **Announcement Box → Glow
Colors** field in the addon does this. Look at the inline JS under
the `annGlow` IIFE in `kwickflixstyle.php` for the ~50-line pattern
that gives you the same drag, delete, renumber, and `injectHexTwins`
behavior without the `<details>` overhead.

---

## Compatibility

- **WHMCS:** 8.x (built and tested against 8.10+)
- **Base template family:** Twenty-One (same Bootstrap 4.5.3 / jQuery 1.12.4
  / FontAwesome 5.10.1 stack)
- **PHP:** 8.1+
- **Browsers:** Chrome / Edge / Firefox / Safari latest. Uses `:has()`
  (Safari 15.4+, Chrome 105+, Firefox 121+) which is well-supported in 2026.

---

## Uninstalling

### Just the addon
1. Setup → Addon Modules → KwickFlix Style → **Deactivate**.
2. Settings are preserved in the `mod_kwickflixstyle_settings` table — you
   can reactivate later and pick up exactly where you left off.
3. To fully wipe: `DROP TABLE mod_kwickflixstyle_settings;` (or use the
   commented-out drop statement in `kwickflixstyle_deactivate()`).

### The template
1. Setup → General Settings → General → "Template" → set back to your
   previous template → Save.
2. The `templates/kwickflix/` directory can be safely deleted.

### The cart
1. Setup → General Settings → Ordering → "Default Order Form Template" →
   set back to your previous order form → Save.
2. The `templates/orderforms/kwick_cart/` directory can be safely deleted.

---

## Quick reference card

```
Admin UI:           Addons → KwickFlix Style
Settings table:     mod_kwickflixstyle_settings
Template path:      /templates/kwickflix/
Cart path:          /templates/orderforms/kwick_cart/
Addon path:         /modules/addons/kwickflixstyle/
Bridge CSS:         /templates/orderforms/kwick_cart/css/kwickflix.css
Dynamic CSS hook:   ClientAreaHeadOutput → Renderer::dynamicCss()
Preview without activating: /index.php?systpl=kwickflix
After every upload: WHMCS Cleanup → Compiled Templates → Run
                    AND: Browser hard refresh (Ctrl/Cmd+Shift+R)
```

---

If something doesn't work, check the activity log first (Setup → System Logs).
Most issues are stale templates_c or a missing folder upload.

Built with care. Don't use `custom.css`. Clear `templates_c` after every
upload. Ship it. 🚀
