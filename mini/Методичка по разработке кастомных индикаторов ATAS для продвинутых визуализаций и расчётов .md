
---

# Методичка по разработке кастомных индикаторов ATAS для продвинутых визуализаций и расчётов

Эта методичка предназначена для создания высококачественных пользовательских индикаторов в ATAS, с акцентом на сложные визуализации (рисование объектов) и расчёты (тики, трейды, футпринт, Market Depth, MBO, статистика). Она ориентирована на пользователей с торговым опытом, но без навыков программирования, чтобы ИИ мог реализовать ваши идеи через технические задания.

## Основы разработки индикатора

### Создание индикатора
1. **Создайте проект** в Visual Studio (тип: Class Library).
2. **Добавьте ссылку** на `ATAS.Indicators.dll` из папки установки ATAS (`bin` или `версия`).
3. **Переопределите класс `Indicator`**:
   ```csharp
   using ATAS.Indicators;
   public class CustomIndicator : Indicator
   {
       protected override void OnCalculate(int bar, decimal value) { }
   }
   ```
   **Пояснение**: Класс `Indicator` — основа для всех индикаторов. Метод `OnCalculate` вызывается для каждого бара и содержит логику расчёта.

4. **Добавьте `DataSeries`** для хранения данных:
   ```csharp
   public class CustomIndicator : Indicator
   {
       private ValueDataSeries _values;
       public CustomIndicator() : base(true)
       {
           _values = new ValueDataSeries("Values") { VisualType = VisualMode.Histogram };
           DataSeries.Add(_values);
           Panel = IndicatorDataProvider.NewPanel;
           DenyToChangePanel = true;
       }
   }
   ```
   **Пояснение**: `ValueDataSeries` хранит числовые значения (например, для гистограммы). `base(true)` скрывает выбор источника данных, если он не нужен.

5. **Опишите логику** в `OnCalculate`:
   ```csharp
   protected override void OnCalculate(int bar, decimal value)
   {
       var candle = GetCandle(bar);
       _values[bar] = candle.Delta; // Пример: запись дельты свечи
   }
   ```
   **Пояснение**: Здесь вы задаёте, как индикатор обрабатывает данные (например, дельту свечи).

6. **Скомпилируйте** проект в DLL.
7. **Поместите DLL** в `Documents/ATAS/Indicators`.
8. **Обновите индикаторы** в ATAS через мигающую кнопку.

### Настройка параметров
Для гибкости добавляйте публичные свойства с пересчётом:
```csharp
private int _period = 10;
[Display(Name = "Period", GroupName = "Settings", Order = 1)]
public int Period
{
    get => _period;
    set { _period = Math.Max(1, value); RecalculateValues(); }
}
```
**Пояснение**: Свойство `Period` позволяет пользователю задавать период через интерфейс ATAS. `RecalculateValues()` обновляет индикатор при изменении.

### Визуализация по умолчанию
Задайте режим отображения:
```csharp
public CustomIndicator() : base(true)
{
    ((ValueDataSeries)DataSeries[0]).VisualType = VisualMode.Histogram;
    ((ValueDataSeries)DataSeries[0]).Color = Colors.Blue;
}
```
**Пояснение**: `VisualMode.Histogram` отображает данные как гистограмму, `Color` задаёт цвет.

## Рисование и визуализация объектов

Для создания сложных визуализаций используйте метод `OnRender` и пространство имён `OFT.Rendering`. Это позволяет рисовать линии, метки, гистограммы, кластеры и кастомные объекты.

### Основы отрисовки
Включите пользовательскую отрисовку:
```csharp
public CustomIndicator() : base(true)
{
    EnableCustomDrawing = true;
    SubscribeToDrawingEvents(DrawingLayouts.LatestBar | DrawingLayouts.Historical);
    DrawAbovePrice = true; // Рисовать поверх графика
}
protected override void OnRender(RenderContext context, DrawingLayouts layout)
{
    // Логика отрисовки
}
```
**Пояснение**:
- `EnableCustomDrawing`: Активирует кастомную отрисовку.
- `SubscribeToDrawingEvents`: Указывает, когда вызывать `OnRender` (на последнем баре или для истории).
- `RenderContext`: Предоставляет методы для рисования (линии, текст, фигуры).

### Виды визуальных объектов

#### Линии
Рисуйте горизонтальные или трендовые линии:
```csharp
protected override void OnRender(RenderContext context, DrawingLayouts layout)
{
    var price = GetCandle(CurrentBar - 1).Close;
    var y = ChartInfo.GetYByPrice(price);
    var x1 = ChartInfo.PriceChartContainer.Region.Width / 2;
    var x2 = ChartInfo.PriceChartContainer.Region.Width;
    var pen = new RenderPen(Colors.Red, 2);
    context.DrawLine(pen, x1, y, x2, y); // Горизонтальная линия
}
```
**Пояснение**: `ChartInfo.GetYByPrice` преобразует цену в координату Y. `RenderPen` задаёт стиль линии.

