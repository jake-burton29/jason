<script lang="ts">
    let jsonInput1 = '';
    let jsonInput2 = '';
    let jsonOutput1 = '';
    let jsonOutput2 = '';
    let errorMsg1 = '';
    let errorMsg2 = '';
    let status1 = '';
    let status2 = '';
  
    function formatJSON(json: string) {
      let output = '';
      let error = '';
      let status = '';
  
      if (json.trim() === '') {
        output = '';
        error = '';
        status = 'empty';
      } else {
        try {
          const parsed = JSON.parse(json);
          output = JSON.stringify(parsed, null, 2);
          error = '';
          status = 'valid';
        } catch (err) {
          output = '';
          error = 'Invalid JSON';
          status = 'invalid';
        }
      }
  
      return { output, error, status };
    }
  
    function compareJSON(json1: string, json2: string) {
      const { output: output1, error: error1, status: status1Result } = formatJSON(json1);
      const { output: output2, error: error2, status: status2Result } = formatJSON(json2);
  
      jsonOutput1 = output1;
      jsonOutput2 = output2;
      errorMsg1 = error1;
      errorMsg2 = error2;
      status1 = status1Result;
      status2 = status2Result;
    }
  
    function handleInputChange() {
      compareJSON(jsonInput1, jsonInput2);
    }
  </script>
  
  <main>
    <div class="container">
      <h1>JSON Comparator</h1>
      <div class="input-container">
        <h2>JSON 1</h2>
        <textarea bind:value={jsonInput1} on:input={handleInputChange} rows="10" placeholder="Enter JSON 1"></textarea>
      </div>
      <div class="input-container">
        <h2>JSON 2</h2>
        <textarea bind:value={jsonInput2} on:input={handleInputChange} rows="10" placeholder="Enter JSON 2"></textarea>
      </div>
      <div class="output-container">
        <h2>Comparison Result</h2>
        {#if status1 === 'empty' && status2 === 'empty'}
          <p class="empty-message">No JSON entered yet.</p>
        {:else if status1 === 'invalid' || status2 === 'invalid'}
          <p class="error-message">{errorMsg1 || errorMsg2}</p>
        {:else}
          <pre>{jsonOutput1}</pre>
          <pre>{jsonOutput2}</pre>
        {/if}
      </div>
    </div>
  </main>
  
  <style>
    .container {
      max-width: 800px;
      margin: 0 auto;
      padding: 2rem;
      font-family: Arial, sans-serif;
    }
  
    h1 {
      text-align: center;
      margin-bottom: 2rem;
    }
  
    .input-container {
      margin-bottom: 2rem;
    }
  
    h2 {
      margin-bottom: 1rem;
    }
  
    textarea {
      width: 100%;
      padding: 1rem;
      font-size: 16px;
      border: 2px solid #d5d5d5;
      border-radius: 4px;
      resize: vertical;
    }
  
    .output-container {
      margin-top: 2rem;
    }
  
    pre {
      background-color: #f4f4f4;
      padding: 1rem;
      border-radius: 4px;
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
  </style>