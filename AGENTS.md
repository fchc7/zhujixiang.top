# AGENTS.md

This file contains conventions and guidelines for agentic coding agents working on this repository.

---

## Commands

### Development
- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build

### Testing
No test framework is currently configured in this project.

---

## Tech Stack
- **Framework**: Astro 5+ with TypeScript
- **Styling**: Tailwind CSS v4
- **Package Manager**: pnpm
- **Content**: Astro Content Collections

---

## Code Style Guidelines

### Imports
- Use explicit imports with ES modules
- Import layouts first in page files: `import Layout from "../layouts/Layout.astro"`
- Group Astro imports at the top: `import { getCollection } from "astro:content"`
- Import from `simple-icons` directly: `import { siGithub } from "simple-icons"`

### Formatting & Structure
- Use Astro frontmatter `---` sections for component logic
- Place all logic in frontmatter, template below
- Use TypeScript strict mode (configured in tsconfig.json)
- Keep components small and focused

### Naming Conventions
- **Components**: PascalCase (PostCard, Layout, Header)
- **CSS classes**: kebab-case (.article-header, .meta-row)
- **Variables/Functions**: camelCase (isActive, splitLastN, dateObj)
- **CSS variables**: double-dash prefix (--brand, --content-w, --header-height)
- **Constants**: camelCase or UPPER_CASE for truly global constants

### TypeScript
- Use type annotations for component props with `interface Props`
- Use type guards: `const dateObj = typeof date === "string" ? new Date(date) : date`
- Define content collection schemas with Zod: `z.object({...})`
- Use generic types from Astro: `getCollection<T>()`

### Styling Conventions

#### Tailwind CSS v4
- Import at top of global.css: `@import "tailwindcss";`
- Custom dark variant: `@custom-variant dark (&:where(.dark, .dark *));`
- Use Tailwind utilities primarily
- Prefer arbitrary values for consistent spacing: `w-16 h-16`, `gap-5`

#### Scoped Styles
- Use `<style>` blocks in components for component-specific styles
- Use `:global()` to escape Astro scoping when needed (e.g., `:global(.dark)`)
- Group related styles together
- Use CSS variables for theme-sensitive values

#### Dark Mode
- Class-based dark mode with `.dark` class on `html` element
- Define mode-specific colors in global.css under `.dark`
- Always use CSS variables for theme colors: `color: var(--brand)`
- FOUC prevention: inline script in `<head>` before any content

#### CSS Variables
Define core variables in `:root`:
```css
--brand: #991b1b; /* Primary brand color */
--content-w: min(100%, calc(100vw - 2rem)); /* Content width */
--header-height: 7rem;
--header-height-shrunk: 56px;
```

#### Performance-First Animations
- Use `transform` (scaleX, translate) instead of width/height/top/left
- Use `requestAnimationFrame` for scroll-linked animations
- Use `IntersectionObserver` for scroll state detection (not scroll events)
- Use pseudo-elements (::before/::after) with `inset` for hover backgrounds
- Transition timing: `cubic-bezier(0.4, 0, 0.2, 1)`
- Transition duration: 0.2s for layout shifts, 0.3s for color/opacity

### Layout Architecture

#### Grid Layout Pattern
Three-column responsive grid for main content:
```html
<div class="grid grid-cols-[minmax(0,0fr)_minmax(0,var(--content-w))_minmax(0,0fr)] md:grid-cols-[1fr_minmax(0,var(--content-w))_1fr]">
  <div></div>
  <div class="min-w-0">Content</div>
  <div></div>
</div>
```

#### Toolbar Area
Fixed vertical line at `calc(50% + min(calc(var(--content-w)/2), 50vw))`
This area contains auxiliary tools: Theme Toggle, Back to Top, Scroll Progress

### Error Handling
- Wrap localStorage access in try-catch (may not be available)
- Use optional chaining: `document.getElementById('favicon')?.href`
- Use type guards for runtime type checking

### Event Handling
- Use `<script>` + `addEventListener` instead of inline `onclick`
- Group similar interaction logic together
- Search and update similar patterns when refactoring
- Clean up listeners when needed

### Static Rendering First
- Prefer server-side rendering and static generation
- Add client-side JavaScript only when necessary
- Use `is:inline` on scripts that must execute before content renders

### Code Principles
1. **Concise and readable** - Avoid verbosity, every line should earn its place
2. **Static rendering first** - Minimize client-side JavaScript
3. **Best engineering practice** - Add event listeners properly, search patterns together

### Content Collections
- Define schemas in `src/content/config.ts` with Zod
- Use `getCollection("posts")` to fetch content
- Filter drafts: `getCollection("posts", ({ data }) => !data.draft)`
- Sort by date: `sort((a, b) => b.data.date.getTime() - a.data.date.getTime())`
