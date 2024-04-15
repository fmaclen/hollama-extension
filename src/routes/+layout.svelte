<script lang="ts">
	import 'open-props/open-props.min.css';
</script>

<div class="layout">
	<slot />

	<aside class="aside">
		<img src="favicon.png" alt="Hollama logo" />
		<h1>Hollama</h1>
	</aside>
</div>

<style lang="scss">
	:global(body) {
		margin: unset;
		font-family: var(--font-sans);
	}

	.layout {
		display: flex;
		height: 100%;
		width: max-content;
	}

	.aside {
		display: flex;
		justify-content: space-between;
		flex-direction: column;
		align-items: center;
		row-gap: var(--size-6);
		color: var(--gray-800);
		border-left: var(--border-size-1) solid var(--gray-3);
		
		img {
			display: block;
			padding: var(--size-3);
			width: 40px;
			height: 40px;
		}

		h1 {
      font-size: var(--font-size-3);
			width: max-content;
			height: 7ch;
			line-height: 7ch;
			transform: rotate(270deg);
			margin: unset;
			padding-block: var(--size-2);
		}
	}
</style>
