<style>
  .epub-reader {
    position: relative;
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 100%;
    max-width: 100%;
    overflow: hidden;
    text-align: left;
    background: var(--background);
  }

  .reader-body {
    position: relative;
    display: flex;
    flex: 1;
    min-height: 0;
    min-width: 0;
    width: 100%;
    max-width: 100%;
    overflow: hidden;
  }

  /* Top Bar */
  .bar {
    box-sizing: border-box;
    display: flex;
    align-items: center;
    gap: 6px;
    height: 40px;
    padding: 0 12px;
    background: var(--background);
    border-bottom: 1px solid var(--card-color);
    z-index: 10;
  }

  .cluster {
    flex: 1 1 0;
    display: flex;
    align-items: center;
    gap: 4px;
  }

  .cluster:last-child {
    justify-content: flex-end;
  }

  .title {
    flex: 0 1 auto;
    min-width: 0;
    margin: 0 8px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    text-align: center;
    color: var(--text-color-default);
    font-size: var(--font-xs);
    font-weight: var(--weight-bold);
  }

  .icon-btn {
    all: unset;
    box-sizing: border-box;
    width: 32px;
    height: 32px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    color: var(--text-color-light);
    border-radius: var(--radius-sm, 6px);
    cursor: pointer;
    transition: color 120ms ease, background 120ms ease;
  }

  .icon-btn:hover {
    color: var(--text-color-default);
    background: var(--card-subtle);
  }

  .icon-btn.active {
    color: var(--color-primary-500);
    background: var(--card-subtle);
  }

  /* Sidebar TOC Drawer */
  .sidebar-host {
    width: 280px;
    height: 100%;
    background: var(--card-subtle);
    border-right: 1px solid var(--card-color);
    display: flex;
    flex-direction: column;
    z-index: 15;
    transition: transform 200ms ease;
    overflow: hidden;
  }

  .sidebar-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 14px;
    border-bottom: 1px solid var(--card-color);
    font-size: var(--font-xs);
    font-weight: var(--weight-bold);
    color: var(--text-color-default);
  }

  .toc-list {
    flex: 1;
    overflow-y: auto;
    padding: 8px 0;
    list-style: none;
    margin: 0;
  }

  .toc-item {
    display: block;
    width: 100%;
    box-sizing: border-box;
    padding: 8px 16px;
    font-size: var(--font-xs);
    color: var(--text-color-light);
    text-align: left;
    background: none;
    border: none;
    cursor: pointer;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    transition: background 120ms ease, color 120ms ease;
  }

  .toc-item:hover {
    color: var(--text-color-default);
    background: var(--card-color);
  }

  .toc-item.active {
    color: var(--color-primary-500);
    font-weight: var(--weight-bold);
    background: var(--card-color);
  }

  .toc-subitem {
    padding-left: 28px;
    font-size: var(--font-2xs);
  }

  .toc-subsubitem {
    padding-left: 40px;
    font-size: var(--font-2xs);
    opacity: 0.85;
  }

  .scrim {
    position: absolute;
    inset: 0;
    background: rgba(0, 0, 0, 0.4);
    z-index: 12;
  }

  /* Viewer Area */
  .viewer-wrap {
    position: relative;
    flex: 1;
    height: 100%;
    width: 100%;
    min-width: 0;
    max-width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }

  .viewer-container {
    width: 100%;
    max-width: 900px;
    height: 100%;
    margin: 0 auto;
    position: relative;
    overflow: hidden;
    overflow-anchor: none;
    transition: opacity 150ms ease;
  }

  .viewer-container.hidden-viewer {
    opacity: 0;
    pointer-events: none;
  }

  .viewer-container :global(iframe) {
    border: none !important;
  }

  /* Floating Navigation Pill */
  .reader-pill {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 25;
    box-sizing: border-box;
    display: inline-flex;
    align-items: center;
    gap: 4px;
    width: max-content;
    height: 36px;
    padding: 0 8px;
    border: 1px solid var(--card-contrast, var(--card-color));
    border-radius: 999px;
    background: var(--card-color);
    box-shadow: var(--elevation-2);
    white-space: nowrap;
    user-select: none;
    pointer-events: auto;
  }

  .pill-btn {
    all: unset;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 28px;
    height: 28px;
    color: var(--text-color-light);
    font-size: var(--font-lg);
    cursor: pointer;
    border-radius: 999px;
    transition: color 120ms ease, background 120ms ease;
  }

  .pill-btn:hover {
    color: var(--text-color-default);
    background: var(--card-subtle);
  }

  .pill-label {
    display: inline-flex;
    align-items: baseline;
    gap: 4px;
    padding: 0 6px;
    color: var(--text-color-default);
    font-size: var(--font-xs);
    font-variant-numeric: tabular-nums;
  }

  .pill-sub {
    color: var(--text-color-light);
    font-size: var(--font-2xs);
    margin-left: 2px;
  }

  /* Navigation Margin Zones */
  .nav-zone {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 10%;
    min-width: 40px;
    max-width: 80px;
    z-index: 3;
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0;
    cursor: pointer;
    transition: opacity 160ms ease;
  }

  .nav-zone:hover {
    opacity: 0.7;
  }

  .nav-zone.left {
    left: 0;
  }

  .nav-zone.right {
    right: 0;
  }

  .nav-zone-btn {
    all: unset;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 36px;
    height: 36px;
    border-radius: 999px;
    background: var(--card-contrast, rgba(0, 0, 0, 0.4));
    color: var(--text-color-default, #fff);
  }

  /* Loading & Error States */
  .state-message {
    position: absolute;
    inset: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 12px;
    color: var(--text-color-light);
    font-size: var(--font-sm);
  }

  @media (--small-viewport) {
    .sidebar-host {
      position: absolute;
      top: 0;
      bottom: 0;
      left: 0;
      width: 80vw;
      max-width: 300px;
      box-shadow: 6px 0 20px rgba(0, 0, 0, 0.4);
    }

    .nav-zone {
      display: none;
    }
  }
</style>

<script lang="ts">
  import { onMount, onDestroy, tick } from 'svelte'
  import { useNavigate } from 'svelte-navigator'
  import Icon from '@iconify/svelte'
  import ePub, { type Book, type Rendition, type NavItem } from 'epubjs'
  import { selectedTheme } from '../../../stores/options'
  import { isDarkTheme, updateTheme } from '../../../lib/theme'
  import { savePageProgress, loadEpubProgress } from '../../../stores/reader'
  import { usingAuth } from '../../../stores/auth'
  import { getToken } from '../../../lib/localstorage'
  import { get } from 'svelte/store'
  import { routeUrl } from '../../../utils/path'
  import Jumper from 'svelte-loading-spinners/Jumper.svelte'

  export let file = ''
  export let fileId = ''
  export let title = ''
  export let initialPage: number = 1
  export let initialCfi: string = ''
  export let mode: 'reader' | 'fullscreen' | 'embed' = 'reader'

  const navigate = useNavigate()

  let viewerContainer: HTMLDivElement
  let book: Book | null = null
  let rendition: Rendition | null = null

  let loading = true
  let errorMsg = ''
  let sidebarOpen = false
  let isMobile = false

  let toc: NavItem[] = []
  let currentChapterIndex = 0
  let totalChapters = 0
  let currentCfi = ''
  let activeHref = ''

  let flowMode: 'paginated' | 'scrolled' = 'paginated'
  let isNavigating = false

  let locationsReady = false
  let currentLocation = 0
  let totalLocations = 0
  let progressPercentage = 0
  let displayedPage = 1
  let displayedTotalPages = 1

  $: darkIcon = isDarkTheme($selectedTheme)

  $: if (rendition && $selectedTheme) {
    applyTheme($selectedTheme)
  }

  const authHeaders = (): Record<string, string> => (get(usingAuth) ? { Authorization: `Bearer ${getToken()}` } : {})

  function applyTheme(themeName: string) {
    if (!rendition) return
    let activeKey = themeName
    if (activeKey === 'system') {
      activeKey = isDarkTheme('system') ? 'dark' : 'light'
    }
    rendition.themes.select(`theme-${activeKey}`)
  }

  type EpubThemePalette = { background: string; text: string; heading: string; link: string }

  const baseBodyRules = {
    'font-family': 'system-ui, -apple-system, sans-serif !important',
    'line-height': '1.6 !important',
    'padding': '0 24px !important',
    'box-sizing': 'border-box !important',
  }
  const mediaRules = {
    'max-width': '100% !important',
    'height': 'auto !important',
  }
  const textRules = {
    'word-break': 'break-word !important',
    'overflow-wrap': 'break-word !important',
  }

  /**
   * Build epub content theme rules scoped to the theme class that epub.js
   * adds to the content body on `themes.select()` (e.g. `body.theme-dark`).
   *
   * epub.js injects object-registered rules into one <style> node per theme
   * and never removes them, so unscoped rules (plain `body { ... }`) from
   * previously selected themes keep matching and the last injected one wins.
   * Scoping every selector to the body class keeps only the selected theme
   * active, regardless of cascade order.
   */
  function epubThemeRules(name: string, palette: EpubThemePalette) {
    const scope = `body.theme-${name}`
    const headingScope = `${scope} h1, ${scope} h2, ${scope} h3, ${scope} h4, ${scope} h5, ${scope} h6`
    const textScope = `${scope} p, ${scope} div, ${scope} span, ${scope} li, ${scope} blockquote`
    const linkScope = `${scope} a, ${scope} a:link, ${scope} a:visited`
    const mediaScope = `${scope} img, ${scope} svg, ${scope} table`
    return {
      [scope]: { ...baseBodyRules, background: `${palette.background} !important`, color: `${palette.text} !important` },
      [textScope]: { ...textRules, color: `${palette.text} !important` },
      [headingScope]: { color: `${palette.heading} !important` },
      [linkScope]: { color: `${palette.link} !important` },
      [mediaScope]: mediaRules,
    }
  }

  function toggleTheme() {
    const isDark = isDarkTheme($selectedTheme)
    updateTheme(isDark ? 'light' : 'dark')
  }

  function resizeRendition() {
    if (!rendition) return
    // epub.js creates the manager asynchronously (Rendition.start()), so
    // resize() can be called before `manager` exists - guard against that
    // instead of crashing with a TypeError on the reader.
    if (!(rendition as any).manager) return
    try {
      rendition.resize()
    } catch (err) {
      console.warn('Rendition resize error:', err)
    }
  }

  async function toggleFlowMode() {
    if (!book || loading) return
    flowMode = flowMode === 'paginated' ? 'scrolled' : 'paginated'
    // Recreate the rendition - the manager cannot be switched in place.
    const target = currentCfi || undefined
    await createRendition(target)
    await tick()
    resizeRendition()
  }

  function handleBack() {
    if (window.history.length > 1) {
      navigate(-1)
    } else {
      navigate('/')
    }
  }

  async function toggleFullscreen() {
    if (mode === 'fullscreen' || mode === 'embed') {
      if (!document.fullscreenElement) {
        document.documentElement.requestFullscreen().catch(() => {})
      } else {
        document.exitFullscreen().catch(() => {})
      }
    } else {
      const targetUrl = `${routeUrl(`/fullscreen/${fileId}`)}?page=${currentChapterIndex + 1}`
      window.open(targetUrl, '_blank')
    }
  }

  async function next() {
    if (!rendition) return
    try {
      // epub.js serializes page turns on its internal queue, so we don't need
      // a local busy-flag - blocking here would drop rapid input.
      await rendition.next()
    } catch (err) {
      console.warn('Navigation next error:', err)
    }
  }

  async function prev() {
    if (!rendition) return
    try {
      await rendition.prev()
    } catch (err) {
      console.warn('Navigation prev error:', err)
    }
  }

  function normalizePath(p: string): string {
    if (!p) return ''
    try {
      p = decodeURIComponent(p)
    } catch (_) {}
    return p.replace(/\\/g, '/').replace(/^(\.\/|\.\.\/|\/)+/, '').toLowerCase()
  }

  function resolveSpineSection(href: string): any {
    if (!book?.spine) return null
    if (!href) return book.spine.get(currentChapterIndex)

    const cleanHref = href.trim()
    const [pathPart, hashPart] = cleanHref.split('#')
    const cleanPath = pathPart.replace(/^(\.\/|\.\.\/|\/)+/, '')

    // If href is just an anchor (#id), return current section
    if (!cleanPath && hashPart) {
      return book.spine.get(currentChapterIndex)
    }

    // Direct lookup in spine
    let section = book.spine.get(cleanPath) ||
                  book.spine.get(pathPart) ||
                  book.spine.get(cleanHref)
    if (section) return section

    try {
      section = book.spine.get(decodeURIComponent(cleanPath))
      if (section) return section
    } catch (_) {}

    // Resolve via book.path or book.canonical if available
    if (book.path && typeof book.path.resolve === 'function') {
      try {
        const resolved = book.path.resolve(cleanPath)
        section = book.spine.get(resolved)
        if (section) return section
      } catch (_) {}
    }

    // Search across spineItems
    const items = book.spine.spineItems || []
    const normTarget = normalizePath(cleanPath)
    const baseTarget = normTarget.split('/').pop()

    // Pass 1: exact or suffix normalized match
    section = items.find((s: any) => {
      if (!s.href) return false
      const normS = normalizePath(s.href)
      return normS === normTarget || normS.endsWith('/' + normTarget) || normTarget.endsWith('/' + normS)
    })
    if (section) return section

    // Pass 2: match by basename (filename)
    if (baseTarget) {
      section = items.find((s: any) => {
        if (!s.href) return false
        const sBase = normalizePath(s.href).split('/').pop()
        return sBase === baseTarget
      })
      if (section) return section
    }

    // Pass 3: match by idref
    if (hashPart || cleanPath) {
      const idToMatch = hashPart || cleanPath
      section = items.find((s: any) => s.idref === idToMatch)
      if (section) return section
    }

    return null
  }

  async function navigateTo(href: string) {
    if (!rendition || !book || isNavigating) return
    isNavigating = true
    try {
      if (!href) return
      const cleanHref = href.trim()
      const [, hashPart] = cleanHref.split('#')

      const section = resolveSpineSection(cleanHref)
      let targetToDisplay: string | number = cleanHref

      if (section) {
        targetToDisplay = section.href
        if (hashPart) {
          try {
            await section.load(book.load?.bind(book) || (() => Promise.resolve()))
            const doc = section.document
            if (doc) {
              const decodedHash = decodeURIComponent(hashPart)
              let el = doc.getElementById(hashPart) ||
                       doc.getElementById(decodedHash) ||
                       doc.querySelector(`[name="${hashPart}"]`) ||
                       doc.querySelector(`[name="${decodedHash}"]`)

              if (!el) {
                try {
                  el = doc.querySelector(`#${CSS.escape(hashPart)}`) ||
                       doc.querySelector(`#${CSS.escape(decodedHash)}`)
                } catch (_) {}
              }

              if (el && typeof section.cfiFromElement === 'function') {
                const cfi = section.cfiFromElement(el)
                if (cfi) {
                  targetToDisplay = cfi
                }
              }
            }
          } catch (anchorErr) {
            console.warn('Anchor CFI resolution failed, falling back to section href:', anchorErr)
          }
        }
      }

      try {
        await rendition.display(targetToDisplay)
      } catch (dispErr) {
        console.warn('rendition.display failed with resolved target, falling back:', dispErr)
        if (section) {
          try {
            await rendition.display(section.href)
          } catch (_) {
            await rendition.display(section.index)
          }
        } else {
          await rendition.display(cleanHref)
        }
      }

      activeHref = cleanHref
      if (isMobile) {
        sidebarOpen = false
      }
    } catch (err) {
      console.error('Failed to navigate to TOC item:', href, err)
    } finally {
      isNavigating = false
    }
  }

  function isItemActive(itemHref: string): boolean {
    if (!itemHref) return false
    const cleanItem = itemHref.trim()
    if (activeHref) {
      if (activeHref === cleanItem) return true
      if (activeHref.includes(cleanItem) || cleanItem.includes(activeHref)) return true
    }
    const [path] = cleanItem.split('#')
    const norm = normalizePath(path)
    const currentSection = book?.spine?.get(currentChapterIndex)
    if (currentSection?.href) {
      const normSec = normalizePath(currentSection.href)
      if (normSec === norm || normSec.endsWith('/' + norm) || norm.endsWith('/' + normSec)) {
        return true
      }
      if (norm.split('/').pop() === normSec.split('/').pop()) {
        return true
      }
    }
    return false
  }

  function updateLocationProgress(cfi: string) {
    if (!book?.locations) return
    try {
      const loc = book.locations.locationFromCfi(cfi)
      if (typeof loc === 'number' && loc >= 0) {
        // epub.js `locations.total` is the last 0-based location index
        // (_locations.length - 1), so the human-readable count is total + 1.
        totalLocations = (book.locations.total || 0) + 1
        currentLocation = Math.min(loc + 1, totalLocations)
        const pct = Math.min(Math.max(book.locations.percentageFromCfi(cfi) || 0, 0), 1)
        progressPercentage = Math.round(pct * 100)
      }
    } catch (_) {}
  }

  function handleKeydown(e: KeyboardEvent) {
    // Let form controls, dialogs, and editable areas keep their normal keys;
    // dialogs are rendered top-layer, so pagination would happen behind them.
    const target = e.target as HTMLElement | null
    if (target?.closest?.('dialog, input, textarea, select, button, a, [role="textbox"], [contenteditable]')) {
      return
    }
    if (e.key === 'ArrowRight' || e.key === 'PageDown' || e.key === ' ') {
      e.preventDefault()
      next()
    } else if (e.key === 'ArrowLeft' || e.key === 'PageUp') {
      e.preventDefault()
      prev()
    }
  }

  /**
   * Create (or recreate) the rendition after a flow-mode switch.
   *
   * epub.js cannot switch a rendition's manager after creation, and its
   * `flow()` only relayouts within the existing manager - the "continuous"
   * manager keeps rendering fixed horizontal columns for scrolled flows.
   * To actually toggle between paginated and continuous scroll we destroy
   * the old rendition and build a new one with the right manager.
   */

  // Content hooks that successive renditions registered on the book's spine
  // (see createRendition); removed on rebuild to avoid stale closures.
  let staleSpineHooks: ((...args: any[]) => void)[] = []

  function cleanupStaleSpineHooks() {
    if (!staleSpineHooks.length) return
    const spineHook = (book?.spine as any)?.hooks?.content
    if (spineHook?.deregister) {
      for (const fn of staleSpineHooks) {
        try { spineHook.deregister(fn) } catch (_) {}
      }
    }
    staleSpineHooks = []
  }

  async function createRendition(target?: string | number) {
    if (!book || !viewerContainer) return

    if (rendition) {
      try { rendition.destroy() } catch (_) {}
      rendition = null
      viewerContainer.innerHTML = ''
      // epub.js's destroy() never deregisters the `injectIdentifier` content
      // hook it registered on the book's spine, so every rebuild would leave
      // a hook pointing at a rendition whose `book` reference was nulled -
      // one console error per chapter load. Remove stale hooks before
      // building the next rendition.
      cleanupStaleSpineHooks()
    }

    const spineContentHook = (book.spine as any)?.hooks?.content
    const hooksBeforeCreate: unknown[] = spineContentHook?.list?.() ?? []

    rendition = book.renderTo(viewerContainer, {
      // 'continuous' prevents chapter-skipping bugs when paginating;
      // a real vertical scroll layout needs the default manager.
      manager: flowMode === 'paginated' ? 'continuous' : 'default',
      flow: flowMode === 'paginated' ? 'paginated' : 'scrolled-doc',
      width: '100%',
      height: '100%',
      spread: 'none',
    })
    // Track the spine content hook this rendition registered (epub.js adds
    // `injectIdentifier` to the book on every construction) so it can be
    // removed when this rendition is destroyed.
    if (spineContentHook) {
      const added = (spineContentHook.list?.() ?? []).find((fn: unknown) => !hooksBeforeCreate.includes(fn))
      if (added) staleSpineHooks.push(added)
    }

    // Register content hook for robust word wrapping and responsive media
    rendition.hooks.content.register(registerContentStyles)

    // Register theme styles
    rendition.themes.register('theme-dark', epubThemeRules('dark', { background: '#1a1625', text: '#e4e5f1', heading: '#ffffff', link: '#9171f8' }))
    rendition.themes.register('theme-light', epubThemeRules('light', { background: '#ffffff', text: '#484b6a', heading: '#1a202c', link: '#5e43f3' }))
    rendition.themes.register('theme-glassmorphism', epubThemeRules('glassmorphism', { background: '#0f172a', text: '#f8fafc', heading: '#38bdf8', link: '#38bdf8' }))
    rendition.themes.register('theme-neomorphism', epubThemeRules('neomorphism', { background: '#e6ecf5', text: '#2d3748', heading: '#1a202c', link: '#6366f1' }))
    rendition.themes.register('theme-aurora', epubThemeRules('aurora', { background: '#090d16', text: '#f1f5f9', heading: '#10b981', link: '#34d399' }))
    rendition.themes.register('theme-neo-brutalism', epubThemeRules('neo-brutalism', { background: '#fdfbf7', text: '#000000', heading: '#ff5277', link: '#e02854' }))
    rendition.themes.register('theme-cyberpunk', epubThemeRules('cyberpunk', { background: '#0d0221', text: '#00f0ff', heading: '#ff007f', link: '#ffe600' }))
    rendition.themes.register('theme-monochrome', epubThemeRules('monochrome', { background: '#0a0a0a', text: '#ededed', heading: '#ffffff', link: '#ffffff' }))

    applyTheme($selectedTheme)

    // Listen to relocation events for progress tracking
    rendition.on('relocated', (location: any) => {
      if (!location?.start) return
      currentCfi = location.start.cfi || ''
      const spineIndex = location.start.index ?? 0
      currentChapterIndex = spineIndex

      if (location.start.displayed) {
        displayedTotalPages = location.start.displayed.total || 1
        // epub.js can report a page beyond the section total while the
        // continuous manager relayouts across section boundaries.
        displayedPage = Math.max(1, Math.min(location.start.displayed.page || 1, displayedTotalPages))
      }

      if (locationsReady) {
        updateLocationProgress(currentCfi)
      }

      // Save reading progress
      if (fileId) {
        savePageProgress(fileId, spineIndex + 1, currentCfi)
      }

      if (location.start.href) {
        activeHref = location.start.href
      }
    })

    // Always display the target (falls back to the first linear spine
    // section when undefined) - without this the reader could open blank.
    try {
      await rendition.display(target as string | number | undefined)
    } catch (dispErr) {
      console.warn('rendition.display failed:', dispErr)
      await rendition.display()
    }
  }

  // Generate background locations for page numbering (done once per book)
  function generateLocations() {
    book?.ready.then(() => {
      return book?.locations.generate(1024)
    }).then(() => {
      locationsReady = true
      if (currentCfi) {
        updateLocationProgress(currentCfi)
      }
    }).catch((e) => {
      console.warn('Epub locations generation skipped:', e)
    })
  }

  // Injected into each chapter document: robust word wrapping + responsive media
  function registerContentStyles(contents: any) {
    const doc = contents?.document
    if (!doc) return
    const style = doc.createElement('style')
    style.textContent = `
      * {
        box-sizing: border-box !important;
        word-wrap: break-word !important;
        overflow-wrap: break-word !important;
        overflow-anchor: none !important;
      }
      html, body {
        margin: 0 !important;
        padding: 0 !important;
        overflow-anchor: none !important;
      }
      body {
        padding: 20px 32px 60px 32px !important;
        -webkit-font-smoothing: antialiased;
      }
      p, div, span, li, blockquote {
        word-break: normal !important;
        overflow-wrap: break-word !important;
        hyphens: auto !important;
      }
      img, svg, video, image {
        max-width: 100% !important;
        max-height: 80vh !important;
        height: auto !important;
        object-fit: contain !important;
        page-break-inside: avoid !important;
        break-inside: avoid !important;
      }
      table {
        max-width: 100% !important;
        table-layout: fixed !important;
        word-break: break-word !important;
      }
    `
    doc.head.appendChild(style)
  }

  let _mql: MediaQueryList | null = null
  let _mediaListener: ((e: MediaQueryListEvent) => void) | null = null
  let resizeObserver: ResizeObserver | null = null

  onDestroy(() => {
    resizeObserver?.disconnect()
    if (_mql && _mediaListener) {
      _mql.removeEventListener('change', _mediaListener)
    }
    window.removeEventListener('keydown', handleKeydown)
    if (rendition) {
      try { rendition.destroy() } catch (_) {}
    }
    if (book) {
      try { book.destroy() } catch (_) {}
    }
  })

  onMount(async () => {
    _mql = window.matchMedia('(max-width: 45rem)')
    isMobile = _mql.matches
    _mediaListener = (e: MediaQueryListEvent) => {
      isMobile = e.matches
    }
    _mql.addEventListener('change', _mediaListener)

    try {
      loading = true
      errorMsg = ''

      // Fetch the EPUB file as an ArrayBuffer with auth headers
      const res = await fetch(file, { headers: authHeaders() })
      if (!res.ok) {
        throw new Error(`Failed to download EPUB (status: ${res.status})`)
      }
      const arrayBuffer = await res.arrayBuffer()

      // Initialize epub.js book
      book = ePub(arrayBuffer)
      await book.opened

      // Load table of contents
      const nav = await book.loaded.navigation
      toc = nav?.toc || []

      // Check spine length
      const spine = await book.loaded.spine
      totalChapters = spine?.length || 0

      // Render to container (see createRendition)
      const savedProgress = loadEpubProgress(fileId)
      let target: string | number | undefined = undefined

      if (initialCfi) {
        target = initialCfi
      } else if (initialPage > 1 && totalChapters > 0) {
        const spineItem = spine?.get(initialPage - 1)
        target = spineItem?.href || initialPage - 1
      } else if (savedProgress?.cfi) {
        target = savedProgress.cfi
      } else if (savedProgress?.page && savedProgress.page > 1) {
        const spineItem = spine?.get(savedProgress.page - 1)
        target = spineItem?.href || savedProgress.page - 1
      }

      await createRendition(target)

      generateLocations()
      loading = false

      await tick()
      if (rendition) {
        // The manager is created asynchronously inside epub.js's internal
        // queue (Rendition.start() runs after `book.opened`), so a resize
        // call can arrive before it exists. Wait for `started` first.
        const initialRendition = rendition
        Promise.resolve().then(() => (initialRendition as any).started).then(() => {
          if (rendition === initialRendition) resizeRendition()
        }).catch(() => {})
      }

      // Observe size changes
      if (viewerContainer) {
        resizeObserver = new ResizeObserver(() => {
          if (rendition && !loading) {
            resizeRendition()
          }
        })
        resizeObserver.observe(viewerContainer)
      }
    } catch (e: any) {
      console.error('Failed to load EPUB:', e)
      errorMsg = e?.message || 'Failed to open EPUB document'
      loading = false
    }

    window.addEventListener('keydown', handleKeydown)
  })
</script>

<div class="epub-reader">
  <!-- Top Bar -->
  <div class="bar">
    <div class="cluster">
      {#if mode === 'reader'}
        <button class="icon-btn" on:click="{handleBack}" title="Back">
          <Icon icon="material-symbols:arrow-back-rounded" width="20" height="20" />
        </button>
      {/if}
      <button
        class="icon-btn"
        class:active="{sidebarOpen}"
        on:click="{() => (sidebarOpen = !sidebarOpen)}"
        title="Table of Contents"
      >
        <Icon icon="material-symbols:format-list-bulleted-rounded" width="20" height="20" />
      </button>
    </div>

    <div class="title" title="{title}">{title || 'EPUB Reader'}</div>

    <div class="cluster">
      <button
        class="icon-btn"
        class:active="{flowMode === 'scrolled'}"
        on:click="{toggleFlowMode}"
        title="{flowMode === 'paginated' ? 'Switch to Continuous Scroll Mode' : 'Switch to Paginated Mode'}"
      >
        <Icon
          icon="{flowMode === 'paginated' ? 'material-symbols:auto-stories-outline-rounded' : 'material-symbols:format-align-justify-rounded'}"
          width="20"
          height="20"
        />
      </button>
      <button class="icon-btn" on:click="{toggleTheme}" title="Toggle Theme">
        <Icon icon="{darkIcon ? 'material-symbols:light-mode-outline-rounded' : 'material-symbols:dark-mode-outline-rounded'}" width="20" height="20" />
      </button>
      <button class="icon-btn" on:click="{toggleFullscreen}" title="Fullscreen">
        <Icon icon="material-symbols:fullscreen-rounded" width="20" height="20" />
      </button>
    </div>
  </div>

  <div class="reader-body">
    <!-- TOC Sidebar Drawer -->
    {#if sidebarOpen}
      <div class="sidebar-host">
        <div class="sidebar-header">
          <span>Table of Contents</span>
          <button class="icon-btn" on:click="{() => (sidebarOpen = false)}" title="Close">
            <Icon icon="material-symbols:close-rounded" width="18" height="18" />
          </button>
        </div>
        <ul class="toc-list">
          {#if toc.length === 0}
            <li class="toc-item" style="cursor: default; opacity: 0.6;">No chapters found</li>
          {:else}
            {#each toc as item}
              <li>
                <button
                  class="toc-item"
                  class:active="{isItemActive(item.href)}"
                  on:click="{() => navigateTo(item.href)}"
                >
                  {item.label?.trim() || 'Untitled Chapter'}
                </button>
                {#if item.subitems && item.subitems.length > 0}
                  {#each item.subitems as sub}
                    <button
                      class="toc-item toc-subitem"
                      class:active="{isItemActive(sub.href)}"
                      on:click="{() => navigateTo(sub.href)}"
                    >
                      {sub.label?.trim() || 'Untitled'}
                    </button>
                    {#if sub.subitems && sub.subitems.length > 0}
                      {#each sub.subitems as subsub}
                        <button
                          class="toc-item toc-subsubitem"
                          class:active="{isItemActive(subsub.href)}"
                          on:click="{() => navigateTo(subsub.href)}"
                        >
                          {subsub.label?.trim() || 'Untitled'}
                        </button>
                      {/each}
                    {/if}
                  {/each}
                {/if}
              </li>
            {/each}
          {/if}
        </ul>
      </div>
      {#if isMobile}
        <!-- Backdrop scrim on mobile -->
        <div class="scrim" role="button" tabindex="0" on:click="{() => (sidebarOpen = false)}" on:keydown="{(e) => (e.key === 'Escape' || e.key === 'Enter') && (sidebarOpen = false)}"></div>
      {/if}
    {/if}

    <!-- Main Viewer Area -->
    <div class="viewer-wrap">
      {#if loading}
        <div class="state-message">
          <Jumper size="60" color="var(--button-primary, #6366f1)" unit="px" />
          <span>Opening EPUB...</span>
        </div>
      {:else if errorMsg}
        <div class="state-message">
          <Icon icon="material-symbols:error-outline-rounded" width="48" height="48" style="color: var(--color-danger, #ef4444);" />
          <span>{errorMsg}</span>
        </div>
      {/if}

      <!-- Page turn click zones (only in paginated mode) -->
      {#if flowMode === 'paginated'}
        <div class="nav-zone left" role="button" tabindex="0" on:click="{prev}" on:keydown="{(e) => (e.key === 'Enter' || e.key === ' ') && prev()}" title="Previous Page">
          <div class="nav-zone-btn">
            <Icon icon="material-symbols:chevron-left-rounded" width="24" height="24" />
          </div>
        </div>
        <div class="nav-zone right" role="button" tabindex="0" on:click="{next}" on:keydown="{(e) => (e.key === 'Enter' || e.key === ' ') && next()}" title="Next Page">
          <div class="nav-zone-btn">
            <Icon icon="material-symbols:chevron-right-rounded" width="24" height="24" />
          </div>
        </div>
      {/if}

      <div
        bind:this="{viewerContainer}"
        class="viewer-container"
        class:hidden-viewer="{loading || errorMsg}"
      ></div>

      <!-- Floating Bottom Navigation Pill -->
      {#if !loading && !errorMsg}
        <div class="reader-pill">
          <button class="pill-btn" on:click="{prev}" title="Previous">
            <Icon icon="material-symbols:chevron-left-rounded" width="20" height="20" />
          </button>
          <div class="pill-label">
            {#if locationsReady && totalLocations > 0}
              <span>Page {currentLocation} / {totalLocations}</span>
              <span class="pill-sub">({progressPercentage}%)</span>
            {:else if displayedTotalPages > 1}
              <span>Ch. {currentChapterIndex + 1} • Pg. {displayedPage} / {displayedTotalPages}</span>
            {:else if totalChapters > 0}
              <span>Ch. {currentChapterIndex + 1} / {totalChapters}</span>
            {:else}
              <span>Reading</span>
            {/if}
          </div>
          <button class="pill-btn" on:click="{next}" title="Next">
            <Icon icon="material-symbols:chevron-right-rounded" width="20" height="20" />
          </button>
        </div>
      {/if}
    </div>
  </div>
</div>
