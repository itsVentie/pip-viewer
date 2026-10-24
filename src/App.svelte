<script>
  import TopBar from './components/TopBar.svelte';
  import Player from './components/Player.svelte';

  let url = $state('');
  let embedUrl = $state('');
  let showControls = $state(true);

  function loadVideo() {
    if (!url) return;
    let trimmed = url.trim();

    if (trimmed.includes('youtube.com/watch?v=')) {
      const id = trimmed.split('v=')[1]?.split('&')[0];
      embedUrl = `https://www.youtube.com/embed/${id}?autoplay=1`;
    } else if (trimmed.includes('youtu.be/')) {
      const id = trimmed.split('youtu.be/')[1]?.split('?')[0];
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

<main>
  {#if showControls}
    <TopBar bind:url onPlay={loadVideo} onToggleControls={toggleControls} />
  {/if}

  <Player {embedUrl} {showControls} onToggleControls={toggleControls} />
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
  }
</style>