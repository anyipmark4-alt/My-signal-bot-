here<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Trading Bot Dashboard</title>
<link rel="manifest" href="manifest.json">
<script src="https://cdn.jsdelivr.net/npm/lightweight-charts@3.7.0/dist/lightweight-charts.standalone.production.js"></script>
<script src="https://cdn.jsdelivr.net/npm/technicalindicators@3.0.0/dist/browser.js"></script>
<style>
body { font-family: Arial, sans-serif; margin: 20px; }
#signalDisplay { margin-top: 20px; font-size: 18px; }
#chart { width: 100%; height: 400px; margin-top: 30px; }
select, button { padding: 10px; font-size: 16px; margin-right: 10px; }
</style>
</head>
<body>
<h1>Real-Time Crypto & Forex Trading Bot</h1>

<select id="pairSelect">
  <option>BTCUSDT</option>
  <option>ETHUSDT</option>
  <option>EURUSD</option>
</select>
<button onclick="getSignal()">Get Signal</button>

<div id="signalDisplay"></div>
<div id="chart"></div>

<script>
// Request permission for notifications
if ('Notification' in window) {
  Notification.requestPermission();
}

// Fetch market data for crypto or forex
async function fetchMarketData(symbol, interval='1h', limit=100) {
  try {
    if(symbol.endsWith('USDT')) {
      const res = await fetch(`https://api.binance.com/api/v3/klines?symbol=${symbol}&interval=${interval}&limit=${limit}`);
      const data = await res.json();
      return data.map(k => ({
        time: k[0]/1000,
        open: parseFloat(k[1]),
        high: parseFloat(k[2]),
        low: parseFloat(k[3]),
        close: parseFloat(k[4])
      }));
    } else {
      // Placeholder Forex data - integrate live API for real signals
      let prices = [];
      let now = Date.now()/1000;
      for(let i=limit;i>0;i--){
        let price = 1.1 + Math.random()*0.01;
        prices.push({time: now - i*3600, open: price, high: price*1.001, low: price*0.999, close: price});
      }
      return prices;
    }
  } catch(e) {
    console.error(e);
    return [];
  }
}

// EMA + RSI signal generator
function generateSignal(candles){
  const closes = candles.map(c=>c.close);
  const ema20 = technicalindicators.EMA.calculate({ period: 20, values: closes });
  const rsi = technicalindicators.RSI.calculate({ period: 14, values: closes });
  const lastEma = ema20[ema20.length-1];
  const lastClose = closes[closes.length-1];
  const lastRsi = rsi[rsi.length-1];

  if(lastClose > lastEma && lastRsi < 70) return 'BUY';
  if(lastClose < lastEma && lastRsi > 30) return 'SELL';
  return 'HOLD';
}

// ATR-based SL/TP
function calculateSLTP(lastPrice, signal){
  const atr = lastPrice*0.005;
  if(signal==='BUY') return {SL: (lastPrice-atr).toFixed(5), TP: (lastPrice+atr*2).toFixed(5)};
  if(signal==='SELL') return {SL: (lastPrice+atr).toFixed(5), TP: (lastPrice-atr*2).toFixed(5)};
  return {SL: null, TP: null};
}

// Multi-timeframe confirmation
async function multiTimeframeSignal(symbol){
  const h1 = await fetchMarketData(symbol,'1h',100);
  const h4 = await fetchMarketData(symbol,'4h',100);
  const d1 = await fetchMarketData(symbol,'1d',100);

  const sigH1 = generateSignal(h1);
  const sigH4 = generateSignal(h4);
  const sigD1 = generateSignal(d1);

  // Simple majority rule
  const signals = [sigH1, sigH4, sigD1];
  const buyCount = signals.filter(s=>s==='BUY').length;
  const sellCount = signals.filter(s=>s==='SELL').length;

  if(buyCount>sellCount) return 'BUY';
  if(sellCount>buyCount) return 'SELL';
  return 'HOLD';
}