#### Гистограммы
Для отображения объемов или дельты:
```csharp
protected override void OnRender(RenderContext context, DrawingLayouts layout)
{
    var candle = GetCandle(CurrentBar - 1);
    var delta = candle.Delta;
    var y = ChartInfo.GetYByPrice(delta > 0 ? delta : 0);
    var x = ChartInfo.PriceChartContainer.Region.Width - 50;
    var height = Math.Abs(delta) * 10; // Масштабирование
    var brush = new RenderSolidBrush(delta > 0 ? Colors.Green : Colors.Red);
    context.FillRectangle(brush, x, y, 20, (int)height);
}
```
**Пояснение**: `FillRectangle` рисует столбец гистограммы. Цвет зависит от знака дельты.

#### Метки и текст
Добавляйте текстовые метки:
```csharp
protected override void OnRender(RenderContext context, DrawingLayouts layout)
{
    var price = GetCandle(CurrentBar - 1).Close;
    var y = ChartInfo.GetYByPrice(price);
    var font = new RenderFont("Arial", 10);
    var format = new RenderStringFormat { Alignment = StringAlignment.Near };
    context.DrawString($"Price: {price}", font, Colors.Black, ChartInfo.PriceChartContainer.Region.Width - 100, y, format);
}
```
**Пояснение**: `DrawString` отображает текст на графике. `RenderStringFormat` задаёт выравнивание.

#### Кластеры (Footprint)
Рисуйте кастомные кластеры:
```csharp
protected override void OnRender(RenderContext context, DrawingLayouts layout)
{
    var candle = GetCandle(CurrentBar - 1);
    var levels = candle.GetAllPriceLevels();
    var x = ChartInfo.PriceChartContainer.Region.Width - 50;
    var font = new RenderFont("Arial", 8);
    foreach (var level in levels)
    {
        if (level == null) continue;
        var y = ChartInfo.GetYByPrice(level.Price);
        var volume = level.Volume;
        var brush = new RenderSolidBrush(volume > 1000 ? Colors.Blue : Colors.Gray);
        context.FillRectangle(brush, x, y - 5, 20, 10);
        context.DrawString(volume.ToString(), font, Colors.White, x + 25, y - 5);
    }
}
```
**Пояснение**: Рисует кластеры для каждого уровня цены с объёмом, выделяя крупные объёмы цветом.

#### Кастомные фигуры
Рисуйте эллипсы, треугольники и другие объекты:
```csharp
protected override void OnRender(RenderContext context, DrawingLayouts layout)
{
    var price = GetCandle(CurrentBar - 1).High;
    var y = ChartInfo.GetYByPrice(price);
    var x = ChartInfo.PriceChartContainer.Region.Width - 50;
    var brush = new RenderSolidBrush(Colors.Orange);
    context.FillEllipse(brush, x, y - 10, 20, 20); // Эллипс на максимуме
}
```
**Пояснение**: `FillEllipse` рисует эллипс. Аналогично можно использовать `FillTriangle`, `FillRectangle`.

### Настройка стилей
Используйте `ColorsStore` для согласованности:
```csharp
protected override void OnRender(RenderContext context, DrawingLayouts layout)
{
    var pen = new RenderPen(ChartInfo.ColorsStore.UpCandleColor, 2);
    var brush = new RenderSolidBrush(ChartInfo.ColorsStore.DownCandleColor);
    // Используйте pen и brush для рисования
}
```
**Пояснение**: `ColorsStore` берёт цвета из настроек графика, обеспечивая единый стиль.

### Визуальные режимы
Задавайте режим через `VisualMode`:
```csharp
public CustomIndicator()
{
    var series = (ValueDataSeries)DataSeries[0];
    series.VisualType = VisualMode.Dots; // Точки
    // Другие варианты: Line, Histogram, Cross, Square, UpArrow, DownArrow, Hide
}
```
**Пояснение**: `VisualMode` определяет, как данные отображаются на графике.

## Расчёты данных для индикаторов

### Данные свечей
Получайте данные через `GetCandle`:
```csharp
protected override void OnCalculate(int bar, decimal value)
{
    var candle = GetCandle(bar);
    _values[bar] = candle.Close - candle.Open; // Разница цены закрытия и открытия
}
```
**Ключевые свойства**:
- `Open`, `High`, `Low`, `Close`: Цены свечи.
- `Volume`: Объём.
- `Delta`: Дельта (разница покупок и продаж).
- `LastTime`: Время последней сделки.
- `MaxDelta`, `MinDelta`: Экстремальные значения дельты.

