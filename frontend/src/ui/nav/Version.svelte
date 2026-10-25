<style>
  .version {
    display: flex;
    align-items: center;
    align-self: center;
    gap: 10px;
    color: var(--text-color-light);
    font-size: var(--font-sm);
    line-height: 1.3;
  }

  .version-text {
    display: flex;
    flex-direction: column;
    text-align: center;
  }

  .version-name {
    color: var(--dark-5);
    font-size: var(--font-xs);
  }

  .hidden {
    visibility: hidden;
  }

  .bmc-link {
    all: unset;
    cursor: pointer;
    width: 30px;
    height: 30px;
    border-radius: var(--radius-sm);
    display: inline-flex;
    align-items: center;
    justify-content: center;
    color: var(--text-color-light);
    transition:
      color 0.1s,
      background-color 0.1s;
  }

  .bmc-link:hover {
    background: var(--card-color);
    color: var(--color-primary-400);
  }

  .bmc-link:focus-visible {
    box-shadow: var(--focus-ring);
  }
</style>

<script lang="ts">
  import Icon from '@iconify/svelte'

  const version = __APP_VERSION__
  const codename = __APP_CODENAME__

  export let compact = false

  const BMC_URL = 'https://buymeacoffee.com/ar_kenny'
</script>

<div class="version">
  <span class="version-text">
    <span class="version-number">{compact ? version : `Version ${version}`}</span>
    <span class="version-name" class:hidden="{compact}">({codename})</span>
  </span>
  <a
    class="bmc-link"
    href="{BMC_URL}"
    target="_blank"
    rel="noopener noreferrer"
    aria-label="Buy me a coffee"
    title="Buy me a coffee"
  >
    <Icon icon="mdi:coffee" width="{compact ? 18 : 20}" />
  </a>
</div>
