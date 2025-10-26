<!-- Timeline.svelte -->
<script>
  // Props for timeline events
  export let events = [
    { date: "2025-10-01", title: "Project Started", description: "Initiated the core project." },
    { date: "2025-10-15", title: "Milestone 1", description: "Completed first phase." },
    { date: "2025-10-27", title: "Current Day", description: "Ongoing development." },
  ];
</script>

<!-- <div class="font-mono text-green-400 bg-black p-4 rounded-lg max-w-2xl mx-auto"> -->
<div>
  {#each events as event, index}

    <div class="flex">
      <!-- Timeline line -->
      <div class="flex flex-col items-center mr-4">
        <div class="w-3 h-3 bg-green-400 rounded-full"></div>
        {#if index < events.length - 1}
          <div class="w-0.5 h-16 bg-green-400"></div>
        {/if}
      </div>
      <!-- Event content -->
      <div class="mb-6">
        <div class="text-sm text-green-200">{event.date}</div>
        <div class="text-lg font-bold">{event.title}</div>
        <div class="text-sm">{@html event.description}</div>
      </div>
    </div>
  {/each}
</div>

<style>
  /* Ensure Tailwind classes handle dark mode and terminal aesthetic */
  :global(body) {
    background-color: #000;
  }
</style>
