# Mobile View Skill

The `mobile-view` skill simulates a genuine mobile browsing environment for AI coding agents. Unlike simple viewport resizing, it overrides the **User Agent**, **Viewport**, **Device Pixel Ratio**, and **Touch Emulation**.

## Why this matters

Many websites use server-side detection (e.g., WordPress `wp_is_mobile()`) or CDN-level branching to serve different content to mobile users. If you only change the window width, the server still sees a desktop User Agent and serves the desktop version of the page, which often behaves differently or contains different markup than the actual mobile site.

By setting the User Agent **before** navigation, the AI agent ensures the server responds with the genuine mobile experience.

## Installation

### Using npx (Recommended)

You can install this skill directly into your project using the `skills` CLI:

```bash
npx skills add <repository-url-or-name>
```

### Manual Installation

1. Copy the `SKILL.md` from `skills/mobile-view/` to your agent's skill directory:
   - Cursor: `.cursor/skills/mobile-view/SKILL.md`
   - General Agents: `.agents/skills/mobile-view/SKILL.md`

## Usage

When using an agent with browser capabilities, ask it to "view the site in mobile view" or "inspect the iPhone experience". The agent will:

1. Apply the mobile preset (UA + Metrics).
2. Navigate to the URL.
3. Verify the environment using `navigator.userAgent`.

## Supported Harnesses

- **Cursor IDE Browser**: Fully supported via CDP overrides.
- **Playwright/Puppeteer**: Supported via `emulate` and `setExtraHTTPHeaders`.
- **General AI Agent Harnesses**: Any harness that supports CDP or Playwright device emulation.

## Presets Included

- **iPhone 14** (Default): Optimized for iOS Safari.
- **Pixel 7**: Optimized for Android Chrome.
- **Desktop Reset**: Quickly return to standard browsing.
