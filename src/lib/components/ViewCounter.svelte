<script>
	let count = $state('-');

	// runs on client mount
	$effect(() => {
		(async () => {
			try {
				const res = await fetch(
					'https://counter-api.vaisakhkm2625-cloudflare.workers.dev/?id=vaisakh-portfolio-e33d921ebc16'
				);
				const data = await res.json();
				count = data.value ?? data.count ?? null;
			} catch {
				count = '–';
			}
		})();
	});
</script>

<span class="font-semibold">
    {count ?? '...'}
</span>

<!-- <div -->
<!-- 	class="inline-flex items-center gap-2 rounded-md bg-gray-100 px-3 py-1.5 text-sm dark:bg-gray-800" -->
<!-- > -->
<!-- 	<!-- <span class="font-medium grayscale-100">&#128065;</span> --> 
<!-- 	<span class="font-semibold"> -->
<!-- 		{count ?? '...'} -->
<!-- 	</span> -->
<!-- </div> -->
