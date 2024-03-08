<script>
	import c from '$lib/assets/skills-icons/c-icon.png';
	import css from '$lib/assets/skills-icons/css-icon.png';
	import cuda from '$lib/assets/skills-icons/cuda-icon.png';
	import git from '$lib/assets/skills-icons/git-icon.png';
	import linux from '$lib/assets/skills-icons/linux-icon.png';
	import node from '$lib/assets/skills-icons/node-icon.png';
	import python from '$lib/assets/skills-icons/python-icon.png';
	import rust from '$lib/assets/skills-icons/rust-icon.png';
	import svelte from '$lib/assets/skills-icons/svelte-icon.png';
	import tauri from '$lib/assets/skills-icons/tauri-icon.svg';
	import tensorflow from '$lib/assets/skills-icons/tensorflow-icon.png';
	import unity from '$lib/assets/skills-icons/unity-icon.png';

	Array.prototype.shuffle = function () {
		var i = this.length,
			j,
			temp;
		if (i == 0) return this;
		while (--i) {
			j = Math.floor(Math.random() * (i + 1));
			temp = this[i];
			this[i] = this[j];
			this[j] = temp;
		}
		return this;
	};

	let icones = [
		c,
		css,
		cuda,
		git,
		linux,
		node,
		python,
		rust,
		svelte,
		tauri,
		tensorflow,
		unity
	].shuffle();
    // while(icones.length<16){
    //     icones.push("");
    // }

</script>

<main>
	{#each icones as item}
		<img src={item} alt="" />
	{/each}
</main>

<style>
	main {
		width: 100%;
		height: 100%;
		border: 1px solid black;
		position: relative;

		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
		align-content: center;
		background-color: gray;
	}
	img {
		height: 33.3%;
		width: 25%;
        margin: 0;
        padding: 6%;
		border: 1px solid rgba(0, 0, 0, 0.04);
		transition:
			box-shadow 0.2s,
			transform 0.2s,
			padding 0.1s;
		background-color: gray;
	}
	img:hover {
		/* transition: box-shadow .3s, transform .3s ; */
		box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.486);
		transform: scale(1.03);
		z-index: 10;
        /* padding: 25px; */
	}
</style>
