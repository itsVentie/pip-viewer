<script>
  import { getCurrentWindow } from '@tauri-apps/api/window';
  import { onMount } from 'svelte';

  let { onClose, windowOpacity = 1.0, onOpacityChange } = $props();
  let alwaysOnTop = $state(true);

  onMount(async () => {
    try {
      const win = getCurrentWindow();
      alwaysOnTop = await win.isAlwaysOnTop();
    } catch (e) {
      console.error(e);
    }
  });

  const toggleAlwaysOnTop = async () => {
    try {
      const win = getCurrentWindow();
      const newState = !alwaysOnTop;
      await win.setAlwaysOnTop(newState);
      alwaysOnTop = newState;
      localStorage.setItem('pip_always_on_top', JSON.stringify(newState));
    } catch (e) {
      console.error(e);
    }
  };

  const handleOpacityInput = (e) => {
    onOpacityChange(e);
    localStorage.setItem('pip_opacity', e.target.value);
  };
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<!-- svelte-ignore a11y_click_events_have_key_events -->
<div class="modal-backdrop" onclick={onClose}>
  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <div class="modal-content" onclick={(e) => e.stopPropagation()}>
    <div class="modal-header">
      <h3>Settings</h3>
      <button class="close-btn" onclick={onClose}>&times;</button>
    </div>
    
    <div class="modal-body">
      <label class="setting-item">
        <span>Always on Top</span>
        <input type="checkbox" checked={alwaysOnTop} onchange={toggleAlwaysOnTop} />
      </label>

      <div class="setting-column">
        <div class="setting-row">
          <span>Opacity</span>
          <span class="value-label">{Math.round(windowOpacity * 100)}%</span>
        </div>
        <input 
          type="range" 
          min="0.3" 
          max="1.0" 
          step="0.05" 
          value={windowOpacity} 
          oninput={handleOpacityInput} 
        />
      </div>
    </div>
  </div>
</div>

<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.6);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
  }

  .modal-content {
    background: #1a1a1a;
    border: 1px solid #444;
    border-radius: 8px;
    width: 280px;
    padding: 16px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.6);
  }

  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 14px;
    border-bottom: 1px solid #333;
    padding-bottom: 8px;
  }

  .modal-header h3 {
    margin: 0;
    font-size: 14px;
    color: #fff;
  }

  .close-btn {
    background: none;
    border: none;
    color: #aaa;
    font-size: 18px;
    cursor: pointer;
    padding: 0;
    line-height: 1;
  }

  .close-btn:hover {
    color: #fff;
  }

  .modal-body {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .setting-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #ddd;
    font-size: 13px;
    cursor: pointer;
  }

  .setting-column {
    display: flex;
    flex-direction: column;
    gap: 6px;
    color: #ddd;
    font-size: 13px;
  }

  .setting-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .value-label {
    font-size: 11px;
    color: #888;
  }

  input[type="range"] {
    width: 100%;
    accent-color: #3b82f6;
    cursor: pointer;
  }
</style>