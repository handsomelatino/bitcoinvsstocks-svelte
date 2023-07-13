<script>
    import Calculator from "./Calculator.svelte";
    import Splash from "./Splash.svelte";

</script>

<svelte:head>
    <title>Bitcoin VS Stocks</title> 
</svelte:head>

<main>
  <Splash />
  <Calculator />
</main>

<style>
  main {
    scroll-behavior: smooth;
  }
</style>