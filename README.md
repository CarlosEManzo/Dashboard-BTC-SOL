// --- CONEXIÓN API BINANCE & MOTOR TÉCNICO (OPTIMIZADO PARA SCALPING 15M) ---

async function fetchAndAnalyze(symbol) {
  const asset = symbol.replace('USDT', '');
  document.getElementById('autoPrice').innerText = "Cargando...";

  try {
    // 1. Pedir 100 velas de 15 MINUTOS (Temporalidad de Scalping)
    const response = await fetch(`https://api.binance.com/api/v3/klines?symbol=${symbol}&interval=15m&limit=100`);
    const data = await response.json();

    const closes = data.map(d => parseFloat(d[4]));
    const highs = data.map(d => parseFloat(d[2]));
    const lows = data.map(d => parseFloat(d[3]));

    const currentPrice = closes[closes.length - 1];

    // 2. RSI Rápido (periodo 9 para 15m)
    const rsi = calculateRSI(closes, 9);

    // 3. Cálculo de ATR (Average True Range de 14 periodos) para Stop Loss Dinámico
    const atr = calculateATR(highs, lows, closes, 14);

    // 4. Detección de Impulso Rápido (Últimas 3 velas de 15m)
    const last3Closes = closes.slice(-3);
    const momentum = last3Closes[2] - last3Closes[0]; // Positivo = Impulso comprador, Negativo = Vendedor

    // 5. Estructura de Tendencia Corta (EMA 20 vs EMA 50)
    const ema20 = calculateEMA(closes, 20);
    const ema50 = calculateEMA(closes, 50);

    let trend = 'SIDEWAYS';
    if (currentPrice > ema20 && ema20 > ema50) {
      trend = 'UP';
    } else if (currentPrice < ema20 && ema20 < ema50) {
      trend = 'DOWN';
    }

    // 6. Configuración de Stop Loss e Invalidez con ATR
    // En Long: Entrada - (1.5 * ATR) | En Short: Entrada + (1.5 * ATR)
    let suggestedStop = 0;
    let isLong = false;

    if (trend === 'UP' && momentum > 0) {
      isLong = true;
      suggestedStop = currentPrice - (atr * 1.5);
    } else if (trend === 'DOWN' && momentum < 0) {
      isLong = false;
      suggestedStop = currentPrice + (atr * 1.5);
    } else {
      // En rango, usar ATR apretado (1.2x)
      isLong = rsi < 50;
      suggestedStop = isLong ? currentPrice - (atr * 1.2) : currentPrice + (atr * 1.2);
    }

    // Actualizar UI con métricas de Scalping
    document.getElementById('autoPrice').innerText = `$${currentPrice.toLocaleString('en-US', {minimumFractionDigits: 2})}`;
    document.getElementById('autoTrend').innerText = trend === 'UP' ? '📈 Alcista 15m' : trend === 'DOWN' ? '📉 Bajista 15m' : '↔️ Consolidación';
    document.getElementById('autoRSI').innerText = `${rsi.toFixed(1)} (Fast 9)`;
    document.getElementById('autoLevel').innerText = `Rango ATR: ±$${atr.toFixed(2)}`;

    // Score Algorítmico de Scalping
    let score = 0;
    if (trend === 'UP') score += 2;
    if (trend === 'DOWN') score -= 2;
    if (momentum > 0) score += 1;
    if (momentum < 0) score -= 1;
    if (rsi < 30) score += 2; // Sobreventa rápida
    if (rsi > 70) score -= 2; // Sobrecompra rápida

    const resultBox = document.getElementById('predictionResult');
    const biasTag = document.getElementById('predBias');
    const analysisText = document.getElementById('predAnalysis');
    const actionText = document.getElementById('predAction');

    resultBox.classList.remove('hidden');

    if (score >= 2) {
      biasTag.innerText = `SCALPING LONG / COMPRA EN ${asset}`;
      biasTag.className = "px-3 py-1 text-xs font-bold rounded bg-brandGreen text-black";
      analysisText.innerText = `Estructura de 15m con momentum alcista. ATR actual: $${atr.toFixed(2)}. Ruido controlado con Stop Loss dinámico.`;
      actionText.innerText = `Entrada sugerida: $${currentPrice.toFixed(2)} | SL Dinámico (ATR): $${suggestedStop.toFixed(2)}`;
    } else if (score <= -2) {
      biasTag.innerText = `SCALPING SHORT / VENTA EN ${asset}`;
      biasTag.className = "px-3 py-1 text-xs font-bold rounded bg-brandRed text-white";
      analysisText.innerText = `Estructura de 15m con fuerte presión vendedora. Evitar compras inmediatas.`;
      actionText.innerText = `Si buscas entrada: SL sugerido en $${suggestedStop.toFixed(2)}`;
    } else {
      biasTag.innerText = `SIN VENTAJACLAR EN 15M (${asset})`;
      biasTag.className = "px-3 py-1 text-xs font-bold rounded bg-brandYellow text-black";
      analysisText.innerText = `Mercado sucio o en compresión de mechas en 15m. El riesgo de barrido es alto.`;
      actionText.innerText = "No operar en este minuto o esperar a que el RSI rompa extremos (<30 o >70).";
    }

    // Guardar para la calculadora
    currentAnalysisData = {
      entry: currentPrice,
      stop: suggestedStop,
      isLong: isLong
    };

  } catch (error) {
    alert("Error al conectar con la API de Binance: " + error.message);
  }
}

// --- FUNCIONES MATEMÁTICAS DE AUXILIO TÉCNICO ---

// Cálculo de ATR (Average True Range) para volatilidad
function calculateATR(highs, lows, closes, period = 14) {
  let trs = [];
  for (let i = 1; i < closes.length; i++) {
    const tr = Math.max(
      highs[i] - lows[i],
      Math.abs(highs[i] - closes[i - 1]),
      Math.abs(lows[i] - closes[i - 1])
    );
    trs.push(tr);
  }
  const recentTRs = trs.slice(-period);
  return recentTRs.reduce((a, b) => a + b, 0) / period;
}

// Cálculo de EMA (Exponential Moving Average)
function calculateEMA(closes, period) {
  const k = 2 / (period + 1);
  let ema = closes[0];
  for (let i = 1; i < closes.length; i++) {
    ema = (closes[i] * k) + (ema * (1 - k));
  }
  return ema;
}
