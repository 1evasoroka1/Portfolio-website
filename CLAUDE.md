# CLAUDE.md: Yeva Soroka portfolio website (version 3)

Для Евы: это файл с главными правилами и информацией о сайте. Claude читает его в начале каждой новой сессии в этой папке, поэтому правила не нужно объяснять заново.

## Status: read this first (Nov 2026)
**Для Евы:** сайт готов и опубликован. Ниже самое важное.
- **Live site:** https://soroka-webdesign.vercel.app (hosted on Vercel, auto-deploys from GitHub `main`).
- **Repo:** `git@github.com:1evasoroka1/Portfolio-website.git`. Workflow: edit here, test locally, **commit and push only after she says yes**; Vercel redeploys in 1 to 2 minutes. She signs in herself, Claude never types passwords or tokens.
- **Files:** `index.html` (the whole site), `imprint.html`, `privacy.html`, `img/` (WebP images, QR codes, `og-image.jpg`), `icons/` + `favicon.ico`, `fonts/` (self-hosted), `robots.txt`, `sitemap.xml`. `originals/` holds the untouched PNG/JPG sources (not needed on the server). v1 and v2 stay untouched.
- **Languages:** EN (default) and RU. Static meta tags and share preview are English only (crawlers do not run JS); the browser tab title and description switch with the language.
- **Phones:** page frame **375×812**, every section min-height 812px, 16px side margins, compact layouts. Her rule: she looks at the phone version a lot, so re-check it after every change (EN and RU, widths 320 to 430).
- **Key decisions she made:** v3 look (burgundy/beige, Unbounded + Manrope); Terms as numbered rows; card titles 32px and text 24px; favicon = beige circle, burgundy "YS"; no Cloudinary; no analytics or cookies (so no cookie banner); hosting on Vercel.

