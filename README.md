# WDG Website Builder

A [Claude](https://claude.ai) skill for building professional client websites and web apps, from brief to deployed code. Made by **Arshia Moradi** for WDG, part of MORA.

## What it does

When you ask Claude to design, build or change a website, this skill makes it work like a senior designer-developer:

1. **Understand the brief:** the business, the one action a visitor should take, content, language (LTR/RTL) and how alive the site should feel.
2. **Choose the mode:** every project is React with Next.js, either a static export (small marketing sites) or a full app (accounts, payments, dashboards, AI).
3. **Read only the parts it needs:** frontend, API, backend, database and security, changes, deploy, motion and inspiration.
4. **Plan, check, build, review:** with a quality checklist (mobile widths, every button working, accessibility, no "AI-made" look).
5. **Deliver:** with a note on placeholders, the accounts the client needs, and a DEPLOY guide.

## Stack it uses

Next.js (App Router, TypeScript) · CSS Modules + design tokens · Motion for React / GSAP · Zod · Better Auth · PostgreSQL with Drizzle or MongoDB with Mongoose · Stripe Checkout · Resend · Claude API · Vercel + Neon.

## Install

- **Claude app:** upload `SKILL.md` (or a zip of this folder) in the Skills section of your Claude settings.
- **Claude Code:** copy this folder to `~/.claude/skills/wdg-website-builder/`.

Then ask, for example: *"Build a website for a bakery in London with online orders."*

## Built with it

- MORA website and MORA Store (Apple-style, accounts, bag, bookings, WDG SI assistant).

## License

MIT © Arshia Moradi
