<script>
  import { blur, scale } from 'svelte/transition';
</script>

<div class="backdrop" out:blur={{ duration: 350 }} />

<div class="modal" in:scale={{ duration: 300 }} out:blur={{ duration: 350 }}>
  <header>
    <slot name="header" />
  </header>
  <slot />
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>

<style>
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background: rgba(0, 0, 0, 0.75);
  }

  .modal {
    padding: 1rem;
    position: fixed;
    top: 10vh;
    left: 39%;
    background: transparent;
    z-index: 100;
    width: fit-content;
  }
</style>
