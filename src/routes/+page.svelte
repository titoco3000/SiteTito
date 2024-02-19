<script>
	import SeletorLinguagem from '$lib//components/seletorLinguagem.svelte';
	import NomeDestaque from '../lib/components/nomeDestaque.svelte';
	import Profile from '$lib/assets/profile.png';
	import Spiral from '$lib/assets/spiral.svg';

	import { t } from '$lib/texto.js';
</script>

<main>
	<header><SeletorLinguagem /></header>
	<div id="body">
		<div id="badge">
			<div id="perfil">
				<div id="foto-normal">
					<img src={Profile} alt="foto de tito" />
				</div>
				<div id="foto-estilizada">
					<img src={Profile} alt="foto estilizada de tito" />
				</div>
			</div>
			<div id="nome-holder">
				<NomeDestaque/>
			</div>
		</div>
		<div id="botoes">
			<button><span>{$t('sobremim')}</span></button>
			<button><span>{$t('projetos')}</span></button>
			<button><span>{$t('tec')}</span></button>
			<button><span>{$t('formacao')}</span></button>
		</div>
	</div>
</main>

<style>
	main {
		--cor-fundo:rgb(32, 32, 32);
		background-color: var(--cor-fundo);
		min-height: 100%;
		display: flex;
		flex-direction: column;
	}
	*{
		font-family: monospace;
	}
	header {
		display: flex;
		justify-content: end;
		height: 22px;
		flex-shrink: 0;
	}
	#body {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}
	#body > div {
		flex-grow: 1;
	}
	#badge {
		padding: 10px 10px 10px 40px;
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	#perfil {
		border: 1px solid black;
		width: 250px;
		height: 300px;
		position: relative;
		padding: 10px;
		border-radius: 10px;
		background-color: white;
	}
	#perfil > div {
		overflow: hidden;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 10px;
	}
	#perfil img {
		width: 130%;
	}
	#foto-estilizada {
		position: relative;
		top: -100%;
		background-color: rgb(123, 203, 225);
		transition: opacity 1s;
	}
	#foto-normal {
		background-color: rgb(179, 200, 206);
	}
	#foto-estilizada:hover {
		opacity: 0;
	}
	#foto-estilizada > img {
		filter: brightness(0) invert();
	}
	#botoes {
		min-width: 250px;
		display: flex;
		flex-direction: column;
		align-items: end;
		overflow: hidden;
	}
	#botoes > button {
		margin: 30px 0px 30px 20px;
		border: 0px solid black;
		border-radius: 4px 0 0 4px;
		border-right-width: 0;
		position: relative;
        font-size: 40px;
        overflow: visible;
        color: rgb(130, 130, 130);
		background-color: transparent;
		width: 100%;
		max-width: 700px;
		text-align: right;
		padding-right: 30px;
		transition: color 0.3s;
	}
	#botoes > button:hover{
		color: white;
	}
    #botoes > button>span{
        position: relative;
    }
	#botoes > button::before {
		content: '';
		position: absolute;
		top: -50px;
		right: 0;
		background-image: url($lib/assets/spiral.png);
		background-repeat: no-repeat;
		background-size: 600px 100%;
		background-position: right;
		width: 0px;
		height: 150px;
		transition: width .5s linear, filter 2s;
		box-shadow: inset 4px 0 20px var(--cor-fundo);
		filter: blur(30px);
	}
	#botoes > button:hover::before {
		width: 600px;
		transition: width 4s linear, filter .5s;
		filter: blur(0.5px);
	}
</style>
