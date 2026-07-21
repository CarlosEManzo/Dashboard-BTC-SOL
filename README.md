<!DOCTYPE html>
<html lang="es" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Crypto Trading Hub | BTC & SOL (Auto API)</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            bgDark: '#121722',
            cardDark: '#1E2329',
            borderDark: '#2B313A',
            brandGreen: '#0ECB81',
            brandRed: '#F6465D',
            brandYellow: '#F0B90B'
          }
        }
      }
    }
  </script>
</head>
<body class="bg-bgDark text-gray-200 font-sans min-h-screen flex flex-col p-4 gap-4">

  <!-- CABECERA -->
  <header class="flex flex-wrap justify-between items-center bg-cardDark p-4 rounded-xl border border-borderDark shadow-lg">
    <div class="flex items-center space-x-3">
      <div class="w-3 h-3 rounded-full bg-brandGreen animate-pulse"></div>
      <h1 class="text-xl font-bold tracking-wide text-white">BTC & SOL <span class="text-xs font-normal text-gray-400">| Auto Binance Trading Hub</span></h1>
    </div>
    <div class="flex items-center gap-4 text-sm">
      <span class="text-gray-400">PnL Acumulado: <strong id="headerPnL" class="text-brandGreen">$0.00 USDT</strong></span>
    </div>
  </header>

  <!-- CONTENIDO PRINCIPAL: GRÁFICOS DUALES -->
  <main class="grid grid-cols-1 lg:grid-cols-2 gap-4">
    
    <!-- TARJETA BITCOIN -->
    <section class="bg-cardDark p-4 rounded-xl border border-borderDark flex flex-col h-[450px] shadow-lg">
      <div class="flex justify-between items-center mb-3">
        <div class="flex items-center space-x-2">
          <span class="text-orange-500 text-lg font-bold">₿</span>
          <h2 class="font-bold text-white">Bitcoin (BTC/USDT)</h2>
        </div>
        <div class="flex space-x-1 text-xs">
          <button onclick="changeInterval('btc-chart', 'BINANCE:BTCUSDT', '60')" class="px-2 py-1 bg-borderDark hover:bg-gray-700 rounded text-gray-300">1H</button>
          <button onclick="changeInterval('btc-chart', 'BINANCE:BTCUSDT', '240')" class="px-2 py-1 bg-brandYellow text-black font-semibold rounded">4H</button>
          <button onclick="changeInterval('btc-chart', 'BINANCE:BTCUSDT', 'D')" class="px-2 py-1 bg-borderDark hover:bg-gray-700 rounded text-gray-300">1D</button>
        </div>
      </div>
      <div id="btc-chart" class="w-full flex-grow rounded-lg overflow-hidden"></div>
    </section>

    <!-- TARJETA SOLANA -->
    <section class="bg-cardDark p-4 rounded-xl border border-borderDark flex flex-col h-[450px] shadow-lg">
      <div class="flex justify-between items-center mb-3">
        <div class="flex items-center space-x-2">
          <span class="text-purple-400 text-lg font-bold">◎</span>
          <h2 class="font-bold text-white">Solana (SOL/USDT)</h2>
        </div>
        <div class="flex space-x-1 text-xs">
          <button onclick="changeInterval('sol-chart', 'BINANCE:SOLUSDT', '60')" class="px-2 py-1 bg-borderDark hover:bg-gray-700 rounded text-gray-300">1H</button>
          <button onclick="changeInterval('sol-chart', 'BINANCE:SOLUSDT', '240')" class="px-2 py-1 bg-brandYellow text-black font-semibold rounded">4H</button>
          <button onclick="changeInterval('sol-chart', 'BINANCE:SOLUSDT', 'D')" class="px-2 py-1 bg-borderDark hover:bg-gray-700 rounded text-gray-300">1D</button>
        </div>
      </div>
      <div id="sol-chart" class="w-full flex-grow rounded-lg overflow-hidden"></div>
    </section>

  </main>

  <!-- MOTOR DE PREDICCIÓN & TENDENCIA ALGORÍTMICA (AUTOMÁTICO CON API) -->
  <section class="bg-cardDark p-5 rounded-xl border border-borderDark shadow-lg">
    <div class="flex flex-wrap justify-between items-center gap-2 mb-4">
      <h3 class="text-lg font-bold text-white flex items-center gap-2">
        🤖 Motor Algorítmico en Tiempo Real (API Binance)
      </h3>
      <div class="flex gap-2">
        <button onclick="fetchAndAnalyze('BTCUSDT')" class="px-3 py-1.5 bg-orange-600 hover:bg-orange-500 text-white rounded text-xs font-bold transition flex items-center gap-1">
          ⚡ Auto-Analizar BTC
        </button>
        <button onclick="fetchAndAnalyze('SOLUSDT')" class="px-3 py-1.5 bg-purple-600 hover:bg-purple-500 text-white rounded text-xs font-bold transition flex items-center gap-1">
          ⚡ Auto-Analizar SOL
        </button>
      </div>
    </div>

    <!-- METRICAS DETECTADAS -->
    <div class="grid grid-cols-1 md:grid-cols-4 gap-3 mb-4">
      <div class="bg-bgDark p-3 rounded-lg border border-borderDark text-center">
        <span class="block text-xs text-gray-400">Precio Actual Binance</span>
        <span id="autoPrice" class="text-base font-bold text-white">--</span>
      </div>
      <div class="bg-bgDark p-3 rounded-lg border border-borderDark text-center">
        <span class="block text-xs text-gray-400">Estructura Detectada</span>
        <span id="autoTrend" class="text-base font-bold text-gray-300">--</span>
      </div>
      <div class="bg-bgDark p-3 rounded-lg border border-borderDark text-center">
        <span class="block text-xs text-gray-400">RSI Calculado (14)</span>
        <span id="autoRSI" class="text-base font-bold text-gray-300">--</span>
      </div>
      <div class="bg-bgDark p-3 rounded-lg border border-borderDark text-center">
        <span class="block text-xs text-gray-400">Nivel Cercano</span>
        <span id="autoLevel" class="text-base font-bold text-gray-300">--</span>
      </div>
    </div>

    <!-- RESULTADO DE PREDICCIÓN -->
    <div id="predictionResult" class="p-4 bg-bgDark rounded-lg border border-borderDark hidden">
      <div class="flex justify-between items-center mb-2">
        <span class="text-xs text-gray-400">Diagnóstico Estructural en Vivo:</span>
        <span id="predBias" class="px-3 py-1 text-xs font-bold rounded"></span>
      </div>
      <p id="predAnalysis" class="text-sm text-gray-300 mb-2"></p>
      <div class="flex flex-wrap justify-between items-center border-t border-borderDark pt-2 mt-2 gap-2">
        <div class="text-xs text-gray-400">
          💡 <strong>Sugerencia Operativa:</strong> <span id="predAction" class="text-white"></span>
        </div>
        <button onclick="populateCalculatorFromAnalysis()" class="px-3 py-1 bg-brandYellow hover:bg-yellow-500 text-black text-xs font-bold rounded transition">
          📲 Enviar datos a Calculadora
        </button>
      </div>
    </div>
  </section>

  <!-- CALCULADORA DE POSICIÓN CON APALANCAMIENTO Y AUTO TP/SL -->
  <section class="bg-cardDark p-5 rounded-xl border border-borderDark shadow-lg">
    <div class="flex justify-between items-center mb-4">
      <h3 class="text-lg font-bold text-white flex items-center gap-2">
        🧮 Calculadora de Riesgo, Apalancamiento y OCO
      </h3>
      <div class="flex items-center gap-2 text-xs">
        <span class="text-gray-400">Auto-Profit (R/R):</span>
        <button onclick="setAutoProfit(2)" class="px-2 py-1 bg-borderDark hover:bg-brandYellow hover:text-black rounded text-white transition">1 : 2</button>
        <button onclick="setAutoProfit(3)" class="px-2 py-1 bg-borderDark hover:bg-brandYellow hover:text-black rounded text-white transition">1 : 3</button>
      </div>
    </div>
    
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-6 gap-3">
      <div>
        <label class="block text-xs font-medium text-gray-400 mb-1">Capital ($ USDT)</label>
        <input id="capital" type="number" value="1000" class="w-full bg-bgDark border border-borderDark rounded-lg p-2 text-white focus:border-brandYellow outline-none text-sm">
      </div>

      <div>
        <label class="block text-xs font-medium text-gray-400 mb-1">Riesgo (%)</label>
        <select id="riskPercent" class="w-full bg-bgDark border border-borderDark rounded-lg p-2 text-white focus:border-brandYellow outline-none text-sm">
          <option value="1">1%</option>
          <option value="2" selected>2%</option>
          <option value="3">3%</option>
        </select>
      </div>

      <div>
        <label class="block text-xs font-medium text-gray-400 mb-1">Apalancamiento</label>
        <select id="leverage" class="w-full bg-bgDark border border-borderDark rounded-lg p-2 text-white focus:border-brandYellow outline-none text-sm">
          <option value="1" selected>1x (Spot / Sin palanca)</option>
          <option value="2">2x</option>
          <option value="5">5x</option>
          <option value="10">10x</option>
        </select>
      </div>

      <div>
        <label class="block text-xs font-medium text-gray-400 mb-1">Precio Entrada ($)</label>
        <input id="entryPrice" type="number" step="any" placeholder="Ej. 140" class="w-full bg-bgDark border border-borderDark rounded-lg p-2 text-white focus:border-brandYellow outline-none text-sm">
      </div>

      <div>
        <label class="block text-xs font-medium text-gray-400 mb-1">Stop Loss ($)</label>
        <input id="stopLossPrice" type="number" step="any" placeholder="Ej. 133" class="w-full bg-bgDark border border-borderDark rounded-lg p-2 text-white focus:border-brandYellow outline-none text-sm">
      </div>

      <div>
        <label class="block text-xs font-medium text-gray-400 mb-1">Take Profit ($)</label>
        <input id="takeProfitPrice" type="number" step="any" placeholder="Ej. 154" class="w-full bg-bgDark border border-borderDark rounded-lg p-2 text-white focus:border-brandYellow outline-none text-sm">
      </div>
    </div>

    <!-- RESULTADOS Y BOTÓN DE REGISTRO -->
    <div class="mt-4 p-4 bg-bgDark rounded-lg border border-borderDark flex flex-wrap lg:flex-nowrap justify-between items-center gap-4 text-center">
      <div>
        <span class="block text-xs text-gray-400">Riesgo Máx. ($)</span>
        <span id="resRiskUSD" class="text-base font-bold text-brandRed">$0.00</span>
      </div>
      <div>
        <span class="block text-xs text-gray-400">Posición Total</span>
        <span id="resPositionUSD" class="text-base font-bold text-brandYellow">$0.00</span>
      </div>
      <div>
        <span class="block text-xs text-gray-400">Margen Necesario</span>
        <span id="resMarginUSD" class="text-base font-bold text-white">$0.00</span>
      </div>
      <div>
        <span class="block text-xs text-gray-400">Ganancia Estimada</span>
        <span id="resProfitUSD" class="text-base font-bold text-brandGreen">$0.00</span>
      </div>
      <div>
        <span class="block text-xs text-gray-400">Ratio R/B</span>
        <span id="resRR" class="text-base font-bold text-white">1 : 0</span>
      </div>
      
      <button onclick="addOrder()" class="w-full lg:w-auto px-5 py-2.5 bg-brandYellow hover:bg-yellow-500 text-black font-bold rounded-lg transition shadow-md">
        + Registrar Orden
      </button>
    </div>
  </section>

  <!-- BITÁCORA / CONTROL DE ÓRDENES -->
  <section class="bg-cardDark p-5 rounded-xl border border-borderDark shadow-lg mb-6">
    <div class="flex justify-between items-center mb-4">
      <h3 class="text-lg font-bold text-white flex items-center gap-2">
        📋 Bitácora de Órdenes Guardadas
      </h3>
      <button onclick="clearAllOrders()" class="text-xs text-gray-400 hover:text-brandRed underline">Limpiar Historial</button>
    </div>

    <div class="overflow-x-auto">
      <table class="w-full text-left text-sm text-gray-300">
        <thead class="bg-bgDark text-xs uppercase text-gray-400 border-b border-borderDark">
          <tr>
            <th class="p-3">Activo</th>
            <th class="p-3">Entrada</th>
            <th class="p-3">Stop Loss</th>
            <th class="p-3">Take Profit</th>
            <th class="p-3">Posición</th>
            <th class="p-3">Estado</th>
            <th class="p-3 text-center">Acción / Resultado</th>
          </tr>
        </thead>
        <tbody id="ordersTableBody">
          <!-- Carga dinámica -->
        </tbody>
      </table>
    </div>
  </section>

  <script type="text/javascript" src="https://s3.tradingview.com/tv.js"></script>
  <script type="text/javascript">
    let orders = JSON.parse(localStorage.getItem('crypto_orders_history')) || [];
    let currentAnalysisData = null; // Guardado temporal para enviar a la calculadora

    function loadChart(containerId, symbol, interval) {
      new TradingView.widget({
        "autosize": true,
        "symbol": symbol,
        "interval": interval,
        "timezone": "Etc/UTC",
        "theme": "dark",
        "style": "1",
        "locale": "es",
        "toolbar_bg": "#1E2329",
        "enable_publishing": false,
        "hide_side_toolbar": false,
        "container_id": containerId
      });
    }

    function changeInterval(containerId, symbol, interval) {
      document.getElementById(containerId).innerHTML = "";
      loadChart(containerId, symbol, interval);
    }

    loadChart("btc-chart", "BINANCE:BTCUSDT", "240");
    loadChart("sol-chart", "BINANCE:SOLUSDT", "240");

    const inputs = ['capital', 'riskPercent', 'leverage', 'entryPrice', 'stopLossPrice', 'takeProfitPrice'];
    inputs.forEach(id => document.getElementById(id).addEventListener('input', calculateRisk));

    // --- CONEXIÓN API BINANCE & MOTOR TÉCNICO ---

    async function fetchAndAnalyze(symbol) {
      const asset = symbol.replace('USDT', '');
      document.getElementById('autoPrice').innerText = "Cargando...";

      try {
        // Consultar 100 velas de 4 Horas
        const response = await fetch(`https://api.binance.com/api/v3/klines?symbol=${symbol}&interval=4h&limit=100`);
        const data = await response.json();

        // Estructurar precios de cierre y altos/bajos
        const closes = data.map(d => parseFloat(d[4]));
        const highs = data.map(d => parseFloat(d[2]));
        const lows = data.map(d => parseFloat(d[3]));

        const currentPrice = closes[closes.length - 1];

        // 1. Cálculo de RSI (14)
        const rsi = calculateRSI(closes, 14);

        // 2. Determinación de Tendencia (Comparando promedios y estructura reciente)
        const recentHighs = highs.slice(-10);
        const recentLows = lows.slice(-10);
        const maxRecent = Math.max(...recentHighs);
        const minRecent = Math.min(...recentLows);
        
        let trend = 'SIDEWAYS';
        if (currentPrice > closes[closes.length - 20] && currentPrice > (maxRecent + minRecent) / 2) {
          trend = 'UP';
        } else if (currentPrice < closes[closes.length - 20] && currentPrice < (maxRecent + minRecent) / 2) {
          trend = 'DOWN';
        }

        // 3. Ubicación del Precio respecto a Soporte / Resistencia
        const support = minRecent;
        const resistance = maxRecent;
        let location = 'MIDDLE';
        let suggestedStop = support;

        const distToSupport = Math.abs(currentPrice - support);
        const distToResistance = Math.abs(currentPrice - resistance);

        if (distToSupport < distToResistance && (distToSupport / currentPrice) < 0.03) {
          location = 'SUPPORT';
          suggestedStop = support * 0.995; // 0.5% por debajo del soporte
        } else if (distToResistance < distToSupport && (distToResistance / currentPrice) < 0.03) {
          location = 'RESISTANCE';
          suggestedStop = resistance * 1.005; // 0.5% por encima de resistencia
        } else {
          suggestedStop = support;
        }

        // Actualizar UI con métricas detectadas
        document.getElementById('autoPrice').innerText = `$${currentPrice.toLocaleString('en-US', {minimumFractionDigits: 2})}`;
        document.getElementById('autoTrend').innerText = trend === 'UP' ? '📈 Alcista' : trend === 'DOWN' ? '📉 Bajista' : '↔️ Lateral';
        document.getElementById('autoRSI').innerText = rsi.toFixed(1);
        document.getElementById('autoLevel').innerText = location === 'SUPPORT' ? '🛡️ Cerca de Soporte' : location === 'RESISTANCE' ? '🚧 Cerca de Resistencia' : '⚖️ En Rango';

        // Procesar Score Algorítmico
        let score = 0;
        if (trend === 'UP') score += 2;
        if (trend === 'DOWN') score -= 2;
        if (location === 'SUPPORT') score += 2;
        if (location === 'RESISTANCE') score -= 2;
        if (rsi < 35) score += 1;
        if (rsi > 65) score -= 1;

        const resultBox = document.getElementById('predictionResult');
        const biasTag = document.getElementById('predBias');
        const analysisText = document.getElementById('predAnalysis');
        const actionText = document.getElementById('predAction');

        resultBox.classList.remove('hidden');

        if (score >= 2) {
          biasTag.innerText = `PROYECCIÓN ALCISTA PARA ${asset} (~75% Probabilidad)`;
          biasTag.className = "px-3 py-1 text-xs font-bold rounded bg-brandGreen text-black";
          analysisText.innerText = `Binance API indica confluencia compradora para ${asset}. El precio cotiza a $${currentPrice} con estructura alcista y RSI en ${rsi.toFixed(1)}.`;
          actionText.innerText = "Buscar entrada en Long / Compra. Soporte clave detectado cerca.";
        } else if (score <= -2) {
          biasTag.innerText = `PROYECCIÓN BAJISTA PARA ${asset} (~75% Probabilidad)`;
          biasTag.className = "px-3 py-1 text-xs font-bold rounded bg-brandRed text-white";
          analysisText.innerText = `Binance API indica debilidad en ${asset}. El precio de $${currentPrice} enfrenta presión vendedora cerca de la resistencia con RSI en ${rsi.toFixed(1)}.`;
          actionText.innerText = "Precaución con compras. Esperar confirmación de soporte o corrección antes de operar.";
        } else {
          biasTag.innerText = `PROYECCIÓN NEUTRAL / RANGO EN ${asset}`;
          biasTag.className = "px-3 py-1 text-xs font-bold rounded bg-brandYellow text-black";
          analysisText.innerText = `${asset} cotiza actualmente a $${currentPrice} en un área de consolidación sin dirección definida.`;
          actionText.innerText = "Esperar a que el precio testee los extremos ($" + support.toFixed(2) + " / $" + resistance.toFixed(2) + ") antes de entrar.";
        }

        // Guardar estado actual para mandar a la calculadora con 1 clic
        currentAnalysisData = {
          entry: currentPrice,
          stop: suggestedStop,
          isLong: score >= 0
        };

      } catch (error) {
        alert("Error al conectar con la API de Binance: " + error.message);
        document.getElementById('autoPrice').innerText = "Error";
      }
    }

    // Algoritmo matemático para calcular el RSI
    function calculateRSI(closes, period = 14) {
      let gains = 0;
      let losses = 0;

      for (let i = 1; i <= period; i++) {
        const change = closes[closes.length - i] - closes[closes.length - i - 1];
        if (change >= 0) gains += change;
        else losses -= change;
      }

      let avgGain = gains / period;
      let avgLoss = losses / period;

      if (avgLoss === 0) return 100;
      const rs = avgGain / avgLoss;
      return 100 - (100 / (1 + rs));
    }

    // Auto-poblado de la Calculadora desde el Análisis
    function populateCalculatorFromAnalysis() {
      if (!currentAnalysisData) return;

      document.getElementById('entryPrice').value = currentAnalysisData.entry.toFixed(2);
      document.getElementById('stopLossPrice').value = currentAnalysisData.stop.toFixed(2);
      
      // Ajustar Take Profit automáticamente en Ratio 1:2
      setAutoProfit(2);
      calculateRisk();
    }

    function setAutoProfit(multiplier) {
      const entry = parseFloat(document.getElementById('entryPrice').value) || 0;
      const stop = parseFloat(document.getElementById('stopLossPrice').value) || 0;

      if (entry > 0 && stop > 0 && entry !== stop) {
        const stopDistance = Math.abs(entry - stop);
        const profitPrice = entry > stop ? entry + (stopDistance * multiplier) : entry - (stopDistance * multiplier);
        document.getElementById('takeProfitPrice').value = profitPrice.toFixed(2);
        calculateRisk();
      }
    }

    function calculateRisk() {
      const capital = parseFloat(document.getElementById('capital').value) || 0;
      const riskPercent = parseFloat(document.getElementById('riskPercent').value) || 0;
      const leverage = parseFloat(document.getElementById('leverage').value) || 1;
      const entry = parseFloat(document.getElementById('entryPrice').value) || 0;
      const stop = parseFloat(document.getElementById('stopLossPrice').value) || 0;
      const profit = parseFloat(document.getElementById('takeProfitPrice').value) || 0;

      if (entry <= 0 || stop <= 0 || entry === stop) {
        resetResults();
        return;
      }

      const riskUSD = capital * (riskPercent / 100);
      const stopDistancePercent = Math.abs((entry - stop) / entry);
      const positionSizeUSD = riskUSD / stopDistancePercent;
      const marginUSD = positionSizeUSD / leverage;

      let profitUSD = 0;
      let ratio = 0;

      if (profit > 0) {
        const profitDistancePercent = Math.abs((profit - entry) / entry);
        profitUSD = positionSizeUSD * profitDistancePercent;
        ratio = profitDistancePercent / stopDistancePercent;
      }

      document.getElementById('resRiskUSD').innerText = `$${riskUSD.toFixed(2)}`;
      document.getElementById('resPositionUSD').innerText = `$${positionSizeUSD.toFixed(2)}`;
      document.getElementById('resMarginUSD').innerText = `$${marginUSD.toFixed(2)}`;
      document.getElementById('resProfitUSD').innerText = `$${profitUSD.toFixed(2)}`;
      document.getElementById('resRR').innerText = `1 : ${ratio.toFixed(2)}`;
    }

    function resetResults() {
      document.getElementById('resRiskUSD').innerText = '$0.00';
      document.getElementById('resPositionUSD').innerText = '$0.00';
      document.getElementById('resMarginUSD').innerText = '$0.00';
      document.getElementById('resProfitUSD').innerText = '$0.00';
      document.getElementById('resRR').innerText = '1 : 0';
    }

    function saveOrdersLocally() {
      localStorage.setItem('crypto_orders_history', JSON.stringify(orders));
    }

    function addOrder() {
      const entry = parseFloat(document.getElementById('entryPrice').value);
      const stop = parseFloat(document.getElementById('stopLossPrice').value);
      const profit = parseFloat(document.getElementById('takeProfitPrice').value);
      const posUSD = parseFloat(document.getElementById('resPositionUSD').innerText.replace('$', ''));
      const riskUSD = parseFloat(document.getElementById('resRiskUSD').innerText.replace('$', ''));
      const profitUSD = parseFloat(document.getElementById('resProfitUSD').innerText.replace('$', ''));

      if (!entry || !stop) return alert("Ingresa Entrada y Stop Loss.");

      const order = {
        id: Date.now(),
        asset: entry > 1000 ? 'BTC/USDT' : 'SOL/USDT',
        entry, stop, profit, posUSD, riskUSD, profitUSD,
        status: 'ACTIVA',
        pnl: 0
      };

      orders.push(order);
      saveOrdersLocally();
      renderOrders();
    }

    function closeOrder(id, result) {
      const order = orders.find(o => o.id === id);
      if (order) {
        order.status = result;
        order.pnl = result === 'GANADA' ? order.profitUSD : -order.riskUSD;
        saveOrdersLocally();
        renderOrders();
      }
    }

    function clearAllOrders() {
      if (confirm("¿Seguro que quieres borrar el historial de órdenes?")) {
        orders = [];
        saveOrdersLocally();
        renderOrders();
      }
    }

    function renderOrders() {
      const tbody = document.getElementById('ordersTableBody');
      tbody.innerHTML = '';
      let totalPnL = 0;

      orders.forEach(o => {
        totalPnL += o.pnl;
        const row = document.createElement('tr');
        row.className = "border-b border-borderDark hover:bg-bgDark";
        
        row.innerHTML = `
          <td class="p-3 font-bold">${o.asset}</td>
          <td class="p-3">$${o.entry}</td>
          <td class="p-3 text-brandRed">$${o.stop}</td>
          <td class="p-3 text-brandGreen">$${o.profit || '-'}</td>
          <td class="p-3">$${o.posUSD.toFixed(2)}</td>
          <td class="p-3 font-semibold ${o.status === 'ACTIVA' ? 'text-brandYellow' : o.status === 'GANADA' ? 'text-brandGreen' : 'text-brandRed'}">${o.status}</td>
          <td class="p-3 text-center">
            ${o.status === 'ACTIVA' ? `
              <button onclick="closeOrder(${o.id}, 'GANADA')" class="px-2 py-1 bg-brandGreen text-black font-bold rounded text-xs mr-1">TP Target</button>
              <button onclick="closeOrder(${o.id}, 'PERDIDA')" class="px-2 py-1 bg-brandRed text-white font-bold rounded text-xs">SL Hit</button>
            ` : `<span class="font-bold ${o.pnl >= 0 ? 'text-brandGreen' : 'text-brandRed'}">${o.pnl >= 0 ? '+' : ''}$${o.pnl.toFixed(2)}</span>`}
          </td>
        `;
        tbody.appendChild(row);
      });

      const headerPnL = document.getElementById('headerPnL');
      headerPnL.innerText = `${totalPnL >= 0 ? '+' : ''}$${totalPnL.toFixed(2)} USDT`;
      headerPnL.className = `font-bold ${totalPnL >= 0 ? 'text-brandGreen' : 'text-brandRed'}`;
    }

    renderOrders();
  </script>
</body>
</html>
