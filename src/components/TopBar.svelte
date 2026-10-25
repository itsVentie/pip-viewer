<script>
  import playIcon from '../assets/play.svg';
  import eyeIcon from '../assets/eye-closed.svg';
  import settingsIcon from '../assets/settings.svg';

  let { url = $bindable(''), onPlay, onToggleControls, onOpenSettings } = $props();
</script>

<div class="top-bar" data-tauri-drag-region>
  <input 
    type="text" 
    bind:value={url} 
    placeholder="Paste YouTube, Twitch or video URL..." 
    onkeydown={(e) => e.key === 'Enter' && onPlay()}
  />
  
  <button class="icon-btn play-btn" onclick={onPlay} title="Play">
    <img src={playIcon} alt="Play" class="icon" />
  </button>
  
  <button class="icon-btn" onclick={onToggleControls} title="Hide controls">
    <img src={eyeIcon} alt="Hide controls" class="icon" />
  </button>
  
  <button class="icon-btn" onclick={onOpenSettings} title="Settings">
    <img src={settingsIcon} alt="Settings" class="icon" />
  </button>
</div>

<style>
  .top-bar {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 6px;
    background: #1a1a1a;
    border-bottom: 1px solid #333;
    user-select: none;
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

  input:focus {
    border-color: #3b82f6;
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