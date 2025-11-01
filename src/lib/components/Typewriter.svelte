<script>
  /**
   * @typedef {Object} Props
   * @property {number} [speed=50]      - Milliseconds per character
   * @property {number} [cursorBlink=500] - Cursor blink interval (ms)
   * @property {boolean} [loop=false]   - Loop the animation
   * @property {() => void} [onComplete] - Called when typing finishes (once)
   */

  /** @type {Props} */
  export let speed = 50;
  export let cursorBlink = 500;
  export let loop = false;
  export let onComplete = undefined;

  /** @type {import('svelte').Snippet} */
  export let children;

  import { onMount, onDestroy, tick } from 'svelte';

  let fullText = '';
  let displayed = '';
  let typing = true;
  let interval = null;
  let blinkInterval = null;
  let hiddenEl = null;

  // -----------------------------------------------------------------
  // 1. Extract plain text from the hidden children render
  // -----------------------------------------------------------------
  async function extractText() {
    await tick();                     // wait for hiddenEl to be in the DOM
    fullText = hiddenEl?.textContent ?? '';
    startTyping();
    startCursorBlink();
  }

  // -----------------------------------------------------------------
  // 2. Typewriter logic
  // -----------------------------------------------------------------
  function startTyping() {
    let i = 0;
    displayed = '';
    typing = true;

    interval = setInterval(() => {
      if (i < fullText.length) {
        displayed = fullText.slice(0, ++i);
      } else {
        clearInterval(interval);
        typing = false;
        onComplete?.();
        if (loop) setTimeout(startTyping, 1000);
      }
    }, speed);
  }

  // -----------------------------------------------------------------
  // 3. Cursor blink
  // -----------------------------------------------------------------
  function startCursorBlink() {
    const cursor = document.querySelector('.typewriter-cursor');
    blinkInterval = setInterval(() => cursor?.classList.toggle('opacity-0'), cursorBlink);
  }

  onMount(() => extractText());

  onDestroy(() => {
    clearInterval(interval);
    clearInterval(blinkInterval);
  });
</script>

<div class="font-mono text-lg inline-block">
  <!-- Visible typed output (keeps HTML tags) -->
  <span class="inline-block min-h-[1.5em] align-bottom">
    {@html displayed.replace(/\n/g, '<br>')}
    <span
      class="typewriter-cursor inline-block w-0.5 h-5 bg-current align-middle ml-0.5 transition-opacity duration-100"
      class:opacity-0={!typing && !loop}
    ></span>
  </span>

  <!-- Hidden render only to extract plain text -->
  <div bind:this={hiddenEl} class="hidden">
    {@render children()}
  </div>
</div>

<style>
  .typewriter-cursor { animation: blink 1s step-end infinite; }
  @keyframes blink { 0%,100% { opacity:1; } 50% { opacity:0; } }
</style>
