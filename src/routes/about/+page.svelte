<script>
  import { goto } from "$app/navigation";
  import { onMount } from 'svelte';
  import dev_icon from "../../assets/music_player/icons/next.png";

  let currentTab = 'desktop';

  const setTab = (tab) => {
    currentTab = tab;
  };
</script>

<div class="window kenz-exe" style="width: 380px;">
  <div class="title-bar">
    <div class="title-bar-text">
      <img src={dev_icon} alt="icon" class="exe-icon" />
      kenz.exe
    </div>
    <div class="title-bar-controls">
      <button aria-label="Minimize"></button>
      <button aria-label="Maximize"></button>
      <button aria-label="Close" on:click={() => goto('/')}></button>
    </div>
  </div>

  <div class="window-body">
    <menu role="tablist">
      <li role="tab" aria-selected={currentTab === 'desktop'}>
        <a on:click|preventDefault={() => setTab('desktop')}>Desktop</a>
      </li>
      <li role="tab" aria-selected={currentTab === 'mycomputer'}>
        <a on:click|preventDefault={() => setTab('mycomputer')}>My Computer</a>
      </li>
      <li role="tab" aria-selected={currentTab === 'controlpanel'}>
        <a on:click|preventDefault={() => setTab('controlpanel')}>Control Panel</a>
      </li>
    </menu>

    <div class="window inner-tab" role="tabpanel">
      <div class="window-body profile">
        {#if currentTab === 'desktop'}
          <div class="section">
            <div class="section-header">System Properties</div>
            <div class="section-content">
              <p><strong>👤 User:</strong> Kenz E S</p>
              <p><strong>🧑‍💻 Role:</strong> Full Stack Dev & Designer</p>
              <p><strong>🏫 College:</strong> S6 CSE, CET</p>
              <p><strong>💻 System Uptime:</strong> Always on caffeine ☕</p>
            </div>
          </div>
        {/if}

        {#if currentTab === 'mycomputer'}
          <div class="section">
            <div class="section-header">Installed Software</div>
            <div class="section-content">
              <p><strong>🧠 Installed Stack:</strong></p>
              <ul>
                <li>🟢 MongoDB</li>
                <li>⚙️ Express.js</li>
                <li>⚛️ React / Svelte / Next.js</li>
                <li>🎨 Tailwind CSS</li>
                <li>🔧 Git, VS Code, Postman</li>
              </ul>
              <p><strong>📁 Hobbies Directory:</strong> Designing, Gaming, Figma-ing</p>
            </div>
          </div>
        {/if}

        {#if currentTab === 'controlpanel'}
          <div class="section">
            <div class="section-header">System Settings</div>
            <div class="section-content">
              <p><strong>🚀 Current Mission:</strong> Bringing back the 90s, one pixel at a time</p>
              <p><strong>✨ Dream Mode:</strong> Build nostalgic tools that feel magical</p>
              <p><strong>📂 Featured Projects:</strong></p>
              <ul>
                <li>🎤 Aaravam 2024</li>
                <li>🤝 Helping Hands</li>
                <li>💸 Trinity Transactions</li>
              </ul>
            </div>
          </div>
        {/if}
      </div>
    </div>
    
    <div class="status-bar">
      <div class="status-bar-field">Ready</div>
      <div class="status-bar-field">CPU Usage: 42%</div>
      <div class="status-bar-field">Memory: 64MB / 128MB</div>
    </div>
  </div>
</div>

<style>
  .exe-icon {
    width: 16px;
    height: 16px;
    margin-right: 5px;
    vertical-align: text-bottom;
  }

  .kenz-exe .title-bar-text {
    display: flex;
    align-items: center;
  }

  menu[role="tablist"] {
    margin: 8px 0;
    padding: 0;
    display: flex;
    gap: 4px;
    font-size: 12px;
  }

  menu[role="tablist"] li {
    list-style: none;
    border: 1px solid #808080;
    border-bottom: none;
    padding: 3px 8px;
    background-color: #d4d0c8;
    cursor: pointer;
    border-top-left-radius: 3px;
    border-top-right-radius: 3px;
  }

  menu[role="tablist"] li a {
    text-decoration: none;
    color: #000;
  }

  menu[role="tablist"] li[aria-selected="true"] {
    background-color: #ece9d8;
    font-weight: bold;
    position: relative;
    top: 1px;
    z-index: 1;
    border-bottom: 1px solid #ece9d8;
  }

  .inner-tab {
    margin-top: -1px;
    border: 1px solid #808080;
    position: relative;
    background-color: #ece9d8;
    padding: 10px;
  }

  .profile p, .profile ul {
    margin: 6px 0;
    font-size: 12px;
    line-height: 1.4;
    font-family: "MS Sans Serif", "Segoe UI", sans-serif;
  }

  .profile ul {
    padding-left: 20px;
    list-style-type: square;
  }

  .window-body {
    background-color: #d4d0c8;
    display: flex;
    flex-direction: column;
  }
  
  .section {
    margin-bottom: 10px;
    border: 1px solid #808080;
    border-radius: 0;
    background-color: #d4d0c8;
  }
  
  .section-header {
    background: linear-gradient(to right, #000080, #1084d0);
    color: white;
    padding: 4px 8px;
    font-weight: bold;
    font-size: 12px;
  }
  
  .section-content {
    padding: 8px;
    background-color: #ece9d8;
  }
  
  .status-bar {
    display: flex;
    margin-top: 10px;
    border-top: 1px solid #808080;
    height: 20px;
    font-size: 11px;
    padding: 2px;
    background-color: #d4d0c8;
  }
  
  .status-bar-field {
    padding: 0 8px;
    border-right: 1px solid #808080;
  }
  
  .status-bar-field:first-child {
    font-weight: normal;
  }
</style>