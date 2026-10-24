<script>
  import eyeIcon from '../assets/eye.svg';

  let { embedUrl, showControls, onToggleControls } = $props();
</script>

<div class="player-container" data-tauri-drag-region={!showControls}>
  {#if embedUrl}
    <iframe 
      src={embedUrl} 
      title="PiP Player" 
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
      allowfullscreen
    ></iframe>
  {:else}
    <div class="placeholder" data-tauri-drag-region>Enter URL above</div>
  {/if}

  {#if !showControls}
    <button class="floating-show-btn" onclick={onToggleControls} title="Show controls">
      <img src={eyeIcon} alt="Settings" class="icon" />
    </button>
  {/if}
</div>

<style>
  .player-container {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #000;
    position: relative;
    width: 100%;
    height: 100%;
  }

  iframe {
    width: 100%;
    height: 100%;
    border: none;
  }

  .placeholder {
    color: #666;
    font-size: 14px;
    user-select: none;
  }

  .floating-show-btn {
    position: absolute;
    top: 8px;
    right: 8px;
    background: rgba(0, 0, 0, 0.6);
    border: 1px solid rgba(255, 255, 255, 0.2);
    padding: 6px;
    border-radius: 4px;
    cursor: pointer;
    opacity: 0;
    transition: opacity 0.2s ease;
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .player-container:hover .floating-show-btn {
    opacity: 1;
  }

  .floating-show-btn:hover {
    background: rgba(59, 130, 246, 0.8);
  }

  .icon {
    width: 16px;
    height: 16px;
    display: block;
    filter: invert(1);
  }
</style>