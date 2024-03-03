<section id='faq'>
  <h3>What is Dollar Cost Average (DCA)?</h3>

  <p>Dollar cost averaging is an investment strategy that consists of buying the same dollar amount of an asset on a regular basis, regardless of price.</p>
  <p><strong>For example:</strong> Buying $250 worth of bitcoin every month, without any exception, no matter the price.</p>
  <p>DCA is considered one of the most reliable and effective strategies when it comes to investing.</p>

  <h3>Isn't Bitcoin too volatile?</h3>

  <p>That's the point. When you dollar cost average, you basically neutralize bitcoin's volatility.</p>
  <p>If you buy bitcoin at $60k and then the same amount again at $20k, your average cost is $40k. If you continue buying all the time, your cost per purchase will even out over time.</p>
  <p>Despite its volatility, you will notice that Bitcoin outperforms most stocks, indexes and precious metals.</p>
  <p>Feel free to play around with the calculator above. The results might surprise you.</p>

  <h3>How can I verify the results of this calculator?</h3>
  <p>The code is open source, you can verify how it works by checking our <a href="https://github.com/GianlucaTassara/bitcoinvsstocks">Python API Backend</a> and <a href="https://github.com/handsomelatino/bitcoinvsstocks-svelte">Svelte Frontend</a> source code.</p>
  <p>The historical prices for both bitcoin and stocks are extracted from Yahoo Finance's API and then the results are calculated according to the options you selected.</p>

  <h3 id="donate">Find this site useful?</h3>

  <p>Bitcoin VS Stocks was created by <a href="https://twitter.com/GianlucaTassara">Gianluca Tassara</a> and <a href='https://handsomelatino.com'>Handsome Latino</a>.
    
  <p>We are bitcoiners creating bitcoin-only projects and helping bring awareness to the fiat-minded world. Your donations will be spent improving and making more projects.</p>

  <p class="center"><a class="donate" href="https://pay.handsomelatino.com/api/v1/invoices?storeId=3zecRWVXGTMtpsE4157U8hjzL9iwBfPFBroABh3oWD3r&currency=USD">Donate (On Chain)</a></p>
  <p class="center">Donate using Lightning ⚡️ below 👇</p>
  <lightning-widget name="Gianluca Tassara" accent="#ff7300" to="gianluca@pay.bitcoinjungle.app" image="https://bitcoinfairprice.com/gianluca.jpg" />
  
  <p>Check out <a href='https://bitcoinfairprice.com'>Bitcoin Fair Price</a> to predict what Bitcoin's price might be in the future and feel free to reach out to me on <a href="https://twitter.com/GianlucaTassara">Twitter</a>.</p>
</section>

<style>
  section {
    max-width: 600px;
    padding: 16px;
    font-size: 24px;
    line-height: 32px;
    margin: 0 auto;

    @media screen and (max-width: 480px) {
      font-size: 18px;
      line-height: 24px;
    }
  }

  h3 {
    color: var(--text-color);
  }
  
  h3:not(:first-of-type) {
    margin-top: 44px;
  }

  p {
    color: var(--dark-bg);
  }

  p.center {
    text-align: center;
  }

  a {
    color: var(--dark-bg);
  }

  a:hover {
    color: var(--emphasis);
  }

  a.donate {
    display: inline-block;
    color: var(--emphasis);
    border: 2px solid var(--emphasis);
    border-radius: 32px;
    text-decoration: none;
    padding: 8px 16px;
    font-weight: bold;
  }
</style>