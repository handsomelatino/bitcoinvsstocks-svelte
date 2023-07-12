<script>
  import ThemeToggle from "./ThemeToggle.svelte";
</script>

<nav>
  <a href='/'>Home</a>
  <a href='/about'>About</a>
</nav>

<ThemeToggle />

<slot />

<style>
  :global(body) {
    margin: 0;
    background-color: var(--main-bg);
    color: var(--text-color);
    font-family: var(--maven-pro);
  }
</style>