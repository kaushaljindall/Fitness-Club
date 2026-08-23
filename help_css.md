# THE VYBE Fitness Club: HTML, CSS and JavaScript Viva Guide

This document explains the implementation currently present in this project. It is intended for viva preparation: each section describes what the code does, why it is used, and how the browser processes it.

## 1. Project Overview

THE VYBE is a static, multi-page fitness club website. It uses HTML5 for structure, CSS3 for layout and visual design, and one JavaScript file for interactive behavior.

There is currently no backend, database, framework, build process, or server-side form submission. Links and forms are frontend demonstrations.

### Files and responsibilities

| File | Responsibility |
| --- | --- |
| `index.html` | Loading screen that redirects to `home.html` after five seconds |
| `home.html` | Hero, program cards, contact strip, shared header and footer |
| `strength-training.html` | Strength details, routine, coach, and plans |
| `cardio-fitness.html` | Cardio details, schedule, coach, and plans |
| `flexibility-mobility.html` | Mobility details, sessions, coach, and plans |
| `nutrition-coaching.html` | Nutrition details, roadmap, coach, and plans |
| `contact.html` | Contact form, hours, location, contact details, and FAQs |
| `css/style.css` | Global reset, header, navigation, theme, contact strip, and footer |
| `css/home.css` | Home hero and program card styles |
| `css/program.css` | Shared styles for the four program pages |
| `css/contact.css` | Contact form, information cards, hours, and FAQ styles |
| `css/loading.css` | Loading page and progress animation |
| `script.js` | Theme toggle, mobile menu, sign-up alerts, and form behavior |
| `assets/logo.png` | Shared club logo |

## 2. How a Page Loads

1. The browser opens an HTML document.
2. The `<head>` sets character encoding, viewport behavior, title, and linked stylesheets.
3. CSS files are downloaded and combined. More specific or later rules can override earlier rules.
4. The browser builds the DOM from the HTML.
5. Images and external background images are requested.
6. The browser calculates layout and paints the page.
7. `script.js` waits for `DOMContentLoaded`, then attaches event listeners.

The normal entry point is `index.html`. Its meta refresh performs this redirect:

```html
<meta http-equiv="refresh" content="5;url=home.html" />
```

The loading page is styled by `loading.css`. `.progress-fill` animates from `width: 0%` to `width: 100%` during the same five-second period.

## 3. HTML Concepts Used

### Document foundation

- `<!DOCTYPE html>` enables modern HTML5 standards mode.
- `<html lang="en">` declares the document language for accessibility tools and search engines.
- `<meta charset="UTF-8">` sets the document character encoding.
- `<meta name="viewport" content="width=device-width, initial-scale=1.0">` makes the layout use the device width on mobile screens.
- `<title>` supplies the browser tab title.
- Relative paths such as `css/style.css` and `assets/logo.png` keep the project portable.

### Semantic layout

- `<header>` contains site-wide navigation.
- `<nav>` groups navigation links.
- `<main>` contains the primary content of a page.
- `<section>` groups related content such as a hero, programs, schedule, or FAQ.
- `<article>` represents an independent item such as a schedule card.
- `<address>` contains contact information.
- `<footer>` contains closing information and navigation.
- `<form>`, `<label>`, `<input>`, `<select>`, and `<textarea>` create the contact interaction.

Semantic elements improve readability, accessibility, SEO, and maintenance compared with using only generic `<div>` elements.

### Links and IDs

An anchor uses `href` to navigate:

- `home.html#hero` opens the home page and jumps to `id="hero"`.
- `home.html#programs` jumps to the programs section.
- `contact.html` opens the contact page.
- `mailto:` opens an email client.
- `tel:` can open a phone application on supported devices.

An `id` should identify one unique element. A class such as `.program-card` can be reused by many elements.

### Images and accessibility

Images use `alt` text to describe their purpose. Home-page program images also use `loading="lazy"`, allowing the browser to delay images outside the initial viewport.

External images are used for hero backgrounds, program cards, and coach portraits. These images require network access and can fail if a remote URL changes.

### Header and mobile navigation

The shared header includes:

- `.logo` for the brand link and image.
- `.desktop-nav` for large-screen links.
- `.mobile-menu`, implemented with native `<details>` and `<summary>`.
- `#themeToggle` for dark/light mode.

`<details>` provides a browser-managed open/closed state. CSS uses `.mobile-menu[open]` to turn the three menu bars into an X.

### Contact form

The form in `contact.html` has `id="contactForm"`. Fields use:

