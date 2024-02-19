<script>
export let titulo = "Tito";
export let subtitulo = "Guidotti";
</script>
<main>
    <div>
        {#each titulo.split('') as letra}
             <h1>{letra}</h1>
        {/each}
    </div>
    <h2>{subtitulo}</h2>
</main>
<style>
    main{
        text-align: right;
        margin-top: 30px;
    }
    h1, h2{
        font-family: 'Public Sans', sans-serif;
        display: inline;
        color: white;
        margin: 0;
    }
    h1{
        font-size: 180px;
        line-height: 140px;
        -webkit-text-stroke-width: 4px;
		-webkit-text-stroke-color: black;
    }
    h2{
        font-size: 40px;
        -webkit-text-stroke-width: 1px;
		-webkit-text-stroke-color: black;
    }
</style>