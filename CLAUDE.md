# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a modern portfolio website built with **Astro** (SSG framework), **React** (for interactive components), and **Tailwind CSS** with animations powered by **Framer Motion**. The site is deployed to GitHub Pages via GitHub Actions.

## Common Development Commands

```bash
# Start development server (runs on http://localhost:4321)
bun run dev
# or
npm run dev

# Build for production
bun run build
# or
npm run build

# Preview the production build locally
bun run preview
# or
npm run preview

# Run any Astro CLI command directly
bun astro [command]
```

## Project Structure

```
src/
├── pages/
│   └── index.astro          # Main portfolio page - composes all sections
├── layouts/
│   └── Layout.astro         # Base HTML layout, theme toggle setup, typography
├── components/
│   ├── HeroSection.tsx      # Hero section with profile picture and intro
│   ├── GlassHeader.tsx      # Header with navigation and theme toggle
│   ├── ExperienceSection.tsx # Work experience timeline
│   ├── SkillsSection.tsx    # Technical skills grouped by category
│   ├── EducationSection.tsx # Education timeline
│   ├── ProjectsSection.tsx  # Project showcase
│   ├── AwardsSection.tsx    # Awards and achievements
│   ├── Footer.tsx           # Footer with contact links
│   ├── TimelineItem.tsx     # Reusable timeline item component
│   ├── MotionWrapper.tsx    # Wrapper for Framer Motion animations
│   └── ui/                  # Shadcn/ui components (Button, Card, etc.)
├── lib/
│   ├── data.ts             # Single source of truth for all portfolio content
│   └── utils.ts            # Utility functions (cn() for className merging)
└── styles/
    └── global.css          # Global styles and Tailwind directives
```

## Architecture & Key Patterns

### Content Management
- **All portfolio content lives in `src/lib/data.ts`** — personal info, work experience, education, skills, projects, and awards
- Content is structured as TypeScript constants exported from data.ts
- Each section component imports only the data it needs

### Component Architecture
- **Astro handles server-side rendering** (`src/pages/index.astro`) — composes all React sections with `client:only="react"` directive (hydrates only in browser)
- **React components** handle interactivity and animations — each section is a standalone component that imports data and renders it
- **UI components** in `src/components/ui/` use Shadcn patterns with Tailwind CSS for styling
- **MotionWrapper** component provides consistent animation patterns across sections using Framer Motion

### Styling System
- **Tailwind CSS v4** with custom configuration
- **Dark mode support** built into Layout.astro with localStorage persistence
- **Radial gradient background** in Layout.astro applied to entire page
- **Glassmorphism effects** using CSS backdrop-blur for visual depth
- `cn()` utility in utils.ts merges Tailwind classes conflict-free

### Animation Strategy
- **Framer Motion** powers all animations
- Common pattern: `containerVariants` + `childVariants` for staggered animations
- Components use `whileHover`, `whileTap`, and `animate` properties for interactivity

### Path Aliases
- Use `@/*` to import from `src/*` (configured in tsconfig.json)
- Example: `import { personalInfo } from "@/lib/data"`

## Deployment

The site is deployed to GitHub Pages via GitHub Actions (`.github/workflows/deploy.yml`):
- Triggered on push to `main` branch
- Uses `withastro/action@v5` for building and deploying
- Static output configured in `astro.config.mjs` with `output: "static"`
- Site URL: https://wilmer-corrales-1992.github.io

## Common Customization Tasks

**Updating portfolio content**: Edit `src/lib/data.ts` — all sections automatically reflect changes
**Adding a new section**: Create a component in `src/components/`, add data to `data.ts`, import and add `client:only="react"` in `src/pages/index.astro`
**Styling changes**: Use Tailwind classes or add styles in component files (scoped or global in `src/styles/global.css`)
**Animation tweaks**: Modify Framer Motion variants in component files or create new motion patterns in MotionWrapper.tsx

## Dependencies Note

- Uses **bun** as the package manager (see `bun.lock`)
- Core deps: `astro`, `@astrojs/react`, `react`, `framer-motion`, `tailwindcss`, `lucide-react`
- UI library: `clsx`, `tailwind-merge`, `class-variance-authority` for component styling
