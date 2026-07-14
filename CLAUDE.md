# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page architecture/design portfolio for Shun-Pin Tseng, deployed via **GitHub Pages** to the custom domain `shunpintseng.com` (see `CNAME`). It is built on the third-party **"Aria" Bootstrap 4 landing page template by Inovatik** (Jul 2019), which has been heavily customized into a portfolio.

There is **no build system, package manager, or test suite** — despite the generic Node-flavored `.gitignore`, this is plain static HTML/CSS/JS. Editing means editing files directly; deploying means pushing to the default branch (GitHub Pages serves it).

## Running / previewing locally

Open `index.html` directly, or serve the folder statically (e.g. `python -m http.server`). Note: the contact forms POST to the `php/` handlers via AJAX, which require a PHP host — they will not function under GitHub Pages or a plain static server. On the live site the forms are effectively non-functional unless the host runs PHP.

## Architecture

Essentially everything user-facing lives in **`index.html`** (~2000 lines, single page). The rest is vendored template assets plus a few custom additions.

- **`js/scripts.js`** is the only hand-written JS and wraps two concerns:
  - The template's original jQuery IIFE (`(function($){...})(jQuery)`): preloader, navbar collapse-on-scroll, smooth page-scroll (jQuery Easing), **Morphext** rotating text, **Swiper** card slider, **Magnific Popup** lightboxes, **Isotope** project-grid filtering, CountTo counters, and the three AJAX contact forms.
  - A **custom W3Schools-style slideshow** grafted on top (`showSlides` / `plusSlides` / `currentSlide`), driving per-project image galleries via class names `mySlides` through `mySlides12`. This is separate from the template's Swiper slider and is what powers the project galleries.
- **`js/*.min.js`** are vendored libraries (jQuery, Bootstrap, Popper, Isotope, Magnific Popup, Swiper, Morphext, Easing, validator). Do not edit these.
- **`css/`**: `styles.css` holds the site's custom styling; `bootstrap.css`, `swiper.css`, `magnific-popup.css`, `fontawesome-all.css` are vendored.
- **`php/`**: three near-identical `mail()` handlers (`contactform-process.php`, `callmeform-process.php`, `privacyform-process.php`), one per form in `scripts.js`. Each echoes `"success"` or an error string that the AJAX handler checks against literal `"success"`.

### Projects grid

The portfolio grid lives in the `#projects` section. Each project is a `.element-item` whose extra classes (`design`, `coding`, `arch`, `travel`) are Isotope filter categories — the filter buttons that use them are currently commented out in the markup, so all items show. Project images live under `images/Web_<Project>/` (e.g. `images/Web_Boeing/`, `images/Web_Getty/`), named sequentially, and are wired to the `mySlidesN` galleries.

## When editing

- To add/change a project: add its images under a new `images/Web_<Name>/` folder, add an `.element-item` block in `#projects`, and (if it needs its own gallery) wire up a `mySlidesN` slideshow group consistent with the existing pattern in `index.html` + `scripts.js`.
- The `php/` handlers ship with a placeholder recipient `$EmailTo = "yourname@domain.com"` and a template subject line ("Aria landing page") — these must be updated for the forms to actually deliver mail.
- `key2025` (a private key) and `key2025.pub` are committed at the repo root. Treat the private key as compromised — do not rely on it, and flag it if the user asks about repository hygiene.
