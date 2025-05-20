<script>
  import { goto } from "$app/navigation";
  import image from "../../assets/bottom_bar/windows.png"
  import track1 from "../../assets/music_player/track_1.mp3"
  import track2 from "../../assets/music_player/track_2.mp3"
  import play_icon from "../../assets/music_player/icons/play.png"
  import pause_icon from "../../assets/music_player/icons/pause.png"
  import previous_icon from "../../assets/music_player/icons/previous.png"
  import next_icon from "../../assets/music_player/icons/next.png"
  import repeat_icon from "../../assets/music_player/icons/repeat.png"
  import volume_up_icon from "../../assets/music_player/icons/volume_up.png"
  import volume_down_icon from "../../assets/music_player/icons/volume_down.png"
  import divide from "../../assets/music_player/album_covers/divide.jpeg"
  import revival from "../../assets/music_player/album_covers/revival.jpeg"
  import { onDestroy, onMount } from "svelte";

  let songs = [
    {
      title: "Perfect",
      artist: "Ed Sheeran",
      album: "Divide",
      src: track1,
      cover: divide,
    },
    {
      title: "Same old love",
      artist: "Selena Gomez",
      album: "Revival",
      src: track2,
      cover: revival,
    },
  ];

  let audio = null;
  let currentTrackIndex = 0;
  let currentTime = 0;
  let duration = 0;
  let progressInterval;
  let isPlaying = false;
  
  // Format time in MM:SS format
  function formatTime(seconds) {
    seconds = Math.floor(seconds);
    const minutes = Math.floor(seconds / 60);
    seconds = seconds % 60;
    return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
  }

  function startProgressTracking() {
    if (progressInterval) {
      clearInterval(progressInterval);
    }
    
    progressInterval = setInterval(() => {
      if (audio) {
        currentTime = audio.currentTime;
        duration = audio.duration || 0;
      }
    }, 1000);
  }

  async function loadAudio() {
    if (audio) {
      // Pause and clean up existing audio
      audio.pause();
      audio.src = "";
      audio = null;
    }

    // Load new audio
    audio = new Audio(songs[currentTrackIndex].src);
    audio.onended = () => next(); // Auto play next when ended
    
    // Wait for metadata to load to get duration
    audio.onloadedmetadata = () => {
      duration = audio.duration;
    };

    startProgressTracking();
    isPlaying = true;

    try {
      await audio.play();
    } catch (err) {
      console.error("Playback failed:", err);
      isPlaying = false;
    }
  }

  async function play() {
    if (!audio) {
      await loadAudio();
    } else {
      try {
        await audio.play();
        startProgressTracking();
        isPlaying = true;
      } catch (err) {
        console.error("Playback failed:", err);
        isPlaying = false;
      }
    }
  }

  function pause() {
    if (audio) {
      audio.pause();
      isPlaying = false;
      if (progressInterval) {
        clearInterval(progressInterval);
        progressInterval = null;
      }
    }
  }

  function restart() {
    if (audio) {
      audio.currentTime = 0;
      audio.play();
      isPlaying = true;
    }
  }

  function next() {
    currentTrackIndex = (currentTrackIndex + 1) % songs.length;
    loadAudio();
  }

  function previous() {
    currentTrackIndex =
      (currentTrackIndex - 1 + songs.length) % songs.length;
    loadAudio();
  }

  function repeat() {
    if (audio) {
      audio.currentTime = 0;
      audio.play();
      isPlaying = true;
    }
  }

  function volumeUp() {
    if (audio && audio.volume < 1.0) {
      audio.volume = Math.min(1.0, audio.volume + 0.1);
    }
  }

  function volumeDown() {
    if (audio && audio.volume > 0.0) {
      audio.volume = Math.max(0.0, audio.volume - 0.1);
    }
  }

  // Seek to a new position when progress bar is clicked
  function seek(event) {
    if (audio && audio.duration) {
      const progressBar = event.currentTarget;
      const rect = progressBar.getBoundingClientRect();
      const position = (event.clientX - rect.left) / rect.width;
      audio.currentTime = position * audio.duration;
      currentTime = audio.currentTime;
    }
  }

  // Toggle play/pause
  function togglePlay() {
    if (isPlaying) {
      pause();
    } else {
      play();
    }
  }

  // Clean up on component destroy
  onDestroy(() => {
    if (progressInterval) {
      clearInterval(progressInterval);
    }
    
    if (audio) {
      audio.pause();
      audio.src = "";
      audio = null;
    }
  });
</script>

