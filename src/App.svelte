<script>
  function getCards() {
    fetch('https://us.api.blizzard.com/hearthstone/cards?locale=en_US', {
      headers: { Authorization: `Bearer ${import.meta.env.VITE_API_KEY}` },
    });
  }
</script>

<main>
  <button on:click={() => getCards()}>Hae kortteja</button>
</main>

<style>
  button {
    background-color: black;
    color: burlywood;
  }
</style>
