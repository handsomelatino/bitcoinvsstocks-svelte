<script>
  import { PUBLIC_API_URL } from '$env/static/public';

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
    { id:  "^DJI", label: "Dow Jones" },
    { id: "^IXIC", label: "NASDAQ" },
    { id: "GC=F",  label: "Gold" },
    { id: "SI=F",  label: "Silver" },
  ]

  const handleCalculate = async () => {
    fetching = true;
    dcaData = await fetch(`${PUBLIC_API_URL}/?mode=${mode}&amount=${amount}&frequency=${frequency}&years=${years}&ticker=${ticker}`).then(x => x.json());
    console.log('data:', dcaData);
    fetching = false;
  }

</script>

<section id="calculate">

  <div class="inputs">
    <p>
      <span>Saving </span>
        <span class='dollar-container'>
          <span class='dollar-sign'>$</span>
          <input bind:value={amount} type="number" placeholder="Dollar" />
        </span>
      <span>every </span>
      <select bind:value={frequency}>
        {#each FREQUENCIES as f}
          <option value={f.id}>{f.text}</option>
        {/each}
      </select>
    </p>

    <!-- <p>
    </p> -->

    <p>
      <span>For the past </span>
      <select bind:value={years}>
        {#each YEARS as y}
          <option value={y.id}>{y.text}</option>
        {/each}
      </select>
      <span>in <strong>Bitcoin</strong></span>
    </p>

    <p><span>compared to </span>
      <select bind:value={ticker}>
        {#each COMMON_STOCKS as s}
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
        <th>{ COMMON_STOCKS.find(s => s.id === dcaData.Stocks.ticker).label }</th>
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
        <td>BTC Amount</td>
        <td class="centered">{ dcaData.Bitcoin.btc_amount }</td>
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

  input {
    width: 140px;
  }

  input, select {
    border: none;
    font-family: var(--maven-pro);
    padding: 8px 12px;
    border-radius: 8px;
    background-color: var(--light-bg);
    color: var(--text-color);
    font-size: 24px;
    /* width: 140px; */
    margin: 0 12px;
    /* font-weight: bold; */
    text-align: right;
  }

  button.calculate {
    all: unset;
    cursor: pointer;
    text-decoration: none;
    color: var(--main-bg);
    background-color: var(--emphasis);
    padding: 12px 72px;
    font-size: 24px;
    border-radius: 8px;
    text-transform: uppercase;
    font-weight: bold;
    transition: filter 0.3s;
    margin: 20px 0;
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
  }

  th {
    font-size: 28px;
  }

  th, td {
    padding: 16px 28px;
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