- `required` to prevent empty submission.
- `type="email"` for browser email validation.
- `type="tel"` for phone input semantics.
- `<label for="...">` to associate labels with controls.
- `<select>` for a fixed list of program interests.

The current form does not send data to a server. JavaScript prevents the default submit, displays an alert, and resets the form.

## 4. CSS Foundation

### Universal reset

```css
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
```

The universal selector targets every element. `::before` and `::after` include generated pseudo-elements. `box-sizing: border-box` makes declared width and height include padding and borders, making responsive sizing easier to predict.

### Global rules in `style.css`

- `html` enables smooth scrolling and sets the base font size.
- `body` sets minimum height, fonts, colors, background, line height, transitions, and `overflow-x: hidden`.
- `img, svg` are block-level, responsive, and limited to their container width.
- `a` removes the default underline and adds shared transition behavior.
- `button` inherits the site font and colors and shows a pointer cursor.
- `:focus-visible` creates a visible keyboard focus ring.

### Fixed header and stacking

The header uses `position: fixed`, `top: 0`, `left: 0`, `right: 0`, and `z-index: 1000`. It stays visible while scrolling. Fixed elements are removed from normal document flow, so `main` receives top padding of `68px` on small screens and `80px` on larger screens.

`z-index` controls stacking order when positioned elements overlap. The header and mobile menu use high values so navigation remains above page content.

### Colors and visual language

| Color | Main use |
| --- | --- |
| `#0a0b0f` | Main dark background |
| `#07080c` | Footer background |
| `#12141e` | Card surfaces |
| `#0d0f17` | Secondary sections and controls |
| `#f5c842` | Dark-theme gold accent |
| `#20b2aa` | Teal accent |
| `#e8e0d0` | Main light text on dark surfaces |
| `#9a9282` | Secondary text |
| `#f7f4ef` | Light-theme page background |
| `#ffffff` | Light-theme card surfaces |
| `#b38728` | Light-theme gold accent |

RGBA colors are used for transparent borders, glows, overlays, and gradients. `box-shadow` and `text-shadow` create depth without extra HTML elements.

### Typography

The body uses the `'Inter'` font stack. Headings and action text generally use `'Outfit'`. Fallback fonts keep the page readable if the preferred font is unavailable.

`font-size`, `font-weight`, `line-height`, `letter-spacing`, and `text-transform` establish visual hierarchy. `clamp()` is used for some headings so text scales between a minimum and maximum size.

## 5. Layout Systems

### Flexbox

Flexbox is used when items mainly flow in one direction:

- `.navbar` places logo, navigation, and theme control in a row.
- `.hero-copy` stacks hero content vertically.
- `.footer-brand` and `.footer-column` stack their contents.
- `.contact-links` and `.benefits-list` stack links or benefits.
- `.coach-card` changes from a vertical mobile layout to a horizontal desktop layout.

Important properties are `display: flex`, `flex-direction`, `justify-content`, `align-items`, `gap`, `flex-wrap`, and `flex-grow`.

### CSS Grid

Grid is used for two-dimensional layouts:

- `.programs-grid` displays program cards.
- `.stats-grid` displays program metrics.
- `.overview-grid` pairs overview text with an information card.
- `.schedule-cards-grid` displays weekly sessions.
- `.pricing-grid` displays plans.
- `.contact-layout-grid` pairs the form with information cards.
- `.footer-content` displays brand, quick links, programs, and contact columns.

`1fr` means one fractional unit of available space. `repeat(4, 1fr)` creates four equal tracks. `1.2fr 0.8fr` gives the first track 60 percent and the second 40 percent of the available fraction space.

### Responsive breakpoints

The CSS is mobile-first: base rules target small screens, then `min-width` media queries enhance the layout.

| Breakpoint | Behavior |
| --- | --- |
| Below `640px` | Single-column content, mobile menu, stacked cards |
| `640px` and above | Two-column home cards, two-column footer, two-column forms and FAQs |
| `768px` and above | Larger top offset, wider hero spacing, horizontal coach card, two-column overview |
| `992px` and above | Desktop navigation, four-column home cards, desktop contact layout and four-column footer |
| `1280px` and above | Wider spacing and taller home program images |

## 6. Page-Specific CSS

### `home.css`

- `.hero-section` creates the full-width image hero with dark overlays.
- `.hero-copy` centers text and the call-to-action.
- `.hero-cta` is the primary action link.
- `.programs-section` creates the program area.
- `.programs-grid` changes from one to two to four columns.
- `.program-card` is a clickable link to a program page.
- `.program-image` fixes the image area height.
- `.image-banner` uses `object-fit: cover` to fill its box without distortion.
- `.program-card:hover .image-banner` slightly zooms the image.

