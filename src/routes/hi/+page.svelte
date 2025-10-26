<script>

  import Timeline from '$lib/components/SimpleTimeline.svelte';
  const events = [
    { date: '2024-01-01', title: 'Project kickoff', description: 'Kickoff meeting and requirements.' },
    { date: '2024-02-15', title: 'Alpha release', description: 'Alpha tested by internal team.' , href: 'https://example.com'},
    { date: '2024-04-01', title: 'Public launch', description: 'Launched to customers.'},
  ];
</script>
  <!-- Section 1 -->

  <section class="snap-start h-screen flex justify-center bg-gradient-to-r from-blue-400 to-purple-600 text-white">
    <h1 class="text-6xl font-bold">Hi, I'm Vaisakh</h1>
    <Timeline {events} ></Timeline>

  </section>

  <section class="snap-start h-screen flex justify-center bg-gradient-to-r from-blue-400 to-purple-600 text-white">
    <h1 class="text-6xl font-bold">Hi, I'm Vaisakh</h1>
  </section>

  <!-- Section 2 -->
  <section class="snap-start h-screen flex items-center justify-center bg-gradient-to-r from-green-400 to-blue-500 text-white">
    <div class="text-center">
      <h2 class="text-5xl font-semibold mb-4">About Me</h2>
      <p class="text-xl max-w-xl mx-auto">I create beautiful and interactive web experiences using modern technologies like Tailwind CSS, Svelte, and React.</p>
    </div>
  </section>

  <!-- Section 3 -->
  <section class="snap-start h-screen flex items-center justify-center bg-gradient-to-r from-pink-400 to-red-500 text-white">
    <div class="text-center">
      <h2 class="text-5xl font-semibold mb-4">Projects</h2>
      <ul class="space-y-4 text-xl">
        <li>📌 Real-time Messaging Platform</li>
        <li>📌 Image PDF Tool</li>
        <li>📌 Watermark Tool for DOCX Images</li>
      </ul>
    </div>
  </section>

  <!-- Section 4 -->
  <section class="snap-start h-screen flex items-center justify-center bg-gradient-to-r from-yellow-400 to-orange-500 text-white">
    <h2 class="text-5xl font-semibold">Contact Me</h2>
  </section>
