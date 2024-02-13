<script>
	import Gear from './gear.svelte';
	const dentes = 8;
</script>

<main>
	<div><Gear raio={50} {dentes} /></div>
	<div><Gear raio={50} {dentes} /></div>
</main>

<style>
	main {
		height: 210px;
		width: 210px;
		position: relative;
	}
	div {
		position: absolute;
		animation: spin 4s linear infinite;
	}
	div:first-child {
		bottom: 20px;
		left: 76px;
		animation-direction: reverse;
	}
	div:last-child {
		bottom: -80px;
		left: 10px;
	}
	@keyframes spin {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}
</style>
