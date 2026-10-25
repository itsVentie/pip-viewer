<script>
  import playIcon from '../assets/play.svg';
  import eyeIcon from '../assets/eye-closed.svg';
  import settingsIcon from '../assets/settings.svg';

  let { url = $bindable(''), onPlay, onToggleControls, onOpenSettings } = $props();
  
  let showHistory = $state(false);
  let history = $state([]);

  $effect(() => {
    try {
      const saved = localStorage.getItem('pip_history');
      if (saved) {
        history = JSON.parse(saved);
      }
    } catch (e) {
      console.error(e);
    }
  });

  async function handleFocus() {
    showHistory = true;
    if (!url) {
      try {
        if (navigator.clipboard && navigator.clipboard.readText) {
          const clipText = (await navigator.clipboard.readText()).trim();
          // Проверяем, что в буфере именно ссылка (начинается с http/https)
          if (clipText && /^https?:\/\/.+/i.test(clipText)) {
            url = clipText;
            addAndPlay();
          }
        }
      } catch (e) {
        console.error(e);
      }
    }
  }

  function addAndPlay() {
    if (!url.trim()) return;
    const trimmed = url.trim();
    
    // Сохраняем в историю и пускаем только если это валидная ссылка
    if (!/^https?:\/\/.+/i.test(trimmed)) {
      onPlay();
      return;
    }

    const updated = [trimmed, ...history.filter(item => item !== trimmed)].slice(0, 10);
    history = updated;
    localStorage.setItem('pip_history', JSON.stringify(updated));

    showHistory = false;
    onPlay();
  }

  function selectHistoryItem(item) {
    url = item;
    showHistory = false;
    addAndPlay();
  }
</script>

<div class="top-bar-container">
  <div class="top-bar" data-tauri-drag-region>
    <div class="input-wrapper">
      <input 
        type="text" 
        bind:value={url} 
        placeholder="Paste YouTube, Twitch or video URL..." 
        onfocus={handleFocus}
        onblur={() => setTimeout(() => showHistory = false, 200)}
        onkeydown={(e) => e.key === 'Enter' && addAndPlay()}
      />
      
      {#if showHistory && history.length > 0}
        <div class="history-dropdown">
          {#each history as item}
            <div class="history-item" onmousedown={() => selectHistoryItem(item)}>
              {item}
            </div>
          {/each}
        </div>
      {/if}
    </div>
    
    <button class="icon-btn play-btn" onclick={addAndPlay} title="Play">
      <img src={playIcon} alt="Play" class="icon" />
    </button>
    
    <button class="icon-btn" onclick={onToggleControls} title="Hide controls">
      <img src={eyeIcon} alt="Hide controls" class="icon" />
    </button>
    
    <button class="icon-btn" onclick={onOpenSettings} title="Settings">
      <img src={settingsIcon} alt="Settings" class="icon" />
    </button>
  </div>
</div>

<style>
  .top-bar-container {
    position: relative;
    width: 100%;
    z-index: 100;
  }

  .top-bar {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 6px;
    background: #1a1a1a;
    border-bottom: 1px solid #333;
    user-select: none;
  }

  .input-wrapper {
    position: relative;
    flex: 1;
    display: flex;
  }

  input {
    width: 100%;
    background: #2a2a2a;
    border: 1px solid #444;
    color: #fff;
    padding: 6px 10px;
    border-radius: 4px;
    outline: none;
    font-size: 13px;
    box-sizing: border-box;
  }

  input:focus {
    border-color: #3b82f6;
  }

  .history-dropdown {
    position: absolute;
    top: calc(100% + 4px);
    left: 0;
    right: 0;
    background: #1f1f1f;
    border: 1px solid #444;
    border-radius: 4px;
    max-height: 150px;
    overflow-y: auto;
    box-shadow: 0 4px 12px rgba(0,0,0,0.5);
    z-index: 1000;
  }

  .history-item {
    padding: 8px 10px;
    font-size: 12px;
    color: #ccc;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    cursor: pointer;
  }

  .history-item:hover {
    background: #2a2a2a;
    color: #fff;
  }

  .play-btn {
    background: #3b82f6;
    border-color: #3b82f6;
  }

  .play-btn:hover {
    background: #2563eb;
    border-color: #2563eb;
  }

  .icon-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    background: #2a2a2a;
    border: 1px solid #444;
    border-radius: 4px;
    padding: 6px;
    cursor: pointer;
    width: 30px;
    height: 30px;
    box-sizing: border-box;
  }

  .icon-btn:hover {
    background: #3a3a3a;
    border-color: #555;
  }

  .icon {
    width: 16px;
    height: 16px;
    display: block;
    filter: invert(1);
  }
</style>