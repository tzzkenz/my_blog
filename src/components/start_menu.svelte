<script>
  export let isOpen = false;
  export let items = [];

  function handleClick(item) {
    const event = new CustomEvent("select", {
      detail: item,
    });
    dispatchEvent(event);
  }
</script>

{#if isOpen}
  <div class="start-menu">
    <div class="left-bar">
      <span class="win-label"><span class="win-label-windows">Windows</span>98</span>
    </div>
    <ul class="menu-items">
      {#each items as item}
      <li class="menu-item" on:click={item.action}>
        <img src={item.icon} alt={item.label} class="menu-icon" />
        {item.label}
      </li>
      {/each}
    </ul>
  </div>
{/if}

<style>
  .start-menu {
    position: absolute;
    border: 2px solid #f9f9f9;
    border-bottom : 0;
    bottom: 32px;
    left: 0;
    width: 200px;
    background: #c3c3c3;
    z-index: 1000;
  }

  .left-bar {
    position: absolute;
    top: 0;
    left: 0;
    width: 20px;
    height: 100%;
    background: #010081;
    display: flex;
    align-items: flex-end;
  }

  .menu-items {
    list-style: none;
    padding: 0;
    padding-left: 20px;
    margin: 0;
  }

  .menu-item {
    display: flex;
    align-items: center;
    padding: 8px 10px;
    cursor: pointer;
  }

  .menu-item:hover {
    background: #010081;
    color: white;
  }

  .menu-icon {
    width: 20px;
    height: 20px;
    margin-right: 10px;
  }

  .win-label {
    color: white;
    font-size: 14px;
    writing-mode: vertical-rl;
    transform: rotate(180deg);
    white-space: nowrap;
    font-family: "Tahoma", sans-serif;
    border: 2px solid #010081;
  }
  
  .win-label-windows {
    font-weight: bold;
    background-color: #5252e3;
    margin-bottom: 2px;
  }

</style>
