<script>
	import { onMount } from 'svelte';
	import Diploma from '$lib/components/diploma.svelte';
	import Gears from '$lib/components/gears.svelte';
	import Lamp from '$lib/components/lamp.svelte';
	import Perfil from '$lib/components/perfil.svelte';
	import SeletorLinguagem from '$lib/components/seletorLinguagem.svelte';

	let nomeEstilizado;

	Number.prototype.clamp = function (min, max) {
		return Math.min(Math.max(this, min), max);
	};

	let h1Shadowed = [];
	let h2Shadowed = [];

	let shadowCaster = {
		trueX: 0,
		trueY: 0,
		lerpedX: 0,
		lerpedY: 0,
		readMouseEvent(event) {
			if(maxOff == 10){
				setTimeout(() => {
					maxOff = 100;
				}, 500);
			}
			this.trueX = event.clientX;
			this.trueY = event.clientY;
		},
		updateShadows() {
			if (this.trueX != this.lerpedX || this.trueY != this.lerpedY) {
				this.lerpedX = moveTowards(this.lerpedX, this.trueX);
				this.lerpedY = moveTowards(this.lerpedY, this.trueY);

				h1Shadowed.forEach((element) => {
					let scenter = getCenter(element);
					let offs = [
						50 * suavizar(scenter.x - this.lerpedX),
						50 * suavizar(scenter.y - this.lerpedY)
					];
					element.style.textShadow =
						'calc( max(0, calc( calc(2 *  var(--progresso)) - 1 ) ) * ' +
						offs[0] +
						'px) calc( max(0, calc( calc(2 *  var(--progresso)) - 1 ) ) * ' +
						offs[1] +
						'px) 5px';
				});
				h2Shadowed.forEach((element) => {
					let scenter = getCenter(element);
					let offs = [
						10 * suavizar(scenter.x - this.lerpedX),
						10 * suavizar(scenter.y - this.lerpedY)
					];
					element.style.textShadow =
						'calc( var(--progresso) * ' +
						offs[0] +
						'px) calc(var(--progresso) * ' +
						offs[1] +
						'px) ' +
						10 * suavizar(Math.abs(scenter.x - this.lerpedX) + Math.abs(scenter.y - this.lerpedY)) +
						'px';
				});
			}
		}
	};

	let maxOff = 10;
	function moveTowards(value, target){
		return lerp(value, target, 0.1).clamp(value-maxOff, value+maxOff);
	}
	function lerp(start, end, amt) {
		return (1 - amt) * start + amt * end;
	}

	function suavizar(x) {
		return 1 / (1 + Math.pow(Math.E, -x / 100)) - 0.5;
	}

	/**
	 * @param {HTMLElement} el
	 */
	function getCenter(element) {
		var rect = element.getBoundingClientRect();
		return { x: rect.left + rect.width / 2, y: rect.top + rect.height / 2 };
	}

	function onUpdate() {
		shadowCaster.updateShadows();
		requestAnimationFrame(onUpdate);
	}

	let waterEmergeTime = 2000;
	let waterEmergeTimeDelay = 1000;
	function emergeFromWater(t) {
		if(t<waterEmergeTimeDelay){
			requestAnimationFrame(emergeFromWater);
		}
		else{
			t -= waterEmergeTimeDelay;
			let progresso = Math.min(1, t / waterEmergeTime);
			console.log(progresso);
			nomeEstilizado.style.setProperty('--progresso', progresso);
	
			if (t < waterEmergeTime) requestAnimationFrame(emergeFromWater);
		}
	}

	onMount(() => {
		h1Shadowed = [...document.querySelectorAll('h1 .shadowed')];
		h2Shadowed = [...document.querySelectorAll('h2 .shadowed')];
		
		let rect = nomeEstilizado.getBoundingClientRect();
		shadowCaster.trueX = rect.left + rect.width/2;
		shadowCaster.lerpedX = shadowCaster.trueX;
		shadowCaster.trueY = rect.top + rect.height/2;
		shadowCaster.lerpedY = shadowCaster.trueY;

		onUpdate();
	});
</script>

