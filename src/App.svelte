<script>
  import Modal from './Modal.svelte';
  import Otsikko from './otsikko.svelte';

  let manaCost;
  let attack;
  let health;
  let kortit = [];
  let naytaModal = false;
  let kortinkuva = null;
  $: tyhjakentta = !(manaCost >= 0 && attack >= 0 && health >= 0);

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

  function avaaUusi(kortti) {
    naytaModal = true;
    kortinkuva = kortti;
  }

  function suljeUusi() {
    naytaModal = false;
    kortinkuva = null;
  }
</script>

<main>
  <Otsikko />

  <div id="syotto">
    <label for="mana">Mana cost:</label>
    <input id="mana" type="number" bind:value={manaCost} />
    <label for="attack">Attack:</label>
    <input id="attack" type="number" bind:value={attack} />
    <label for="health">Health:</label>
    <input id="health" type="number" bind:value={health} />
    <button id="haekortteja" on:click={getCards} disabled={tyhjakentta}
      >Hae kortteja</button
    >
  </div>

  {#if kortit.length > 0}
    <div>
      <h2>Kortit:</h2>
      <ul>
        {#each kortit as kortti}
          <li>
            <button id="kortinnimi" on:click={() => avaaUusi(kortti)}
              >{kortti.name}</button
            >
          </li>
        {/each}
      </ul>
    </div>
  {/if}

  {#if naytaModal}
    <Modal>
      <button id="modalbutton" on:click={suljeUusi}>Sulje</button>
      <div>
        <h3>{kortinkuva.name}</h3>
        <img src={kortinkuva.image} alt="kortinkuva" />
      </div>
    </Modal>
  {/if}
</main>

<style>
  button {
    background-color: black;
    color: burlywood;
  }

  #syotto {
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    size: 6cm;
    color: brown;
  }

  h2 {
    color: brown;
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

  #kortinnimi {
    background: rgb(21, 66, 71);
    color: aliceblue;
    transition:
      background-color 0.1s ease,
      transform 0.1s ease;
  }

  button:hover {
    transform: scale(1.05);
    background-color: #b12a1b;
  }

  h3 {
    color: white;
    text-align: center;
  }

  button:disabled {
    color: rgb(255, 255, 255);
    background-color: grey;
    transform: scale(1);
  }
</style>
