# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static multi-page site for **Успевайка** (Uspevaika) — a Russian-language online tutoring center for students in grades 1–11. Deployed to GitHub Pages under the custom domain `uspevaika-online.ru` (see `CNAME`).

## Working with the site

There is no build system, package manager, or dependencies. Every page is a hand-written, self-contained HTML file in the repo root.

- **Edit** the HTML files directly.
- **Preview** by opening a file in a browser (`open index.html` on macOS, or drag it into a tab). All links are relative, so `file://` works.
- **Deploy** by pushing to `main` — GitHub Pages serves the result automatically. По указанию владельца сайта: закончил правку — сразу сливай её в `main` и пушь, не оставляй изменения только в рабочей ветке.

## Files

| File | Purpose |
|------|---------|
| `index.html` | Landing page (the only page with the enrollment form) |
| `games.html` | Catalog of educational games with subject filters |
| `game-*.html` | One educational game per file (see below) |
| `podgotovka-k-shkole.html` | SEO landing page: teaching a child to read |
| `rabota-repetitorem.html` | Tutor vacancies page |
| `404.html` | Custom 404 page (`noindex`) |
| `sitemap.xml`, `robots.txt` | SEO; `sitemap.xml` must list every public page |
| `images/` | Logo, mascots, photos — **referenced by relative path** and served as-is |
| `favicon-32.png`, `apple-touch-icon.png` | Icons |

Each page has its own inline `<style>` block (no shared stylesheet), its own copy of the nav and footer, and its own `<script>`. Footer: dark pages show the logo on a white rounded badge (the logo lettering is dark and invisible on the dark footer) with a short tagline; `game-english.html` and `game-vowels.html` have a light footer. Shared pieces (nav, footer, design tokens, burger menu script) are **duplicated** across pages — when changing the nav, footer, phone number or social links, update every page.

External resources: Google Fonts only (`Nunito`, `Rubik`, `Comfortaa`).

## Landing page sections (`index.html`)

Navigation uses smooth-scroll anchors. Sections in order:

| ID | Content |
|----|---------|
| `#hero` | Brand headline, CTA, mascot |
| `#story` | Brand story |
| `#loyalty` | Why families stay for years |
| `#lessons` | How lessons go |
| `#teachers` | Tutor team |
| `#pricing` | Prices (Стандарт / Премиум, 40 and 60 minute lessons) |
| `#subjects` | Subjects offered |
| `#games` | Teaser cards linking to `games.html` |
| `#faq` | FAQ |
| `#contact` | Social links + enrollment form |
| `#join` | Tutor recruiting block (links to `rabota-repetitorem.html`) |

## Design system (CSS custom properties)

Defined on `:root` and repeated in every page:

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

CSS class naming follows BEM on the landing page (e.g. `.nav__logo`, `.hero__content`). Game pages use short prefixes per game family (`.gp-*` page chrome, `.cn-*` shared win/start screens, `.sc-*` / `.mn-*` game-specific).

## Educational games

Each `game-*.html` is a standalone page: nav, hero with breadcrumb, game board, rules/tips section, CTA, footer, and a `<script>` IIFE with the game logic. Current games:

| File | Game |
|------|------|
| `game-vowels.html` | Гласные и согласные |
| `game-consonants.html` | Домики согласных |
| `game-stress.html` | Ударения в словах |
| `game-yot.html` | Йотированные гласные |
| `game-roots.html` | Однокоренные слова |
| `game-speech.html` | Части речи |
| `game-english.html` | Английские слова |
| `game-count.html` | Счёт до 100 |
| `game-times.html` | Таблица умножения |
| `game-truth.html` | Правда или ложь? (сумма, разность, слагаемые) |
| `game-components.html` | Компоненты сложения и вычитания |
| `game-compose-10.html` | Состав числа от 6 до 10 |
| `game-compose-20.html` | Состав числа в пределах 20 |
| `game-minuend.html` | Найди уменьшаемое (и вычитаемое / разность), числа до 20 |
| `game-twins.html` | Двойняшки: состав чисел 12, 14, 16, 18 из одинаковых слагаемых (6+6 … 9+9). Три уровня: раздать угощение двум медвежатам поровну / пример «? + ? = 14» и «7 + 7 = ?» / найти двойняшку среди ловушек (сумма та же, слагаемые разные); префикс `.tw-*` |
| `game-actions.html` | Кто что делает? Чтение и речь, в том числе для билингвов: 20 рисованных картинок (`images/act-*.webp`), три уровня (действие / предложение / предмет), ударения и озвучка; префикс `.ac-*` |
| `game-clothes.html` | Одежда и цвета. Для билингвов: 16 картинок одежды (`images/clo-*.webp`), три уровня (название одежды / цвет в нужном роде и числе / фраза целиком вроде «красное платье»), ударения и озвучка; префикс `.cl-*` |

Conventions when adding a game — copy the closest existing game page and change:

1. `<title>`, meta description, canonical, `og:*`/`twitter:*` tags and the JSON-LD (`LearningResource` + `BreadcrumbList`).
2. Hero text, game markup and script. Keep pure game logic (generators, answer checking) between `/*LOGIC-START*/` and `/*LOGIC-END*/` markers where present, so it can be extracted and tested in Node.
3. Register the game in **`games.html`** (a `.gh-card` in the grid with `data-subject` plus a `ListItem` in its JSON-LD) and in **`sitemap.xml`**.

The win overlay (`#cnOverlay`, confetti canvas `#cnCf`) and the `.cn-*` start/win styles are shared by copy-paste between games.

## Enrollment form (`index.html`)

`#contactForm` collects: child's name, parent's phone/Telegram, grade, and subject. On submit it builds a pre-filled message and opens a messenger-choice modal (`#messengerModal`): WhatsApp opens `api.whatsapp.com/send?phone=79097923750&text=…` with the message; Telegram, VK and Instagram open the profile and copy the message to the clipboard. There is no server-side handler.

Contacts: phone / WhatsApp `+7 909 792-3750`; Telegram `@uspevaika` (channel link in the contact block and footer) and `@svetapedagog` (hero button and Telegram option in the form modal); VK `vk.ru/club.uspevaika`; Instagram `instagram.com/sveta.pedagog`. YouTube-кнопки в подвалах убраны намеренно (канал `@uspevaika` остался только в `sameAs` разметки на главной).

## Content language

All visible content is in Russian (Cyrillic). Preserve this when making edits; do not translate or transliterate content. Page `<html lang>` is `ru`.
