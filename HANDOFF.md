# Steps Summit 2026 · Print & QR Collateral: Handoff

Date: 16 Oct 2026

## 1. What was built

| # | Design | File | Output | Pages |
|---|--------|------|--------|-------|
| 1 | Partner Interview Schedules | `code/Partner Interview Schedules.dc.html` | A5 print + mobile web (QR target) | 87 (one per partner) |
| 2 | Media Partner Schedules | `code/Media Partner Schedules.dc.html` | A5 print + mobile web (QR target) | 13 (7 media partners, long lists split) |
| 3 | Visitor Agenda | `code/Visitor Agenda.dc.html` | Mobile web (default) + A5 print | Web: filterable agenda + map tab · Print: 8 track pages + 1 map page |
| 4 | Event Map | `code/Event Map.dc.html` | Reusable component, used inside the Visitor Agenda | n/a |

All files open directly in a browser. Keep the `code/` folder structure intact, because the designs load the shared runtime, data and logos by relative path.

## 2. Brand system applied (from the Steps Summit Branding Guide)

- **Colours:** Navy `#0B1351` (base), Purple `#551FF8`, Pink `#F735A7`, Teal `#11D1D0`, Orange `#FB6F41`, Lime `#CBDF38`, Violet `#B92DE5`
- **Type:** Headings use Figtree (a web stand-in for Gibson; swap to Gibson if licensed). Body text is Helvetica Neue.
- **Pattern:** the 8-tile rounded geometric strip under every header, taken from the guide's shape language.
- **Header:** Steps Summit logo (colour) on white, plus "Powered by AHC".
- **Footer on every sheet:** Main sponsor SODIC Sports Club · Platinum Swiss Gold · Silver Wycombe Abbey Cairo East + WMC-EF · Connecting Minds wordmark.

## 3. Design details

### 3.1 Partner Interview Schedules
- Source: Exclusive Interviews List (Partner Overview + NOWAH Podcast sheets).
- Each sheet shows the partner name, interview window (PM), a numbered and colour-coded list of media partners with tickboxes, and a NOWAH slot time where one applies.
- QR → `?p=<partner-slug>` opens that partner's mobile page.
- Tweaks: `view` (Print / Web), `webPartner` (preview), `qrBaseUrl`.

### 3.2 Media Partner Schedules
- One schedule per media partner: Raht Baly (42), Cairo Spots (48), Identity (7), AS Media (87), The Briefline (46), Lemonade (53), NOWAH Podcast (10).
- Line-up sorted by time. Up to 14 rows use a single large column; longer lists use 2 columns and split across pages ("Page x of y").
- QR → `?m=<media-slug>`.
- Tweaks: `view`, `webMedia`, `rowsPerPage` (default 30), `qrBaseUrl`.

### 3.3 Visitor Agenda
- Source: Steps_Summit_Agenda_2026 (10).xlsx, "Agenda" tab.
- 5 colour-coded spaces: Main Stage (purple), Workshop Room 1 (pink), Workshop Room 2 (teal), Talent Stage (orange), Outdoor Garden (lime).
- Web: hero with QR, a filter bar that stays in view (All / 5 spaces / Event Map), sessions grouped by start time, and cards with space, time, title, speaker and audience.
- Print: one A5 page per space (Main Stage and both workshop rooms split into 2 parts) plus an Event Map page with its own QR (`?tab=map`).
- All em dashes removed from content.
- Tweaks: `view`, `qrBaseUrl`.

### 3.4 Event Map
- Schematic redraw of the aerial reference in brand colours: sports hall, club house, amphitheatre, pools, play courts, sand courts, pitch, garden, roads, walkway, and an entrance arrow.
- 17 numbered pins. Legend colour logic: **session spaces use their agenda colour** (Stage = Main Stage purple, Stage Talent Zone = Talent orange, Workshops = pink/teal split, Garden = lime). **Services and other areas are violet.** Main Gate is shown as a white outline.

## 4. Data decisions & assumptions (please verify)
- All interview and agenda times are PM, except gates opening at 11:30 AM.
- Rania Abdelaziz "9:55–9:10" was corrected to 9:55–10:10.
- Aya Taher shows 4 interviews (no total in the sheet). Rany Refaat shows 1 (the sheet said 2), because counts come from the ticks.
- Indoor WS 1/2 were renamed to Workshop Room 1/2.
- Generic titles were added where the sheet only had a provider name (Swiss Gold Session, Wycombe Abbey Workshop, Outdoor Play, ATM on Stage).
- Back-to-back sessions were merged: ATM Outdoor 6:05–7:00 and Antie Jossie Education Show 8:05–9:00.
- The Venue Area and Provider columns, the hidden Sheet2 and the per-speaker tabs were not used.
- AHC is treated as the "Powered by" partner.
- Map zone shapes are approximate. Confirm the pools, courts and pitch labels on site.

## 5. To go live (QR codes)
1. Host the `code/` folder (any static host: Netlify, Vercel, GitHub Pages, S3).
2. In each design, set the `qrBaseUrl` Tweak to the public URL of that file (e.g. `https://yoursite.com/Visitor%20Agenda.dc.html`), **or** edit the default in the file's data-props.
3. Re-export the print PDFs so the printed QR codes point to the live pages.

URL parameters:
- Partner sheets: `Partner Interview Schedules.dc.html?p=sodic`
- Media sheets: `Media Partner Schedules.dc.html?m=raht-baly`
- Agenda map tab: `Visitor Agenda.dc.html?tab=map`

## 6. Printing
- A5 (148 × 210 mm), one design per page, no margins added by the browser.
- Switch the `view` Tweak to Print, then Print / Save as PDF.
- QR codes are generated in the browser (qrcode-generator via jsDelivr), so an internet connection is needed when rendering.

## 7. Code map
```
code/
  Partner Interview Schedules.dc.html   partner sheets + partner web page
  Media Partner Schedules.dc.html       media sheets + media web page
  Visitor Agenda.dc.html                agenda web + print, map tab
  Event Map.dc.html                     map component (imported by the agenda)
  schedule-data.js                      interview data (partners × media, NOWAH slots)
  agenda-data.js                        agenda tracks + sessions (edit sessions here)
  doc-page.js                           A5 page / print shell
  support.js                            component runtime
  assets/sm/                            logos: steps, ahc, main (SODIC SC), plat (Swiss Gold), silver1 (Wycombe), silver2 (WMC-EF), cm (Connecting Minds)
```
Updating content: edit `agenda-data.js` (one session per line: `track|start|end|title|speaker|tag`) or `schedule-data.js` (`Name|RahtBaly,CairoSpots,Identity,ASMedia,Briefline,Lemonade bits|time`). Note: Partner Interview Schedules currently keeps its own copy of the interview data inside the file, so update both if the data changes.
