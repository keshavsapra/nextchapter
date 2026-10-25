# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Added

- (nothing yet)

### Changed

- New NextChapter app logo everywhere: sidebar rail icon, favicon, apple-touch-icon, and the OpenGraph/social preview card; legacy Inkheart graphics removed

## [0.3.0] - Paperbound (2026-10-25)

### Added

- Support for EPUB (.epub) files across the entire library alongside PDFs
- EPUB cover image extraction from metadata and resources, with automatic fallback
- Full-text search indexing for EPUB chapters with heading and table of contents detection
- Reading progress tracking and chapter counts for EPUBs with persistence
- Integrated web EPUB reader built on epub.js with table of contents sidebar, navigation pill, and light/dark theme support
- Direct linking into EPUB chapters and CFI locations

### Fixed

- EPUB themes no longer bleed into each other - switching themes (including switching back to a previously used one) now always restyles the book content correctly
- EPUB page/location pill showed an off-by-one total (e.g. `Page 5 / 4 (100%)`); location and percentage are now clamped and accurate
- Rapid page turns and keyboard navigation no longer drop input while epub.js is still processing
- Keyboard shortcuts no longer paginate while a dialog is open or while a button, input, or link has focus
- Continuous scroll mode toggle now visually switches to a vertical scrolling layout and preserves the reading position
- Resize/relayout of the EPUB reader no longer crashes on load while epub.js is still initializing its view manager
- Toggle switches are visible in all themes (monochrome previously rendered them as an unreadable solid white block)
- Per-theme `color-scheme` on the document root so native form controls match the selected theme

## [1.4.0] - Factotum (2026-08-11)

### Added

- Integrated PDF reader built on PDF.js, replacing the embedded viewer, with a redesigned interface and a proper mobile layout
- Page counts and reading progress on document cards, with view options for card presentation
- Breadcrumb navigation, including in the mobile app bar
- Collapsed sidebar now provides access to the same features as the expanded sidebar
- `Use legacy reader` setting to fall back to the previous viewer

### Changed

- App-wide UI refresh: sidebar, search bar, modals, buttons, document and folder cards
- Search scope can be set to any ancestor folder, not just the current folder or the whole library
- Improved accessibility of interactive controls

### Fixed

- Folder names containing dots are no longer truncated
- Concurrent PDF operations could break all PDF processing until restart

## [1.3.1] - Primer (2026-05-11)

### Fixed

- back navigation no longer disappears on smaller screens

## [1.3.0] - Primer (2026-05-05)

### Added

- Full-text search across PDF content with per-page results
- Search scope selector to limit searches to the current folder or search globally
- Indexing progress indicator shown while the search index is being built
- `TEXT_INDEX_ENABLED` config variable to control text indexing

### Changed

- Search now runs server-side rather than fetching the full library index to the client

## [1.2.0] - Memorious (2026-01-28)

### Added

- Reading progress tracking for PDFs with localStorage persistence
- Page spread mode configuration (No spreads, Odd spreads, Even spreads) synced with PDF.js preferences
- Reader settings section in Settings modal for browser-only preferences

## [1.1.2] - Arbiter (2026-01-11)

### Fixed

- `CONFIG_DIR` now defaults to parent directory of `CONFIG_PATH` when not explicitly set, ensuring backward compatibility with existing deployments

## [1.1.1] - Arbiter (2026-01-11)

### Added

- `CONFIG_DIR` configuration variable for user data storage location

## [1.1.0] - Arbiter (2025-12-09)

### Added

- Collections for organizing PDFs into custom groups
- Pinned folders for quick sidebar access to frequently used directories
- Input validation for collection and folder names

### Changed

- Improved error handling in collection and pin operations

### Fixed

- Simplified store subscription pattern in search

## [1.0.5] - Ever-Faithful (2025-01-15)

### Added

- Optional anonymous usage telemetry using self-hosted Plausible Analytics
  - Tracks active instances, version numbers, CPU architecture, and library size buckets to guide development
  - Can be disabled via `TELEMETRY_ENABLED=false` config, consent banner, or Settings
  - Telemetry code can be compiled out entirely with `--no-default-features` build flag
- Ko-fi donation link in Settings and README for users who want to support development
- Expanded Privacy & Telemetry documentation section in README

## [1.0.4] - Ever-Faithful (2025-01-13)

### Added

- Multi-architecture Docker image support (AMD64 and ARM64)

## [1.0.3] - Ever-Faithful (2025-11-12)

### Fixed

- Library scanning no longer crashes when encountering corrupted or malformed PDFs

## [1.0.2] - Ever-Faithful (2025-11-12)

### Added

- Subdirectory hosting support via `BASE_PATH` configuration.

## [1.0.1] - Ever-Faithful (2025-06-01)

### Fixed

- Improved indexer status checking reliability
- Minor code cleanup and consistency improvements

## [1.0.0] - Ever-Faithful (2025-05-31)

### Added

- Health check endpoint for container monitoring and orchestration
- Configurable size for extracted images through configuration options
- Manual re-indexing button
- Back button in library view
- Back button in reader toolbar for easier navigation back to library
- Dark mode toggle in reader toolbar

### Changed

- Refactored API paths for improved organization and readability
- Improved state management for indexer status tracking
- Styled toast notifications to match application theme

### Fixed

- Cover image migration bug when upgrading from older versions
- Implemented cover filename sanitation for improved filesystem safety
- Various code quality improvements and fixes

## [0.9.1] - Clarity (2025-05-26)

### Changed

- Firebase configuration now requires providing the full Firebase config as a JSON file instead of just the project ID.

### Fixed

- Issues with development scripts due to the new Firebase config method.
- minor UI issues

### Added

- Lazy loading of images.

## [0.8.3] - Crispin (2024-12-31)

### Added

- Collapsible sidebar

## [0.7.0] - Horatio (2024-01-29)

### Added

- Sorting functionality for name, modified, and created fields.
- Simple search bar.
- Various UI improvements.
- Dark Mode Toggle
- Use of codenames for versions because why not? It's fun.

### Changed

- Replaced [tinro](https://github.com/AlexxNB/tinro) with [Svelte-Navigator](https://github.com/Nystik-gh/svelte-navigator) for routing. _(Using a custom fork to solve dependency issues)._
- Refactored versioning into a single source of truth.

### Updated

- Upgraded to Svelte 4 and upgraded dependencies accordingly.

### Fixed

- Issue in routing related to a [bug](https://bugzilla.mozilla.org/show_bug.cgi?id=1742396) in Firefox.
- Various minor issues.
