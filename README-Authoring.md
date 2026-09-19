# Authoring and Publishing the Mobile View Skill

This document provides guidance for contributors on the structure of this repository and the process for publishing it as a portable AI skill.

## Repository Structure: Source vs Deployment

You will notice that the skill is mirrored in both `skills/mobile-view/` and `.cursor/skills/mobile-view/`. This is a deliberate architectural pattern designed for portability across different AI agent harnesses.

The `skills/` directory serves as the canonical source of truth. It is a harness-independent location where the skill definition lives. This allows the skill to be discovered by various tools and agents regardless of their specific configuration.

The `.cursor/` directory is the deployment target. It is the specific location where the Cursor IDE looks for skills to provide to its agent.

By separating the source from the deployment, we ensure that:
- The skill remains portable to non-Cursor agents (e.g., those using `.agents/`).
- The `skills` CLI can predictably find the definition when installing via npx.
- We maintain a clean "source of truth" for updates.

## Contribution and Publication

To publish this skill and make it available to the broader community, follow these steps:

### 1. Publish the Repository
Push this repository to a public git host like GitHub. Ensure the `README.md` is clearly defined as the landing page so users understand the value of combined User-Agent and Viewport overrides.

### 2. Submit to Skills Directories
When submitting to an AI-Agent Registry or skills directory, link directly to the `skills/mobile-view/SKILL.md` file. Highlight that this skill solves server-side mobile detection issues (such as WordPress `wp_is_mobile`) which simple viewport resizing cannot handle.

### 3. Enable `npx skills add` Installation
To ensure the skill can be installed via `npx skills add <repo>`, the repository must include a `package.json` with the following:
- A clear `name` and `description`.
- A `files` array that includes the `skills/` directory.
- A structure where the skill resides at `skills/<skill-name>/SKILL.md`.

Once published to npm or a reachable git URL, users can install it using:
```bash
npx skills add github:username/mobile-view-skill
```

### 4. Verification
Verify the installation by running the command in a fresh project and checking if the `.cursor/skills/mobile-view/SKILL.md` file (or the equivalent for other harnesses) was correctly created.
