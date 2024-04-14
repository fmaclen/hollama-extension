<script lang="ts">
	let extractedText = '';

	async function extractText() {
    let [tab] = await chrome.tabs.query({ active: true, currentWindow: true });

    if (tab.id) {
      const html = await chrome.scripting.executeScript({
        target: { tabId: tab.id },
        func: () => {
          return document.body.innerHTML;
        },
      });

      extractedText = html[0].result || '';
    }
	}

	function copyText() {
		navigator.clipboard.writeText(extractedText);
	}

	function saveAsFile() {
		const blob = new Blob([extractedText], { type: 'text/plain' });
		const url = URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.href = url;
		a.download = 'extracted-text.txt';
		a.click();
		URL.revokeObjectURL(url);
	}
</script>

<h1>Hollama</h1>

<hr />

<button on:click={extractText}>Extract</button>
<button on:click={copyText}>Copy</button>
<button on:click={saveAsFile}>Save as file</button>

<hr />

<pre>
  <code>
    {extractedText}
  </code>
</pre>

<style>
	pre,
	code {
		width: 100%;
		overflow: hidden;
		max-height: 320px;
	}
</style>
