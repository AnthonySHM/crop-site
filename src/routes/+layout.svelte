<script lang="ts">
  import { browser } from '$app/environment';
  import { onMount } from 'svelte';

  onMount(async () => {
    if (browser) {
      await import('bootstrap');
    }
  });
</script>

<main class="container mt-4">
  <slot />
</main>
