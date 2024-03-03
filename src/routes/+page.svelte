<script>
  import Calculator from "./Calculator.svelte";
  import Splash from "./Splash.svelte";
  import FAQ from './FAQ.svelte';
</script>

<main>
  <Splash />
  <Calculator />
  <FAQ />
</main>

<style>
  main {
    scroll-behavior: smooth;
  }
</style>