// Draw chart
function drawChart(candles){
  const chartDiv = document.getElementById('chart');
  chartDiv.innerHTML='';
  const chart = LightweightCharts.createChart(chartDiv,{width: chartDiv.clientWidth,height:400});
  const lineSeries = chart.addLineSeries();
  const formatted = chere<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Trading Bot Dashboard</title>
<link rel="manifest" href="manifest.json">
<script src="https://cdn.jsdelivr.net/npm/lightweight-charts@3.7.0/dist/lightweight-charts.standalone.production.js"></script>
<script src="https://cdn.jsdelivr.net/npm/technicalindicators@3.0.0/dist/browser.js"></script>
<style>
body { font-family: Arial, sans-serif; margin: 20px; }
#signalDisplay { margin-top: 20px; font-size: 18px; }
#chart { width: 100%; height: 400px; margin-top: 30px; }
select, button { padding: 10px; font-size: 16px; margin-right: 10px; }
</style>
</head>
<body>
<h1>Real-Time Crypto & Forex Trading Bot</h1>

<select id="pairSelect">
  <option>BTCUSDT</option>
  <option>ETHUSDT</option>
  <option>EURUSD</option>
</select>
<button onclick="getSignal()">Get Signal</button>

<div id="signalDisplay"></div>
<div id="chart"></div>

<script>
// Request permission for notifications
if ('Notification' in window) {
  Notification.requestPermission();
}

// Fetch market data for crypto or forex
async function fetchMarketData(symbol, interval='1h', limit=100) {
  try {
    if(symbol.endsWith('USDT')) {
      const res = await fetch(`https://api.binance.com/api/v3/klines?symbol=${symbol}&interval=${interval}&limit=${limit}`);
      const data = await res.json();
      return data.map(k => ({
        time: k[0]/1000,
        open: parseFloat(k[1]),
        high: parseFloat(k[2]),
        low: parseFloat(k[3]),
        close: parseFloat(k[4])
      }));
    } else {
      // Placeholder Forex data - integrate live API for real signals
      let prices = [];
      let now = Date.now()/1000;
      for(let i=limit;i>0;i--){
        let price = 1.1 + Math.random()*0.01;
        prices.push({time: now - i*3600, open: price, high: price*1.001, low: price*0.999, close: price});
      }
      return prices;
    }
  } catch(e) {
    console.error(e);
    return [];
  }
}

// EMA + RSI signal generator
function generateSignal(candles){
  const closes = candles.map(c=>c.close);
  const ema20 = technicalindicators.EMA.calculate({ period: 20, values: closes });
  const rsi = technicalindicators.RSI.calculate({ period: 14, values: closes });
  const lastEma = ema20[ema20.length-1];
  const lastClose = closes[closes.length-1];
  const lastRsi = rsi[rsi.length-1];

  if(lastClose > lastEma && lastRsi < 70) return 'BUY';
  if(lastClose < lastEma && lastRsi > 30) return 'SELL';
  return 'HOLD';
}

// ATR-based SL/TP
function calculateSLTP(lastPrice, signal){
  const atr = lastPrice*0.005;
  if(signal==='BUY') return {SL: (lastPrice-atr).toFixed(5), TP: (lastPrice+atr*2).toFixed(5)};
  if(signal==='SELL') return {SL: (lastPrice+atr).toFixed(5), TP: (lastPrice-atr*2).toFixed(5)};
  return {SL: null, TP: null};
}

// Multi-timeframe confirmation
async function multiTimeframeSignal(symbol){
  const h1 = await fetchMarketData(symbol,'1h',100);
  const h4 = await fetchMarketData(symbol,'4h',100);
  const d1 = await fetchMarketData(symbol,'1d',100);

  const sigH1 = generateSignal(h1);
  const sigH4 = generateSignal(h4);
  const sigD1 = generateSignal(d1);

  // Simple majority rule
  const signals = [sigH1, sigH4, sigD1];
  const buyCount = signals.filter(s=>s==='BUY').length;
  const sellCount = signals.filter(s=>s==='SELL').length;

  if(buyCount>sellCount) return 'BUY';
  if(sellCount>buyCount) return 'SELL';
  return 'HOLD';
}

// Draw chart
function drawChart(candles){
  const chartDiv = document.getElementById('chart');
  chartDiv.innerHTML='';
  const chart = LightweightCharts.createChart(chartDiv,{width: chartDiv.clientWidth,height:400});
  const lineSeries = chart.addLineSeries();
  const formatted = candles.map(c=>({time:c.time,value:c.close}));
  lineSeries.setData(formatted);
}

// Display signal
async function getSignal(){
  const symbol = document.getElementById('pairSelect').value;
  const candles = await fetchMarketData(symbol);
  const lastPrice = candles[candles.length-1].close;
  const signal = await multiTimeframeSignal(symbol);
  const {SL, TP} = calculateSLTP(lastPrice, signal);

  document.getElementById('signalDisplay').innerHTML=`
    <p><strong>Pair:</strong> ${symbol}</p>
    <p><strong>Signal:</strong> ${signal}</p>
    <p><strong>Last Price:</strong> ${lastPrice.toFixed(5)}</p>
    <p><strong>Stop Loss:</strong> ${SL}</p>
    <p><strong>Take Profit:</strong> ${TP}</p>
  `;
  drawChart(candles);

  // Push notification
  if(Notification.permission==='granted'){
    new Notification(`New Signal: ${symbol}`,{body:`Signal: ${signal} | Last Price: ${lastPrice.toFixed(5)}`});
  }
}

// Auto-update every 1 min
setInterval(getSignal,60000);
</script>
</body>
</htm
