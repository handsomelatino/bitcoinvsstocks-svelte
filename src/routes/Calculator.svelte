<script>
  import { PUBLIC_API_URL } from '$env/static/public';
  import POPULAR_STOCKS from '../utils/popular_stocks';

  const formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',

    // These options are needed to round to whole numbers if that's what you want.
    minimumFractionDigits: 0, // (this suffices for whole numbers, but will print 2500.10 as $2,500.1)
    maximumFractionDigits: 0, // (causes 2500.99 to be printed as $2,501)
  });

  let mode = 'simple';
  let amount = 250;
  let frequency = 'm';
  let years = 5;
  let ticker = '^GSPC';

  let dcaData;
  let fetchError;

  let fetching = false;
  let calculationDate;
  let showTickerMenu = false;

  let FREQUENCIES = [
    // { id: "d", text: "day" }, // <- day goes crazy because stock markets are closed on weekends
    { id: "w", text: "week" },
    { id: "b", text: "two weeks" },
    { id: "m", text: "month" },
  ];

  let YEARS = [
    { id: 1, text: "1 years" },
    { id: 2, text: "2 years" },
    { id: 3, text: "3 years" },
    { id: 4, text: "4 years" },
    { id: 5, text: "5 years" },
  ];

  let COMMON_STOCKS = [
    { id: "^GSPC", label: "S&P 500" },
    { id: "^DJI", label: "Dow Jones" },
    { id: "^IXIC", label: "NASDAQ" },
    { id: "GC=F",  label: "Gold" },
    { id: "SI=F",  label: "Silver" },
  ];

  const handleCalculate = async () => {
    plausible("Calculate", { props: { ticker } });

    fetching = true;
    fetchError = false;

    try {
      const tickerParam = encodeURIComponent(ticker);
      const response = await fetch(`${PUBLIC_API_URL}/?mode=${mode}&amount=${amount}&frequency=${frequency}&years=${years}&ticker=${tickerParam}`);

      if (response.ok) {
        dcaData = await response.json();
        calculationDate = new Date().toLocaleDateString('en-us', { year: "numeric", day: "numeric", month: "short" });
      }
      else {
        const data = await response.json();
        throw new Error(data.error);
      } 
    }
    catch (e) {
      dcaData = null;
      fetchError = e.message;
    }
    finally {
      fetching = false;
    }
  }

  const toggleTickerMenu = () => {
    if (showTickerMenu) {
      showTickerMenu = false;
      window.document.body.classList.remove('fixed-body');
    }
    else {
      showTickerMenu = true;
      window.document.body.classList.add('fixed-body');
    }
  }

  const changeTicker = value => {
    ticker = value;
    resetCalculator();
    toggleTickerMenu();
  }

  const resetCalculator = () => {
    dcaData = false;
    fetchError = false;
  }

  const scroll = node => {
    node.childNodes.forEach(childNode => {
      if (childNode.classList?.contains("selected")) {
        node.scrollTo(0, childNode.offsetTop - (node.offsetHeight / 2));
      }
    });
  }

  const handleShare = async () => {
    try {
      await navigator.share({
        title : 'Bitcoin vs Stocks: DCA Calculator',
        text  : "Compare the results of dollar cost averaging (DCA) in Bitcoin versus stocks, indexes and precious metals:",
        url   : "https://bitcoinvsstocks.com",
      });

      plausible("Share");
    }
    catch {}
  }

</script>

