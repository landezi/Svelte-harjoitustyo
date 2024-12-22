<script>
  import Modal from './Modal.svelte';
  import Otsikko from './otsikko.svelte';

  let manaCost;
  let attack;
  let health;
  let kortit = [];
  let naytaModal = false;

  function getCards() {
    const queryString = luoHakuUrl();
    fetch(
      `https://us.api.blizzard.com/hearthstone/cards?locale=en_US${queryString}`,
      {
        headers: { Authorization: `Bearer ${import.meta.env.VITE_API_KEY}` },
      }
    )
      .then((response) => response.json())
      .then((data) => {
        kortit = data.cards;
      });
  }

  function serialize(obj) {
    const str = [];
    for (const p in obj)
      if (obj.hasOwnProperty(p)) {
        str.push(encodeURIComponent(p) + '=' + encodeURIComponent(obj[p]));
      }
    return '&' + str.join('&');
  }

  function luoHakuUrl() {
    const queryString = serialize({ manaCost, attack, health });
    console.log(queryString);
    return queryString;
  }

  function avaaUusi() {
    naytaModal = true;
  }

  function suljeUusi() {
    naytaModal = false;
  }
</script>

<main>
  <Otsikko />

  <div>
    <p>Mana cost:</p>
    <input id="mana" type="number" bind:value={manaCost} />
    <button
      on:click={() => {
        getCards();
        avaaUusi();
      }}>Hae kortteja</button
    >
    <p>Attack:</p>
    <input id="attack" type="number" bind:value={attack} />
    <p>Health:</p>
    <input id="health" type="number" bind:value={health} />
  </div>

  {#if (kortit.length > 0, naytaModal)}
    <Modal>
      <button id="modalbutton" on:click={suljeUusi}>Sulje</button>
      <div>
        <h2>Kortit:</h2>
        <ul>
          {#each kortit as kortit}
            <li>
              <h3>{kortit.name}</h3>
              <img src={kortit.image} alt="" />
            </li>
          {/each}
        </ul>
      </div>
    </Modal>
  {/if}
</main>

<style>
  button {
    background-color: black;
    color: burlywood;
  }

  p {
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    size: 6cm;
    color: brown;
  }

  h2 {
    color: brown;
  }

  h3 {
    color: rgb(255, 255, 255);
  }

  #modalbutton {
    position: fixed;
    top: 10px;
    right: 10px;
    background-color: rgb(0, 0, 0);
    color: aliceblue;
    border-radius: 5vh;
    border-width: 7px;
  }
</style>
