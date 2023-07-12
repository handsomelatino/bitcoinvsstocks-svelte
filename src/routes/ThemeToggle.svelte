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

<button on:click={toggleTheme}>{ color === 'dark' ? 'Light' : 'Dark'}</button>