### What changed recently (newest first)
- Teacher's edits (Nov 2026): "Visit website" button bigger; project tag outlines more transparent (28%). A scroll animation in Terms (plate, then fading text) was tried and removed on her request: Terms stays as plain rows, except the hover effect her teacher asked for: the row under the mouse grows 3% and gets a beige plate with burgundy text (`.term:hover`, only on devices with a mouse, uses the CSS `scale` property so it does not clash with the fade-in). On touch devices and on any phone-width layout (<= 760px, even in a narrow desktop window with a mouse; hover effects only apply above 760px) the same plate follows the scroll: SERVICES are gradual (her request, she likes it): the script sets `--p` (0 to 1, from how close the card is to the middle of the screen, reach 16% of the screen height, smoothstep) and the card `scale` follows it. TERMS stay a whole plate that turns on and off (she did not like a fading/gradual version): the nearest row gets `.active`, only the transition is slow and soft (0.7s ease on phones and touch devices, `@media (max-width: 760px), (hover: none)`). `litOnScroll(sec, items, gradual)` handles both. Wide screens with a mouse only use hover. the row nearest the middle of the screen gets `.active` (JS `updateTerms`, positions from the ResizeObserver cache; first row, then second, third, fourth), her choice. Phones: sections are as tall as their content (only the hero is 812px), 48px padding above and below each section, the slide-over works on phones too (sticky hero, CSS only), `scroll-padding-top: 24px` so anchor jumps stop below the logo bar. Services cards (teacher's edit, colours unchanged): a mouse grows the card under the cursor 3% (`.service:hover`); on touch devices the card nearest the middle of the screen grows 4% while scrolling (`.active`: landing page first, then multi-page, reels, ad creatives, presentation). Terms and Services share the helper `litOnScroll`. Phone text sizes raised (her request, readability for weak eyesight): card texts in How I work, Services and Terms 15px, Terms titles 15 to 18px, project Client/Idea/What I did 15px, tags 13px, chips 14px; Services cards on phones: title 15 to 18px (a bit bigger than the 15px text, like Terms), the price sits under the title so long Russian names fit. Russian version only: a "Связаться в Telegram" button (https://t.me/yevaa_s, no QR code) under WhatsApp and Instagram in Contact, as wide as the two of them (`.tg-btn`, hidden unless `html[lang=ru]`, key `contact.tg`). How I work on phones: one column of long cards, titles 15 to 18px. How I work on phones (her request Nov 18 2026): long cards that stack like a staircase while scrolling (`.step` is `position: sticky`, `top` 76px plus 56px per card, so the number and title of every card stay visible; thin light edge on each card). Once the last card is in place the whole staircase scrolls away as one piece (not piling up, no slide-over; her decision): JS `measureStairs`/`updateStairs` sets `--e` = scroll past the moment the last card sticks, `.step` moves up by `--e`; `.steps::after` (626px) keeps the sticky cards from clamping and `#services { margin-top: -640px }` cancels the spacer, so the page length is unchanged. If the step (56px) or the spacer changes, keep spacer + gap = the negative margin. She tried a Services version with two big cards and three compact rows, and a Terms version with big figures (✓, 2–3, 100%, $), and did not like them: both were reverted to the plain phone versions (Services = rows with star, title, price under the title; Terms = rows with number, title, text and thin lines, with the scroll/hover plate). Services copy shortened (her request): landing page "Одностраничный сайт, который презентует ваши услуги и подводит к заявке или записи", reels "Для Instagram: нарезка, субтитры, музыка и графика", presentation "Презентация для вашего бизнеса, продукта или обучения в фирменном стиле" (EN matches). Phone hero buttons use `font-size: clamp(12px, 4.1vw, 14px)` so the Russian "Написать мне" stays inside the screen at 320px; the product pills in the hero are `nowrap` with `font-size: clamp(13px, 5vw, 16px)` so "Многостраничные сайты от $500" stays on one line even on a 300px screen.
- Mobile PageSpeed round 2: extra small image sizes (`-480` screenshots and reels, `-240` phone screenshots, `yeva-cover-480/-720`, `yeva-about-480` via `image-set`), contact circle no longer loads the full portrait; scroll effects read positions from a ResizeObserver cache (no forced reflow); `<main>` landmark; phone "Projects" h2 is visually hidden instead of `display:none` (heading order).
- Speed and PageSpeed fixes: resized WebP screenshots with `srcset`, preloaded hero image and fonts, "PORTFOLIO" fitted without layout shift or forced reflow.
- Pre-deploy audit: meta tags, Open Graph and Twitter preview with a 1200×630 image, canonical, sitemap, heading order, alt/size/lazy on images, `noopener noreferrer`.
- Favicon (YS circle, optically centred), Imprint and Privacy pages (Vercel named as host), portraits as WebP.
- Terms redesigned (numbered rows), card font sizes unified, Russian "PORTFOLIO" overflow fixed, phone layouts reworked (375×812, no sideways overflow on narrow phones).

### Still open
- Re-run pagespeed.web.dev after each deploy and send the screenshots; check the preview on opengraph.xyz (press "Scrape again" if it is old).
- Optional: a separate `/ru` page for Russian search, a custom domain (then change the 4 address tags and `sitemap.xml`), GA4/Meta Pixel (needs a cookie banner and an updated Privacy page).

## Who and what
- Owner: **Yeva Soroka**, web designer (Instagram @soroka.webdesign, WhatsApp +41 76 309 95 53, Switzerland). She writes in **Russian** (sometimes English): answer in the language she uses.
- Bilingual **EN / RU** one-page portfolio. It will be linked from her Instagram and offers paid services (landing pages, multi-page websites, reels editing, ad creatives, presentations).
- She has a limited token budget: prefer targeted edits, few screenshots, and say when something is expensive.

## Folders and versions (never delete an old version, never edit v1 or v2)
- `/Users/yevka/web design/portfolio-site` = v1, the original style. It is a git repo (remote `https://github.com/1evasoroka1/Portfolio.git`, still empty). **Do not change it.**
- `/Users/yevka/web design/portfolio-site-v2` = v2 (cards look). She did not prefer it. Do not change it.
- `/Users/yevka/web design/portfolio-site-v3` = **this folder, the current version.** Work only here.
- Design source: her Figma file `8JVByjZDs8d9ow11xOEGIY` (Frame 9 = hero, Slides 2 to 4 = projects). She decided to work in local HTML, not in Figma.

## Tech
- Single file `index.html` (HTML + CSS + JS) plus `img/` and `fonts/`. No build step. Two small legal pages, `imprint.html` and `privacy.html` (EN/RU switch, same fonts, linked from the footer).
- **Favicon** (her choice, Nov 2026): round, beige circle with a burgundy border and burgundy "YS" (Unbounded 800, letters 68% of the circle width, her choice of three sizes, nudged 2.4% left and 0.65% down because the S is heavier than the Y and looked off-centre), solid so it shows on light and dark tabs. Files: `favicon.ico` (16/32/48) at the root and `icons/` (32, 192, 512 PNG and a 180 `apple-touch-icon` with a burgundy background). Linked in `index.html`, `imprint.html`, `privacy.html`. Redraw all sizes if the look changes (Safari uses the PNG and apple-touch icon, not SVG).
- **Speed (PageSpeed, Nov 2026):** project screenshots and reels are WebP in two sizes with `srcset` (`name-720.webp` and `name-1440.webp`, phones `-480/-390/-360`), the JPG originals are in `originals/`. Hero image and Latin fonts are preloaded; Cyrillic fonts are preloaded only for Russian visitors (inline script in the head, which also sets `lang=ru` early). New screenshot: `cwebp -q 78 -resize W 0 in.jpg -o name-W.webp` for two widths.
- **Photos:** the two portraits are WebP (`img/yeva-cover.webp`, `img/yeva-about.webp`, about 150 to 230 KB, transparent). The original PNGs are kept in `originals/` and are not needed on the server. Use WebP (`cwebp -q 85 -alpha_q 90`) for any new portrait; project screenshots and reels are WebP too (see Speed above). No Cloudinary on purpose (third party, IP goes to them, one more dependency).
- Translations live in the `T` object in the script, `data-i18n="key"` on elements, `setLang()`. English defaults are also written in the HTML. Russian: `?lang=ru`. A missing key falls back to English.
- Run: `cd "/Users/yevka/web design/portfolio-site-v3" && python3 -m http.server 8124`, open http://localhost:8124 (she uses Safari; testing is done with headless Chrome).
- **Fonts are self-hosted** in `fonts/` (Unbounded for headings, Manrope for text; Latin and Cyrillic). Never add a Google Fonts link again (the visitor's IP would go to Google). Upload `fonts/` together with `img/`.

## Layout rules (apply to every page)
- Design frame **1920×1080**. Every section is exactly **one screen tall**: nothing cut off, nothing scrolling inside a section.
- The whole page scales with the window: `--u = min(1vw, 1.7778svh)`. All sizes are written as `calc(N * var(--u))`, with small px floors that shrink on desktop (`--fk`).
- Margins: **50px** on the sides and top (`--m`), **30px** between blocks (`--g`). Header 50px from the top; titles start 30px under it (`--pad-top` 118px); content ends 84px above the bottom (`--pad-bottom`).
- Section titles: Unbounded ExtraBold, uppercase, **120px**, exactly **centred** on every page, between two thin lines (grid `1fr auto 1fr`).
- Font sizes (from her Figma): body text 24px, small text 20px, tags 18px, project name 44px.
- **All cards (How I work, Services, Terms): title Unbounded 800, 32px; text Manrope, 24px (at 1920, scaled down with the window).** Her rule (Nov 2026). Phones keep the compact sizes.
- Style: the v1 look (centred titles with lines, tilted pill badges, burgundy cards, beige and light sections, grain texture) on the Figma grid. Colours: burgundy `#5E0B16`, beige `#EBDCC6`.
- Logo "Yeva Soroka / web design" stays in English in both languages.
- **PORTFOLIO in the hero**: CSS gives it almost the final size from the first paint (`calc((100vw - 2 * var(--m)) * .14233)`, RU `.1184`), then a ResizeObserver fine-tunes it without forcing layout (it reads the hidden `#megaMeasure` word at 100px, the `#megaM` margin probe and the window width). It re-fits by itself on resize, language change and font swap. Keep these constants if the font or letters change: remeasure `font-size / (width - 2 * margin)` for EN and RU.
- **No sideways overflow on phones**: grids use `minmax(0, 1fr)` and step/service titles shrink with `4vw`, because a long Russian word (Предложение, Многостраничный) used to make the page wider than a 334px screen and the phone panned it sideways. Test widths 320, 334, 360, 375, 390 (an iframe of that width works) after changing phone text.
- **Phone page frame 375×812** (she tried 390×844 and went back): every section is min-height 812px. She tried a "safe area" (335×660, then 335×710 with 20px margins) and went back to the normal margins (16px sides, 68px top, 24px bottom), so there is no separate safe-area rule. "PROJECTS" title is hidden on phones for projects 01 and 03 (order: badge, name, type, mockups, text, tags, button). Re-measure after any change.
- **Phones (≤760px): every section fits one 375×812 screen** (her choice, Nov 2026). Compact phone block at the end of the section CSS: How I work = one column of long cards (number and title on one line, 15px text under them; her choice Nov 18 2026, the 2-column cards were too narrow for big text) with no spotlight dimming; Services = compact rows (star, name, price on one line, text under it); smaller mockups, terms and about photo. Re-measure at 375×812 in EN and RU after any copy change.
- Phone hero (<860px): the photo is pulled up so the hair overlaps "PORTFOLIO", text and product pills (wrapped in rows) follow tightly, so the whole first screen fits on a phone.

## Style (the look she loved in v1, kept in v3)
She said about v1: "I loved it." The look comes from v1 and only the grid, margins and font sizes come from her Figma rules. When adding anything new, copy these patterns instead of inventing new ones.

**Colours (CSS variables):** burgundy `#5E0B16` (main), mid `#7A1220`, deep `#3A050C`, ink `#2A0409` (page background and footer), beige `#EBDCC6` (text and buttons on dark), light beige `#F4EADB` (light sections), cream `#FFF8EE` (service cards), accent = beige.

**Two kinds of sections, alternating:**
- **Wine (dark):** radial gradient from mid burgundy at the top centre to burgundy to deep burgundy at the bottom, beige text.
- **Light:** flat light beige, burgundy text.
- Both get the **crumpled-paper grain**: an SVG noise (feTurbulence) over the whole section, `overlay` blend at 25% on dark, `multiply` at 14% on light.

**Type:**
- Headings and big numbers: **Unbounded ExtraBold (800)**, UPPERCASE, line-height .95 (1.05 in Russian so Й and Ё do not clash), letter-spacing −0.02em.
- Text: **Manrope** (400 to 800). Labels like "Client / The idea / What I did" are Unbounded 800 in normal case.
- Subtitles use Unbounded 600 (`serif-i` class). Logo: "Yeva Soroka" Unbounded 20px, tight letter-spacing, with "WEB DESIGN" under it in small spaced capitals (14px, letter-spacing .25em).

**Shapes:**
- Everything small is a **pill** (radius 99px): buttons, tags, language switch, prices, counters.
- Cards use `--radius`: 24px at 1920 (it scales down).
- **Buttons:** 1.5px beige border. The solid one is beige with burgundy text and turns transparent only while the cursor is on the button itself. The ghost one has a beige outline and gets a faint beige tint on hover. Hover lifts the button 2px.
- **Tags:** thin beige outline (about 55% opacity), 18px text.
- **Number badges** ("01", "02", "03"): Unbounded, beige 2px outline pill, tilted −8°.

**Cards and blocks:**
- **How I work:** solid burgundy cards, big beige Unbounded number, step title in the same font and size as the service names (Unbounded 800, 32px at 1920, normal case), 24px text. Spotlight on hover (the card under the mouse is bright and slightly bigger, the others dim). On phones it follows the scroll instead.
- **Services:** cream cards with a soft burgundy shadow and a ✦ star icon; the middle "Multi-page website" card is the burgundy "featured" card. Big Unbounded price at the bottom.
- **Terms:** four full-width numbered rows with thin beige lines between them (no cards, her choice "option 1", Nov 2026): number 01–04 left, title in the middle (32px), text right (24px). On phones the number and title share one line and the text sits under them.
- **About:** the photo with a soft shadow and a burgundy pill tag "hi, I'm Yeva" tilted −4°, overlapping the photo corner. Chips are pills: the first one filled burgundy, the others outlined.
- **Contact:** photo in a circle with a 4px beige border and a deep shadow. QR codes sit on beige rounded squares, exactly as wide as their buttons.

**Project mockups (the signature of v1):**
- Two **browser windows** (beige title bar with three burgundy dots, rounded, big shadow) tilted a few degrees in opposite directions, overlapping, plus a **phone** (thick near-black frame `#1a0306`, rounded) on top. Back window ≈3° (Olha) or −3° (LightUp), front window ≈−2° or 2°.
- Hover: the screen under the cursor comes to the front and grows (`scale` 1.05, the tilt does not change, so the hover area never flickers).
- **Reels phones:** photos with a 6px near-black frame and rounded corners, alternately tilted −2° and 2° (every even one also lower by 16px), a caption pill on the bottom edge (beige, burgundy Unbounded text, tilted −3°). Hover straightens and enlarges a phone.

**Hero photo:** her cut-out portrait is centred and overlaps "PORTFOLIO"; the bottom of the photo fades out with a gradient mask.

**Motion:** content fades and rises 28px when it scrolls into view (`.reveal`, 0.8s). Smooth scrolling. The nav is transparent at the top and turns into a dark blurred bar after 40px of scrolling. Respect `prefers-reduced-motion` (animations off).

**Changed from v1 on purpose (do not bring back):** the timeline line with dots in How I work; the narrow 1240px centred column and the huge 72 to 140px gaps (now the 50px/30px grid); small cards in 2 rows of 3 instead of 6 tall cards; the "PROJECTS" counter "01 / 03" (now just the number badge); text-transform on project names stays uppercase.

## Page structure (approved)
1. **Hero** (Frame 9): "PORTFOLIO" fills the width between the margins, photo in the middle, offer text and buttons bottom-left, product pills bottom-right.
   - Slide-over effect: hero and first project sit in `.stack` (200svh) with a sticky `.pin`. The first project slides up over the hero from the very first scroll and is done after one screen of scrolling (rounded 48px top corners). Only on windows ≥1201px wide and without "reduce motion". Links to `#projects` scroll to the end of the stack.
2. **Projects**, in order of size: **01 The LightUp Agency** (landing page; text left, mockups right), **02 Reels & creatives**, **03 Olha's Beauty** (multi-page website; mockups left, text right).
   - Text block: tilted number badge, name in capitals, type subtitle, Client / The idea / What I did, tags, "Visit website" button. Text and pictures sit close together.
   - Reels page: title "REELS & CREATIVES" at 104px, centred, with lines, badge "02" at the left, one-line description, four big tilted phones with caption pills.
3. **How I work**: 2 rows of 3 burgundy cards, no timeline, spotlight hover animation kept (on phones: one column of long cards, no dimming).
4. **Services**, then 5. **Terms** (Services comes first; the menu has the same order).
6. **About**: photo 3:4 (head not cut), two paragraphs, straight left edge and uneven right edge.
7. **Contact**: centred heading "Let's work together", left text with WhatsApp and Instagram buttons (QR codes exactly as wide as the buttons), big circle photo close to the text on the right. No frame around the text.
8. **Footer** (Nov 18 2026, her request): ink background. The logo "Yeva Soroka / WEB DESIGN" is huge: on a computer about 70% of the footer width (`font-size: calc(100cqw * .138 * .7)`, centred, "a bit smaller than full width" was her wish), on a phone the full width (`calc(100cqw * .138)`; .138 = a little under 100 / the width of "Yeva Soroka" at 100px in Unbounded 800, remeasure if the font or letter spacing changes). Under it: Menu on the left and Contact on the right (links named just "Email" with `mailto:1evasoroka1@gmail.com`, WhatsApp, Instagram, and Telegram in the Russian version only via `.tg-only`), a thin line, then the copyright on the left and Imprint / Privacy on the right. No tagline (she removed it, the hero already says it). On phones Menu and Contact sit side by side. Texts: keys `foot.menu`, `foot.contact`, `footer`, `foot.imprint`, `foot.privacy`. `imprint.html` and `privacy.html` keep their own small footers.

## Effects
- **Star cursor**: a four-point star replaces the mouse cursor on mouse devices. Always beige (thin dark hairline edge), no size change on hover (it caused jumping), moves via `transform` with light smoothing. Phones and tablets keep the normal cursor.

## Copy rules
- **No full stop at the end of any text**, paragraph, button or caption (a dot is bad tone), in EN and RU, both in the HTML defaults and in `T`. Keep `?`, `!` and emoji. Dots between two sentences inside a paragraph are still there (ask her if she wants them changed).
- Russian copy must sound **natural**; she dictates corrections, so apply them exactly and keep EN and RU matching in meaning.
- Business facts on the site: landing pages from $300, multi-page websites from $500, **reels editing from $15** (complex reels with graphics from $30), ad creatives and presentations on request. The hero says "businesses and experts" (not "small businesses").
- How I work = 6 steps: Contact (her idea, a call), Offer, Structure (she writes texts only if the client needs), Moodboard (style, colours, draft of the first pages), Website, Revisions (up to 3 big rounds, then the project ends).
- Terms = 4 numbered rows, each with **one heading and text** (no separate subtitle): "Everything agreed" / "2–3 rounds of revisions" / "100% prepayment" (can be split into 2–3 payments for long projects) / "Changes after the project" (paid separately, depending on the task).
- Olha's Beauty "What I did": AI content generation (images and video, mostly re-made existing visuals); the tag says "AI content generation".

## Checklist before saying "done"
- Measure every section at 1920×1080, 1470×825, 1440×900, 1366×768 and 1280×720, in **EN and RU**: no section taller than the window. Russian text is longer and breaks layouts first.
- Look at screenshots of the changed pages; check the phone layout (<860px) too.
- Do not touch v1 or v2. Keep memory notes (`design-rules-v3`) in sync with changes.

## Pre-deploy audit (done Nov 2026)
- Head has: title, description (EN/RU switch with the language), canonical, Open Graph and Twitter tags, theme colour, favicon set, GA4 and Meta Pixel comment stubs. The site address in canonical, og:url, og:image and twitter:image is `https://soroka-webdesign.vercel.app` (change all four if a custom domain is connected). Share image: `img/og-image.jpg` (1200×630).
- Images have alt, width and height (global `img { height: auto }` keeps proportions; the contact portrait needs `width: auto`), below-the-fold ones are lazy, the hero portrait is `fetchpriority="high"`. Step titles in How I work are `h3` (heading order h1 > h2 > h3). All `target="_blank"` links have `rel="noopener noreferrer"`. `robots.txt` allows everything and points to `sitemap.xml` (three pages).
- If GA4 or Meta Pixel are ever added: cookies appear, so a consent banner and an updated Privacy page are needed.

## Open items
- **Imprint and Privacy**: done (`imprint.html`, `privacy.html`, footer links). Address on them: Yeva Soroka, Webermühle 33, 5432 Neuenhof, Switzerland (postcode 5432 confirmed by her), email 1evasoroka1@gmail.com. No UID/VAT number is stated (she has not given one). Hosting is **Vercel** (named in section 3 of the privacy page, with a note that data may be processed in the USA). The pages are generated from a small script; edit the HTML files directly from now on. No cookie banner is needed (no cookies, no analytics, fonts are self-hosted).
- **GitHub**: v3 is linked to `git@github.com:1evasoroka1/Portfolio-website.git` (branch `main`, everything pushed). Never push without her go-ahead; she signs in herself (no passwords or tokens are entered by Claude).
