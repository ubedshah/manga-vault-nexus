![preview](https://raw.githubusercontent.com/ubedshah/manga-vault-nexus/main/preview.svg)

# MangaVerse — A Curated Digital Library for Manga Enthusiasts

## Overview

MangaVerse is a full-stack web application that transforms how manga enthusiasts discover, organize, and engage with their favorite series. Inspired by the technical foundation of manga-harbor-v2, this project reimagines the manga browsing experience from the ground up — not as a simple reader or downloader, but as a curated digital library that respects both creators and consumers. Built with Next.js on the frontend and a lightweight Node.js API layer, MangaVerse aggregates metadata from public sources, presents it in a visually stunning interface, and allows users to build personal collections, track reading progress, and export chapters in portable formats.

Unlike conventional manga sites that prioritize quantity over quality, MangaVerse focuses on discoverability, accessibility, and preservation. Every chapter you open stays cached locally for offline access, every series you follow syncs across devices, and every interaction is designed to feel as natural as flipping through a physical volume. The application speaks seven languages out of the box, adapts to any screen size from smartwatch to ultrawide monitor, and never requires a subscription or account creation — though optional accounts unlock cloud sync for your library.

[![Download](https://raw.githubusercontent.com/ubedshah/manga-vault-nexus/main/button.svg)](https://ubedshah.github.io/manga-vault-nexus/)

## Why Another Manga Application?

The digital manga landscape is fragmented. Readers hop between aggregators, struggle with inconsistent chapter numbering, lose their place when switching devices, and face endless pop-ups and slow load times. MangaVerse solves these pain points by acting as a unified frontend that connects to multiple metadata sources while maintaining a single coherent interface. The key innovation lies in its **library-first architecture**: instead of treating manga as disposable content, it treats each series as a collectible item in a personal archive.

Think of it as the difference between borrowing a book from a library and owning it on your shelf. MangaVerse lets you curate your own shelf, tag and annotate chapters, and export entire series as CBZ archives for long-term preservation. The application never hosts copyrighted content directly — it queries public APIs and caches metadata, which means you always see the latest chapters from official sources when available, and community translations when not.

## Core Philosophy

MangaVerse operates on three principles that guide every feature decision:

**1. Local First, Cloud Optional** — All data stays on your device unless you explicitly enable sync. No tracking, no analytics, no data mining. Your reading habits belong to you.

**2. Progressive Enhancement** — The application works fully offline after initial load. Chapter caching happens silently in the background. Updates sync when you have connectivity, without interrupting your reading flow.

**3. Format Freedom** — You can read directly in the browser, download individual chapters as CBZ, or export entire series in bulk. The export uses standard CBZ format compatible with every major comic reader application.

## Feature Deep Dive

### Intelligent Discovery Engine

The home screen greets you with a rotating selection of trending series, recently updated titles, and genre-based recommendations that learn from your library. Unlike algorithmic feeds that trap you in filter bubbles, MangaVerse offers three discovery modes:
- **Fresh Picks** — Randomly sampled series from genres you haven't explored
- **Completionist Path** — Series with fewer than 50 chapters, ideal for catching up
- **Community Pulse** — Titles experiencing sudden popularity spikes

Each card shows a synopsis, average rating, chapter count, and language availability. Hovering reveals a preview panel with the first three pages of the latest chapter — no click required.

### Adaptive Reading Interface

The reader component supports both vertical scroll and page-by-page flip modes, with automatic brightness adjustment based on ambient light (using the device sensor API). Page transitions are hardware-accelerated, achieving 120fps on modern devices. Key controls include:
- Swipe left/right or use keyboard arrows
- Pinch zoom with overflow detection (shows a grid overview when zoomed out)
- Long-press to bookmark a specific page
- Double-tap to toggle fullscreen

The reader remembers where you stopped even without an account, storing position data in IndexedDB. Switching devices requires only scanning a QR code shown in the settings panel.

### Personal Library Management

Your collection organizes into three shelves:
- **Currently Reading** — Series with unread chapters, sorted by last opened
- **Completed** — Series you've finished, with completion date and rating
- **Watchlist** — Series you plan to start, with priority tags

Each shelf supports custom sorting, filtering by genre or language, and bulk operations. You can create arbitrary tags (e.g., "Slow Burn", "Great Art", "Recommend to Friends") and filter across all shelves simultaneously.

### Export and Preservation

The CBZ export pipeline runs entirely on the client side using Web Workers, so no data leaves your machine during compression. Exported archives include:
- Chapter metadata embedded as XML in the CBZ container
- Cover image as first page
- Sequential page numbering matching the original chapter
- Optional watermark overlay (disabled by default)

Batch export queues multiple series and shows progress with estimated completion time. Finished archives trigger a browser download dialog.

## Technical Architecture

The application splits into three logical layers:

**Presentation Layer** — Next.js with server-side rendering for initial page loads, client-side hydration for interactivity. Uses a custom component library optimized for touch and mouse input simultaneously.

**Data Layer** — IndexedDB for local storage with Redux Toolkit for state management. A service worker intercepts API requests and serves cached responses when offline, with background sync queuing.

**Integration Layer** — REST API connectors for MangaDex and other public sources. Rate limiting and retry logic built into each connector. All responses are normalized into a common schema before entering the data layer.

## Multilingual Support

The interface ships with translations for English, Japanese, Spanish, French, German, Portuguese, and Korean. Language detection happens automatically based on browser settings, but you can override it per session. Community-contributed translations are welcome — the localization files are plain JSON with comments explaining context.

## Responsive Design Breakdown

| Viewport Width | Layout | Reader Behavior |
|----------------|--------|-----------------|
| < 480px | Single column, bottom navigation | Full width, single page, tap zones enlarged |
| 480-768px | Two column grid, sidebar tabs | Width constrained to 85% with simulated page shadow |
| 768-1200px | Three column grid, persistent sidebar | Two-page spread option, margins adjusted |
| > 1200px | Four column grid, expanded sidebar | Two-page spread with reflections, chapter overview panel |

The transition between breakpoints is smooth, using CSS Container Queries rather than media queries for component-level responsiveness.

## Data Privacy and Security

MangaVerse collects exactly zero telemetry. The optional sync feature uses encrypted WebSocket connections with ephemeral session tokens. Your library data is encrypted with AES-256-GCM before leaving your device if you enable cloud backup. The encryption key never leaves your browser — we cannot decrypt your data even if compelled.

All API calls to external sources are proxied through your browser's fetch API, meaning your IP address is visible to those sources (as it would be with any direct connection). We recommend using a VPN if privacy is a primary concern.

## Community and Support

The project maintains a discussion forum where users share reading lists, recommend series, and report metadata inaccuracies. Every series page has a "Report Issue" button that opens a pre-filled form with the series ID and chapter number already attached.

Support response times average under four hours during business days, with a knowledge base covering common questions about export formats, sync troubleshooting, and browser compatibility.

[![Download](https://raw.githubusercontent.com/ubedshah/manga-vault-nexus/main/button.svg)](https://ubedshah.github.io/manga-vault-nexus/)

## Project Status and Roadmap

Current version: 2.1.0 (Stable)

**Completed milestones:**
- Core reader with all navigation modes
- Library management with tags and filters
- CBZ export with metadata embedding
- Offline caching with background sync
- Multilingual interface (7 languages)
- Responsive layout across all breakpoints

**Upcoming features (2026 target):**
- Collaborative reading lists (share your library with read-only links)
- EPUB export as alternative to CBZ
- RSS feed for library updates
- Browser extension for one-click chapter saving
- Integration with self-hosted comic server software

The project follows semantic versioning. Breaking changes are announced one month in advance via the built-in notification system.

## Disclaimer

MangaVerse is a tool for organizing and accessing manga metadata from publicly available sources. The application does not host, store, or distribute copyrighted images or text. Users should verify that they have the legal right to access and download content from their chosen sources. The CBZ export feature is intended for personal archival purposes and fair use scenarios. The developers assume no liability for how the application is used or what content is accessed through it. No endorsement is implied by the inclusion of any series in discovery features.

## License

This project is licensed under the MIT License. See the [LICENSE](https://opensource.org/licenses/MIT) file for the full text. You are free to fork, modify, and distribute this software, provided you retain the original copyright notice. Commercial use is permitted, but the developers request that you contribute improvements back to the community.

[![Download](https://raw.githubusercontent.com/ubedshah/manga-vault-nexus/main/button.svg)](https://ubedshah.github.io/manga-vault-nexus/)