<script>
</script>

<div class="backdrop" />

<div class="modal">
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
    z-index: 10;
  }

  .modal {
    padding: 1rem;
    position: fixed;
    top: 10vh;
    left: 30%;
    max-height: 80vh;
    background: linear-gradient(rgba(28, 167, 236, 0), rgba(31, 47, 152, 0));
    border-radius: 5px;
    z-index: 100;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
    overflow: auto;
    width: 40%;
    text-align: center;
  }
</style>
