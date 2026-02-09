# AGENTS.md

This file provides guidelines for agentic coding agents working on the Bento codebase.

## Project Overview

Bento is a minimalist, elegant startpage/dashboard application built with vanilla HTML, CSS, and JavaScript. It features a bento-grid layout with configurable widgets for time, weather, greetings, links, and buttons.

**Tech Stack:** Vanilla HTML5, CSS3, JavaScript (ES6+), Lucide Icons
**No Build System:** Static files served directly
**No Testing Framework:** Manual testing only

---

## Commands

### Development
```bash
# Serve locally (use any static file server)
python3 -m http.server 8000
# or
npx serve
# or open index.html directly in browser
```

### Build
No build process required. This is a static site - files are served as-is.

### Lint
No linter configured. Follow the code style guidelines below.

### Test
No automated tests. Manual testing required:
- Open index.html in browser
- Test dark/light theme toggle
- Test weather widget (requires API key in config.js)
- Test all link buttons open correctly
- Test responsive layout at different screen sizes

---

## Code Style Guidelines

### JavaScript

**Indentation:** Use tabs (file uses tabs, visually aligned as 2 spaces)

**Variable Declaration:**
- Use `const` for constants and values that won't be reassigned
- Use `let` for variables that need reassignment
- Avoid `var`

**Naming Conventions:**
- camelCase for functions and variables: `generateButtons`, `darkTheme`
- PascalCase is not used (no classes/constructors)
- Global constants in UPPER_SNAKE_CASE: `KELVIN`
- IDs from config referenced directly: `CONFIG.firstButtonsContainer`

**Function Style:**
- Arrow functions preferred: `const generateButtons = () => { ... }`
- Functions are concise and single-purpose
- Functions are called immediately after definition

**Comments:**
- ASCII art banners at top of each file (optional but maintain if present)
- Brief inline comments for complex logic
- Section comments in all-caps: `// GENERATORS`, `// CONFIG`

**String Style:**
- Single quotes for strings: `'hello'`
- Template literals for multi-line HTML: `` `<div>${content}</div>` ``
- String concatenation is used occasionally

**DOM Manipulation:**
- Use `document.getElementById()` for single elements
- Use `document.querySelector()` for CSS selectors
- Use `insertAdjacentHTML()` for efficient DOM updates
- Use `innerText` for text content, `innerHTML` for HTML content

**Error Handling:**
- Minimal error handling in current codebase
- Use `console.error()` for logging errors
- Try/catch for API calls (weather.js has fetch chain)

### CSS

**Indentation:** 2 spaces (app.css uses 2 spaces)

**Variables:** Use CSS custom properties for theming
```css
:root {
  --variable-name: value;
}
```

**Naming:**
- kebab-case for class names: `buttonsContainer`, `darktheme`
- PascalCase for IDs: `linksBlockLeft`, `themeButton`
- Prefix variables by category: `--fg-` for foreground, `--bg-` for background

**Organization:**
- Comment sections: `/* V A R I A B L E S */`, `/* S T Y L E S */`
- Group related properties together
- Use BEM-like naming for components: `card button`, `listItem`

**Transitions:**
- Global transition on `*` selector: `transition: 0.2s ease-in-out`
- Use `.notransition` class to disable transitions when needed

### HTML

**Indentation:** Tabs (aligned visually as 2 spaces)

**Structure:**
- Semantic HTML5 tags where appropriate
- Comments to mark sections: `<!-- Clock and Greetings -->`
- IDs for JavaScript hooks: `hour`, `minutes`, `themeButton`

**Attributes:**
- Self-closing void elements without slash: `<br>`, `<img>`
- Double quotes for attribute values
- `target="_blank"` conditionally based on CONFIG.openInNewTab

---

## Project Structure

```
/
├── index.html          # Main HTML file
├── config.js           # User configuration (all customizable settings)
├── app.css             # All styles including themes
├── assets/
│   ├── js/             # Modular JavaScript files
│   │   ├── layout.js   # Layout generation
│   │   ├── theme.js    # Theme switching logic
│   │   ├── time.js     # Clock functionality
│   │   ├── greeting.js # Greeting messages
│   │   ├── weather.js  # Weather widget
│   │   ├── buttons.js  # Button card generation
│   │   └── lists.js    # List card generation
│   ├── icons/          # Icon assets
│   └── background.jpg  # Background image (optional)
├── package.json        # Minimal package info
├── Dockerfile          # Docker configuration
└── docker-compose.yml  # Docker compose configuration
```

---

## Configuration

All user-facing configuration is in `config.js`. The `CONFIG` object contains:
- General settings: name, theme options, 12/24h format
- Greeting messages for different times of day
- Layout selection: 'bento', 'lists', or 'buttons'
- Weather API key, location, icon set, units
- Theme auto-change settings (OS-based or time-based)
- Button containers with id, name, icon, link
- List containers with icon and nested links array

---

## Key Patterns

### DOM Generation
```javascript
const generateComponent = () => {
  for (const item of CONFIG.items) {
    let html = `<div class="card">${item.name}</div>`;
    document.getElementById('container').insertAdjacentHTML('beforeend', html);
  }
};
generateComponent();
```

### Theme Management
- Theme state stored in localStorage as `'darkTheme'`
- Use `.darktheme` class on body for dark mode
- Call `lucide.createIcons()` after updating icon HTML

### Conditional Rendering
- Switch statements for layout types
- Ternary operators for attributes: `target="${CONFIG.openInNewTab ? '_blank' : ''}"`

---

## External Dependencies

- **Lucide Icons:** Loaded from CDN: `<script src="https://unpkg.com/lucide@latest"></script>`
- **OpenWeatherMap API:** For weather data (requires API key)
- **Google Fonts:** Open Sans font loaded from CSS @import

---

## Browser Compatibility

Modern browsers only (ES6+ features: arrow functions, const/let, template literals, fetch API). No polyfills included.
