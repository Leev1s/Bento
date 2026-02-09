# AGENTS.md

Guidelines for agentic coding agents working on the Bento codebase.

## Project Overview

Minimalist startpage/dashboard with vanilla HTML5, CSS3, JavaScript (ES6+), Lucide Icons. No build system, no automated tests.

---

## Commands

```bash
# Development
python3 -m http.server 8000
# or
npx serve

# Build: None (static files served as-is)
# Lint: None configured
# Test: Manual testing only
#   - Open index.html in browser
#   - Test theme toggle, weather widget, links, responsive layout
```

---

## Code Style Guidelines

### JavaScript

**Indentation:** Tabs (visually 2 spaces)
**Variables:** `const` for immutable, `let` for mutable, avoid `var`
**Naming:** camelCase functions/vars (`generateButtons`), UPPER_SNAKE_CASE constants (`KELVIN`)
**Functions:** Arrow functions preferred (`const fn = () => {}`), called immediately after definition
**Strings:** Single quotes (`'hello'`), template literals for HTML
**DOM:** `getElementById()`/`querySelector()`, `insertAdjacentHTML()`, `innerText`/`innerHTML`
**Error:** `console.error()` for logging, try/catch for API calls
**Comments:** ASCII art banners (maintain if present), inline comments, all-caps section headers

### CSS

**Indentation:** 2 spaces
**Variables:** CSS custom properties (`:root { --name: value; }`)
**Naming:** kebab-case classes (`buttonsContainer`), PascalCase IDs (`linksBlockLeft`)
**Organization:** Section comments (`/* V A R I A B L E S */`), BEM-like naming (`card button`)
**Transitions:** Global `* { transition: 0.2s ease-in-out; }`, use `.notransition` to disable

### HTML

**Indentation:** Tabs (visually 2 spaces)
**Structure:** Semantic HTML5, section comments (`<!-- Clock -->`), IDs for JS hooks
**Attributes:** Self-closing without slash (`<br>`, `<img>`), double quotes, conditional attributes

---

## Project Structure

```
/
├── index.html          # Main HTML
├── config.js           # User configuration (CONFIG object)
├── app.css             # All styles + themes
├── assets/js/          # Modular JS files
│   ├── layout.js       # Layout generation
│   ├── theme.js        # Theme switching
│   ├── time.js         # Clock
│   ├── greeting.js     # Greetings
│   ├── weather.js      # Weather widget
│   ├── buttons.js      # Button generation
│   └── lists.js        # List generation
├── assets/icons/       # Icon assets
├── package.json        # Minimal info
└── Dockerfile/docker-compose.yml
```

---

## Configuration

All in `config.js`:
- General: name, 12/24h format, image background
- Greetings: time-based messages
- Layout: 'bento', 'lists', or 'buttons'
- Weather: API key, location, icon set, units
- Theme: OS-based or time-based auto-change
- Buttons/Lists: arrays with id, name, icon, link

---

## Key Patterns

**DOM Generation:**
```javascript
const generate = () => {
  for (const item of CONFIG.items) {
    let html = `<div class="card">${item.name}</div>`;
    document.getElementById('container').insertAdjacentHTML('beforeend', html);
  }
};
generate();
```

**Theme:** localStorage `'darkTheme'`, `.darktheme` class on body, call `lucide.createIcons()` after icon updates
**Conditional:** Switch statements for layouts, ternary for attributes

---

## External Dependencies

- Lucide Icons: `https://unpkg.com/lucide@latest`
- OpenWeatherMap API: weather data (requires key)
- Google Fonts: Open Sans via CSS @import

---

## Browser Compatibility

Modern browsers only (ES6+, fetch, arrow functions, const/let). No polyfills.
