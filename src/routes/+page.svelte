<script lang="ts">
    import { onMount } from "svelte";
    let jsonInput = '';
    let jsonOutput = '';
    let errorMsg = '';
    let status = '';
    let darkMode = false;
  
    function formatJSON(json: string) {
      if (json.trim() === '') {
        jsonOutput = '';
        errorMsg = '';
        status = 'empty';
      } else {
        try {
          const parsed = JSON.parse(json);
          jsonOutput = JSON.stringify(parsed, null, 2);
          errorMsg = '';
          status = 'valid';
        } catch (error) {
          jsonOutput = '';
          errorMsg = 'Invalid JSON';
          status = 'invalid';
        }
      }
    }
  
    function copyToClipboard() {
      navigator.clipboard.writeText(jsonOutput);
    }
  
    function toggleDarkMode() {
      darkMode = !darkMode;
      document.body.classList.toggle('dark-mode', darkMode);
      localStorage.setItem('darkMode', darkMode.toString());
    }
  
    onMount(() => {
      const storedDarkMode = localStorage.getItem('darkMode');
    if (storedDarkMode !== null) {
      darkMode = storedDarkMode === 'true';
      document.body.classList.toggle('dark-mode', darkMode);
    } else if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
      darkMode = true;
      document.body.classList.add('dark-mode');
      localStorage.setItem('darkMode', 'true');
    }
    });
  
    $: formatJSON(jsonInput);
  </script>
  
  <main>
    <div class="container">
      <h1>JSON Formatter</h1>
      <div class="toggle-container">
        <label class="toggle-label">
          <input type="checkbox" class="toggle-input" bind:checked={darkMode} on:click={toggleDarkMode} />
          <span class="toggle-slider"></span>
          <span class="icon icon-light">☀️</span>
          <span class="icon icon-dark">🌙</span>
        </label>
      </div>
      <div class="input-container">
        <textarea bind:value={jsonInput} rows="10" placeholder="Enter JSON here"></textarea>
      </div>
      <div class="output-container">
        {#if status === 'empty'}
          <p class="empty-message">No JSON entered yet.</p>
        {:else if status === 'valid'}
          <pre>{jsonOutput}</pre>
          <button class="copy-button" on:click={copyToClipboard}>
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
              <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>
            </svg>
            <span class="tooltip">Copy to Clipboard</span>
          </button>
        {:else if status === 'invalid'}
          <p class="error-message">{errorMsg}</p>
        {/if}
      </div>
    </div>
  </main>
  
  <style>
    :global(body) {
      transition: background-color 0.5s ease;
    }
  
    :global(body.dark-mode) {
      background-color: #1c1c1c;
      color: #fff;
    }
  
    .container {
      max-width: 800px;
      margin: 0 auto;
      padding: 2rem;
      font-family: Arial, sans-serif;
    }
  
    h1 {
      text-align: center;
      margin-bottom: 2rem;
      color: #3c3c3c;
    }
  
    .input-container {
      margin-bottom: 2rem;
    }
  
    textarea {
      width: 100%;
      padding: 1rem;
      font-size: 16px;
      border: 2px solid #d5d5d5;
      border-radius: 4px;
      resize: vertical;
      transition: border-color 0.3s ease;
    }
  
    textarea:focus {
      outline: none;
      border-color: #6cb33f;
      box-shadow: 0 0 5px rgba(108, 179, 63, 0.5);
    }
  
    .output-container {
      position: relative;
    background-color: #f4f4f4;
    padding: 2rem;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    color: #1c1c1c;
    }
  
  

  pre {
    font-family: 'Courier New', monospace;
    font-size: 14px;
    white-space: pre-wrap;
    word-wrap: break-word;
  }

    .error-message {
      color: #d32f2f;
      font-weight: bold;
    }
  
    .empty-message {
      color: #888;
      font-style: italic;
    }
  
    .copy-button {
      position: absolute;
      top: 10px;
      right: 10px;
      background: none;
      border: none;
      cursor: pointer;
      outline: none;
    }
  
    .copy-button svg {
      width: 20px;
      height: 20px;
      color: #888;
      transition: color 0.3s ease;
    }
  
    .copy-button:hover svg {
      color: #6cb33f;
    }
  
    .tooltip {
      position: absolute;
      top: -30px;
      left: 50%;
      transform: translateX(-50%);
      background-color: #3c3c3c;
      color: #fff;
      padding: 5px 10px;
      border-radius: 4px;
      font-size: 12px;
      opacity: 0;
      visibility: hidden;
      transition: opacity 0.3s ease, visibility 0.3s ease;
      transition-delay: 0.5s;
    }
  
    .copy-button:hover .tooltip {
      opacity: 1;
      visibility: visible;
      transition-delay: 0.5s;
    }
  
    .toggle-container {
      position: absolute;
      top: 20px;
      right: 20px;
    }
  
    .toggle-label {
      position: relative;
      display: inline-block;
      width: 60px;
      height: 34px;
    }
  
    .toggle-input {
      display: none;
    }
  
    .toggle-slider {
      position: absolute;
      cursor: pointer;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: #ccc;
      transition: 0.4s;
      border-radius: 34px;
    }
  
    .toggle-slider:before {
      position: absolute;
      content: "";
      height: 26px;
      width: 26px;
      left: 4px;
      bottom: 4px;
      background-color: white;
      transition: 0.4s;
      border-radius: 50%;
    }
  
    .toggle-input:checked + .toggle-slider {
      background-color: #6cb33f;
    }
  
    .toggle-input:checked + .toggle-slider:before {
      transform: translateX(26px);
    }
  
    .icon {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      font-size: 16px;
      transition: opacity 0.3s ease;
    }
  
    .icon-light {
      left: 8px;
      opacity: 1;
    }
  
    .icon-dark {
      right: 8px;
      opacity: 0;
    }
  
    .toggle-input:checked ~ .icon-light {
      opacity: 0;
    }
  
    .toggle-input:checked ~ .icon-dark {
      opacity: 1;
    }
  </style>