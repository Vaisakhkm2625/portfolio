<script>

	let { children } = $props();


</script>

<marquee class="border-2 border-green-500"
    style=" background-image: repeating-linear-gradient(
        45deg,
        #22c55e 0,
        #22c55e 10px,
        transparent 10px,
        transparent 20px
        );
    " >
    ⚠️
    <span class="backdrop-blur-sm">

        {#if children}
            {@render children()}
        {:else}
            This page is under construction 
        {/if}

    </span>
</marquee>
