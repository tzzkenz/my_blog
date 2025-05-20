<script>
  import { goto } from "$app/navigation";

  let display = "";
  let memory = 0;

  function handleClick(value) {
    if (value === "C") {
      display = "";
    } else if (value === "CE") {
      display = "";
    } else if (value === "Back") {
      display = display.slice(0, -1);
    } else if (value === "=") {
      try {
        display = eval(display).toString();
      } catch (e) {
        display = "Error";
      }
    } else if (value === "sqrt") {
      try {
        display = Math.sqrt(eval(display)).toString();
      } catch {
        display = "Error";
      }
    } else if (value === "1/x") {
      try {
        display = (1 / eval(display)).toString();
      } catch {
        display = "Error";
      }
    } else if (value === "+/-") {
      if (display.startsWith("-")) {
        display = display.slice(1);
      } else {
        display = "-" + display;
      }
    } else if (value === "%") {
      try {
        display = (eval(display) / 100).toString();
      } catch {
        display = "Error";
      }
    } else if (value === "MC") {
      memory = 0;
    } else if (value === "MR") {
      display += memory.toString();
    } else if (value === "MS") {
      try {
        memory = eval(display);
      } catch {
        memory = 0;
      }
    } else if (value === "M+") {
      try {
        memory += eval(display);
      } catch {
        memory = memory;
      }
    } else {
      display += value;
    }
  }
</script>

<div class="window" style="width: 500px">
  <div class="title-bar">
    <div class="title-bar-text">Calculator</div>
    <div class="title-bar-controls">
      <button aria-label="Minimize"></button>
      <button aria-label="Maximize"></button>
      <button aria-label="Close" on:click={() => goto('/')}></button>
    </div>
  </div>

  <div class="window-body calculator-body">
    <input type="text" class="calculator-input" bind:value={display}  />

    <!-- First row: 4 buttons -->
    <div class="row-grid row-1">
      <button disabled aria-label="deco"> </button>
      <button on:click={() => handleClick("Back")}>Back</button>
      <button on:click={() => handleClick("CE")}>CE</button>
      <button on:click={() => handleClick("C")}>C</button>
    </div>

    <!-- Other rows: 6 buttons -->
    <div class="row-grid row-2">
      <button id="red" on:click={() => handleClick("MC")}>MC</button>
      <button on:click={() => handleClick("7")}>7</button>
      <button on:click={() => handleClick("8")}>8</button>
      <button on:click={() => handleClick("9")}>9</button>
      <button on:click={() => handleClick("/")}>/</button>
      <button on:click={() => handleClick("sqrt")}>sqrt</button>

      <button id="red" on:click={() => handleClick("MR")}>MR</button>
      <button on:click={() => handleClick("4")}>4</button>
      <button on:click={() => handleClick("5")}>5</button>
      <button on:click={() => handleClick("6")}>6</button>
      <button on:click={() => handleClick("*")}>*</button>
      <button on:click={() => handleClick("%")}>%</button>

      <button id="red" on:click={() => handleClick("MS")}>MS</button>
      <button on:click={() => handleClick("1")}>1</button>
      <button on:click={() => handleClick("2")}>2</button>
      <button on:click={() => handleClick("3")}>3</button>
      <button on:click={() => handleClick("-")}>-</button>
      <button on:click={() => handleClick("1/x")}>1/x</button>

      <button id="red" on:click={() => handleClick("M+")}>M+</button>
      <button on:click={() => handleClick("0")}>0</button>
      <button on:click={() => handleClick("+/-")}>+/-</button>
      <button on:click={() => handleClick(".")}>.</button>
      <button on:click={() => handleClick("+")}>+</button>
      <button on:click={() => handleClick("=")}>=</button>
    </div>
  </div>
</div>

<style>
  .calculator-body {
    padding: 8px;
    box-sizing: border-box;
  }

  .calculator-input {
    width: 100%;
    height: 28px;
    font-size: 14px;
    margin-bottom: 8px;
    box-sizing: border-box;
    padding: 4px;
  }

  .row-grid {
    display: grid;
    gap: 2px;
    margin-bottom: 8px;
  }

  .row-1 {
    grid-template-columns: repeat(4, 1fr);
  }

  .row-2 {
    grid-template-columns: repeat(6, 1fr);
  }

  .row-1 button,
  .row-2 button {
    font-size: 13px;
    font-family: inherit;
    padding: 4px;
    cursor: pointer;
  }

  #red {
    color: red;
  }

  .title-bar-text {
    font-weight: bold;
  }

  .window-body {
    overflow: hidden;
  }
</style>
