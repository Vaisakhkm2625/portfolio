<script>
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

  import Background from "$lib/components/Background.svelte";

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>


<!-- <div class="image"></div> -->

<Background />

{@render children?.()}



<style>
  .image {
    position: absolute;
    height: 100%;
    width: 100%;
    top: 0;
    /* background-image: url("https://st3.depositphotos.com/4478807/19082/i/450/depositphotos_190824428-stock-photo-white-geometric-hexagonal-abstract-background.jpg"); */
    background-image: url("myroom.png");
    background-size: contain;
    background-position: center;
    /* background-repeat: no-repeat; */
      filter: blur(5px);               
    z-index: -1;
  }



</style>