<div class="window" style="width: 320px">
  <div class="title-bar">
    <div class="title-bar-text">Windows Media Player</div>
    <div class="title-bar-controls">
      <button aria-label="Close" id="Close" on:click={() => goto('/')}></button>

    </div>
  </div>
  <div class="window-body">
    <div class="player-container">
      <!-- Album art and metadata section -->
      <div class="player-top">
        <div class="album-container">
          <img src={songs[currentTrackIndex].cover} alt="album cover" class="album-art"/>
        </div>
        <div class="metadata">
          <div class="metadata-field">
            <span>Title:</span>
            <span class="metadata-value">{songs[currentTrackIndex].title}</span>
          </div>
          <div class="metadata-field">
            <span>Artist:</span>
            <span class="metadata-value">{songs[currentTrackIndex].artist}</span>
          </div>
          <div class="metadata-field">
            <span>Album:</span>
            <span class="metadata-value">{songs[currentTrackIndex].album}</span>
          </div>
        </div>
      </div>
      
      <!-- Progress bar and time display -->
      <div class="progress-container">
        <div class="time-display">{formatTime(currentTime)}</div>
        <div class="progress-bar" on:click={seek}>
          <div class="progress-fill" style="width: {duration > 0 ? (currentTime / duration) * 100 : 0}%"></div>
        </div>
        <div class="time-display">{formatTime(duration)}</div>
      </div>
      
      <!-- Control buttons -->
      <div class="controls-panel">
        <div class="main-controls">
          <button aria-label="previous" class="control-button" on:click={previous}>
            <img src={previous_icon} alt="previous" class="icon"/>
          </button>
          <button aria-label="play/pause" class="control-button play-pause" on:click={togglePlay}>
            <img src={isPlaying ? pause_icon : play_icon} alt={isPlaying ? "pause" : "play"} class="icon"/>
          </button>
          <button aria-label="next" class="control-button" on:click={next}>
            <img src={next_icon} alt="next" class="icon"/>
          </button>
        </div>
        
        <div class="secondary-controls">
          <button aria-label="repeat" class="control-button" on:click={repeat}>
            <img src={repeat_icon} alt="repeat" class="icon"/>
          </button>
          <div class="volume-controls">
            <button aria-label="volume down" class="volume-button" on:click={volumeDown}>
              <img src={volume_down_icon} alt="volume down" class="icon small"/>
            </button>
            <button aria-label="volume up" class="volume-button" on:click={volumeUp}>
              <img src={volume_up_icon} alt="volume up" class="icon small"/>
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Status bar -->
    <div class="status-bar">
      <div class="status-bar-field">Track {currentTrackIndex + 1} of {songs.length}</div>
      <div class="status-bar-field">{isPlaying ? "Playing" : "Paused"}</div>
    </div>
  </div>
</div>

<style>
  .window {
    font-family: "MS Sans Serif", Arial, sans-serif;
    font-size: 11px;
  }
  
  .player-container {
    padding: 8px;
  }
  
  /* Album art and metadata layout */
  .player-top {
    display: flex;
    margin-bottom: 10px;
    gap: 8px;
  }
  
  .album-container {
    border: 1px inset #808080;
    padding: 2px;
    background: white;
    width: 85px;
    height: 85px;
    flex-shrink: 0;
  }
  
  .album-art {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  
  .metadata {
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  
  .metadata-field {
    display: flex;
    background: white;
    border: 1px inset #808080;
    padding: 3px 5px;
    height: 22px;
    align-items: center;
    font-size: 11px;
  }
  
  .metadata-field span:first-child {
    font-weight: bold;
    width: 35px;
  }
  
  .metadata-value {
    margin-left: 5px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  /* Progress bar styles */
  .progress-container {
    display: flex;
    align-items: center;
    margin: 8px 0;
    gap: 5px;
  }

  .time-display {
    font-size: 11px;
    min-width: 35px;
    text-align: center;
  }

  .progress-bar {
    flex-grow: 1;
    height: 10px;
    background-color: white;
    border: 1px inset #808080;
    position: relative;
    cursor: pointer;
  }

  .progress-fill {
    height: 100%;
    background-color: #000080;
    position: absolute;
    top: 0;
    left: 0;
  }
  
  /* Control buttons */
  .controls-panel {
    display: flex;
    flex-direction: column;
    gap: 6px;
  }
  
  .main-controls {
    display: flex;
    justify-content: center;
    gap: 10px;
  }
  
  .secondary-controls {
    display: flex;
    justify-content: space-between;
    margin-top: 4px;
  }
  
  .volume-controls {
    display: flex;
    gap: 5px;
  }
  
  .control-button, .volume-button {
    min-width: 24px;
    height: 24px;
    padding: 2px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .play-pause {
    min-width: 32px;
    height: 32px;
  }
  
  .icon {
    width: 16px;
    height: 16px;
  }
  
  .icon.small {
    width: 14px;
    height: 14px;
  }
  
  /* Status bar */
  .status-bar {
    display: flex;
    height: 20px;
    background-color: #d4d0c8;
    border-top: 1px solid #808080;
    padding: 2px 0;
    margin-top: 8px;
  }
  
  .status-bar-field {
    padding: 0 8px;
    font-size: 11px;
    border-right: 1px solid #808080;
    display: flex;
    align-items: center;
  }
  
  /* Global button styles */
  button {
    min-width: 0;
    background-color: #d4d0c8;
    border: 1px solid;
    border-color: #ffffff #808080 #808080 #ffffff;
    border-style: solid;
    color: black;
    cursor: pointer;
    padding: 0;
  }
  
  button:active {
    border-color: #808080 #ffffff #ffffff #808080;
  }

  #Close {
    width : 20px;
  }
</style>