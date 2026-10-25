<style>
  .toggle-wrapper {
    display: inline-block;
  }

  .toggle {
    position: relative;
    display: inline-block;
    width: 3rem;
    height: 1.5rem;
  }

  .toggle input {
    opacity: 0;
    width: 0;
    height: 0;
  }

  .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: var(--toggle-track, var(--card-color));
    border: 0.15rem solid var(--separator-1);
    border-radius: var(--radius-xl);
    transition: all 0.2s ease;
  }

  .slider:before {
    position: absolute;
    content: '';
    height: 1rem;
    width: 1rem;
    left: 0.15rem;
    bottom: 0.1rem;
    background-color: var(--text-color-light);
    border-radius: var(--radius-full);
    transition: all 0.2s ease;
  }

  input:checked + .slider {
    background-color: var(--button-primary);
    border-color: var(--button-primary);
  }

  input:checked + .slider:before {
    transform: translateX(1.5rem);
    /* Complements the checked slider background (--button-primary).
       Themes that use a light primary color provide a dark value
       (e.g. monochrome) so the knob stays visible. */
    background-color: var(--toggle-knob-checked, white);
  }

  input:focus + .slider {
    box-shadow: 0 0 0.3rem var(--button-primary);
  }

  input:disabled + .slider {
    opacity: 0.5;
    cursor: not-allowed;
  }

  .slider:hover:not(input:disabled + .slider) {
    box-shadow: 0 0 0.2rem var(--separator-1);
  }
</style>

<script lang="ts">
  export let checked = false
  export let disabled = false
  export let ariaLabel = 'Toggle'
</script>

<div class="toggle-wrapper">
  <label class="toggle">
    <input
      type="checkbox"
      bind:checked="{checked}"
      disabled="{disabled}"
      aria-label="{ariaLabel}"
    />
    <span class="slider"></span>
  </label>
</div>
