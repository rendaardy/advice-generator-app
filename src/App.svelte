<script lang="ts">
  import { onMount } from 'svelte';

  import patternDividerDesktop from './assets/images/pattern-divider-desktop.svg';
  import iconDice from './assets/images/icon-dice.svg';

  interface Slip {
    id: number;
    advice: string;
  }

  let slip: Slip;

  async function getRandomAdvice(): Promise<Slip> {
    const response = await fetch('https://api.adviceslip.com/advice');
    const data = await response.json();
    return data.slip;
  }

  async function generate(): Promise<void> {
    slip = await getRandomAdvice();
  }

  onMount(async () => {
    await generate();
  });
</script>

<main>
  <div class="container">
    {#if slip}
      <figure>
        <h4>advice #{slip.id}</h4>
        <blockquote cite="https://api.adviceslip.com/advice/{slip.id}">
          "{slip.advice}"
        </blockquote>
        <img class="divider" src={patternDividerDesktop} alt="divider" />
        <button on:click={generate} aria-label="Roll the dice">
          <img class="dice" src={iconDice} alt="dice" />
        </button>
      </figure>
    {/if}
  </div>
</main>

<style>
  .container {
    height: 100vh;
    display: grid;
    grid-template-columns: 1fr;
    place-items: center;
  }

  figure {
    position: relative;
    width: 64rem;
    padding: 4.8rem 4.8rem 6.4rem 4.8rem;
    border-radius: 17px;
    background-color: hsl(var(--color-dark-grayish-blue));
    box-shadow: 0 1.2rem 2.4rem rgba(0, 0, 0, 0.2);
    text-align: center;
  }

  h4 {
    font-size: 1.2rem;
    font-weight: 800;
    text-transform: uppercase;
    letter-spacing: 4px;
    color: hsl(var(--color-primary-2));
    margin-bottom: 2.4rem;
  }

  blockquote {
    font-size: 2.8rem;
    font-weight: 800;
    color: hsl(var(--color-primary-1));
    margin-bottom: 4.8rem;
  }

  .divider {
    width: 100%;
  }

  button {
    position: absolute;
    bottom: 0;
    left: calc(50% - (6.4rem / 2));
    transform: translateY(50%);
    width: 6.4rem;
    height: 6.4rem;
    border-radius: 50%;
    background-color: hsl(var(--color-primary-2));
    cursor: pointer;
    transition: transform 0.3s ease-in-out;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  button:hover {
    box-shadow: 0 0 2.4rem 0.6rem hsl(var(--color-primary-2));
  }
</style>
