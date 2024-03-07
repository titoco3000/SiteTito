<script>
	import SeletorLinguagem from '$lib//components/seletorLinguagem.svelte';
	import NomeDestaque from '../lib/components/nomeDestaque.svelte';
	import Profile from '$lib/assets/profile.png';
	import ImpressaoDigital from '$lib/assets/impdig.png';
	import Spiral from '$lib/assets/spiral.svg';
	import { onMount } from 'svelte';
	import { t } from '$lib/texto.js';
	import PainelIcones from '$lib/components/painelIcones.svelte';

	let mainEl;
	let contentEl;
	let rotSqrEl;
	let painelIconesEl;
	let barrasTransicaoEl;
	const nBarras = 4;

	function resizeRotSqr() {
		let s = Math.max(contentEl.offsetWidth, contentEl.offsetHeight) + 'px';
		rotSqrEl.style.width = contentEl.offsetWidth + 'px';
		rotSqrEl.style.height = contentEl.offsetWidth + 'px';
	}
	onMount(() => {
		window.addEventListener('scroll', (e) => {
			let progress = 1 - Math.min(window.scrollY / rotSqrEl.offsetHeight, 1);
			rotSqrEl.style.transform = 'rotate(' + progress * -90 + 'deg)';
		});

		resizeRotSqr();
		new ResizeObserver(resizeRotSqr).observe(contentEl);

		new IntersectionObserver((entries) => {
			entries.forEach((entry) => {
				if (entry.intersectionRatio > 0) {
					console.log('apareceu ', entry.target);
					entry.target.style.animation = 'none';
					entry.target.offsetHeight; /* trigger reflow */
					entry.target.style.animation = null;
				}
			});
		}).observe(painelIconesEl);

		let observer = new IntersectionObserver((entries) => {
			entries.forEach((entry) => {
				// console.log(entry.intersectionRatio);

				if (entry.intersectionRatio > 0) {
					let c = barrasTransicaoEl.children;
					for (let i = 0; i < nBarras; i++) {
						let s = (entry.intersectionRatio - i / nBarras) / (1 - i / nBarras);
						// console.log(i,"=>",(entry.intersectionRatio - i/nBarras), " / ", 1 - i/nBarras);
						c[i].style.right = 'calc(  calc(var(--r) * ' + s + ') + ' + -(1 - s) * 100 + '% )';
						c[i].style.transform = 'scale(' + (0.9 + s * 0.1) + ')';
					}
					observer.unobserve(barrasTransicaoEl);
					requestAnimationFrame(() => {
						observer.observe(barrasTransicaoEl);
					});
				}
			});
		});
		observer.observe(barrasTransicaoEl);
	});
</script>

<main
	bind:this={mainEl}
	on:mousewheel={(e) => {
		if (e.deltaY > 0 && window.scrollY == 0) {
			contentEl.style.height = 'auto';
			contentEl.style.overflow = 'clip visible';
		}
	}}
