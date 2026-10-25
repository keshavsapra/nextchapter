<style>
  .rail {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    margin-top: 8px;
    text-align: left;
  }

  .rail-logo {
    width: 36px;
    height: 36px;
    display: block;
    margin-bottom: 0.5rem;
  }

  .rail-icon {
    all: unset;
    cursor: pointer;
    width: 32px;
    height: 32px;
    border-radius: var(--radius-sm);
    display: inline-flex;
    align-items: center;
    justify-content: center;
    color: var(--text-color-light);
    font-size: var(--font-md);
    transition:
      color 0.1s,
      background-color 0.1s;
  }

  .rail-icon:hover {
    background: var(--card-color);
    color: var(--text-color-default);
  }

  .rail-icon:focus-visible {
    box-shadow: var(--focus-ring);
  }

  .rail-icon.active {
    background: var(--foreground);
    color: var(--text-color-default);
  }

  .rail-icon.empty {
    opacity: 0.4;
  }

  .popover-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
  }

  .add-btn {
    all: unset;
    box-sizing: border-box;
    width: 20px;
    height: 20px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border-radius: var(--radius-sm);
    color: var(--text-color-light);
    font-size: var(--font-sm);
    cursor: pointer;
    transition: color 0.1s;
  }

  .add-btn:hover,
  .add-btn:focus-visible {
    color: var(--text-color-default);
  }

  .add-btn:focus-visible {
    box-shadow: var(--focus-ring);
  }

  .popover-row-wrapper {
    position: relative;
  }

  .popover-row {
    all: unset;
    box-sizing: border-box;
    width: 100%;
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 7px 2.2rem 7px 10px;
    border-radius: var(--radius-sm);
    color: var(--text-color-light);
    font-size: var(--font-sm);
    cursor: pointer;
    transition: background-color 0.1s;
  }

  .popover-row:hover {
    background: var(--card-color);
    color: var(--text-color-default);
  }

  .popover-row:focus-visible {
    box-shadow: var(--focus-ring);
  }

  .popover-row.active {
    background: var(--foreground);
    color: var(--text-color-default);
  }

  .popover-row.missing {
    color: var(--error-color);
    opacity: 0.7;
    cursor: default;
  }

  .popover-row.missing .row-name {
    text-decoration: line-through;
  }

  .popover-row :global(.icon) {
    flex-shrink: 0;
    color: var(--color-primary-400);
    font-size: var(--font-base);
  }

  .row-name {
    flex: 1;
    min-width: 0;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .row-action {
    all: unset;
    box-sizing: border-box;
    position: absolute;
    right: 0.8rem;
    top: 50%;
    transform: translateY(-50%);
    width: 20px;
    height: 20px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border-radius: var(--radius-sm);
    color: var(--text-color-light);
    opacity: 0.6;
    font-size: var(--font-sm);
    cursor: pointer;
    transition:
      color 0.1s,
      opacity 0.1s;
  }

  .row-action:hover,
  .row-action:focus-visible {
    opacity: 1;
  }

  .row-action:focus-visible {
    box-shadow: var(--focus-ring);
  }

  .row-action.missing {
    color: var(--error-color);
  }

  .row-action.unpin:not(.missing):hover,
  .row-action.unpin:not(.missing):focus-visible {
    color: var(--danger-color);
  }

  .row-action.options:hover,
  .row-action.options:focus-visible {
    color: var(--text-color-default);
  }

  .popover-empty {
    padding: 7px 10px;
    color: var(--text-color-light);
    font-size: var(--font-xs);
  }
</style>

<script lang="ts">
  import { onMount, tick } from 'svelte'
  import { useNavigate, useLocation } from 'svelte-navigator'
  import Icon from '@iconify/svelte'
  import { toast } from '@zerodevx/svelte-toast'
  import Popover from '../components/popover/Popover.svelte'
  import CollectionOptionsModal from '../components/collections/CollectionOptionsModal.svelte'
  import NewCollectionModal from '../components/collections/NewCollectionModal.svelte'
  import { pinnedFolders, missingPins, loadPins } from '../../stores/pins'
  import { collections, loadCollections } from '../../stores/collections'
  import { addPin, removePin, removeMissingPin } from '../../services/pin-service'
  import { pushUndoToast } from '../../lib/undo-toast'

  const navigate = useNavigate()
  const location = useLocation()

  let optionsModalOpen = false
  let newCollectionModalOpen = false
  let selectedCollectionId = ''
  let selectedCollectionName = ''

  onMount(() => {
    loadPins()
    loadCollections()
  })

  $: allPins = [
    ...$pinnedFolders.map((f) => ({ ...f, isMissing: false })),
    ...$missingPins.map((p) => ({ ...p, isMissing: true })),
  ]

  $: currentPath = $location.pathname

  const goHome = () => {
    navigate('/')
  }

  const goToFolder = (id: string, isMissing: boolean, close: () => void) => {
    if (isMissing) {
      return
    }

    navigate(`/library/${id}`)
    close()
  }

  const goToCollection = (id: string, close: () => void) => {
    navigate(`/collection/${id}`)
    close()
  }

  const restorePin = async (id: string, displayName?: string) => {
    try {
      await addPin(id, displayName)
      await loadPins()
    } catch (error) {
      console.error('Failed to restore pin:', error)
      toast.push('Failed to restore pin')
    }
  }

  const focusPanelItem = (position: number) => {
    const panel = document.querySelector('.panel[role="menu"]')

    if (!panel) {
      return
    }

    const items = Array.from(panel.querySelectorAll<HTMLElement>('[data-popover-item]'))

    if (items.length === 0) {
      return
    }

    items[Math.min(position, items.length - 1)].focus()
  }

  const handleUnpin = async (index: number, id: string, isMissing: boolean, displayName?: string) => {
    try {
      if (isMissing) {
        await removeMissingPin(id)
        await loadPins()
        toast.push('Removed missing pin')
      } else {
        await removePin(id)
        await loadPins()
        pushUndoToast('Folder unpinned', () => restorePin(id, displayName))
      }

      await tick()

      // Each row renders a row button and an action button, so row N starts at item index N * 2.
      focusPanelItem(index * 2)
    } catch (error) {
      console.error('Failed to remove pin:', error)
      toast.push('Failed to remove pin')
    }
  }

  const openCollectionOptions = (id: string, name: string, close: () => void) => {
    close()
    selectedCollectionId = id
    selectedCollectionName = name
    optionsModalOpen = true
  }

  const openNewCollection = (close: () => void) => {
    close()
    newCollectionModalOpen = true
  }
</script>

<div class="rail">
  <img class="rail-logo" src="static/logo.png" alt="NextChapter" />

  <button class="rail-icon" class:active="{currentPath === '/'}" on:click="{goHome}" aria-label="Files" title="Files">
    <Icon icon="mdi:folder-outline" />
  </button>

  <Popover strategy="fixed" placement="right-start" width="15rem" let:close>
    <button
      slot="trigger"
      let:toggle
      let:open
      class="rail-icon"
      class:active="{open}"
      class:empty="{allPins.length === 0}"
      on:click="{toggle}"
      aria-label="Pinned"
      aria-haspopup="menu"
      aria-expanded="{open}"
      title="Pinned"
    >
      <Icon icon="mdi:pin-outline" />
    </button>

    <span slot="header">Pinned</span>

    {#if allPins.length > 0}
      {#each allPins as pin, index}
        {@const isActive = !pin.isMissing && currentPath === `/library/${pin.id}`}
        {@const name =
          pin.display_name || (!pin.isMissing && 'current_item' in pin ? pin.current_item?.title : null) || 'Unknown'}
        <div class="popover-row-wrapper">
          <button
            class="popover-row"
            class:active="{isActive}"
            class:missing="{pin.isMissing}"
            data-popover-item
            data-selected="{isActive ? 'true' : undefined}"
            role="menuitem"
            tabindex="-1"
            on:click="{() => goToFolder(pin.id, pin.isMissing, close)}"
            title="{pin.last_seen_path}"
          >
            <Icon icon="mdi:pin-outline" class="icon" />
            <span class="row-name">{name}</span>
          </button>

          <button
            class="row-action unpin"
            class:missing="{pin.isMissing}"
            data-popover-item
            role="menuitem"
            tabindex="-1"
            on:click="{() => handleUnpin(index, pin.id, pin.isMissing, pin.display_name || undefined)}"
            aria-label="Unpin {name}"
            title="{pin.isMissing ? 'Remove this pin' : 'Unpin folder'}"
          >
            <Icon icon="mdi:close" />
          </button>
        </div>
      {/each}
    {:else}
      <div class="popover-empty">No pinned folders</div>
    {/if}
  </Popover>

  <Popover strategy="fixed" placement="right-start" width="15rem" let:close>
    <button
      slot="trigger"
      let:toggle
      let:open
      class="rail-icon"
      class:active="{open}"
      class:empty="{$collections.length === 0}"
      on:click="{toggle}"
      aria-label="Collections"
      aria-haspopup="menu"
      aria-expanded="{open}"
      title="Collections"
    >
      <Icon icon="mdi:layers-outline" />
    </button>

    <span slot="header" class="popover-header" let:close="{closeHeader}">
      <span>Collections</span>
      <button
        class="add-btn"
        data-popover-item
        role="menuitem"
        tabindex="-1"
        on:click="{() => openNewCollection(closeHeader)}"
        aria-label="New collection"
        title="New collection"
      >
        <Icon icon="mdi:plus" />
      </button>
    </span>

    {#if $collections.length > 0}
      {#each $collections as collection}
        {@const isActive = currentPath === `/collection/${collection.id}`}
        <div class="popover-row-wrapper">
          <button
            class="popover-row"
            class:active="{isActive}"
            data-popover-item
            data-selected="{isActive ? 'true' : undefined}"
            role="menuitem"
            tabindex="-1"
            on:click="{() => goToCollection(collection.id, close)}"
            title="{collection.name}"
          >
            <Icon icon="mdi:layers-outline" class="icon" />
            <span class="row-name">{collection.name}</span>
          </button>

          <button
            class="row-action options"
            data-popover-item
            role="menuitem"
            tabindex="-1"
            on:click="{() => openCollectionOptions(collection.id, collection.name, close)}"
            aria-label="Options for {collection.name}"
            title="Collection options"
          >
            <Icon icon="mdi:dots-horizontal" />
          </button>
        </div>
      {/each}
    {:else}
      <div class="popover-empty">No collections</div>
    {/if}
  </Popover>
</div>

<CollectionOptionsModal
  bind:open="{optionsModalOpen}"
  collectionId="{selectedCollectionId}"
  collectionName="{selectedCollectionName}"
/>
<NewCollectionModal bind:open="{newCollectionModalOpen}" />
