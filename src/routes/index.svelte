<script>
  let frequency = 1
  let colors = ['#ffffff', '#000000']
  let colorsCount = colors.length
  let currentColorIndex = 0
  let intervalId = undefined
  let isMenuDisplayed = true
  $: currentColor = colors[currentColorIndex]
  $: oscillationPeriod = (1 / frequency) * 1000

  $: {
    if (colorsCount > 1) {
      colors.length = colorsCount
    }
  }

  $: {
    clearInterval(intervalId)
    intervalId = setInterval(() => {
      if (currentColorIndex + 1 < colors.length) {
        currentColorIndex += 1
      } else {
        currentColorIndex = 0
      }
    }, oscillationPeriod)
  }
</script>

<style>
  :global(html, body, #sapper) {
    width: 100%;
    height: 100%;
  }

  main {
    width: 100%;
    height: 100%;

    display: flex;
    justify-content: center;
    align-items: center;
  }

  main > * {
    position: absolute;
  }

  .screen {
    width: 100%;
    height: 100%;
  }

  menu {
    background: white;
    border: 2px solid black;
    border-radius: 20px;
    margin: 0;
    padding: 20px;
    text-align-last: right;
  }

  menu > * {
    display: block;
    margin: 10px 0;
  }

  input:first-child {
    margin-left: 10px;
  }

  input[type='number'] {
    width: 50px;
    text-align: center;
  }

  input[type='number']::-webkit-inner-spin-button,
  input[type='number']::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  input[type='color'] {
    width: 20px;
    border: none;
  }
</style>

<svelte:head>
  <title>Oscillator</title>
</svelte:head>

<main>
  <div
    class="screen"
    style="background-color: {currentColor}; cursor: {isMenuDisplayed ? 'auto' : 'none'}"
    on:click={() => (isMenuDisplayed = !isMenuDisplayed)} />

  <menu style="display: {isMenuDisplayed ? 'block' : 'none'}">
    <label>
      Frequency:
      <input type="number" bind:value={frequency} />
    </label>
    <label>
      Colors:
      {#each colors as color}
        <input type="color" bind:value={color} />
      {/each}
    </label>
    <label>
      Colors count:
      <input type="number" bind:value={colorsCount} />
    </label>
  </menu>
</main>
