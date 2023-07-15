<script>
  import { onMount } from "svelte";
  let color;

  onMount(() => {
    const defaultColor = window.matchMedia?.("(prefers-color-scheme: dark)")?.matches ? 'dark' : 'light';
    color = localStorage.getItem("bvs__color_scheme") || defaultColor;
    color === 'dark' && window.document.body.classList.add(`theme-dark`);
  });

  const toggleTheme = () => {
    if (color === 'dark') {
      window.document.body.classList.remove(`theme-dark`);
      localStorage.setItem("bvs__color_scheme", 'light');
      color = 'light;'
    }
    else {
      window.document.body.classList.add(`theme-dark`);
      localStorage.setItem("bvs__color_scheme", 'dark');
      color = 'dark';
    }
  }
</script>


<button on:click={toggleTheme}>
  {#if color === 'dark'}
    <svg class="__sun-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M17 12c0 2.762-2.238 5-5 5s-5-2.238-5-5 2.238-5 5-5 5 2.238 5 5zM7.816 6.401 4.222 2.807 2.808 4.221l3.594 3.595a7.048 7.048 0 0 1 1.414-1.415zM12 5c.34 0 .672.033 1 .08V0h-2v5.08c.328-.047.66-.08 1-.08zm5.598 2.815 3.594-3.595-1.414-1.414-3.594 3.595a7.07 7.07 0 0 1 1.414 1.414zM5 12c0-.34.033-.672.08-1H0v2h5.08A7.026 7.026 0 0 1 5 12zm11.185 5.598 3.594 3.593 1.415-1.414-3.594-3.594a7.08 7.08 0 0 1-1.415 1.415zm-9.784-1.414-3.593 3.593 1.414 1.414 3.593-3.593a7.07 7.07 0 0 1-1.414-1.414zM18.92 11c.047.328.08.66.08 1s-.033.672-.08 1H24v-2h-5.08zM12 19c-.34 0-.672-.033-1-.08V24h2v-5.08c-.328.047-.66.08-1 .08z"></path></svg>
  {:else}
    <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" viewBox="0 0 256 256"><g><g style="stroke: none; stroke-width: 0; stroke-dasharray: none; stroke-linecap: butt; stroke-linejoin: miter; stroke-miterlimit: 10; fill: none; fill-rule: nonzero; opacity: 1;"><path d="M87.823 60.7a1.485 1.485 0 0 0-1.695-.214 33.03 33.03 0 0 1-44.232-12.718A33.03 33.03 0 0 1 52.997 3.103a1.481 1.481 0 0 0-.598-2.727c-9.843-1.265-19.59.692-28.193 5.66C13.8 12.041 6.356 21.743 3.246 33.35S1.732 57.08 7.741 67.487c6.008 10.407 15.709 17.851 27.316 20.961A45.26 45.26 0 0 0 46.774 90c7.795 0 15.489-2.044 22.42-6.046 8.601-4.966 15.171-12.43 18.997-21.586a1.484 1.484 0 0 0-.368-1.668z" transform="translate(1.964 1.964) scale(2.8008)" style="stroke: none; stroke-width: 1; stroke-dasharray: none; stroke-linecap: butt; stroke-linejoin: miter; stroke-miterlimit: 10; fill: rgb(255, 255, 255); fill-rule: nonzero; opacity: 1;"></path></g></g></svg>
  {/if}
</button>

<style>
  button {
    all: unset;
    cursor: pointer;
    position: fixed;
    top: 16px;
    right: 16px;
    z-index: 1;
    background-color: black;
    border-radius: 100%;
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: var(--dark-bg);
  }

  button > svg {
    width: 24px;
    height: 24px;
    /* fill: var(--text-dim); */
  }

</style>