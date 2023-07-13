<script>
</script>

<section>
  <h1><span class='btc'>Bitcoin</span> <small>versus</small> <span class='stocks'>Stocks</span></h1>

  <!-- <svg width="1512" height="840" viewBox="0 0 1512 840" fill="none" xmlns="http://www.w3.org/2000/svg"> -->
  <svg viewBox="0 0 1512 840" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M161.136 598.524L27.8849 335.112C20.7572 321.021 -0.5 326.092 -0.5 341.882V824.5C-0.5 832.784 6.21576 839.5 14.5 839.5H1497.5C1505.78 839.5 1512.5 832.784 1512.5 824.5V194.297C1512.5 190.567 1511.11 186.97 1508.6 184.21L1346 5.25181C1339.57 -1.8222 1328.25 -1.1607 1322.69 6.6141L1021.09 428.396C1015.77 435.833 1005.09 436.823 998.494 430.489L970.813 403.905C965.039 398.359 955.928 398.326 950.113 403.828L812.813 533.742C807.028 539.216 797.974 539.214 792.19 533.739L718.903 464.349C713.082 458.838 703.956 458.877 698.183 464.439L568.829 589.049C563.051 594.616 553.916 594.65 548.097 589.127L354.684 405.563C348.189 399.4 337.776 400.262 332.383 407.41L186.495 600.786C179.809 609.649 166.147 608.43 161.136 598.524Z" fill="black" fill-opacity="0.03"/>
  </svg>

  <p>Compare the profit or loss of doing DCA between investing on your favorite stocks vs <strong>buying bitcoin.</strong></p>

  <a href='#calculate'>Let's Begin</a>
    
</section>

<style>
  section {
    height: 100vh;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 32px;
  }

  svg {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    width: 100%;
    height: auto;
    z-index: -1;
  }

  h1 {
    font-family: var(--alfa-slab);
    font-weight: normal;
    text-transform: uppercase;
    text-align: center;
    width: 320px;
    font-size: 60px;
    margin: 20px auto;
  }
  
  h1 .btc {
    color: var(--emphasis);
    text-shadow: 0 4px 0 var(--emphasis-2);
  }

  h1 .stocks {
    font-size: 64px;
    text-shadow: 0 4px 0 var(--logo-shadow);
  }

  small {
    /* margin: 100px auto; */
    text-align: center;
    font-size: 30px;
    /* max-width: 800px; */
    position: relative;
    display: block;
    font-size: 32px;
  }

  small:before {
    content: "";
    display: block;
    width: 48px;
    height: 8px;
    background: var(--text-color);
    left: 20px;
    top: 50%;
    position: absolute;
    transform: translateY(-50%);
  }

  small:after {
    content: "";
    display: block;
    width: 48px;
    height: 8px;
    background: var(--text-color);
    right: 20px;
    top: 50%;
    position: absolute;
    transform: translateY(-50%);
  }

  p {
    max-width: 480px;
    text-align: center;
    font-size: 24px;
    line-height: 32px;
  }

  p strong {
    color: var(--emphasis);
  }

  a {
    display: block;
    text-decoration: none;
    color: var(--main-bg);
    background-color: var(--emphasis);
    padding: 12px 72px;
    font-size: 24px;
    border-radius: 8px;
    text-transform: uppercase;
    font-weight: bold;
    transition: filter 0.3s;
  }

  a:hover {
    transition: filter 0.3s;
    filter: brightness(1.1);
  }

</style>