### Тики
Обрабатывайте тики через `OnNewTrade`:
```csharp
private decimal _tickSum;
protected override void OnNewTrade(MarketDataArg arg)
{
    _tickSum += arg.Price;
    this[CurrentBar - 1] = _tickSum; // Сумма цен тиков
}
```
**Пояснение**: `MarketDataArg` содержит цену (`Price`), объём и направление сделки.

### Кумулятивные сделки
Обрабатывайте крупные сделки:
```csharp
private CumulativeTrade _lastTrade;
private decimal _lastVolume;
protected override void OnCumulativeTrade(CumulativeTrade trade)
{
    if (trade.Volume >= 10) // Фильтр по объёму
    {
        if (_lastTrade != trade)
        {
            _lastTrade = trade;
            this[CurrentBar - 1] += trade.Volume * (trade.Direction == TradeDirection.Buy ? 1 : -1);
        }
        else
        {
            this[CurrentBar - 1] += (trade.Volume - _lastVolume) * (trade.Direction == TradeDirection.Buy ? 1 : -1);
        }
        _lastVolume = trade.Volume;
    }
}
protected override void OnUpdateCumulativeTrade(CumulativeTrade trade) => OnCumulativeTrade(trade);
```
**Пояснение**: Фильтрует сделки по объёму и учитывает направление (`Buy` или `Sell`).

Для исторических сделок:
```csharp
protected override void OnFinishRecalculate()
{
    var start = GetCandle(0).Time;
    var end = GetCandle(CurrentBar - 1).LastTime;
    var request = new CumulativeTradesRequest(start, end, 10, 0); // Минимум 10 лотов
    RequestForCumulativeTrades(request);
}
protected override void OnCumulativeTradesResponse(CumulativeTradesRequest request, IEnumerable<CumulativeTrade> trades)
{
    foreach (var trade in trades)
        this[CurrentBar - 1] += trade.Volume;
}
```

### Футпринт
Анализируйте данные по уровням цены:
```csharp
protected override void OnCalculate(int bar, decimal value)
{
    var candle = GetCandle(bar);
    var maxVolumeLevel = candle.MaxVolumePriceInfo;
    if (maxVolumeLevel != null)
        _values[bar] = maxVolumeLevel.Price; // Цена максимального объёма
}
```
**Методы**:
- `GetPriceVolumeInfo(decimal price)`: Данные по конкретной цене.
- `GetAllPriceLevels()`: Все уровни цен.
- `MaxVolumePriceInfo`, `MaxTickPriceInfo`, `MaxPositiveDeltaPriceInfo`, `MaxNegativeDeltaPriceInfo`: Экстремальные уровни.

### Market Depth
Обрабатывайте изменения стакана:
```csharp
private ValueDataSeries _domImbalance;
public CustomIndicator() : base(true)
{
    _domImbalance = new ValueDataSeries("Imbalance") { VisualType = VisualMode.Histogram };
    DataSeries.Add(_domImbalance);
}
protected override void MarketDepthChanged(MarketDataArg arg)
{
    var asks = MarketDepthInfo.CumulativeDomAsks;
    var bids = MarketDepthInfo.CumulativeDomBids;
    _domImbalance[CurrentBar - 1] = bids - asks; // Дисбаланс
}
```
**Пояснение**: `CumulativeDomAsks` и `CumulativeDomBids` дают суммарные объёмы.

Для снимка стакана:
```csharp
var snapshot = MarketDepthInfo.GetMarketDepthSnapshot();
foreach (var level in snapshot)
    this.LogInfo($"Price: {level.Price}, Volume: {level.Volume}");
```

### Market By Orders (MBO)
Анализируйте рыночные заявки:
```csharp
private IMarketByOrdersManager _manager;
protected override async void OnInitialize()
{
    _manager = await SubscribeMarketByOrderData();
    _manager.Changed += (orders) =>
    {
        foreach (var order in orders)
            if (order.Type == MarketByOrderUpdateType.New && order.Volume > 50)
                this[CurrentBar - 1] += order.Price;
    };
}
protected override void OnDispose()
{
    if (_manager != null)
        _manager.Changed -= null;
    base.OnDispose();
}
```
**Пояснение**: Фильтрует новые заявки с объёмом > 50 лотов.

### Статистика
Получайте торговую статистику:
```csharp
protected override async void OnInitialize()
{
    var start = GetCandle(0).Time;
    var end = GetCandle(CurrentBar - 1).LastTime;
    var stats = await TradingStatisticsProvider.LoadAsync(start, end);
    foreach (var trade in stats.MyTrades)
        this[CurrentBar - 1] += trade.Profit;
}
```
**Типы данных**:
- `Equity`: Эквити.
- `MyTrades`: Личные сделки.
- `Orders`: Ордера.
- `Statistics`: Прочие параметры.

