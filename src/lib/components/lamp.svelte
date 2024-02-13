<script>
</script>

<main>
	<div><span></span></div>
	<svg width="210" height="210">
		<path
			style="fill:white;stroke-width:4;stroke:black"
			d="
        M 90 150 
        A 20 20 0 0 0 70 140
        A 40 40 0 1 1 100 120
        A 20 20 0 0 0 105 140
        "
		/>
		<rect
			x="170"
			y="-22.5"
			width="30"
			height="30"
			rx="3"
			style="fill:gray;stroke-width:4;stroke:black"
			transform="rotate(59 0 0)"
		/>
	</svg>
    <section>
        <svg width="210" height="210" id="coil">
            <path
                style="fill:none;stroke-width:2;stroke:black"
                d="
            M 88 142
            L 60 100
            A 10 10 0 1 0 78 108
            A 10 10 0 1 0 70 100
            L 95 135
            "
            />
        </svg>
    </section>
</main>

<style>
	main {
		height: 210px;
		width: 210px;
		position: relative;
	}
	svg {
		position: absolute;
		left: 40px;
		bottom: -30px;
	}
	div {
		position: absolute;
		top: 30px;
		left: 5px;
		display: flex;
		height: 200px;
		width: 200px;
		justify-content: center;
		align-items: center;
	}
	span {
		border-radius: 50%;
		height: 100px;
		width: 100px;
		background-color: rgb(255, 255, 151, 0.7);
		/* box-shadow: 0 0 10px rgba(255, 255, 0, 0.5), 0 0 10px inset rgb(255, 255, 0, 0.5); */
		animation: glow 2s linear infinite;
	}
    section{
		animation: coil-glow 2s linear infinite;
    }
	@keyframes glow {
		0% {
			width: 70px;
			height: 70px;
			background-color: rgb(255, 255, 151, 0.5);
		}
		30% {
			background-color: rgb(255, 255, 151, 0.2);
		}
		50% {
			width: 180px;
			height: 180px;
			background-color: transparent;
		}
		100% {
			background-color: transparent;
            color: #ffee9b88;
		}
	}
	@keyframes coil-glow {
		0% {
            opacity: 50%;
		}
        10% {
            opacity: 5%;
		}
		100% {
            opacity: 50%;
		}
	}
</style>
