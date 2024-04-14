<script lang="ts">
	import 'open-props/open-props.min.css';
</script>

<div class="layout">
	<main>
		<slot />
	</main>
	<aside class="aside">
		<a href="https://hollama.fernando.is">
			<img src="favicon.png" alt="Hollama" />
			<h1>Hollama</h1>
		</a>
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
	}

	.aside {
		border-left: var(--border-size-1) solid var(--gray-3);

		a {
      height: 100%;
			display: flex;
			justify-content: space-between;
      box-sizing: border-box;
			flex-direction: column;
			align-items: center;
      row-gap: var(--size-6);
			padding-block: var(--size-3);
			color: var(--gray-800);
			text-decoration: none;

			&:hover {
				background-color: var(--gray-1);
			}
		}

		img {
			width: 48px;
			height: 48px;
		}

		h1 {
      font-size: var(--font-size-3);
			width: max-content;
			height: 7ch;
			line-height: 7ch;
			transform: rotate(270deg);
			margin: unset;
		}
	}
</style>
