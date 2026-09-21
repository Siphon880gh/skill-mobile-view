# Mobile View Skill
![Last Commit](https://img.shields.io/github/last-commit/Siphon880gh/skill-mobile-view/main)
<a target="_blank" href="https://github.com/Siphon880gh" rel="nofollow"><img src="https://img.shields.io/badge/GitHub--blue?style=social&logo=GitHub" alt="Github" data-canonical-src="https://img.shields.io/badge/GitHub--blue?style=social&logo=GitHub" style="max-width:8.5ch;"></a>
<a target="_blank" href="https://www.linkedin.com/in/weng-fung/" rel="nofollow"><img src="https://img.shields.io/badge/LinkedIn-blue?style=flat&logo=linkedin&labelColor=blue" alt="Linked-In" data-canonical-src="https://img.shields.io/badge/LinkedIn-blue?style=flat&amp;logo=linkedin&amp;labelColor=blue" style="max-width:10ch;"></a>
<a target="_blank" href="https://www.youtube.com/@WengTeachesCode/" rel="nofollow"><img src="https://img.shields.io/badge/Youtube-red?style=flat&logo=youtube&labelColor=red" alt="Youtube" data-canonical-src="https://img.shields.io/badge/Youtube-red?style=flat&amp;logo=youtube&amp;labelColor=red" style="max-width:10ch;"></a>

By Weng (Weng Fei Fung).

The `mobile-view` skill simulates a genuine mobile browsing environment for AI coding agents for ADA checks, responsiveness checks, and screenshotting. Unlike simple viewport resizing, it overrides the **User Agent**, **Viewport**, **Device Pixel Ratio**, and **Touch Emulation**.

## Use cases

- **Accessibility / ADA-oriented mobile review:** Ask an agent to critique whether buttons, links, and other controls leave enough room for a thumb. For example, it can check against WCAG 2.2's 24 × 24 CSS-pixel minimum target-size requirement (including its spacing allowance).
- **Screenshot-based visual QA:** Have the agent take mobile screenshots to document issues such as clipped content, overlapping controls, or hard-to-read layouts. Note it requires your AI already has browser tab and screenshot skills (eg. Cursor).
- **Responsive checks before deployment:** Review the genuine iPhone or Android experience locally, without deploying a build and manually checking it on a phone.

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
