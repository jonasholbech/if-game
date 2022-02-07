<script>
  import { fade, fly } from "svelte/transition";
  let statements = [
    { expression: "2>1", evaluated: true, code: `` },
    { expression: "1", evaluated: true, code: `` },
    { expression: "0", evaluated: false, code: `` },
    { expression: "x", evaluated: true, code: `let x = 2` },
    { expression: "name", evaluated: false, code: `let name = ""` },
    { expression: "age", evaluated: false, code: `let age` },
    {
      expression: "items.length",
      evaluated: true,
      code: `let items = ["beer", "cabbage"];`,
    },
    { expression: "hobby", evaluated: false, code: `` },
    { expression: "drinksLeft", evaluated: false, code: `let drinksLeft = 0` },
    { expression: "balance", evaluated: true, code: `let balance = -10000` },
    { expression: "sleepy", evaluated: false, code: `let sleepy = false` },
    {
      expression: "shouldSleep",
      evaluated: true,
      code: `let shouldSleep = true`,
    },
    {
      expression: "!shouldSleep",
      evaluated: false,
      code: `let shouldSleep = true`,
    },
    { expression: "sleepy", evaluated: true, code: `let sleepy = "false"` },
    { expression: "'sleepy'", evaluated: true, code: `let sleepy = false` },
  ];

  let count = 0;
  let isWrong = false;
  let done = false;
  function evaluate(bool) {
    if (bool == statements[count].evaluated) {
      count += 1;
      if (count >= statements.length) {
        done = true;
        count -= 1;
      }
    } else {
      isWrong = true;
    }
  }
</script>

<main>
  {#if !done}
    <code
      >{#key count}<span in:fly={{ y: -20 }}>{statements[count].code}</span
        >{/key}</code
    >
    <code class:isWrong on:animationend={() => (isWrong = false)}>
      if ({#key count}<span in:fly={{ y: -20 }}
          >{statements[count].expression}</span
        >{/key}) &#123;
      <button on:click={() => evaluate(true)}>True</button>
      &#125; else &#123;
      <button on:click={() => evaluate(false)}>False</button>
      &#125;
    </code>
  {:else}
    <p>Well done</p>
  {/if}
  <h1 class="points">{count + 1} / {statements.length}</h1>
</main>

<style>
  .isWrong {
    animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
    transform: translate3d(0, 0, 0);
    backface-visibility: hidden;
    perspective: 1000px;
  }
  @keyframes shake {
    10%,
    90% {
      transform: translate3d(-1px, 0, 0);
    }

    20%,
    80% {
      transform: translate3d(2px, 0, 0);
    }

    30%,
    50%,
    70% {
      transform: translate3d(-4px, 0, 0);
    }

    40%,
    60% {
      transform: translate3d(4px, 0, 0);
    }
  }
  code {
    font-size: 1.4rem;
  }
  button {
    padding: 0.3rem;
    margin: 2rem;
    display: block;
    width: auto;
  }
  span {
    display: inline-block;
    min-width: 150px;
    text-align: center;
  }
  main {
    display: grid;
    grid-template-columns: 1fr 70ch 1fr;
    position: relative;
  }
  main > * {
    grid-column: 2;
  }
  .points {
    position: absolute;
    top: 50px;
    right: 50px;
  }
</style>
