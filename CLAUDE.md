# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static single-page landing site for **Успевайка** (Uspevaika) — a Russian-language online tutoring center for students in grades 1–11. Deployed to GitHub Pages under the custom domain `uspevaika-online.ru`.

## Working with the site

There is no build system, package manager, or dependencies. The entire site is one file:

- **Edit** `index.html` directly.
- **Preview** by opening it in a browser: `open index.html` (macOS) or just drag-and-drop the file into a browser tab.
- **Deploy** by pushing to `main` — GitHub Pages serves the result automatically.

## Architecture: monolithic single file

`index.html` (476 lines) contains everything in one file:

1. **CSS** — all styles in a single `<style>` block inside `<head>`. No external stylesheet.
2. **Images** — all 7 mascot/logo images are embedded as `base64` data URIs directly in the HTML. Source image files live in `/images/` and are the originals used to generate the base64 strings. When replacing an image, update the base64 string in the HTML; the files in `/images/` are not served.
3. **JavaScript** — one `<script>` block at the bottom of `<body>`, handling only the mobile burger-menu toggle and the contact form.
4. **External resources** — Google Fonts only (`Nunito`, `Rubik`, `Comfortaa`).

## Page sections (navigation anchors)

Sections appear in this order and are navigated via smooth-scroll anchor links:

| ID | Content |
|----|---------|
| `#hero` | Brand headline, CTA, mascot photo |
| `#why` | Value propositions |
| `#selection` | How tutors are selected |
| `#features` | Feature highlights |
| `#results` | Student results / outcomes |
| `#pricing` | Pricing table (Стандарт / Премиум / Группа) |
| `#subjects` | Subjects offered |
| `#contact` | Social links + enrollment form |

## Design system (CSS custom properties)

All colors are defined as variables on `:root`:

| Token | Value | Role |
|-------|-------|------|
| `--sky` | `#7BBFD4` | Primary brand color |
| `--sky-light` | `#C8E8F4` | Borders, light backgrounds |
| `--sky-pale` | `#DCF0F8` | Subtle fills |
| `--yellow` | `#F5D76E` | Accent / highlight |
| `--yellow-dark` | `#E8C84A` | Hover state for yellow |
| `--coral` | `#FF8A7A` | Secondary accent |
| `--green` | `#6BC5A0` | Success / positive |
| `--brown` | `#6B4E3D` | Warm accent |
| `--ink` | `#2C3E50` | Body text |
| `--muted` | `#6B7C8D` | Secondary text |
| `--bg` | `#F0F9FF` | Page background |

CSS class naming follows BEM convention (e.g. `.nav__logo`, `.hero__content`, `.contact-link__icon`).

## Contact form behavior

The enrollment form (`#contactForm`) collects: child's name, parent's phone/Telegram, grade, and subject. On submit it builds a pre-filled WhatsApp message and opens `api.whatsapp.com/send?phone=79097923750` — there is no server-side handler.

Social links: Telegram `@uspevaika`, WhatsApp `+7 909 792-3750`, VK `vk.com/uspevaika`, YouTube `@uspevaika`, Instagram `instagram.com/uspevaika`.

## Content language

All visible content is in Russian (Cyrillic). Preserve this when making edits; do not translate or transliterate content.
