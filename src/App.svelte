<script>
  import arrow from './assets/arrow-down-solid.svg';

  const chars = 'AĄBCĆDEĘFGHIJKLŁMNŃOÓPQRSŚTUVWXYZŹŻ'
    .toLocaleLowerCase()
    .split('');

  let decrypted = '';
  let encrypted = '';

  let offset = 3;
  let isDecrypt = false;
  let isOpen = true;

  const bump = (event) => {
    const { target } = event;

    target.style.bottom = '1rem';
    target.style.backgroundColor = 'rgba(255, 255, 255, 0.25)';

    setTimeout(() => {
      target.style.bottom = '0';
      target.style.backgroundColor = 'black';
    }, 100);
  };

  const encrypt = () => {
    decrypted = decrypted.toLocaleLowerCase();

    encrypted = decrypted
      .split('')
      .filter((char) => chars.includes(char))
      .map((char) => {
        const index = chars.indexOf(char);

        return index + offset < chars.length
          ? chars[index + offset]
          : chars[index + offset - chars.length];
      })
      .join('');
  };

  const decrypt = () => {
    encrypted = encrypted.toLocaleLowerCase();

    decrypted = encrypted
      .split('')
      .filter((char) => chars.includes(char))
      .map((char) => {
        const index = chars.indexOf(char);

        return index - offset >= 0
          ? chars[index - offset]
          : chars[chars.length - index - offset];
      })
      .join('');
  };

  const handleOffset = (event) => {
    const { target } = event;
    const inputEvent = new Event('input');

    if (target.value > chars.length - 1) {
      target.value = chars.length - 1;

      target.dispatchEvent(inputEvent);
    }

    if (target.value < 1) {
      target.value = Math.abs(target.value) || 1;

      target.dispatchEvent(inputEvent);
    }

    if (isDecrypt) {
      decrypt();

      return;
    }

    encrypt();
  };
</script>

<main>
  {#if isOpen}
    <div class="window">
      <h1>Marcin Włodyka</h1>

      <h2>indeks: 13860</h2>

      <p>podpowiedź: kliknij na strzałki aby przejść z szyfrowania na deszyfrowania</p>

      <button on:click={() => (isOpen = false)}> close </button>
    </div>
  {/if}

  <div class={isOpen ? 'inputs blur' : 'inputs'}>
    <label for="decrypted" disabled={isDecrypt}>odszyfrowane:</label>

    <textarea
      id="decrypted"
      name="decrypted"
      bind:value={decrypted}
      on:input={encrypt}
      on:input={bump}
      disabled={isDecrypt}
    />

    <div class="mid">
      <img
        src={arrow}
        alt="arrow"
        class={isDecrypt ? 'up' : ''}
        on:click={() => (isDecrypt = !isDecrypt)}
      />

      <div>
        <label for="offset">przesunięcie:</label>

        <input
          type="number"
          name="offset"
          id="offset"
          min="1"
          bind:value={offset}
          on:input={handleOffset}
        />
      </div>

      <img
        src={arrow}
        alt="arrow"
        class={isDecrypt ? 'up' : ''}
        on:click={() => (isDecrypt = !isDecrypt)}
      />
    </div>

    <label for="encrypted">zaszyfrowane:</label>

    <textarea
      id="encrypted"
      name="encrypted"
      bind:value={encrypted}
      on:input={decrypt}
      on:input={bump}
      disabled={!isDecrypt}
    />
  </div>
</main>

<style>
  :root {
    --grey-light: rgb(137, 137, 137);
    --grey-dark: rgb(10, 10, 10);

    --space-sm: 0.25rem;
    --space-md: 0.75rem;
    --space-lg: 2.5rem;

    --border: 0.35rem solid white;
    --border-grey: 0.35rem solid var(--grey-light);
    --rounded: 0.75rem;
    --trans: 0.5s ease;
  }

  main {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .window {
    padding: var(--space-lg);
    position: absolute;

    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-sm);

    background-color: rgba(0, 0, 0, 0.8);
    backdrop-filter: blur(3px);
    border: var(--border);
    border-radius: var(--rounded);

    text-align: center;
    z-index: 10;
  }

  .window > p {
    margin: var(--space-md);
  }

  button {
    width: 10rem;
    margin: var(--space-md);
    padding: var(--space-md);

    color: white;
    background-color: black;
    border: var(--border);
    border-radius: var(--rounded);

    transition: var(--trans);
  }

  button:hover {
    background-color: white;
    color: black;

    cursor: pointer;
  }

  label {
    font-size: 2rem;
    font-weight: bold;
  }

  textarea {
    height: 15rem;
    width: 40rem;
    position: relative;
    padding: var(--space-md);
    font-size: 1.75rem;

    color: white;
    background-color: black;
    border: var(--border);
    border-radius: var(--rounded);

    resize: none;
    transition: var(--trans);
    transition: var(--trans), bottom 0.125s ease-out;
  }

  textarea:disabled {
    color: var(--grey-light);
    background-color: var(--grey-dark);
    border: var(--border-grey);
    animation-name: none;
  }

  img {
    width: 2.5rem;

    transition: var(--trans);
    cursor: pointer;
  }

  img.up {
    transform: rotate(180deg);
  }

  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
  }

  input {
    width: 2rem;
    padding: var(--space-md);
    font-size: 1.125rem;

    color: white;
    background-color: black;
    border: var(--border);
    border-radius: var(--rounded);

    text-align: center;
  }

  .inputs {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-md);

    transition: var(--trans);
  }

  .inputs.blur {
    filter: blur(2px);
    opacity: 0.2;
  }

  .mid {
    margin: var(--space-lg);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: var(--space-lg);
  }

  .mid > div {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-md);
  }
</style>
