<script>
  const formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',

    // These options are needed to round to whole numbers if that's what you want.
    minimumFractionDigits: 0, // (this suffices for whole numbers, but will print 2500.10 as $2,500.1)
    maximumFractionDigits: 0, // (causes 2500.99 to be printed as $2,501)
  });

  let mode = 'simple';
  let amount = 150;
  let frequency = 'm';
  let years = 3;
  let ticker = '^GSPC';

  let dcaData;

  let FREQUENCIES = [
    { id: "d", text: "Daily" },
    { id: "w", text: "Weekly" },
    { id: "b", text: "Bi-weekly" },
    { id: "m", text: "Monthly" },
	];

  let YEARS = [
    { id: 1, text: "one" },
    { id: 2, text: "two" },
    { id: 3, text: "three" },
    { id: 4, text: "four" },
    { id: 5, text: "five" },
  ]

  const handleCalculate = async () => {
    dcaData = await fetch(`http://127.0.0.1:8000/dca/?mode=${mode}&amount=${amount}&frequency=${frequency}&years=${years}&ticker=${ticker}`).then(x => x.json());
    console.log('data:', dcaData);
  }
</script>

<svelte:head>
    <title>Bitcoin VS Stocks</title> 
</svelte:head>

<main>
  <h1>BTC VS STOCKS</h1>

  <p><span>Save amount </span><input bind:value={amount} type="number" placeholder="Dollar" /></p>

  <p>
    <span>every </span>
    <select bind:value={frequency}>
      {#each FREQUENCIES as f}
        <option value={f.id}>{f.text}</option>
      {/each}
    </select>
  </p>

  <p>
    <span>For </span>
    <select bind:value={years}>
      {#each YEARS as y}
        <option value={y.id}>{y.text}</option>
      {/each}
    </select>
    <span>years </span>
  </p>

  <p><span>Ticker </span><input bind:value={ticker} placeholder="Stock ticker" /></p>
  
  <button on:click={handleCalculate}>CALCULATE</button>

  {#if dcaData }
  <table>
    <tr>
      <th />
      <th>BTC</th>
      <th>{ dcaData.Stocks.ticker }</th>
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
      <td style:color={dcaData.Bitcoin.profit > 0 ? 'green' : 'red'}>{ dcaData.Bitcoin.profit }%</td>
      <td style:color={dcaData.Stocks.profit > 0 ? 'green' : 'red'}>{ dcaData.Stocks.profit }%</td>
    </tr>

    <tr>
      <td>BTC Amount</td>
      <td>{ dcaData.Bitcoin.btc_amount }</td>
      <td> - </td>
    </tr>

  </table>
  {/if}

</main>

<style>
  main {
    padding: 20px;
    background-color: black;
    color: white;
  }

  th, td {
    padding: 4px 12px;
  }
</style>