<script>
  function getCards() {
    const queryString = luoHakuUrl();
    fetch(
      `https://us.api.blizzard.com/hearthstone/cards?locale=en_US${queryString}`,
      {
        headers: { Authorization: `Bearer ${import.meta.env.VITE_API_KEY}` },
      }
    );
  }

  let manaCost;
  let attack;

  function serialize(obj) {
    const str = [];
    for (const p in obj)
      if (obj.hasOwnProperty(p)) {
        str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]));
      }
    return '&' + str.join('&');
  }

  function luoHakuUrl() {
    const queryString = serialize({ manaCost, attack });
    console.log(queryString);
    return queryString;
  }
</script>

<main>
  <input id="name" type="text" bind:value={manaCost} />
  <input id="name" type="text" bind:value={attack} />
  <button on:click={() => getCards()}>Hae kortteja</button>
</main>

<style>
  button {
    background-color: black;
    color: burlywood;
  }
</style>
