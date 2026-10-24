<script>
  let url = $state('');
  let embedUrl = $state('');

  function loadVideo() {
    if (!url) return;

    let trimmed = url.trim();
    if (trimmed.includes('youtube.com/watch?v=')) {
      const id = trimmed.split('v=')[1]?.split('&')[0];
      embedUrl = `https://www.youtube.com/embed/${id}?autoplay=1`;
    } else if (trimmed.includes('youtu.be/')) {
      const id = trimmed.split('youtu.be/')[1]?.split('?')[0];
      embedUrl = `https://www.youtube.com/embed/${id}?autoplay=1`;
    } else if (trimmed.includes('youtube.com/embed/')) {
      embedUrl = trimmed.includes('autoplay=1') ? trimmed : `${trimmed}&autoplay=1`;
    } 
    else if (trimmed.includes('twitch.tv/')) {
      const channel = trimmed.split('twitch.tv/')[1]?.split('/')[0];
      const parent = window.location.hostname || 'localhost';
      embedUrl = `https://player.twitch.tv/?channel=${channel}&parent=${parent}&autoplay=true`;
    } 
  
    else {
      embedUrl = trimmed;
    }
  }
</script>

<main>
  <div class="top-bar">
    <input type="text" bind:value={url} placeholder="Paste YouTube, Twitch or video URL..." />
    <button onclick={loadVideo}>Play</button>
  </div>

  <div class="player-container">
    {#if embedUrl}
      <iframe 
        src={embedUrl} 
        title="PiP Player" 
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
        allowfullscreen
      ></iframe>
    {:else}
      <div class="placeholder">Enter URL above</div>
    {/if}
  </div>
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

  .top-bar {
    display: flex;
    gap: 6px;
    padding: 6px;
    background: #1a1a1a;
    border-bottom: 1px solid #333;
  }

  input {
    flex: 1;
    background: #2a2a2a;
    border: 1px solid #444;
    color: #fff;
    padding: 6px 10px;
    border-radius: 4px;
    outline: none;
    font-size: 13px;
  }

  button {
    background: #3b82f6;
    border: none;
    color: #fff;
    padding: 6px 12px;
    border-radius: 4px;
    cursor: pointer;
    font-weight: 500;
  }

  button:hover {
    background: #2563eb;
  }

  .player-container {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #000;
  }

  iframe {
    width: 100%;
    height: 100%;
    border: none;
  }

  .placeholder {
    color: #666;
    font-size: 14px;
  }
</style>