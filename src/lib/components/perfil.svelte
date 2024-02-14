<script>
    import profile from '$lib/assets/profile.png';
</script>
<main>
    <div>
        <img src={profile} alt="" srcset="">
    </div>
</main>
<style>
    main {
		height: 150px;
		width: 150px;
		position: relative;
        border: 1px solid rgba(0, 0, 0, 0.167);
        border-radius: 10px;
        overflow: hidden;
        padding: 10px;
        background-color: white;
	}
    img{
        max-width: 100%;
        max-height: 100%;
        filter: brightness(0) invert();
    }
    div{
        max-height: 100%;
        overflow: hidden;
        background-color: rgb(123, 203, 225);
        border-radius: 10px;
    }
</style>