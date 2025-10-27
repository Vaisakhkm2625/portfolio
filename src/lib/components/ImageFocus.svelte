<script lang="ts">
  import { onMount, tick } from 'svelte';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';

  /**
   * Props for the ImageFocus component.
   * @typedef {Object} ImageFocusProps
   * @property {string} [src=''] - The URL of the image to display.
   * @property {string} [alt=''] - Alt text for the image for accessibility.
   * @property {Array<{x: number, y: number, radius: number, description: string}>} [focusAreas=[]] - Array of focus areas with percentage-based coordinates, radius, and descriptions.
   */

  // Props with Svelte 5 runes
  let {
    src = $bindable(''), // Image source
    alt = $bindable(''), // Image alt text
    focusAreas = $bindable([]), // Array of focus areas
  } = $props();

  /**
   * X-coordinate of the focus position as a percentage of the container width.
   * @type {number}
   */
  let focusX = tweened(0, { duration: 100, easing: cubicOut });

  /**
   * Y-coordinate of the focus position as a percentage of the container height.
   * @type {number}
   */
  let focusY = tweened(0, { duration: 100, easing: cubicOut });

  /**
   * Size of the focus circle as a percentage of the container width.
   * @type {number}
   */
  let focusSize = tweened(10, { duration: 500, easing: cubicOut });

  /**
   * Zoom scale for the focused image.
   * @type {number}
   */
  let zoomScale = tweened(10, { duration: 500, easing: cubicOut });

  /**
   * Whether the mouse is hovering over the image container.
   * @type {boolean}
   */
  let isHovering = $state(false);

  /**
   * The description to display for the current focus area.
   * @type {string}
   */
  let currentDescription = $state('');

  /**
   * Reference to the image container DOM element.
   * @type {HTMLElement | null}
   */
  let container = $state(null);

  /**
   * Index of the current focus area during animation.
   * @type {number}
   */
  let currentFocusIndex = $state(0);

  /**
   * Interval ID for the focus area animation.
   * @type {ReturnType<typeof setInterval> | null}
   */
  let animationInterval = $state<ReturnType<typeof setInterval> | null>(null);

  /**
   * Starts the animation to cycle through focus areas every 3 seconds.
   */
  async function startFocusAnimation() {
    if (focusAreas.length === 0) return;

    // Set initial focus area
    const initialArea = focusAreas[0];
    focusX.set(initialArea.x);
    focusY.set(initialArea.y);
    currentDescription = initialArea.description;
    focusSize.set(10); // Default size
    zoomScale.set(1); // Default zoom

    // Start cycling through focus areas
    animationInterval = setInterval(() => {
      currentFocusIndex = (currentFocusIndex + 1) % focusAreas.length;
      const area = focusAreas[currentFocusIndex];
      focusX.set(area.x);
      focusY.set(area.y);
      currentDescription = area.description;
      focusSize.set(10);
      zoomScale.set(1);
    }, 3000);
  }

  /**
   * Stops the focus area animation.
   */
  function stopFocusAnimation() {
    if (animationInterval !== null) {
      clearInterval(animationInterval);
      animationInterval = null; }
  }

  /**
   * Handles mouse enter to stop the animation and start tracking cursor.
   */
  async function handleMouseEnter() {
    isHovering = true;
    stopFocusAnimation();
    await tick(); // Ensure DOM updates
    focusSize.set(15); // Larger focus area on hover
    zoomScale.set(1); // Reset zoom
  }

  /**
   * Handles mouse movement to update focus position and check for nearby focus areas.
   * @param {MouseEvent} event - The mouse move event.
   */
  async function handleMouseMove(event: MouseEvent) {
    if (!container) return;
    const rect = container.getBoundingClientRect();
    const mouseX = ((event.clientX - rect.left) / rect.width) * 100; // Convert to percentage
    const mouseY = ((event.clientY - rect.top) / rect.height) * 100;

      console.log({mouseX,mouseY});

    // Check for nearby focus area
    let foundArea = null;
    for (const area of focusAreas) {
      const dx = mouseX - area.x;
      const dy = mouseY - area.y;
      const distance = Math.sqrt(dx * dx + dy * dy);
      if (distance <= area.radius) {
        foundArea = area;
        break;
      }
    }

    if (foundArea) {
      // Move to focus area and zoom in
      focusX.set(foundArea.x);
      focusY.set(foundArea.y);
      currentDescription = foundArea.description;
      focusSize.set(15); // Larger focus for defined area
      zoomScale.set(1.6); // Slight zoom
    } else {
      // Follow cursor with default size
      focusX.set(mouseX);
      focusY.set(mouseY);
      currentDescription = '';
      focusSize.set(20); // Larger than auto-animation, smaller than focus area
      zoomScale.set(1.4);
    }
  }

  /**
   * Handles mouse leave to resume the animation.
   */
  async function handleMouseLeave() {
    isHovering = false;
    currentDescription = '';
    focusSize.set(10); // Reset to default size
    zoomScale.set(1); // Reset zoom
    await tick(); // Ensure DOM updates
    startFocusAnimation();
  }

  // Start animation on mount and clean up on destroy
  onMount(() => {
    startFocusAnimation();
    return () => {
      if (animationInterval !== null) {
        clearInterval(animationInterval);
        animationInterval = null;
      }
    };
  });
</script>

<div
  bind:this={container}
  on:mouseenter={handleMouseEnter}
  on:mousemove={handleMouseMove}
  on:mouseleave={handleMouseLeave}
  class="relative inline-block w-full"
>
  <img
    src={src}
    {alt}
    class="w-full h-auto object-cover transition-all duration-300"
    class:blur-md={isHovering}
  />
  <img
    src={src}
    alt=""
    class="absolute inset-0 w-full h-auto object-cover transition-opacity duration-300"
    class:opacity-0={!isHovering}
    class:opacity-100={isHovering}
    style="clip-path: circle({$focusSize}% at {$focusX}% {$focusY}%); transform: scale({$zoomScale}); transform-origin: {$focusX}% {$focusY}%;"
  />
  {#if currentDescription && isHovering}
    <div
      class="absolute bottom-4 left-4 bg-black/70 text-white p-2 rounded-md max-w-xs text-sm transition-opacity duration-300"
    >
      {currentDescription}
    </div>
  {/if}
</div>