<main on:mousemove={(e) => shadowCaster.readMouseEvent(e)}>
	<div id="background">
		<div id="bkg-waves"></div>
		<div id="bkg-stripes"></div>
	</div>
	<div>
		<SeletorLinguagem />
	</div>
	<div>
		<div class="button-holder esquerdo">
			<!-- svelte-ignore a11y-missing-attribute -->
			<a>
				<div class="tchan">
					<div class="moving-tchan">
						<div class="inner-tchan">
							<Perfil />
						</div>
					</div>
				</div>
				<span>Sobre mim</span>
			</a>
		</div>
		<div class="button-holder direito">
			<!-- svelte-ignore a11y-missing-attribute -->
			<a>
				<div class="tchan">
					<div class="moving-tchan">
						<div class="inner-tchan">
							<Lamp />
						</div>
					</div>
				</div>
				<span>Projetos</span>
			</a>
		</div>
	</div>
	<div class="centerpiece">
		<div class="nome-estilizado" bind:this={nomeEstilizado}>
			<div class="shadower">
				<h1>
					{#each 'Tito'.split('') as letra}
						<span class="shadowed">{letra}</span>
					{/each}
				</h1>
				<h2>
					{#each 'Guidotti'.split('') as letra}
						<span class="shadowed">{letra}</span>
					{/each}
				</h2>
			</div>
			<div class="overlay">
				<h1>Tito</h1>
				<h2>Guidotti</h2>
			</div>
		</div>
	</div>
	<div>
		<div class="button-holder esquerdo">
			<!-- svelte-ignore a11y-missing-attribute -->
			<a>
				<div class="tchan">
					<div class="moving-tchan">
						<div class="inner-tchan">
							<Gears />
						</div>
					</div>
				</div>
				<span>Tecnologias</span>
			</a>
		</div>
		<div class="button-holder direito">
			<!-- svelte-ignore a11y-missing-attribute -->
			<a>
				<div class="tchan">
					<div class="moving-tchan">
						<div class="inner-tchan">
							<Diploma />
						</div>
					</div>
				</div>
				<span>Formação</span>
			</a>
		</div>
	</div>
</main>

<style>
	main {
		height: 100%;
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		position: relative;
		--cor-fundo: #45a59d;
		--cor-areia: rgb(226, 221, 114);
	}
	#background{
		z-index: -20;
	}
	#background,
	#bkg-waves,
	#bkg-stripes {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		overflow: hidden;
		background-color: var(--cor-fundo);
	}
	#bkg-waves {
		background-image: url($lib/assets/sin.svg);
		filter: blur(1px);
	}

	#bkg-stripes {
		filter: blur(5px);
		width: calc(100% + 400px);
		--thickness-x: 50px;
		--thickness-y: 100px;
		--thickness-d: 90px;
		--offset-d: 84px;
		background: repeating-linear-gradient(
				0.463647609000005179rad,
				transparent var(--offset-d),
				transparent calc(var(--offset-d) + var(--thickness-d)),
				var(--cor-fundo) calc(var(--offset-d) + var(--thickness-d)),
				var(--cor-fundo) calc(var(--offset-d) + calc(var(--thickness-d) * 2))
			),
			repeating-linear-gradient(
				90deg,
				transparent,
				transparent var(--thickness-y),
				var(--cor-fundo) var(--thickness-y),
				var(--cor-fundo) calc(var(--thickness-y) * 2)
			),
			repeating-linear-gradient(
				0deg,
				transparent,
				transparent var(--thickness-x),
				var(--cor-fundo) var(--thickness-x),
				var(--cor-fundo) calc(var(--thickness-x) * 2)
			);
		animation: scroll-lateral infinite 4s linear;
	}

	@keyframes scroll-lateral {
		0% {
			transform: translate(0, 0);
		}
		100% {
			transform: translate(-400px, 0);
		}
	}
	div {
		width: 100%;
	}
	.button-holder {
		display: flex;
	}
	.button-holder:nth-child(even) {
		justify-content: flex-end;
	}
	a {
		width: 80%;
		max-width: 500px;
		min-width: 250px;
		transition:
			width 0.5s,
			max-width 0.5s;
		position: relative;
		margin: 10px 0;
	}
	a > span {
		font-family: monospace;
		display: block;
		border: 3px solid black;
		padding: 10px;
		text-align: center;
		font-size: 30px;
		background-size: 200% 100%;
		position: relative;
		background-color: rgb(141, 141, 141);
	}
	a > span::after {
		content: '	';
		width: 20px;
		height: 20px;
		position: absolute;
		border-radius: 10px;
		border: 2px solid black;
		background-color: var(--cor-fundo);
	}
	.button-holder:nth-child(even) a > span::after {
		left: 16px;
		top: 16px;
	}
	.button-holder:nth-child(odd) a > span::after {
		right: 16px;
		top: 16px;
	}
	/* .button-holder:nth-child(even) a > span{
		background: linear-gradient(to right, gray, green);
	}
	.button-holder:nth-child(odd) a > span{
		background: linear-gradient(to left, gray, green);
	} */

	a:hover {
		width: 90%;
		max-width: 530px;
	}
	a:hover > span {
		animation: gradient-out 0.5s ease forwards;
	}

	.button-holder:nth-child(even) a > span {
		border-radius: 30px 0 0 30px;
		border-right-width: 0;
		animation: gradient-out 0.5s ease forwards;
	}
	.button-holder:nth-child(odd) a > span {
		background-position: 100% 0%;
		border-radius: 0 30px 30px 0;
		border-left-width: 0;
		animation: gradient-in 0.5s ease forwards;
	}

	.button-holder:nth-child(even) a:hover > span {
		animation: gradient-in 0.5s ease forwards;
	}
	.button-holder:nth-child(odd) a:hover > span {
		animation: gradient-out 0.5s ease forwards;
	}

	.tchan {
		position: absolute;
		height: 300px;
		width: 300px;
		bottom: 0;
		overflow: hidden;
		pointer-events: none;
	}
	.button-holder:nth-child(even) .tchan {
		right: 5vw;
	}
	.button-holder:nth-child(odd) .tchan {
		left: 5vw;
	}

	/* .tchan, .moving-tchan, .inner-tchan{
		border: 1px dashed black;
	} */
	.moving-tchan {
		position: absolute;
		top: -50px;
		height: 200%;
		width: 200%;
		position: relative;
		transition: transform 0.5s;
		transform: rotate(-90deg);
	}
	.button-holder:nth-child(even) .moving-tchan {
		left: 0;
	}
	.button-holder:nth-child(odd) .moving-tchan {
		left: -100%;
		transform: rotate(90deg);
	}

	.button-holder:nth-child(odd) a:hover .moving-tchan {
		transform: rotate(10deg) !important;
	}
	.button-holder:nth-child(even) a:hover .moving-tchan {
		transform: rotate(-10deg) !important;
	}

	.inner-tchan {
		height: 35%;
		width: 35%;
		bottom: calc(50% + 5px);
		position: absolute;
		display: flex;
		align-items: flex-end;
	}
	.button-holder:nth-child(even) .inner-tchan {
		right: 50%;
		justify-content: flex-end;
	}
	.button-holder:nth-child(odd) .inner-tchan {
		left: 50%;
		justify-content: flex-start;
	}

	.centerpiece {
		display: flex;
		justify-content: center;
		pointer-events: none;
	}

	@keyframes gradient-in {
		from {
			background-position: 0% 0%;
		}
		to {
			background-position: 100% 0%;
		}
	}
	@keyframes gradient-out {
		from {
			background-position: 100% 0%;
		}
		to {
			background-position: 0% 0%;
		}
	}

	.nome-estilizado {
		font-size: 120px;
		-webkit-text-stroke-width: 4px;
		-webkit-text-stroke-color: black;
		color: var(--cor-areia);
		font-family: 'Public Sans', sans-serif;
		position: relative;
		width: 430px;
		--progresso: 1;
		z-index: -10;
	}
	.nome-estilizado .shadower {
		color: rgba(25, 49, 29, 0.7);
		text-shadow: 0 0 0;
	}
	.nome-estilizado .shadower h2 {
		color: rgba(25, 49, 29, 0.3);
	}
	.nome-estilizado .overlay {
		position: absolute;
		top: 0;
		left: 0;
	}
	.nome-estilizado h1 {
		line-height: 180px;
		font-weight: 900;
	}
	.nome-estilizado h2 {
		-webkit-text-stroke-width: 0px;
		color: var(--cor-fundo);
		text-align: right;
		word-spacing: -25px;
		font-size: 60px;
	}
	.nome-estilizado * {
		padding: 0;
		margin: 0;
	}
</style>
