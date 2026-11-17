# UI/UX standard

**In short:** a non-designer agent can follow this checklist to ship screens that are clear, calm and work on every width. Plan with the checklist, build, then test in a real browser at phone, tablet and desktop widths and look at every screenshot. A passing build is not proof of good design.

## Before you build
- Use an installed frontend design skill or your team's design system, if you have one. Reuse existing components and tokens before adding new ones.
- Write down: who uses this screen, the one main task, and what they must see first.
- For a new look, make 2 to 4 quick throwaway mockups and let the human pick before you write real code.

## Checklist
### Layout and space
- [ ] One clear main action per screen. It is the most visible element.
- [ ] Spacing uses one scale (for example 4, 8, 12, 16, 24, 32 px). No random values.
- [ ] No box inside a box inside a box. No repeated headings. No four blocks saying the same thing.
- [ ] No large empty areas and no oversized widgets. Content is aligned to a grid.
- [ ] Related controls are grouped. Secondary information is smaller or behind a click.

### Typography
- [ ] At most 2 font families. A clear size scale (for example 12, 14, 16, 20, 24, 32).
- [ ] Body text at least 16 px on phones. Line length about 50 to 80 characters.
- [ ] Headings say what the section is for, in plain words.

### Colour
- [ ] Text contrast meets WCAG AA (4.5:1 for body text, 3:1 for large text).
- [ ] Red only for real errors. Green for success. Do not reuse one colour for two meanings.
- [ ] Never colour alone to carry meaning; add an icon or text.
- [ ] Colours come from defined tokens. Dark mode, if supported, uses the same tokens.

### Controls and states
- [ ] Every list and panel has an empty, loading and error state.
- [ ] A disabled button says why it is disabled.
- [ ] Icon-only buttons have a label or tooltip and an accessible name.
- [ ] Touch targets at least 44 by 44 px. Visible focus outline for keyboard users.
- [ ] Destructive actions ask for confirmation. Long actions show progress.
- [ ] Forms validate inline and keep the user's input after an error.

### Navigation and consistency
- [ ] The current page is highlighted in the navigation.
- [ ] Multi-step flows have back and next, and going back keeps the data.
- [ ] The same job uses the same pattern everywhere (not a popup here and a full page there).
- [ ] Settings, filters and open panels survive a refresh where users expect it.

### Clarity for non-technical people
- [ ] A stranger knows what to do on this screen in 5 seconds.
- [ ] Copy uses plain words. No internal jargon, IDs or backend terms.
- [ ] Order dependencies are visible (for example, choose a country before a region).
- [ ] Numbers say what they count.

### Responsive
- [ ] Works at 390 px (phone), 820 px (tablet) and 1440 px (desktop).
- [ ] No horizontal page scroll. No clipped or overlapping text.
- [ ] Tables become scrollable or stacked on phones. Images scale.

## Browser test procedure
Use a real browser: Playwright (headless, scripted) for repeatable checks, or Claude in Chrome for a live look in a signed-in browser.
1. Start the app locally (`<DEV_COMMAND>`). Use test accounts and seeded data, never real customer data.
2. For each changed page, at 390, 820 and 1440 px wide:
   - Load the page. Record console errors and failed network requests.
   - Click every control on the page, except destructive ones (delete, pay, send, logout) unless on a disposable test record.
   - Flag: dead clicks, console errors, 4xx or 5xx responses, horizontal scroll, clipped text, elements outside the viewport.
   - Save a full-page screenshot.
3. Open and look at every screenshot yourself. Score it against the checklist above. Write each failed item down.
4. Fix, rerun, repeat until clean, or explain each remaining item in the report.
5. If the change touches data two roles see (for example customer and admin), check both sides.
6. Report: the command, exit code, issue count, screenshot folder, and pass or fail per page and width.

A minimal Playwright loop to start from:
```js
// ui-check.mjs: node ui-check.mjs http://localhost:3000 / /settings
import { chromium } from 'playwright';
const [base, ...pages] = process.argv.slice(2);
const browser = await chromium.launch();
for (const width of [390, 820, 1440]) {
  const page = await browser.newPage({ viewport: { width, height: 900 } });
  page.on('console', m => m.type() === 'error' && console.log(`[${width}] console: ${m.text()}`));
  page.on('response', r => r.status() >= 400 && console.log(`[${width}] http ${r.status()} ${r.url()}`));
  for (const p of pages) {
    await page.goto(base + p, { waitUntil: 'networkidle' });
    const overflow = await page.evaluate(() => document.documentElement.scrollWidth > innerWidth);
    if (overflow) console.log(`[${width}] ${p}: horizontal scroll`);
    await page.screenshot({ path: `shot-${width}${p.replace(/\//g, '_')}.png`, fullPage: true });
  }
  await page.close();
}
await browser.close();
```

## Rules
- Never write "verified" for UI without screenshots from this run, looked at in this turn.
- A text-only copy fix needs one screenshot of the changed spot, not the full run.
- Default to dense and calm. Save bold "wow" styling for landing or marketing pages the human names.