>
	<div id="front">
		<div id="nome">
			<h1>Tito</h1>
			<h2>Guidotti</h2>
		</div>
	</div>
	<div id="content" bind:this={contentEl}>
		<div id="rotsqr-container">
			<div id="rotsqr" bind:this={rotSqrEl}></div>
		</div>
		<div id="sobre-mim" class="after-rotsqr">
			<div class="id-card" id="back-card">
				<div id="card-strip"></div>
				<img src={ImpressaoDigital} alt="" />
				<p>
					0100110001101111011100100110010101101101001000000110100101110000011100110111010101101101001000000110010001101111011011000110111101110010001000000111001101101001011101000010000001100001011011010110010101110100001011000010000001100011011011110110111001110011011001010110001101110100011001010111010001110101011100100010000001100001011001000110100101110000011010010111001101101001011000110110100101101110011001110010000001100101011011000110100101110100001011100010000001001001011001000010000001100011011101010110110100101100001000000110000101110011011100000110010101110010011010010110111101110010011001010111001100100000011000010111000001100101011100100110100101100001011011010010000001100100011001010110110001100101011011100110100101110100011010010010000001100001011101010111010000100000011011110110010001101001011101000010000001100101011110000110010101110010011000110110100101110100011000010111010001101001011011110110111001100101011011010010000001100101011011000110100101100111011001010110111001100100011010010010000001110110011011110110110001110101011100000111010001100001011101000111010101101101001000000110001101110101011100000110100101100100011010010111010001100001011101000110010100100000011011100110000101110100011101010111001100100000011000010110110001101001011000010111001100100000011001010110000100100000011000010110001000100000011001010111100000100000011001010111011001100101011011100110100101100101011101000010000001101001011101010111001001100101001000000110110101100001011110000110100101101101011001010010000001100110011101010110011101101001011000010111010000100000011011100110100101101000011010010110110001100100
				</p>
			</div>
			<div class="id-card" id="front-id-card">
				<div id="fotos">
					<div id="f4x4">
						<div id="foto-estilizada">
							<img src={Profile} alt="" />
							<img src={Profile} alt="" />
						</div>
					</div>
				</div>
				<div id="informacoes">
					<h2>Nome</h2>
					<p>Tito Ribeiro de Almeida Guidotti</p>
					<h2>Naturalidade</h2>
					<p>São Paulo - SP</p>
					<h2>Sobre</h2>
					<p>Oiê! Eu sou o Tito, sou um programador apaixonado por tecnologia.</p>
				</div>
			</div>
		</div>
		<div id="wave-dividers">
			{#each ['rgb(108, 0, 0)', 'rgb(0, 31, 98)', 'rgb(83, 0, 91)', 'rgb(0, 144, 110)', 'rgb(108, 0, 0)', 'rgb(0, 31, 98)', 'rgb(83, 0, 91)', 'rgb(0, 144, 110)', 'rgb(108, 0, 0)', 'rgb(0, 31, 98)', 'rgb(83, 0, 91)', 'rgb(0, 144, 110)'] as cor}
				<div class="wavy" style="background-color: {cor};"></div>
			{/each}
		</div>
		<div id="tecnologias" class="wavy">
			<aside><h1>Tecnologias</h1></aside>
			<div bind:this={painelIconesEl}>
				<PainelIcones />
			</div>
		</div>

		<div id="transicao-barras" bind:this={barrasTransicaoEl}>
			{#each { length: nBarras } as item}
				<!-- content here -->
				<div class="barra-transicao" style="--nBarras:{nBarras};"></div>
			{/each}
		</div>
	</div>
</main>

<style>
	* {
		font-family:
			Futura,
			Trebuchet MS,
			Arial,
			sans-serif;
	}
	main {
		min-height: 100%;
		display: flex;
		flex-direction: column;
	}
	#front,
	.after-rotsqr {
		background-color: rgb(39, 39, 39);
	}
	#front {
		height: 100dvh;
		width: 100%;
		display: flex;
		position: sticky;
		top: 0;
		z-index: -1;
	}
	#nome {
		container-type: inline-size;
		width: 400px;
		margin: auto;
	}
	#front h1,
	#front h2 {
		background: -webkit-linear-gradient(right, #9500ff, #f5004e);
		background-clip: text;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		margin: 0;
	}
	#front h1 {
		font-size: 220px;
		text-align: center;
		line-height: 170px;
		/* resize: both;
		overflow: auto; */
	}
	#front h2 {
		font-size: 40px;
		text-align: right;
	}

	#content {
		height: auto;
		overflow: hidden;
		margin: 0;
		padding: 0;
	}
	#rotsqr {
		width: 100vw;
		/* max-width: 100%; */
		/* height: 80vw; */
		height: 500px;
		background-color: black;
		color: white;
		position: relative;
	}
	#rotsqr-container {
		background-image: linear-gradient(transparent 50%, black 50%);
	}
	.after-rotsqr {
		margin: 0;
		padding: 20px;
	}
	#sobre-mim {
		background-color: black;
		padding: 10px;
		position: relative;
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 20vh 0 100vh 0;
	}
	.id-card {
		border: 2px solid gray;
		width: 400px;
		height: 240px;
		border-radius: 10px;
		color: gray;
		background-color: rgb(163, 255, 163);
		display: flex;
	}
	#back-card {
		position: absolute;
		transform: translate(100px, -100px) rotate(10deg);
		transition: transform 0.8s;
	}
	#sobre-mim:hover #back-card {
		transform: translate(110px, -110px) rotate(12deg);
	}
	#card-strip {
		height: 20%;
		width: 100%;
		background-color: black;
		position: relative;
		top: 150px;
	}
	#back-card p {
		word-wrap: break-word;
		position: absolute;
		right: 18px;
		top: 20px;
		width: 200px;
		font-size: 5px;
	}
	#back-card img {
		position: absolute;
		width: 35%;
		top: 30px;
		left: 30px;
		border: 1px dashed rgba(128, 128, 128, 0.4);
	}
	#front-id-card {
		z-index: 1;
	}

	#fotos {
		padding: 20px;
	}
	#f4x4 {
		padding: 10px;
		background-color: white;
		border-radius: 10px;
		height: 100%;
		width: 100%;
		overflow: clip;
	}
	#f4x4 > div {
		border-radius: 10px;
		background-color: rgb(123, 203, 225);
		overflow: clip;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		position: relative;
	}
	#front-id-card img {
		/* width: 100px; */
		width: 120%;
		display: block;
		margin-left: auto;
		margin-right: auto;
	}
	#front-id-card img:last-child {
		filter: brightness(0) invert();
		position: absolute;
		top: -15%;
		transition: opacity 1s;
	}
	#front-id-card:hover img:last-child {
		opacity: 0;
	}
	#informacoes {
		padding: 20px 20px 20px 0;
	}
	#front-id-card h2 {
		font-size: 10px;
		color: black;
		font-weight: 900;
		margin: 0;
		font-family: monospace;
		text-decoration: underline;
	}
	#front-id-card p {
		font-size: 14px;
		margin: 0;
		margin: 2px 0 5px 0;
	}
	#tecnologias {
		background-color: black;
	}
	.wavy {
		min-height: 30px;
		width: 100%;
		/* background-color: blue; */
		position: relative;
		transition:
			transform 0.3s,
			filter 0.3s;
	}
	.wavy::after {
		position: absolute;
		top: -19px;
		left: 0;
		content: '';
		height: 20px;
		width: 100%;
		background-color: inherit;
		--mask: radial-gradient(16.8px at 50% 23.5px, #000 99%, #0000 101%) calc(50% - 20px) 0/40px 100%,
			radial-gradient(16.8px at 50% -13.5px, #0000 99%, #000 101%) 50% 10px/40px 100% repeat-x;
		-webkit-mask: var(--mask);
		mask: var(--mask);
	}
	#wave-dividers .wavy:hover {
		transform: scale(1.01);
		filter: brightness(1.1);
	}

	@keyframes anim-vazia {
		100% {
		}
	}
	@keyframes tecnologias-move {
		from {
			transform: translate(100%, 0);
		}
		to {
			transform: translate(0%, 0);
		}
	}
	#tecnologias {
		padding: 100px 10px;
		background-color: rgb(25, 25, 0);
		position: relative;
	}
	#tecnologias > div {
		margin: auto;
		width: 500px;
		height: 370px;
		animation: tecnologias-move 1s forwards;
	}
	#tecnologias > aside {
		height: 370px;
		width: 70px;
		float: left;
	}
	#tecnologias > aside > h1 {
		transform: rotate(-90deg);
		color: rgb(108, 108, 108);
		font-size: 60px;
		top: 230px;
		position: relative;
	}
	#transicao-barras {
		position: relative;
		background-color: rgb(25, 25, 0);
	}
	.barra-transicao {
		width: calc(100% + calc(100vh / var(--nBarras)));
		height: calc(100vh / var(--nBarras));
		background-color: yellow;
		--r: calc(calc(100vh / var(--nBarras)) / 2);
		border-radius: var(--r) 0 0 var(--r);
		position: relative;
	}
</style>
