<script>
    export let dentes = 8;
    export let raio = 70;
    export let rEixo = 19;
    export let dh = 15;
    export let fill = 'gray';
    
    const b = 3;
</script>
<main>
    <div id="outer" style="
        width:{2*raio}px;
        height: {2*raio}px;
    "></div>    
    {#each {length: dentes} as _, i}
            <div class="dente" style="
            transform: rotate({2*Math.PI*i/dentes }rad); 
            height:{Math.PI*raio/dentes}px;
            width:{raio-rEixo + dh}px;
            top:{(raio - Math.PI*raio/dentes/2)  + Math.sin(2*Math.PI*i/dentes)*(2*rEixo + ((raio-rEixo + dh)-2*rEixo)/2)}px;
            left:{(raio - (raio-rEixo+ dh)/2) + Math.cos(2*Math.PI*i/dentes)*(2*rEixo + ((raio-rEixo + dh)-2*rEixo)/2)}px;
            background-color:{fill};"
            ></div>
    {/each}
    <div class="filler" style="
        width:{2*raio-2*b+1}px;
        height: {2*raio-2*b+1}px;
        top:{b-0.5}px;
        left:{b-0.5}px;
        border-width:{raio-rEixo}px;
        border-color: {fill};
    "></div>
    <div id="inner" style="
        width:{2*rEixo}px;
        height: {2*rEixo}px;
        left: {raio-rEixo}px;
        top: {raio-rEixo}px;
    "></div>
</main>
<style>
    main{
        position: relative;
    }
    div{
        background-color: blue;
        border: 3px solid black;
        position: absolute;
        box-sizing: border-box;
        left: 0;
        top: 0;
    }
    #outer,#inner,.filler{
        border-radius: 50%;
        background-color: transparent;
    }
    .filler{
        border-color: blue;
    }
    #outer{
        position: relative;
    }
</style>