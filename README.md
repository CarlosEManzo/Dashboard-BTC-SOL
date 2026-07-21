<!DOCTYPE html>
<html lang="es" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Crypto Trading Hub | BTC & SOL</title>
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
      <h1 class="text-xl font-bold tracking-wide text-white">BTC & SOL <span class="text-xs font-normal text-gray-400">| Trading Pro Hub</span></h1>
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
        <input id="entryPrice" type="number" placeholder="Ej. 140" class="w-full bg-bgDark border border-borderDark rounded-lg p-2 text-white focus:border-brandYellow outline-none text-sm">
      </div>

      <div>
        <label class="block text-xs font-medium text-gray-400 mb-1">Stop Loss ($)</label>
        <input id="stopLossPrice" type="number" placeholder="Ej. 133" class="w-full bg-bgDark border border-borderDark rounded-lg p-2 text-white focus:border-brandYellow outline-none text-sm">
      </div>

      <div>
        <label class="block text-xs font-medium text-gray-400 mb-1">Take Profit ($)</label>
        <input id="takeProfitPrice" type="number" placeholder="Ej. 154" class="w-full bg-bgDark border border-borderDark rounded-lg p-2 text-white focus:border-brandYellow outline-none text-sm">
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
          <!-- Órdenes cargadas dinámicamente -->
        </tbody>
      </table>
    </div>
  </section>

  <script type="text/javascript" src="https://s3.tradingview.com/tv.js"></script>
  <script type="text/javascript">
    // Cargar órdenes guardadas desde localStorage
    let orders = JSON.parse(localStorage.getItem('crypto_orders_history')) || [];

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
      if (confirm("¿Seguro que quieres borrar todo el historial?")) {
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

    // Renderizar al cargar la página por primera vez
    renderOrders();
  </script>
</body>
</html>
