- 👋 Hi, I’m @Mali44111
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Mali44111/Mali44111 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import backtrader as bt

class MyStrategy(bt.SignalStrategy):
    def __init__(self):
        sma_signal = bt.indicators.SimpleMovingAverage(self.data.close, period=9) > bt.indicators.SimpleMovingAverage(self.data.close, period=21)
        rsi_signal = bt.indicators.RelativeStrengthIndex(self.data.close, period=14) < 30
        macd_signal = bt.indicators.MACDHisto(self.data.close).histo > 0
        bollinger_signal = self.data.close < bt.indicators.BollingerBands(self.data.close).lines.bot

        self.signal_add(bt.SIGNAL_LONG, sma_signal & rsi_signal & macd_signal & bollinger_signal)

# Crear cerebro y agregar datos
cerebro = bt.Cerebro()
data = bt.feeds.PandasData(dataname=data)
cerebro.adddata(data)

# Agregar estrategia
cerebro.addstrategy(MyStrategy)

# Ejecutar backtesting
cerebro.run()

# Mostrar resultados
cerebro.plot() 