## Алерты
Добавляйте уведомления:
```csharp
protected override void OnCalculate(int bar, decimal value)
{
    if (value > 1000)
        AddAlert("alert.wav", InstrumentInfo.Name, $"High value: {value}", Colors.Red, Colors.White);
}
```
**Пояснение**: `AddAlert` вызывает звуковое и визуальное уведомление.

## Оптимизация и тестирование
1. **Производительность**:
   - Минимизируйте операции в `OnCalculate` и `OnRender`.
   - Кэшируйте данные:
     ```csharp
     private Dictionary<int, decimal> _cache = new();
     protected override void OnCalculate(int bar, decimal value)
     {
         if (!_cache.ContainsKey(bar))
             _cache[bar] = ComplexCalculation(value);
         this[bar] = _cache[bar];
     }
     ```

2. **Обработка ошибок**:
   ```csharp
   protected override void OnCalculate(int bar, decimal value)
   {
       try
       {
           var candle = GetCandle(bar);
           if (candle == null) return;
           this[bar] = candle.Close;
       }
       catch (Exception e)
       {
           this.LogError($"Error at bar {bar}: {e.Message}");
       }
   }
   ```

3. **Тестирование**:
   - Проверяйте индикатор на исторических данных.
   - Используйте логи:
     ```csharp
     this.LogInfo($"Bar {bar}: Value = {this[bar]}");
     ```

## Пример сложного индикатора
Индикатор отображает уровни максимального объёма с кастомной визуализацией и алертами:
```csharp
namespace ATAS.Indicators.Technical
{
    using System.ComponentModel.DataAnnotations;
    using System.Drawing;
    using OFT.Rendering.Context;
    using OFT.Rendering.Tools;
    [DisplayName("Volume Levels")]
    public class VolumeLevels : Indicator
    {
        private ValueDataSeries _levels;
        private decimal _threshold = 1000;
        [Display(Name = "Volume Threshold", GroupName = "Settings")]
        public decimal Threshold
        {
            get => _threshold;
            set { _threshold = value; RecalculateValues(); }
        }
        public VolumeLevels() : base(true)
        {
            _levels = new ValueDataSeries("Levels") { VisualType = VisualMode.Dots };
            DataSeries.Add(_levels);
            EnableCustomDrawing = true;
            SubscribeToDrawingEvents(DrawingLayouts.LatestBar);
            DrawAbovePrice = true;
        }
        protected override void OnCalculate(int bar, decimal value)
        {
            var candle = GetCandle(bar);
            var maxVolumeLevel = candle.MaxVolumePriceInfo;
            if (maxVolumeLevel != null && maxVolumeLevel.Volume > _threshold)
            {
                _levels[bar] = maxVolumeLevel.Price;
                if (bar == CurrentBar - 1)
                    AddAlert("alert.wav", InstrumentInfo.Name, $"High Volume at {maxVolumeLevel.Price}", Colors.Blue, Colors.White);
            }
        }
        protected override void OnRender(RenderContext context, DrawingLayouts layout)
        {
            var font = new RenderFont("Arial", 10);
            for (int bar = 0; bar < CurrentBar; bar++)
            {
                if (_levels[bar] == 0) continue;
                var y = ChartInfo.GetYByPrice(_levels[bar]);
                var x = ChartInfo.GetXByBar(bar);
                var brush = new RenderSolidBrush(Colors.Blue);
                context.FillEllipse(brush, x - 5, y - 5, 10, 10);
                context.DrawString(_levels[bar].ToString(), font, Colors.Black, x + 10, y - 5);
            }
        }
    }
}
```
**Пояснение**: Индикатор отмечает уровни с объёмом выше порога точками и метками, выдавая алерты.

## Рекомендации для технических заданий
Чтобы я мог создать индикатор по вашему описанию:
1. **Опишите визуализацию**:
   - Что рисовать (линии, гистограммы, кластеры, фигуры)?
   - Где (над графиком, в отдельной панели)?
   - Какие цвета и стили?
2. **Укажите данные**:
   - Какие данные анализировать (свечи, тики, футпринт, MBO)?
   - Какие расчёты (максимумы, дельта, дисбаланс)?
3. **Задайте параметры**:
   - Какие настройки нужны (период, порог объёма)?
   - Нужны ли фильтры или алерты?
4. **Пример**:
   > Хочу индикатор, который рисует горизонтальную линию на цене максимального объёма за день, с синим цветом и меткой объёма. Если объём превышает 5000, выдавать звуковой алерт. Показывать только на последнем баре.

Я переведу это в код, основываясь на методичке.

---
