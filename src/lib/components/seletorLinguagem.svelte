<script>
    import flagBR from '$lib/assets/flagBR.svg';
    import flagUSA from '$lib/assets/flagUSA.svg';
    import { t, locale, locales } from "$lib/texto.js";
    
    let checked;

    function onChange(){
        if(checked)
            $locale = 'pt';
        else
            $locale = 'en';
    }
</script>
<label>
    <div id="ilha"></div>
    <input type="checkbox" name="linguagem" id="" on:change={onChange} bind:checked={checked}>
    <div id="movel">
        <div class="flag">
            <img src={flagBR} alt="Bandeira brasileira">
        </div>
        <div id="mastro"></div>
        <div class="flag">
            <img src={flagUSA} alt="Bandeira americana">
        </div>
    </div>
    <!-- <div id="mastro">
    </div> -->
</label>
<style>
    @keyframes extender{
        0%{
            width: 0%;
        }
        50%{
            width: 0%;
        }
        100%{
            width: 10.5px;
        }
    }
    @keyframes recolher{
        0%{
            width: 10.5px;
        }
        50%{
            width: 0%;
        }
        100%{
            width: 0%;
        }
    }
    label{
        height: 22px;
        width: 36px;
        display: block;
        position: relative;
        display: flex;
        justify-content: center;
        --tempo-transicao:0.5s;
        cursor: pointer;
        border: 1px solid transparent;
        overflow: hidden;
    }
    #movel{
        border-radius: 50%;
        height: 30px;
        width:30px;
        display: flex;
        justify-content: center;
        position: absolute;
        top: 3px;
        transform: rotate(-20deg);
        transition: transform var(--tempo-transicao);
    }
    #mastro{
        position: absolute;
        background-color: black;
        width: 2px;
        top: -2px;
        height: 18px;
        border-radius: 0 0 2px 2px;
    }
    img{
        width: 100%;
        height: 8px;
        position: absolute;
        object-fit: cover;
        user-select: none;
    }
    .flag{
        position: absolute;
        top: -2px;
    }
    .flag:first-child{
        right: calc(50% + 1px);
        animation: extender var(--tempo-transicao) forwards;
    }
    .flag:last-child{
        left: calc(50% + 1px);
        animation: recolher var(--tempo-transicao) forwards;
    }
    #ilha{
        position: absolute;
        width: 50%;
        height: 80%;
        background-color: rgb(0, 177, 0);
        border-radius: 50%;
        top: 70%;
    }
    input{
        display: none;
    }
    input:checked+#movel{
        transform: rotate(20deg);
    }
    input:checked+#movel .flag:first-child{
        animation: recolher var(--tempo-transicao) forwards;
    }
    input:checked+#movel .flag:last-child{
        animation: extender var(--tempo-transicao) forwards;
    }
</style>
