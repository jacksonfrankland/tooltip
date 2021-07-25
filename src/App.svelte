<script>
	import Tooltip from './components/Tooltip.svelte';
	let elements = [];
	window.addEventListener('resize', () => {
		document.documentElement.style.setProperty('--vh', `${window.innerHeight * 0.01}px`);
	})
	document.documentElement.style.setProperty('--vh', `${window.innerHeight * 0.01}px`);
</script>

<main>
	{#each Array(9) as _, i}
		<div>
			<span bind:this={elements[i]}> Open tooltip </span>
		</div>
	{/each}

	{#each elements as element}
		<Tooltip {element}>You've just opened a tooltip  </Tooltip>
	{/each}
</main>

<style>
	div {
		width: 33.33%;
		height: 33.33%;
		display: flex;
	}
	main {
		height: 100vh;
		height: calc(var(--vh, 1vh) * 100);
		flex-wrap: wrap;
		display: flex;
	}
	span {
		color: blue;
		margin: auto;
		text-decoration: underline;
	}
	div:nth-child(3n) span {
		margin-right: 0;
	}
	div:nth-child(3n - 2) span {
		margin-left: 0;
	}
	div:nth-child(-n + 3) span {
		margin-top: 0;
	}
	div:nth-child(n + 7):nth-child(-n + 9) span {
		margin-bottom: 0;
	}
</style>
