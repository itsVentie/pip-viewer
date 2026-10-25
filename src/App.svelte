<script>
  import TopBar from './components/TopBar.svelte';
  import Player from './components/Player.svelte';
  import Settings from './components/Settings.svelte';
  import { onMount } from 'svelte';

  let url = $state('');
  let embedUrl = $state('');
  let showControls = $state(true);
  let showSettings = $state(false);
  let windowOpacity = $state(1.0);

  onMount(() => {
    const savedUrl = localStorage.getItem('pip_last_url');
    if (savedUrl) {
      url = savedUrl;
      loadVideo();
    }

    const savedOpacity = localStorage.getItem('pip_opacity');
    if (savedOpacity) {
      windowOpacity = parseFloat(savedOpacity);
      document.documentElement.style.setProperty('--app-opacity', windowOpacity);
    }
  });

  function loadVideo() {
    if (!url) return;
    let trimmed = url.trim();
    localStorage.setItem('pip_last_url', trimmed);

    if (trimmed.includes('youtube.com/watch?v=')) {
      const id = trimmed.split('v=')[1]?.split('&')[0];
      embedUrl = `https://www.youtube.com/embed/${id}?autoplay=1`;
    } else if (trimmed.includes('youtu.be/')) {
      const parts = trimmed.split('youtu.be/')[1]?.split('?')[0];
      const id = parts?.split('/')[0];
      embedUrl = `https://www.youtube.com/embed/${id}?autoplay=1`;
    } else if (trimmed.includes('twitch.tv/')) {
      const channel = trimmed.split('twitch.tv/')[1]?.split('/')[0];
      const parent = window.location.hostname || 'localhost';
      embedUrl = `https://player.twitch.tv/?channel=${channel}&parent=${parent}&autoplay=true`;
    } else {
      embedUrl = trimmed;
    }
  }

  function toggleControls() {
    showControls = !showControls;
  }
</script>

<main style="opacity: var(--app-opacity, 1);">
  {#if showControls}
    <TopBar 
      bind:url 
      onPlay={loadVideo} 
      onToggleControls={toggleControls} 
      onOpenSettings={() => showSettings = true} 
    />
  {/if}

  <Player {embedUrl} {showControls} onToggleControls={toggleControls} />

  {#if showSettings}
    <Settings 
      onClose={() => showSettings = false} 
      {windowOpacity} 
      onOpacityChange={(e) => {
        windowOpacity = parseFloat(e.target.value);
        document.documentElement.style.setProperty('--app-opacity', windowOpacity);
        localStorage.setItem('pip_opacity', windowOpacity);
      }}
    />
  {/if}
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    background: #0f0f0f;
    color: #fff;
    font-family: system-ui, -apple-system, sans-serif;
    overflow: hidden;
  }

  main {
    display: flex;
    flex-direction: column;
    height: 100vh;
    width: 100vw;
    box-sizing: border-box;
    position: relative;
  }
</style>