<section id="calculate">
  <h2><span class='btc'>Bitcoin</span> <small>vs</small> <span class='stocks'>Stocks</span></h2>

  <div class="inputs">
    <div class="grid">
      <span>Saving / Investing </span>
      <span class='dollar-container'>
        <span class='dollar-sign'>$</span>
        <input bind:value={amount} type="number" placeholder="amount" on:click={resetCalculator} />
      </span>

      <span>in Bitcoin every</span>

      <select bind:value={frequency} on:change={resetCalculator}>
        {#each FREQUENCIES as f}
          <option value={f.id}>{f.text}</option>
        {/each}
      </select>
      
      <span>for the past</span>
      <select bind:value={years} on:change={resetCalculator}>
        {#each [1, 2, 3, 4, 5, 6, 7, 8, 9] as y}
          <option value={y}>{y} {y === 1 ? 'year' : 'years'}</option>
        {/each}
      </select>
    </div>
      
    <div class="compared-to">
      <span>compared to </span>
      <div class="ticker-select">
        <button class="ticker-toggle" on:click={toggleTickerMenu}>
          <div class="ticker-item">
            <span class="ticker">{ticker}</span>
            <span class="label">{COMMON_STOCKS.find(s => s.id === ticker)?.label || POPULAR_STOCKS.find(s => s.id === ticker)?.label }</span>
            <svg width="14" height="8" viewBox="0 0 14 8" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M1 1L7.44454 5.8463L13.1157 1" stroke="#484848" stroke-width="1.81736"/>
            </svg>            
          </div>
        </button>

        {#if showTickerMenu}
          <div class="menu-overlay" on:click={toggleTickerMenu} tabindex="0" />

          <div class="ticker-menu" role="dialog" use:scroll>
            {#each COMMON_STOCKS as s}
              <div role="button" class="ticker-item from-menu" class:selected={ticker === s.id} on:click={() => changeTicker(s.id)}><span class="ticker">{s.id}</span> <span class="label">{s.label}</span></div>
            {/each}
            <hr />
            {#each POPULAR_STOCKS as s}
              <div role="button" class="ticker-item from-menu" class:selected={ticker === s.id} on:click={() => changeTicker(s.id)}><span class="ticker">{s.id}</span> <span class="label">{s.label}</span></div>
            {/each}
          </div> 
        {/if}

      </div>
    </div>

    {#if dcaData}
      <h2 class="results">Results</h2>
      <p class="results">Saving <strong>{formatter.format(amount)} { frequency === 'b' ? 'every' : 'per' } {FREQUENCIES.find(f => f.id === frequency).text}</strong> for the past {years} years until today ({calculationDate}) results in:</p>

      <table>
        <tr>
          <th />
          <th>BTC</th>
          <th>{ COMMON_STOCKS.find(s => s.id === dcaData.Stocks.ticker)?.label || dcaData.Stocks.ticker }</th>
        </tr>

        <tr>
          <td>Invested</td>
          <td>{ formatter.format(dcaData.Bitcoin.invested) }</td>
          <td>{ formatter.format(dcaData.Stocks.invested) }</td>
        </tr>

        <tr>
          <td>Return</td>
          <td>{ formatter.format(dcaData.Bitcoin.savings) }</td>
          <td>{ formatter.format(dcaData.Stocks.savings) }</td>
        </tr>

        <tr>
          <td>Profit</td>
          <td style:color={dcaData.Bitcoin.profit > 0 ? 'var(--green)' : 'red'}>{ dcaData.Bitcoin.profit }%</td>
          <td style:color={dcaData.Stocks.profit > 0 ? 'var(--green)' : 'red'}>{ dcaData.Stocks.profit }%</td>
        </tr>

        <tr>
          <td>Stacked</td>
          <td class="centered">{ dcaData.Bitcoin.btc_amount } BTC</td>
          <td class="centered"> — </td>
        </tr>
      </table>

    {:else}
      <button class='calculate' disabled={fetching} on:click={handleCalculate}>Calculate</button>
    {/if}

    {#if fetchError}
      <div class="error">
        <p>{ fetchError }</p>
      </div>
    {/if}

    <div class="bottom-links">
      <div class="links">
        <a href="#faq">FAQ</a>
        <a href="#donate">Donate</a>
        <button class="share" on:click={handleShare}>Share</button>
      </div>
      <svg width="20" height="16" viewBox="0 0 14 8" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M1 1L7.44454 5.8463L13.1157 1" stroke="white" stroke-width="1.81736"/>
      </svg>
    </div>
  </section>

<style>  
  section {
    font-size: 24px;
    padding: 16px 16px 80px;
    scroll-behavior: smooth;
    max-width: 600px;
    margin: 0 auto;
    min-height: 100vh;
    min-height: 100dvh;
    text-align: center;
    position: relative;

    @media screen and (max-width: 480px) {
      font-size: 18px;
    }
  }

  h2 {
    font-family: var(--alfa-slab);
    font-weight: normal;
    text-transform: uppercase;
    font-size: 22px;
    margin: 0 auto 20px;
  }

  h2 .btc {
    color: var(--emphasis);
  }

  h2 small {
    font-size: 12px;
    vertical-align: middle;
  }

  h2.results {
    font-size: 18px;
    margin-block: 32px 12px;
  }

  h2.results:before, h2.results:after {
    background-color: var(--text-dim);
    content: "";
    display: inline-block;
    height: 2px;
    position: relative;
    vertical-align: middle;
    width: 28%;
    transform: translateY(-50%);
  }

  h2.results:before {
    right: 1em;
    margin-left: -50%;
  }

  h2.results:after {
    left: 1em;
    margin-right: -50%;
  }

  p.results {
    margin-top: 12px;
  }

  .compared-to {
    margin-block: 20px;
  }

  .ticker-select {
    position: relative;
    margin-top: 8px;
  }

  .menu-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.2);
    z-index: 2;
    animation: show-menu-overlay 0.3s forwards;

    @media screen and (max-width: 480px) {
      background-color: rgba(0, 0, 0, 0.5);
    }
  }

  .ticker-toggle {
    all: unset;
    cursor: pointer;
    box-sizing: border-box;
    display: block;
    background-color: var(--light-bg);
    border-radius: 8px;
    width: 100%;
  }

  .ticker-menu {
    position: absolute;
    top: 100%;
    max-height: 50vh;
    overflow: auto;
    box-shadow: 0 0 4px 0 rgba(0, 0, 0, 15%);
    width: 100%;
    border-radius: 6px;
    background-color: var(--main-bg);
    padding: 8px 12px;
    text-align: left;
    margin-top: 8px;
    font-size: 20px;
    z-index: 3;
    border: 1px solid rgba(255, 255, 255, 0.7);

    @media screen and (max-width: 480px) {
      position: fixed;
      top: 5%;
      left: 5%;
      right: 5%;
      bottom: 5%;
      width: unset;
      max-height: unset;
      margin-top: 0;
    }
  }

  .ticker-menu hr {
    border-top: 1px solid var(--logo-shadow);
  }

  .ticker-toggle .ticker-item {
    gap: 12px;
    justify-content: space-between;
    align-items: center;
  }

  .ticker-toggle .ticker-item .label {
    flex: 1;
    text-align: left;
    color: var(--text-color);
  }

  .ticker-toggle .ticker-item svg {
    margin-top: 4px;
    margin-right: 8px;
  }
  
  .ticker-toggle .ticker-item svg path {
    stroke: var(--text-color);
  }

  .ticker-item {
    display: flex;
    gap: 8px;
    color: var(--text-dim);
    padding: 8px;
    cursor: pointer;
    border-radius: 4px;
  }

  .ticker-item.from-menu:hover {
    background-color: var(--light-bg);
  }

  .ticker-item.from-menu.selected {
    background-color: var(--light-bg);
  }

  .ticker-item .ticker {
    color: var(--text-color);
    font-weight: bold;
    flex-shrink: 0;
  }
  
  .ticker-item.from-menu .ticker {
    min-width: 68px;
  }

  .ticker-item .label {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  
  .error {
    color: var(--alert);
    margin-top: 40px;
    font-weight: bold;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px 16px;
    align-items: center;
  }

  .grid span {
    text-align: left;
  }

  .inputs p strong {
    color: var(--emphasis);
  }

  .dollar-container {
    position: relative;
  }

  .dollar-sign {
    position: absolute;
    left: 20px;
    top: 50%;
    transform: translateY(-50%);
    color: var(--text-dim);
  }

  .dollar-container input {
    padding-inline-start: 32px;
  }

  input, select {
    border: none;
    font-family: var(--maven-pro);
    padding: 8px 12px;
    border-radius: 8px;
    background-color: var(--light-bg);
    color: var(--text-color);
    font-size: 24px;
    text-align: right;
    width: 100%;

    @media screen and (max-width: 480px) {
      font-size: 18px;
    }
  }

  button.calculate {
    all: unset;
    cursor: pointer;
    box-sizing: border-box;
    text-decoration: none;
    color: var(--main-bg);
    background-color: var(--emphasis);
    padding: 12px 72px;
    font-size: 24px;
    border-radius: 8px;
    text-transform: uppercase;
    font-weight: bold;
    transition: filter 0.3s;
    margin: 12px 0;

    @media screen and (max-width: 480px) {
      display: block;
      width: 100%;
      font-size: 20px;
    }
  }

  button.calculate:disabled {
    opacity: 0.5;
    cursor: auto;
  }

  button.calculate:hover {
    transition: filter 0.3s;
    filter: brightness(1.1);
  }

  table {
    margin: 0 auto;
    font-size: 24px;
    border-collapse: collapse;

    @media screen and (max-width: 480px) {
      font-size: 16px;
      width: 100%;
    }
  }

  th {
    font-size: 28px;

    @media screen and (max-width: 480px) {
      font-size: 18px;
    }
  }

  th, td {
    padding: 16px 28px;

    @media screen and (max-width: 480px) {
      padding: 8px 16px;
    }
  }

  td {
    /* color: #DDDDDD; */
  }

  td.centered {
    text-align: center;
  }

  tr:nth-child(even) td {
    background-color: var(--light-bg);
  }

  td:first-child {
    border-top-left-radius: 8px;
    border-bottom-left-radius: 8px;
  }

  td:last-child {
    border-top-right-radius: 8px;
    border-bottom-right-radius: 8px;
  }

  .bottom-links {
    position: absolute;
    bottom: 12px;
    left: 0;
    right: 0;
    font-weight: bold;
  }

  .bottom-links svg path {
    stroke: var(--text-color);
  }

  .bottom-links svg {
    animation: anim-botttom-arrow 2s alternate infinite;
  }
  
  .links {
    display: flex;
    gap: 32px;
    justify-content: center;
    margin-bottom: 8px;
  }

  button.share {
    all: unset;
    display: none;
    box-sizing: border-box;
    cursor: pointer;
    color: var(--text-color);
    
    @media screen and (max-width: 480px) {
      display: block;
    }
  }

  .bottom-links a {
    text-decoration: none;
    color: var(--text-color);
  }

  .bottom-links a:hover {
    text-decoration: underline;
  }

  @keyframes show-menu-overlay {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }

  @keyframes anim-botttom-arrow {
    0% {
      transform: translateY(0);
    }

    60% {
      transform: translateY(0);
    }

    100% {
      transform: translateY(3px);
    }
  }
</style>