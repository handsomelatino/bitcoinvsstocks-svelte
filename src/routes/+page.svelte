<script>
  import Calculator from "./Calculator.svelte";
  import Splash from "./Splash.svelte";
  import FAQ from './FAQ.svelte';
</script>

<svelte:head>
  <title>Bitcoin vs Stocks: DCA Calculator</title>
  <meta name="description" content="Compare the results of dollar cost averaging (DCA) in Bitcoin versus stocks, indexes and precious metals.">
</svelte:head>

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