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

  let fetching = false;

  let FREQUENCIES = [
    { id: "d", text: "day" },
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
    fetching = true;
    dcaData = await fetch(`${PUBLIC_API_URL}/?mode=${mode}&amount=${amount}&frequency=${frequency}&years=${years}&ticker=${ticker}`).then(x => x.json());
    console.log('data:', dcaData);
    fetching = false;
  }

</script>

<section id="calculate">
  <h2><span class='btc'>Bitcoin</span> <small>vs</small> <span class='stocks'>Stocks</span></h2>

  <div class="inputs">
    <div class="grid">
      <span>Saving / Investing </span>
      <span class='dollar-container'>
        <span class='dollar-sign'>$</span>
        <input bind:value={amount} type="number" placeholder="Dollar" />
      </span>

      <span>in Bitcoin every</span>

      <select bind:value={frequency}>
        {#each FREQUENCIES as f}
          <option value={f.id}>{f.text}</option>
        {/each}
      </select>
      
      <span>for the past</span>
      <select bind:value={years}>
        {#each YEARS as y}
        <option value={y.id}>{y.text}</option>
        {/each}
      </select>
    </div>
      
      <p><span>compared to </span>
        <select class="tickers" bind:value={ticker}>
          {#each COMMON_STOCKS as s}
            <option value={s.id}>{s.label}</option>
          {/each}
          {#each POPULAR_STOCKS as s}
            <option value={s.id}>{s.label}</option>
          {/each}
        </select>
        <!-- <input bind:value={ticker} placeholder="Stock ticker" /></p> -->
      </div>
      
      <button class='calculate' disabled={fetching} on:click={handleCalculate}>Calculate</button>
      
  {#if dcaData}
    <p>Saving <strong>{formatter.format(amount)} { frequency === 'b' ? 'every' : 'per' } {FREQUENCIES.find(f => f.id === frequency).text}</strong> for the past {years} years until today results in:</p>

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
  {/if}

  </section>

<style>  
  section {
    font-size: 24px;
    padding: 20px;
    scroll-behavior: smooth;
    max-width: 600px;
    margin: 0 auto;
    min-height: 100vh;
    text-align: center;

    @media screen and (max-width: 480px) {
      font-size: 18px;
    }
  }

  h2 {
    font-family: var(--alfa-slab);
    font-weight: normal;
    text-transform: uppercase;
    text-align: center;
    font-size: 20px;
    margin: 12px auto 32px;
    letter-spacing: 1px;
  }

  h2 .btc {
    color: var(--emphasis);
    text-shadow: 0 2px 0 var(--emphasis-2);
  }

  h2 .stocks {
    text-shadow: 0 2px 0 var(--logo-shadow);
  }

  small {
    font-size: 12px;
    vertical-align: middle;
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

  select.tickers {
    margin-top: 8px;
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

  button:hover {
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
</style>