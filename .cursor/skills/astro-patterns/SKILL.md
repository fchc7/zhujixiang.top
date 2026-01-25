---
name: astro-patterns
description: Enforces code and design principles for this Astro blog. Use when editing .astro, CSS, or content. Covers layout architecture (Toolbar Area), high-performance interactions, and theme patterns.
---

# Astro Blog Conventions

## Layout Architecture: The Toolbar Area

- **Vertical Line**: The fixed vertical line (at `calc(50% + min(var(--content-w)/2, 50vw))`) is the structural anchor of the blog.
- **Toolbar Area**: The region to the right of this vertical line is defined as the **Toolbar Area**.
- **Usage**: This area is reserved for auxiliary tools and indicators (e.g., Theme Toggle, Polaris star, Scroll Progress). Tools should align or interact with the intersection point of this line and the Header.

## High-Performance Interaction Patterns

1. **State Detection**: Prefer `IntersectionObserver` over `scroll` events for triggering UI state changes (e.g., Header shrinking). Use a `sentinel` element at the top.
2. **Scroll Linking**: For continuous scroll animations (e.g., Progress Bar), use `requestAnimationFrame` to throttle updates and ensure frame-rate synchronization.
3. **GPU Acceleration**: Use `transform` (e.g., `scaleX`, `translate`) instead of `width`, `height`, or `top/left` for animations to avoid Layout/Reflow.
4. **Box Model Stability**: Use pseudo-elements (`::before`/`::after`) with `inset` to handle hover backgrounds, shadows, and expanded interactive areas. This prevents the base component from shifting during state changes.

## Theme & Styling Conventions

- **Astro Scoping**: When styles depend on classes added to `body` (e.g., `.header-shrunk`), use the `:global()` selector to ensure the styles are applied correctly across Astro's scoping.
- **Variable Usage**: Always use CSS variables (like `--brand`) for theme-sensitive colors. Define mode-specific overrides in `global.css` under `.dark`.
- **Restrained Motion**: Use `cubic-bezier(0.4, 0, 0.2, 1)` for transitions to ensure a professional, snappy feel. Prefer `0.2s` for layout shifts and `0.3s` for color/opacity fades.

## Code Principles

1. **Concise and readable** — Prefer clear, minimal code. Avoid verbosity and indirection. Every line should earn its place.
2. **Static rendering first** — Use server-side rendering and static generation. Add client-side JavaScript only when necessary.
3. **Best engineering practice** — Use `<script>` + `addEventListener` instead of inline `onclick` handlers. Search and update similar patterns together.