### `program.css`

This file is loaded by all four program pages.

- `.program-hero` provides the common program hero layout.
- `.strength-bg`, `.cardio-bg`, `.flexibility-bg`, and `.nutrition-bg` select different background images.
- `.program-badge` identifies the program category.
- `.hero-actions` arranges enrollment and all-program links.
- `.stats-bar-section` and `.stats-grid` show four quick metrics.
- `.overview-grid` pairs explanatory text with an overview card.
- `.benefits-list` displays checked benefits with inline SVG icons.
- `.schedule-cards-grid` and `.schedule-card` display weekly routines.
- `.coach-card` presents a coach portrait, title, and description.
- `.pricing-grid` and `.pricing-card` show three plans.
- `.pricing-card.featured` emphasizes the recommended plan.
- `.program-cta-banner` styles the final call-to-action section where present.

### `contact.css`

- `.contact-hero-banner` styles the page introduction.
- `.contact-main-wrapper` constrains and spaces contact content.
- `.contact-layout-grid` places form and information side by side on desktop.
- `.form-container` frames the form.
- `.form-row` changes from one column to two columns at `640px`.
- `.form-control` provides common input, select, and textarea styling.
- `.form-control:focus` changes the border and adds a focus shadow.
- `.info-card` presents hours and contact details.
- `.hours-table` and `.hours-row` create a table-like flex layout.
- `.faq-grid` changes from one to two columns at `640px`.
- `.faq-card` displays an individual answer.

### `loading.css`

- `.loading-card` centers loading content in a constrained panel.
- `@keyframes pulseLogo` repeatedly scales the logo slightly.
- `.progress-bar` is the progress track.
- `.progress-fill` is the animated fill.
- `@keyframes fillProgress` changes fill width from `0%` to `100%`.

## 7. Footer Implementation

The footer is shared across the six main pages and styled in `style.css`.

`.footer-content` is a responsive CSS Grid containing four logical areas:

1. `.footer-brand`: logo, brand name, tagline, and social links.
2. The first `.footer-column`: quick links.
3. The second `.footer-column`: program links.
4. The third `.footer-column`: phone, email, and address.

`.footer-accent` is decorative and uses `pointer-events: none`, so it never blocks links. `.copyright` is outside the grid and has its own top border, creating the separate lower strip.

The footer uses real links, `aria-label` values for social controls, accessible logo `alt` text, and a mobile-first grid. It becomes one column on phones, two columns at `640px`, and four columns at `992px`.

## 8. Pseudo-Classes, Pseudo-Elements and Animations

- `:hover` changes links, cards, buttons, and images on pointer interaction.
- `:focus-visible` shows keyboard focus without forcing a ring on every mouse click.
- `:focus` is used on form controls and some navigation states.
- `[data-theme="light"]` selects elements when the HTML root has the light-theme attribute.
- `.mobile-menu[open]` selects an open native `<details>` menu.
- `::before` and `::after` add overlays, section glows, and button shine effects without extra markup.
- `::marker` hides the default disclosure marker on the mobile menu summary.
- `@keyframes` defines animation stages.
- `transition` makes changes such as color, transform, border, and shadow gradual.
- `transform` creates motion without changing normal document flow.

## 9. Dark and Light Theme Flow

The theme is controlled by the `data-theme` attribute on `<html>`.

Dark mode is the default because the root has no `data-theme="light"` attribute. Light mode is activated like this:

```html
<html data-theme="light">
```

CSS selectors such as `[data-theme="light"] body` and `[data-theme="light"] .program-card` override dark values.

JavaScript stores the selected value in `localStorage` under `vybe-theme`. On page load it checks:

1. Whether `localStorage` contains `light` or `dark`.
2. The browser preference from `prefers-color-scheme: dark` when no saved choice exists.
3. The current root attribute.

Clicking `#themeToggle` removes the light attribute for dark mode or adds it for light mode, then saves the choice. `localStorage` persists the choice across pages in the same browser origin.

Both sun and moon SVGs are in the button. CSS displays the sun by default and switches to the moon in light mode.

## 10. JavaScript Explanation

Everything is wrapped in:

```javascript
document.addEventListener('DOMContentLoaded', () => {
  // setup code
});
```

This prevents queries from running before the HTML has been parsed.

### Theme logic

`document.documentElement` refers to `<html>`. `getAttribute`, `setAttribute`, and `removeAttribute` read and change `data-theme`.

