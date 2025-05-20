<script>
  import '98.css';
  import "../styles/global.css"
  import start from "../assets/bottom_bar/windows.png"
  import image from "../assets/folders/image.png"
  import StartMenu from "../components/start_menu.svelte";
  import { goto } from '$app/navigation';

  let showMenu = false;

  const menuItems = [
    { label: 'Blog', icon: image, action: () => goto('/blog_folder') },
    { label: 'Resume', icon: image, action: () => goto('/resume') },
    { label: 'Kenz.exe', icon: image, action: () => goto('/about') },
    { label: 'Windows Music Player', icon: image, action: () => goto('/music_player') },
    { label: 'Calculator', icon: image, action: () => goto('/calculator') },
  ];


  const now = new Date();

  let hours = now.getHours();
  const minutes = now.getMinutes().toString().padStart(2, '0');
  const ampm = hours >= 12 ? 'PM' : 'AM';

  hours = hours % 12;
  hours = hours ? hours : 12; // the hour '0' should be '12'

  const timeString = `${hours}:${minutes} ${ampm}`;
</script>

<div class="desktop">

  <slot />
  <nav class="bottom-bar">
    <StartMenu isOpen={showMenu} items={menuItems} on:select={handleSelect} />
    <button on:click={() => (showMenu = !showMenu)} >
      <image src={start} alt="Start" class="start-button" />
      <span class="start-text">Start</span>
    </button>
    <fieldset>
      {timeString} 
    </fieldset>
  </nav>
</div>