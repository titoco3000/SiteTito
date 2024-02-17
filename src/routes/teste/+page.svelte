<script>
	import { onMount } from "svelte";
    import SeletorLinguagem from "../../lib/components/seletorLinguagem.svelte";
    import { t, locale, locales } from "$lib/texto.js";

</script>
<SeletorLinguagem/>
<p>{@html $t("projetos")}</p>