`localStorage.getItem` reads a saved preference and `localStorage.setItem` writes one. `window.matchMedia` checks the operating system color preference.

### Mobile menu logic

`document.querySelector('.mobile-menu')` finds the native menu. `querySelectorAll('a, button')` finds its controls.

- Clicking a menu link or button removes the `open` attribute.
- A document click closes the menu when the click is outside it.
- Pressing Escape closes an open menu.

`event.target` identifies the clicked element, and `.contains()` checks whether it is inside the menu.

### Sign-up buttons

`querySelectorAll('#signupBtn, #mobileSignupBtn')` finds both sign-up buttons. `forEach` attaches the same click handler to each. The handler currently displays an alert because registration is not connected to a backend.

### Contact form

The script only attaches behavior if `#contactForm` exists, so the same script can be loaded on every page.

On submission:

1. `e.preventDefault()` stops the browser from reloading or sending the form to a server.
2. The name is read from `#fullName`.
3. An acknowledgement alert is displayed.
4. `contactForm.reset()` clears the controls.

## 11. Viva Questions and Answers

### Q1. Why use HTML5 semantic elements?

Semantic elements describe meaning. They improve readability, accessibility, SEO, and maintenance compared with using only generic `<div>` elements.

### Q2. Why is `main` given top padding?

The header is fixed and removed from normal document flow. Top padding creates space so the first content section is not hidden underneath it.

### Q3. What is the difference between a class and an ID?

An ID identifies one unique element and is used by links or JavaScript, such as `#contactForm`. A class can be reused by many elements, such as `.program-card`.

### Q4. Why use CSS Grid for cards?

Grid is suitable for two-dimensional layouts. It makes one, two, or four columns easy to define at different breakpoints while maintaining equal tracks and gaps.

### Q5. Why use Flexbox for navigation?

Navigation is mainly a one-dimensional row. Flexbox handles alignment, spacing, and responsive wrapping efficiently.

### Q6. What does `1fr` mean?

`fr` means fractional unit. `1fr 1fr` divides available grid space equally. `1.2fr 0.8fr` allocates 60 percent and 40 percent of the available fraction space.

### Q7. What does mobile-first mean here?

Base CSS targets small screens first. Larger layouts are added with `@media (min-width: ...)`, so the design progressively gains columns and spacing as more room becomes available.

### Q8. What is the purpose of `object-fit: cover`?

It fills a fixed image box while preserving aspect ratio. Some edges can be cropped, but the image is not stretched.

### Q9. What is a pseudo-class?

A pseudo-class selects an element based on a state, such as `:hover`, `:focus`, or `:focus-visible`, without adding another HTML class.

### Q10. What is a pseudo-element?

A pseudo-element represents a generated part of an element, such as `::before`, `::after`, or `::marker`. It is used here for overlays, decorative lines, button shine, and the menu marker.

### Q11. Why use `preventDefault()` on the form?

There is no backend endpoint. `preventDefault()` stops normal submission and allows the demonstration alert and reset behavior to run.

### Q12. How does theme persistence work?

The selected theme is stored in browser `localStorage`. On the next page load, JavaScript reads it and applies `data-theme="light"` when needed.

### Q13. Is the contact form connected to a database?

No. It is frontend-only. JavaScript shows a confirmation alert and clears the fields. A production version would send data to an API or backend after validation.

### Q14. Why use `aria-label` and `alt` text?

They provide names or descriptions for screen readers, especially when a control contains only an icon, abbreviated text, or an image.

### Q15. What is `z-index` used for?

It controls the stacking order of positioned elements. The fixed header and open mobile menu need to appear above page content.

### Q16. What are the main limitations?

- External images depend on network availability.
- Sign-up is currently an alert, not a registration workflow.
- The contact form does not persist or transmit submissions.
- There is no backend authentication or database.
- `README.md` contains some older architecture claims, so the HTML, CSS, and JavaScript source is the current authority.

## 12. Short Viva Explanation

This project is a mobile-first static fitness website built with semantic HTML5, modular CSS3, and vanilla JavaScript. `style.css` contains shared styles such as the fixed navigation, theme overrides, contact strip, and responsive footer. `home.css`, `program.css`, and `contact.css` contain page-specific styles. CSS Grid is used for card and column layouts, while Flexbox handles one-dimensional alignment. Media queries progressively enhance the layout from one column on mobile to multiple columns on desktop. JavaScript runs after `DOMContentLoaded` and manages theme persistence, the mobile menu, sign-up alerts, and frontend contact-form feedback. The site is visually responsive, but it is not connected to a backend or database.
