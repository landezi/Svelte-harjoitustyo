<script>
  import Validointi from './Validointi.svelte';
  import Modal from './Modal.svelte';
  import Otsikko from './otsikko.svelte';
  import { fly } from 'svelte/transition';

  let manaCost = null;
  let attack = null;
  let health = null;
  let kortit = [];
  let naytaModal = false;
  let kortinkuva = null;

  const validiNumero = (arvo) => {
    return arvo !== null && arvo >= 0;
  };

  $: validiMana = validiNumero(manaCost);
  $: validiAttack = validiNumero(attack);
  $: validiHealth = validiNumero(health);
  $: voiHakea = validiMana && validiAttack && validiHealth;

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
    <div class="kentta">
      <label for="mana">Mana cost:</label>
      <Validointi
        bind:arvo={manaCost}
        onkoValidi={validiMana}
        virheviesti="Ilmoita kortin Mana cost!"
      />
    </div>
    <div class="kentta">
      <label for="attack">Attack:</label>
      <Validointi
        bind:arvo={attack}
        onkoValidi={validiAttack}
        virheviesti="Ilmoita kortin Attack!"
      />
    </div>
    <div class="kentta">
      <label for="health">Health:</label>
      <Validointi
        bind:arvo={health}
        onkoValidi={validiHealth}
        virheviesti="Ilmoita kortin Health!"
      />
    </div>
    <button id="haekortteja" on:click={getCards} disabled={!voiHakea}>
      Hae kortteja
    </button>
  </div>

  {#if kortit.length > 0}
    <div>
      <h2>Kortit:</h2>
      <ul>
        {#each kortit as kortti, index (kortti.id)}
          <div in:fly={{ duration: 1000, x: 300, y: 0, delay: index * 200 }}>
            <button id="kortinnimi" on:click={() => avaaUusi(kortti)}>
              {kortti.name}
            </button>
          </div>
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
  #syotto {
    display: flex;
    align-items: flex-start;
    gap: 1rem;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    color: brown;
  }

  .kentta {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .kentta label {
    margin-bottom: 0.5rem;
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

  button {
    background-color: black;
    color: burlywood;
    height: fit-content;
    align-self: center;
    padding: 0.5rem 1rem;
    border-radius: 0.25rem;
    cursor: pointer;
    margin-top: 15px;
    margin-left: 15px;
  }

  button:disabled {
    background-color: grey;
    cursor: not-allowed;
  }
</style>
