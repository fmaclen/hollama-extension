<script lang="ts">
	import { Readability } from '@mozilla/readability';

	let extractedText = '';

	async function extractText() {
		const doc = document.implementation.createHTMLDocument();
		let [tab] = await chrome.tabs.query({ active: true, currentWindow: true });

		if (tab.id) {
			const pageContent = await chrome.scripting.executeScript({
				target: { tabId: tab.id },
				func: () => {
					return document.body.innerHTML;
				}
			});
			doc.body.innerHTML = pageContent[0].result || '';
		}

		extractedText = new Readability(doc).parse()?.textContent.trim() || '';
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

<main class="main">
	<nav class="nav">
		<button class="button button--primary" on:click={extractText}>Extract</button>
	</nav>

	<textarea class="preview" disabled placeholder="Click 'Extract' to read the text from the page." value={extractedText} />

	<nav class="nav">
		<button class="button" on:click={copyText}>Copy</button>
		<button class="button" on:click={saveAsFile}>Save as file</button>
	</nav>
</main>

<style style="scss">
	.main {
		padding-block: var(--size-2);
		display: flex;
		flex-direction: column;
	}

	.nav {
		display: flex;
		flex-direction: column;
		padding-inline: var(--size-2);
		row-gap: var(--size-1);
	}

	.preview {
		width: 100%;
		width: 256px;
		max-height: 400px;
		overflow-y: auto;
		font-size: var(--font-size-0);
		font-family: var(--font-mono);
		background-color: var(--gray-1);
		padding: var(--size-2);
		border: none;
		resize: vertical;
		border-bottom: var(--border-size-1) solid var(--gray-3);
		border-top: var(--border-size-1) solid var(--gray-3);
		margin-block: var(--size-2);
		line-height: var(--font-lineheight-1);
		height: 16em; /* 16 lines */
	}

	.button {
		border: var(--border-size-1) solid var(--gray-3);
		padding: var(--size-2);
		font-family: var(--font-sans);
		cursor: pointer;
		background-color: white;
		font-weight: var(--font-weight-6);
		border-radius: var(--radius-2);
	}

	.button:hover {
		opacity: .8;
	}
	
	.button--primary {
		color: var(--gray-1);
		background-color: var(--gray-9);
		border-color: var(--gray-9);
	}
</style>
