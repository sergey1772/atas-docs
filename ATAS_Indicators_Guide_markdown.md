


Эта методичка предназначена для разработчиков, создающих пользовательские индикаторы для торговой платформы ATAS с использованием языка программирования C# на платформе .NET 8. Документ основан на предоставленных данных и охватывает технические аспекты работы с пространством имен `ATAS.Indicators`, включая классы, интерфейсы, перечисления и примеры реализации индикаторов.


# Методичка по созданию индикаторов для платформы ATAS (.NET 8) — Часть 1

## 1. Общая структура и пространства имен

### 1.1. Пространства имен

Платформа ATAS предоставляет несколько ключевых пространств имен для разработки индикаторов и стратегий:

- **ATAS.Indicators** — основное пространство имен для создания индикаторов, содержащее базовые классы (`Indicator`, `BaseIndicator`), интерфейсы (`IIndicatorDataProvider`, `IChart`), перечисления (`VisualMode`, `ChartVisualModes`) и классы данных (`IndicatorCandle`, `ValueDataSeries`).
- **ATAS.Indicators.АDrawing** — содержит классы для графических объектов, такие как `TrendLine` и перечисление `DefaultColors`.
- **ATAS.Indicators.Filters** — предоставляет фильтры для параметров индикаторов, включая `Filter<TValue>`, `FilterBool`, `FilterColor` и интерфейс `IFilter`.
- **ATAS.Indicators.Properties** — содержит атрибуты и настройки, такие как `ParameterAttribute` для описания параметров индикаторов.
- **ATAS.DataFeedsCore** — предоставляет доступ к рыночным данным через классы (`InstrumentInfo`) и интерфейсы (`IMarketTimeProvider`).
- **ATAS.Strategies** — включает классы и интерфейсы для торговых стратегий, такие как `Strategy` и `IStrategy`.

### 1.2. Структура директорий

Исходный код индикаторов организован в следующие директории:

- **Indicators** — основная директория, содержащая файлы, такие как `Indicator.cs`, `BaseDataSeries.cs`, `MarketDepthInfoProvider.cs`.
- **Attributes** — содержит атрибуты, например, `ParameterAttribute.cs` и `FeatureId.cs`.
- **Drawing** — включает файлы для графических объектов, такие как `TrendLine.cs` и `DefaultColors.cs`.
- **Filters** — содержит фильтры, такие как `Filter.cs`, `FilterBool.cs`, `FilterRangeBase.cs`.
- **Properties** — включает файлы настроек, например, `AssemblyInfo.cs`.

## 2. Ключевые классы и интерфейсы

### 2.1. Базовые классы для индикаторов

- **BaseIndicator** (`BaseIndicator.cs`)

  - Базовый класс для создания пользовательских индикаторов.
  - Содержит методы для расчетов, настройки отображения и взаимодействия с графиком.
  - Используется как основа для класса `Indicator`.

- **Indicator** (`Indicator.cs`)

  - Основной класс для реализации пользовательских индикаторов.
  - Наследуется от `BaseIndicator`.
  - Поддерживает методы `OnCalculate`, `OnRender` для расчетов и отрисовки, а также настройку через свойства, такие как `Panel`, `EnableCustomDrawing`.

- **ExtendedIndicator** (`BaseIndicator.cs`)

  - Расширенный базовый класс для индикаторов с дополнительной функциональностью: отображение, оповещения, работа с рыночными данными.

### 2.2. Классы для работы с данными

- **BaseDataSeries** (`BaseDataSeries.cs`)

  - Обобщенный базовый класс для серий данных, реализующий интерфейс `IDataSeries<T>`.
  - Используется для хранения данных индикаторов, таких как цены, объемы или пользовательские объекты.

- **ValueDataSeries** (`ValueDataSeries.cs`)

  - Серия данных для хранения значений типа `decimal`.
  - Поддерживает настройку цветов для баров через вложенный класс `BarColors`.

- **CandleDataSeries** (`CandleDataSeries.cs`)

  - Серия данных для хранения объектов `Candle`, представляющих свечи с ценами `Open`, `High`, `Low`, `Close`.

- **IndicatorCandle** (`IndicatorCandle.cs`)

  - Класс для представления свечи индикатора, содержащий дополнительные данные, такие как объем, Bid/Ask, дельта.

- **PaintbarsDataSeries** (`PaintbarsDataSeries.cs`)

  - Серия данных для хранения цветов баров (`Color?`), используемых для визуализации.

- **PriceSelectionDataSeries** (`PriceSelectionDataSeries.cs`)

  - Серия данных для хранения значений выбора цены (`PriceSelectionValue`), используемых в кластерах и барах.

- **RangeDataSeries** (`RangeDataSeries.cs`)

  - Серия данных для хранения диапазонов значений (`RangeValue`), например, максимума и минимума.

- **ObjectDataSeries** (`ObjectDataSeries.cs`)

  - Серия данных для хранения объектов произвольного типа.

- **CustomValueDataSeries** (`CustomValueDataSeries.cs`)

  - Серия данных для хранения пользовательских значений (`CustomValue`) с дополнительными свойствами.

### 2.3. Интерфейсы

- **IDataSeries** (`BaseDataSeries.cs`)

  - Интерфейс для работы с сериями данных, предоставляющий доступ к данным и их свойствам.

- **IIndicatorDataProvider** (`IndicatorDataProvider.cs`)

  - Интерфейс для предоставления данных и сервисов индикатора.
  - Реализуется классом `IndicatorDataProvider`.

- **IChart** (`IIndicatorContainer.cs`)

  - Интерфейс основного графика, предоставляющий методы и свойства для взаимодействия с графиком.

- **IIndicatorContainer** (`IIndicatorContainer.cs`)

  - Интерфейс контейнера индикатора, хранящий данные и методы для работы с индикаторами.

- **IChartCoordinatesManager** (`IIndicatorContainer.cs`)

  - Интерфейс для управления координатами и масштабированием графика.

- **IMarketDepthInfoProvider** (`MarketDepthInfoProvider.cs`)

  - Интерфейс для доступа к данным глубины рынка (стакана заявок).
  - Реализуется классом `MarketDepthInfoProvider`.

- **IMarketTimeProvider** (`IMarketTimeProvider.cs`)

  - Интерфейс для получения текущего времени и данных о торговых сессиях.

- **IPropertiesEditor** (`IPropertiesEditor.cs`)

  - Интерфейс для редактирования свойств индикаторов или объектов на графике.

- **ITradingManager** (`ITradingManager.cs`)

  - Интерфейс для управления торговыми операциями, такими как отправка ордеров.

### 2.4. Классы и интерфейсы для фильтров

- **Filter** (`Filter.cs`)

  - Базовый обобщенный класс фильтров для параметров индикаторов.
  - Наследуется от `FilterBase`.

- **FilterBool**, **FilterColor**, **FilterInt**, **FilterString**, **FilterTimeSpan** (`FilterBool.cs`, `FilterColor.cs`, `FilterInt.cs`, `FilterString.cs`, `FilterTimeSpan.cs`)

  - Специализированные фильтры для логических, цветовых, целочисленных, строковых значений и временных интервалов.

- **FilterRangeBase&lt;TValue, TFilter&gt;** (`FilterRangeBase.cs`)

  - Базовый класс для фильтров с диапазоном значений.

- **FilterRangeValue** (`FilterRangeValue.cs`)

  - Фильтр для диапазона значений обобщенного типа.

- **IFilter** (`IFilter.cs`)

  - Интерфейс для фильтров, определяющий базовые свойства и методы.

- **IFilterEnum** (`IFilterEnum.cs`)

  - Интерфейс для фильтров, основанных на перечислениях.

### 2.5. Классы для работы с рыночными данными

- **InstrumentInfo** (`InstrumentInfo.cs`)

  - Класс, реализующий интерфейс `IInstrumentInfo`, хранящий информацию о торговом инструменте (код, описание, тик-размер).

- **MarketDepthSnapshot** (`MarketDepthSnapshot.cs`)

  - Класс, представляющий снимок глубины рынка за определенный период.

- **MarketDepthSnapshotRequest** (`MarketDepthSnapshotRequest.cs`)

  - Класс для запроса снимка глубины рынка.

- **PriceVolumeInfo** (`PriceVolumeInfo.cs`)

  - Класс, представляющий информацию об объемах на определенной цене.

- **ValueArea** (`ValueArea.cs`)

  - Класс, представляющий информацию о границах зоны стоимости (Value Area High/Low).

- **FixedProfileRequest** (`PriceVolumeInfo.cs`)

  - Класс для запроса фиксированного профиля с определенным периодом.

### 2.6. Классы для отрисовки

- **TrendLine** (`TrendLine.cs`)

  - Класс для создания линий тренда на графике.

- **ChartObject** (`ChartObject.cs`)

  - Базовый класс для графических объектов, таких как линии, уровни, метки.

- **RenderPen** (`FilterRenderPen.cs`)

  - Класс для настройки пера для отрисовки линий.

## 3. Перечисления

### 3.1. Визуализация данных

- **VisualMode** (`VisualMode.cs`)

  - Определяет режимы визуализации данных:
    - `Line` — линейный график.
    - `Histogram` — гистограмма.
    - `Hash`, `Block` — заполненные/простые блоки.
    - `Cross`, `Square`, `Dots` — маркеры (крестики, квадраты, точки).
    - `UpArrow`, `DownArrow` — стрелки вверх/вниз.
    - `OnlyValueOnAxis` — отображение значения на оси.
    - `Hide` — скрытие серии.

- **ChartVisualModes** (`ChartVisualModes.cs`)

  - Режимы отображения графика:
    - `Candles` — японские свечи.
    - `Clusters` — кластеризованные свечи.
    - `TransparentCandles` — прозрачные свечи.
    - `Line` — линейный график.
    - `Bars` — вертикальные бары.
    - `Hidden` — скрытый режим.

- **CandleVisualMode** (`CandleVisualMode.cs`)

  - Режимы визуализации свечей:
    - `Candles` — японские свечи.
    - `Bars` — вертикальные линии (бары).

### 3.2. Типы данных и выбор

- **DataSeriesType** (`BaseDataSeries.cs`)

  - Типы данных серий:
    - `Bars`, `Open`, `High`, `Low`, `Close`, `Volume` — базовые данные свечей.
    - `HL2`, `HLC3`, `OHLC4`, `HLCC4` — вычисляемые средние цены.
    - `Indicator`, `Line`, `Band`, `Value`, `Candle`, `PriceSelection`, `PaintBars`, `CustomValue`, `Object` — типы индикаторов и объектов.

- **SelectionType** (`SelectionType.cs`)

  - Типы выбора уровня цены:
    - `Full` — полное выделение.
    - `Bid` — цена покупки.
    - `Ask` — цена продажи.

- **TradeDirection** (`TradeDirection.cs`)

  - Направления сделок:
    - `Between` — нейтральное состояние.
    - `Buy` — покупка.
    - `Sell` — продажа.

### 3.3. Глубина рынка и профили

- **MarketDataType** (`MarketDataType.cs`)

  - Типы рыночных данных:
    - `Bid` — цена покупки.
    - `Ask` — цена продажи.
    - `Trade` — реальные сделки.

- **FixedProfilePeriods** (`PriceVolumeInfo.cs`)

  - Периоды фиксированного профиля:
    - `CurrentDay` — текущий день.
    - `LastDay` — прошлый день.
    - `CurrentWeek` — текущая неделя.
    - `LastWeek` — прошлая неделя.
    - `CurrentMonth` — текущий месяц.
    - `LastMonth` — прошлый месяц.
    - `Contract` — контракт.

### 3.4. Footprint (следы рыночной активности)

- **FootprintColorSchemes** (`FootprintColorSchemes.cs`)

  - Схемы окраски:
    - `Delta` — цвет по дельте цены.
    - `Solid` — однородный цвет.
    - `VolumeProportion` — цвет по пропорции объема.
    - `TradesProportion` — цвет по количеству сделок.
    - `VolumeBasedBidAsk` — цвет по спросу/предложению.
    - `HeatMapVolume`, `HeatMapTrades`, `HeatMapDelta` — тепловые карты.
    - `None` — отключено.

- **FootprintContentModes** (`FootprintContentModes.cs`)

  - Типы содержимого:
    - `Volume` — объем.
    - `Trades` — количество сделок.
    - `VolumeTrades` — объем и сделки.
    - `VolumeDelta` — объем и дельта.
    - `Delta`, `DeltaCentered` — дельта цены.
    - `BidXAsk`, `BidAskCentered`, `BidAsk` — спрос/предложение.
    - `None` — отключено.

- **FootprintVisualModes** (`FootprintVisualModes.cs`)

  - Режимы визуализации:
    - `FullRow` — полная строка данных.
    - `BidAskHistogram` — гистограмма Bid/Ask.
    - `VolumeHistogram` — гистограмма объема.
    - `TradesHistogram` — гистограмма сделок.
    - `DeltaHistogram` — гистограмма дельты.
    - `BidAskLadder` — лестница Bid/Ask.
    - `PositiveNegativeDeltaProfile` — профиль дельты.

### 3.5. Режимы отрисовки

- **DrawingLayouts** (`DrawingLayouts.cs`)
  - Режимы размещения графических объектов:
    - `None` — отключено.
    - `Historical` — отображение исторических данных.
    - `LatestBar` — отображение последней свечи.
    - `Final` — финальный режим.

## 4. Типы и настройки

### 4.1. Typedefs

- **CrossColor** (`GlobalUsings.cs`) — `System.Windows.Media.Color` для работы с цветами.
- **CrossColors** (`GlobalUsings.cs`) — `System.Windows.Media.Colors` для предопределенных цветов.
- **CrossKey** (`GlobalUsings.cs`) — `System.Windows.Input.Key` для обработки клавиш.
- **CrossKeyEventArgs** (`GlobalUsings.cs`) — `System.Windows.Input.KeyEventArgs` для событий клавиатуры.
- **TimerSubscriptionKey** (`BaseIndicator.cs`) — кортеж `(TimeSpan period, Action callback)` для подписки на таймер.

### 4.2. Атрибуты

- **ParameterAttribute** (`ParameterAttribute.cs`)

  - Атрибут для описания параметров индикаторов, задающий имя, группу и порядок отображения.

- **FeatureId** (`FeatureId.cs`)

  - Идентификатор функциональных возможностей индикатора.







# Методичка по созданию индикаторов для платформы ATAS (.NET 8) — Часть 2



## 6. Подробное описание ключевых классов

### 6.1. Класс `Indicator`

Класс `Indicator` (`Indicator.cs`) является основным для создания пользовательских индикаторов. Он наследуется от `BaseIndicator` и предоставляет методы и свойства для расчетов, отрисовки и настройки.

#### Основные свойства:

- **Panel** — задает панель для отрисовки индикатора (`IndicatorDataProvider.NewPanel` для новой панели).
- **DenyToChangePanel** (`bool`) — запрещает пользователю изменять панель индикатора.
- **EnableCustomDrawing** (`bool`) — включает пользовательскую отрисовку через метод `OnRender`.
- **DrawAbovePrice** (`bool`) — определяет, отрисовывается ли индикатор над ценовым графиком.
- **DataSeries** — коллекция серий данных, где `DataSeries[0]` часто используется как основная серия и может быть скрыта (`IsHidden = true`).
- **ChartInfo** — предоставляет доступ к информации о графике (координаты, размеры, тип графика).

#### Основные методы:

- **OnCalculate(int bar, decimal value)** — вызывается для каждого бара при расчете индикатора. Параметры:
  - `bar` — индекс текущего бара.
  - `value` — значение основной серии данных для текущего бара.
  - Используется для выполнения расчетов и обновления данных индикатора.
- **OnRender(RenderContext context, DrawingLayouts layout)** — вызывается для отрисовки индикатора. Параметры:
  - `context` — контекст рендеринга для рисования (линии, текст, прямоугольники).
  - `layout` — режим отрисовки (`Historical`, `LatestBar`, `Final`).
  - Используется для пользовательской отрисовки графики.
- **SubscribeToDrawingEvents(DrawingLayouts layout)** — подписывает индикатор на события отрисовки для указанного режима.
- **RedrawChart()** — инициирует перерисовку графика при изменении данных или настроек.
- **GetCandle(int bar)** — возвращает объект `IndicatorCandle` для указанного бара.

#### Примечания:

- Для включения пользовательской отрисовки необходимо установить `EnableCustomDrawing = true` и подписаться на события отрисовки через `SubscribeToDrawingEvents`.
- Метод `OnCalculate` может быть пустым, если расчеты выполняются в `OnRender` или другом методе.

### 6.2. Класс `IndicatorDataProvider`

Класс `IndicatorDataProvider` (`IndicatorDataProvider.cs`) реализует интерфейс `IIndicatorDataProvider` и предоставляет доступ к данным и сервисам индикатора.

#### Основные свойства:

- **NewPanel** — возвращает новую панель для размещения индикатора.
- **PriceChartContainer** — контейнер ценового графика, предоставляющий информацию о ширине баров, регионе графика и координатах.
- **BarsWidth** — ширина баров в пикселях.
- **FirstVisibleBarNumber**, **LastVisibleBarNumber** — индексы первого и последнего видимых баров на графике.
- **ChartInfo** — информация о графике (тип, таймфрейм, координаты).

#### Основные методы:

- **GetCandle(int bar)** — возвращает свечу (`IndicatorCandle`) для указанного бара.
- **GetYByPrice(decimal price)** — преобразует цену в Y-координату на графике.
- **GetXByBar(int bar)** — преобразует индекс бара в X-координату на графике.

### 6.3. Класс `IndicatorCandle`

Класс `IndicatorCandle` (`IndicatorCandle.cs`) представляет свечу индикатора, содержащую данные о ценах и рыночной активности.

#### Основные свойства:

- **Open**, **High**, **Low**, **Close** — цены открытия, максимума, минимума и закрытия.
- **Volume** — объем торгов для свечи.
- **Bid**, **Ask** — объемы спроса и предложения.
- **MaxBidPriceInfo**, **MaxAskPriceInfo**, **MaxVolumePriceInfo**, **MaxTickPriceInfo**, **MaxTimePriceInfo** — информация о максимальных уровнях для Bid, Ask, объема, тиков и времени.
- **MaxPositiveDeltaPriceInfo**, **MaxNegativeDeltaPriceInfo** — информация о максимальных уровнях положительной и отрицательной дельты.

### 6.4. Классы серий данных

- **ValueDataSeries**:
  - Хранит значения типа `decimal`.
  - Поддерживает настройку цветов баров через `BarColors`.
- **PaintbarsDataSeries**:
  - Хранит цвета (`Color?`) для визуализации баров.
- **PriceSelectionDataSeries**:
  - Хранит объекты `PriceSelectionValue` для выбора ценовых уровней.
- **RangeDataSeries**:
  - Хранит объекты `RangeValue` для диапазонов значений.
- **ObjectDataSeries**:
  - Хранит объекты произвольного типа.
- **CandleDataSeries**:
  - Хранит объекты `Candle` с данными свечей.

#### Методы серий данных:

- **Add(T value)** — добавляет значение в серию.
- **Clear()** — очищает серию.
- **IsHidden** — скрывает серию от отображения.

### 6.5. Класс `RenderContext`

Класс `RenderContext` (из пространства имен `OFT.Rendering.Context`) используется для отрисовки графических элементов в методе `OnRender`.

#### Основные методы:

- **DrawLine(RenderPen pen, int x1, int y1, int x2, int y2)** — рисует линию.
- **FillRectangle(Color color, Rectangle rectangle)** — заполняет прямоугольник цветом.
- **DrawString(string text, RenderFont font, Color color, Rectangle rectangle)** — рисует текст в указанном прямоугольнике.
- **MeasureString(string text, RenderFont font)** — измеряет размер текста.
- **DrawString(string text, RenderFont font, Color color, int x, int y)** — рисует текст по координатам.

## 7. Примеры индикаторов

Ниже приведены примеры индикаторов, реализованных на основе предоставленных данных. Каждый пример включает полный код и описание функциональности.

### 7.1. Индикатор `VolumeOnChart`

Индикатор отображает объем торгов для каждого бара в виде текста на графике.

#### Код:

```csharp
using ATAS.Indicators;
using ATAS.Indicators.Drawing;
using System.Windows.Media;

namespace ATAS.Indicators.Technical
{
    [DisplayName("Volume on Chart")]
    public class VolumeOnChart : Indicator
    {
        private readonly ValueDataSeries _volumeSeries = new("VolumeSeries", "Volume");
        private bool _showText = true;
        private int _fontSize = 10;
        private Color _textColor = Colors.White;

        [Display(Name = "Show Text", GroupName = "Settings", Order = 10)]
        public bool ShowText
        {
            get => _showText;
            set
            {
                _showText = value;
                RecalculateValues();
            }
        }

        [Display(Name = "Font Size", GroupName = "Settings", Order = 20)]
        [Range(8, 20)]
        public int FontSize
        {
            get => _fontSize;
            set
            {
                _fontSize = value;
                RecalculateValues();
            }
        }

        [Display(Name = "Text Color", GroupName = "Settings", Order = 30)]
        public Color TextColor
        {
            get => _textColor;
            set
            {
                _textColor = value;
                RecalculateValues();
            }
        }

        public VolumeOnChart()
            : base(true)
        {
            DenyToChangePanel = true;
            EnableCustomDrawing = true;
            SubscribeToDrawingEvents(DrawingLayouts.LatestBar);
            DrawAbovePrice = true;

            DataSeries[0].IsHidden = true;
        }

        protected override void OnCalculate(int bar, decimal value)
        {
            var candle = GetCandle(bar);
            _volumeSeries[bar] = candle.Volume;
        }

        protected override void OnRender(RenderContext context, DrawingLayouts layout)
        {
            if (!_showText)
                return;

            var font = new RenderFont("Arial", _fontSize);

            for (int bar = FirstVisibleBarNumber; bar <= LastVisibleBarNumber; bar++)
            {
                var volume = _volumeSeries[bar];
                var candle = GetCandle(bar);
                var x = ChartInfo.GetXByBar(bar);
                var y = ChartInfo.GetYByPrice(candle.Low);

                var text = volume.ToString();
                var size = context.MeasureString(text, font);
                var textRect = new Rectangle(x - size.Width / 2, y - size.Height - 5, size.Width, size.Height);

                context.DrawString(text, font, _textColor.Convert(), textRect);
            }
        }
    }
}
```

#### Описание:

- **Функциональность**: Отображает объем торгов (`Volume`) для каждого бара в виде текста под минимумом свечи.
- **Настройки**:
  - `ShowText` — показывать или скрывать текст (по умолчанию `true`).
  - `FontSize` — размер шрифта (диапазон 8–20, по умолчанию 10).
  - `TextColor` — цвет текста (по умолчанию белый).
- **Особенности**:
  - Использует `ValueDataSeries` для хранения объемов.
  - Отрисовка выполняется в методе `OnRender` с использованием `RenderContext`.
  - Текст позиционируется под минимумом свечи (`candle.Low`).
  - Поддерживает динамическое обновление при изменении настроек через `RecalculateValues`.

### 7.2. Индикатор `CurrentPrice`

Индикатор отображает текущую цену в виде горизонтальной линии и метки на ценовой оси.

#### Код:

```csharp
using ATAS.Indicators;
using ATAS.Indicators.Drawing;
using System.Drawing;
using System.Windows.Media;

namespace ATAS.Indicators.Technical
{
    [DisplayName("Current Price")]
    [Category("Other")]
    public class CurrentPrice : Indicator
    {
        private readonly LineSeries _currentPrice = new("CurrentPrice", "Current Price")
        {
            Color = DefaultColors.Blue.Convert(),
            Width = 2
        };

        public CurrentPrice()
            : base(true)
        {
            DenyToChangePanel = true;
            EnableCustomDrawing = true;
            SubscribeToDrawingEvents(DrawingLayouts.LatestBar);
            DrawAbovePrice = true;

            DataSeries[0].IsHidden = true;
            DataSeries.Add(_currentPrice);
        }

        protected override void OnCalculate(int bar, decimal value)
        {
            if (bar == CurrentBar - 1)
            {
                var candle = GetCandle(bar);
                _currentPrice[bar] = candle.Close;
            }
        }

        protected override void OnRender(RenderContext context, DrawingLayouts layout)
        {
            if (CurrentBar - 1 < 0)
                return;

            var price = _currentPrice[CurrentBar - 1];
            var y = ChartInfo.GetYByPrice(price);
            var pen = new RenderPen(_currentPrice.Color.Convert(), _currentPrice.Width);

            var firstX = ChartInfo.PriceChartContainer.Region.Width / 2;
            var secondX = ChartInfo.PriceChartContainer.Region.Width;

            context.DrawLine(pen, firstX, y, secondX, y);

            var priceStr = price.ToString(ChartInfo.StringFormat);
            this.DrawLabelOnPriceAxis(context, priceStr, y, new RenderFont("Arial", 8), Color.White);
        }
    }
}
```

#### Описание:

- **Функциональность**: Отображает текущую цену закрытия последнего бара в виде горизонтальной линии и метки на ценовой оси.
- **Настройки**: Использует фиксированный цвет линии (`DefaultColors.Blue`) и толщину (`2`).
- **Особенности**:
  - Использует `LineSeries` для хранения текущей цены.
  - Линия рисуется от середины графика до правого края.
  - Метка на ценовой оси отображается шрифтом Arial размером 8.

## 8. Работа с фильтрами

Фильтры (`ATAS.Indicators.Filters`) используются для настройки параметров индикаторов через пользовательский интерфейс.

### 8.1. Основные классы фильтров

- **Filter**:
  - Базовый класс для фильтров с обобщенным типом значения.
  - Свойства: `Value` (текущее значение), `Enabled` (включен/выключен).
- **FilterBool**, **FilterColor**, **FilterInt**, **FilterString**, **FilterTimeSpan**:
  - Специализированные фильтры для соответствующих типов данных.
  - Пример: `FilterColor` позволяет выбирать цвет через палитру.
- **FilterRangeBase&lt;TValue, TFilter&gt;**:
  - Базовый класс для фильтров с диапазоном значений (например, минимум и максимум).
- **FilterRangeValue**:
  - Фильтр для диапазона значений (например, `decimal` или `int`).

### 8.2. Использование фильтров

Фильтры применяются через атрибуты `Display` для свойств индикатора. Пример:

```csharp
[Display(Name = "Text Color", GroupName = "Settings", Order = 30)]
public Color TextColor { get; set; } = Colors.White;
```

- **Name** — отображаемое имя свойства.
- **GroupName** — группа в интерфейсе настроек.
- **Order** — порядок отображения.

Для диапазонов используется атрибут `Range`:

```csharp
[Display(Name = "Font Size", GroupName = "Settings", Order = 20)]
[Range(8, 20)]
public int FontSize { get; set; } = 10;
```







# Методичка по созданию индикаторов для платформы ATAS (.NET 8) — Часть 3

Эта часть методички продолжает описание разработки индикаторов для платформы ATAS с использованием C# на .NET 8. Здесь подробно рассмотрены оставшиеся примеры индикаторов (`ClusterStatisticSample`, `Watermark`, `MaxLevels`) и начато описание работы с рыночными данными. Все данные строго основаны на предоставленных файлах, без потери или искажения технической информации.

## 7. Примеры индикаторов (продолжение)

### 7.3. Индикатор `ClusterStatisticSample`

Индикатор отображает данные о свечах (объем, Bid, Ask) в табличной форме, аналогично индикатору Cluster Statistic.

#### Код:

```csharp
using ATAS.Indicators;

public class ClusterStatisticSample : Indicator
{
    public ClusterStatisticSample()
    {
        EnableCustomDrawing = true;
        SubscribeToDrawingEvents(DrawingLayouts.LatestBar);
        Panel = IndicatorDataProvider.NewPanel;
        DenyToChangePanel = true;
        DataSeries[0].IsHidden = true;
    }

    protected override void OnCalculate(int bar, decimal value)
    {
        // Метод оставлен пустым, так как вычисления выполняются в OnRender
    }

    protected override void OnRender(RenderContext context, DrawingLayouts layout)
    {
        var rowHeight = Container.Region.Height / 3;
        var drawText = ChartInfo.PriceChartContainer.BarsWidth > 50;
        var font = new RenderFont("Roboto", 13);

        Rectangle rectangle;

        for (int bar = FirstVisibleBarNumber; bar <= LastVisibleBarNumber; bar++)
        {
            var candle = GetCandle(bar);
            var x = ChartInfo.GetXByBar(bar);
            var y = Container.Region.Y;

            rectangle = new Rectangle(x, y, (int)ChartInfo.PriceChartContainer.BarsWidth, rowHeight);
            context.FillRectangle(Color.LightBlue, rectangle);
            if (drawText)
                context.DrawString(candle.Volume.ToString(), font, Color.Black, rectangle);

            rectangle.Y += rowHeight;
            context.FillRectangle(Color.IndianRed, rectangle);
            if (drawText)
                context.DrawString(candle.Bid.ToString(), font, Color.Black, rectangle);

            rectangle.Y += rowHeight;
            context.FillRectangle(Color.Lime, rectangle);
            if (drawText)
                context.DrawString(candle.Ask.ToString(), font, Color.Black, rectangle);
        }

        #region draw headers

        rectangle = new Rectangle(0, Container.Region.Y, 100, rowHeight);
        context.FillRectangle(Color.Gray, rectangle);
        context.DrawString("Volume", font, Color.Black, rectangle);

        rectangle.Y += rowHeight;
        context.FillRectangle(Color.Gray, rectangle);
        context.DrawString("Bid", font, Color.Black, rectangle);

        rectangle.Y += rowHeight;
        context.FillRectangle(Color.Gray, rectangle);
        context.DrawString("Ask", font, Color.Black, rectangle);

        #endregion
    }
}
```

#### Описание:

- **Функциональность**: Отображает данные свечи (объем, Bid, Ask) в виде таблицы на отдельной панели. Каждая строка таблицы соответствует одному параметру и окрашена в свой цвет: `LightBlue` для Volume, `IndianRed` для Bid, `Lime` для Ask.
- **Настройки**:
  - Панель: новая панель (`IndicatorDataProvider.NewPanel`).
  - Запрет изменения панели: `DenyToChangePanel = true`.
  - Пользовательская отрисовка: `EnableCustomDrawing = true`.
  - Подписка на события отрисовки: `DrawingLayouts.LatestBar`.
- **Особенности**:
  - Таблица делит высоту региона на три строки (`rowHeight = Container.Region.Height / 3`).
  - Текст отображается только если ширина бара больше 50 пикселей (`BarsWidth > 50`).
  - Заголовки таблицы ("Volume", "Bid", "Ask") рисуются в левой части панели серым цветом.
  - Используется шрифт `Roboto` размером 13.
  - Расчеты отсутствуют, вся логика реализована в `OnRender`.

### 7.4. Индикатор `Watermark`

Индикатор отображает водяной знак на графике с настраиваемым положением, текстом и шрифтом.

#### Код:

```csharp
namespace ATAS.Indicators.Technical
{
    using System;
    using System.ComponentModel;
    using System.ComponentModel.DataAnnotations;
    using System.Drawing;
    using ATAS.Indicators.Editors;
    using ATAS.Indicators.Properties;
    using OFT.Rendering.Context;
    using OFT.Rendering.Tools;
    using Color = System.Windows.Media.Color;

    public class Watermark : Indicator
    {
        #region Nested types

        public enum Location
        {
            [Display(Name = "Center")]
            Center,
            [Display(Name = "TopLeft")]
            TopLeft,
            [Display(Name = "TopRight")]
            TopRight,
            [Display(Name = "BottomLeft")]
            BottomLeft,
            [Display(Name = "BottomRight")]
            BottomRight
        }

        #endregion

        #region Properties

        [Display(Name = "Color", GroupName = "Common", Order = 10)]
        public Color TextColor { get; set; } = Color.FromArgb(255, 225, 225, 225);

        [Display(Name = "TextLocation", GroupName = "Common", Order = 20)]
        public Location TextLocation { get; set; }

        [Display(Name = "HorizontalOffset", GroupName = "Common", Order = 30)]
        public int HorizontalOffset { get; set; }

        [Display(Name = "VerticalOffset", GroupName = "Common", Order = 40)]
        public int VerticalOffset { get; set; }

        [Display(Name = "ShowInstrument", GroupName = "FirstLine", Order = 50)]
        public bool ShowInstrument { get; set; } = true;

        [Display(Name = "ShowPeriod", GroupName = "FirstLine", Order = 60)]
        public bool ShowPeriod { get; set; } = true;

        [Display(Name = "Font", GroupName = "FirstLine", Order = 70)]
        [Editor(typeof(FontEditor), typeof(FontEditor))]
        public FontSetting Font { get; set; } = new FontSetting { Size = 60, Bold = true };

        [Display(Name = "Text", GroupName = "SecondLine", Order = 80)]
        public string AdditionalText { get; set; } = "";

        [Display(Name = "Font", GroupName = "SecondLine", Order = 90)]
        [Editor(typeof(FontEditor), typeof(FontEditor))]
        public FontSetting AdditionalFont { get; set; } = new FontSetting { Size = 55 };

        [Display(Name = "VerticalOffset", GroupName = "SecondLine", Order = 100)]
        public int AdditionalTextYOffset { get; set; } = -40;

        #endregion

        #region ctor

        public Watermark()
            : base(true)
        {
            Font.PropertyChanged += (a, b) => RedrawChart();
            AdditionalFont.PropertyChanged += (a, b) => RedrawChart();

            DataSeries[0].IsHidden = true;
            DenyToChangePanel = true;
            EnableCustomDrawing = true;
            SubscribeToDrawingEvents(DrawingLayouts.Historical);
            DrawAbovePrice = false;
        }

        #endregion

        #region Overrides of BaseIndicator

        protected override void OnCalculate(int bar, decimal value)
        {
            // Метод оставлен пустым, так как отрисовка выполняется в OnRender
        }

        protected override void OnRender(RenderContext context, DrawingLayouts layout)
        {
            var showSecondLine = !string.IsNullOrWhiteSpace(AdditionalText);
            if (!showSecondLine && !ShowInstrument && !ShowPeriod)
                return;

            var textColor = TextColor.Convert();
            var mainTextRectangle = new Rectangle();
            var additionalTextRectangle = new Rectangle();
            var firstLine = string.Empty;

            if (showSecondLine && !string.IsNullOrEmpty(AdditionalText))
            {
                var size = context.MeasureString(AdditionalText, AdditionalFont.Font);
                additionalTextRectangle = new Rectangle(0, 0, (int)size.Width, (int)size.Height);
            }

            if (ShowInstrument || ShowPeriod)
            {
                if (ShowInstrument)
                    firstLine = InstrumentInfo.Instrument;

                if (ShowPeriod)
                {
                    var period = ChartInfo.ChartType == "TimeFrame" ? ChartInfo.TimeFrame : $"{ChartInfo.ChartType} {ChartInfo.Ti";
                    if (ShowInstrument)
                        firstLine += $", {period}";
                    else
                        firstLine += period;
                }
                var size = context.MeasureString(firstLine, Font.Font);
                mainTextRectangle = new Rectangle(0, 0, (int)size.Width, (int)size.Height);
            }

            if (mainTextRectangle.Height > 0 && additionalTextRectangle.Height > 0)
            {
                int firstLineX;
                int secondLineX;
                var y = 0;
                var totalHeight = mainTextRectangle.Height + additionalTextRectangle.Height + AdditionalTextYOffset;

                switch (TextLocation)
                {
                    case Location.Center:
                        firstLineX = ChartInfo.PriceChartContainer.Region.Width / 2 - mainTextRectangle.Width / 2 + HorizontalOffset;
                        secondLineX = ChartInfo.PriceChartContainer.Region.Width / 2 - additionalTextRectangle.Width / 2 + HorizontalOffset;
                        y = ChartInfo.PriceChartContainer.Region.Height / 2 - totalHeight / 2 + VerticalOffset;
                        break;
                    case Location.TopLeft:
                        firstLineX = secondLineX = HorizontalOffset;
                        break;
                    case Location.TopRight:
                        firstLineX = ChartInfo.PriceChartContainer.Region.Width - mainTextRectangle.Width + HorizontalOffset;
                        secondLineX = ChartInfo.PriceChartContainer.Region.Width - additionalTextRectangle.Width + HorizontalOffset;
                        break;
                    case Location.BottomLeft:
                        firstLineX = secondLineX = HorizontalOffset;
                        y = ChartInfo.PriceChartContainer.Region.Height - totalHeight + VerticalOffset;
                        break;
                    case Location.BottomRight:
                        firstLineX = ChartInfo.PriceChartContainer.Region.Width - mainTextRectangle.Width + HorizontalOffset;
                        secondLineX = ChartInfo.PriceChartContainer.Region.Width - additionalTextRectangle.Width + HorizontalOffset;
                        y = ChartInfo.PriceChartContainer.Region.Height - totalHeight + VerticalOffset;
                        break;
                    default:
                        throw new ArgumentOutOfRangeException();
                }
                context.DrawString(firstLine, Font.Font, textColor, firstLineX, y);
                context.DrawString(AdditionalText, AdditionalFont.Font, textColor, secondLineX, y + mainTextRectangle.Height);
            }
            else if (mainTextRectangle.Height > 0)
            {
                DrawString(context, firstLine, Font.Font, textColor, mainTextRectangle);
            }
            else if (additionalTextRectangle.Height > 0)
            {
                DrawString(context, AdditionalText, AdditionalFont.Font, textColor, additionalTextRectangle);
            }
        }

        private void DrawString(RenderContext context, string text, RenderFont font, System.Drawing.Color color, Rectangle rectangle)
        {
            switch (TextLocation)
            {
                case Location.Center:
                    context.DrawString(text, font, color, ChartInfo.PriceChartContainer.Region.Width / 2 - rectangle.Width / 2 + HorizontalOffset,
                        ChartInfo.PriceChartContainer.Region.Height / 2 - rectangle.Height / 2 + VerticalOffset);
                    break;
                case Location.TopLeft:
                    context.DrawString(text, font, color, HorizontalOffset, VerticalOffset);
                    break;
                case Location.TopRight:
                    context.DrawString(text, font, color, ChartInfo.PriceChartContainer.Region.Width - rectangle.Width + HorizontalOffset, VerticalOffset);
                    break;
                case Location.BottomLeft:
                    context.DrawString(text, font, color, HorizontalOffset, ChartInfo.PriceChartContainer.Region.Height - rectangle.Height + VerticalOffset);
                    break;
                case Location.BottomRight:
                    context.DrawString(text, font, color, ChartInfo.PriceChartContainer.Region.Width - rectangle.Width + HorizontalOffset,
                        ChartInfo.PriceChartContainer.Region.Height - rectangle.Height + VerticalOffset);
                    break;
                default:
                    throw new ArgumentOutOfRangeException();
            }
        }
    }
}
```

#### Описание:

- **Функциональность**: Отображает водяной знак на графике, содержащий информацию об инструменте, периоде графика и/или дополнительный текст. Пользователь может настроить положение, цвет, шрифт и смещение текста.
- **Настройки**:
  - `TextColor` — цвет текста (по умолчанию светло-серый, `Color.FromArgb(255, 225, 225, 225)`).
  - `TextLocation` — положение текста (`Center`, `TopLeft`, `TopRight`, `BottomLeft`, `BottomRight`).
  - `HorizontalOffset`, `VerticalOffset` — горизонтальное и вертикальное смещение.
  - `ShowInstrument`, `ShowPeriod` — показывать ли инструмент и период графика (по умолчанию `true`).
  - `Font` — шрифт для первой строки (по умолчанию размер 60, жирный).
  - `AdditionalText` — дополнительный текст для второй строки.
  - `AdditionalFont` — шрифт для второй строки (по умолчанию размер 55).
  - `AdditionalTextYOffset` — вертикальное смещение второй строки (по умолчанию -40).
- **Особенности**:
  - Использует пользовательскую отрисовку (`EnableCustomDrawing = true`) с подпиской на `DrawingLayouts.Historical`.
  - Отрисовка выполняется под ценовым графиком (`DrawAbovePrice = false`).
  - Поддерживает две строки текста: первая — инструмент и период, вторая — дополнительный текст.
  - Положение текста вычисляется динамически в зависимости от `TextLocation`.
  - Изменение шрифтов вызывает перерисовку графика (`RedrawChart`).

### 7.5. Индикатор `MaxLevels`

Индикатор отображает максимальные уровни цены для выбранного типа данных (Volume, Bid, Ask, Delta, Ticks, Time) за определенный период.

#### Код:

```csharp
namespace ATAS.Indicators.Technical
{
    using System;
    using System.ComponentModel;
    using System.ComponentModel.DataAnnotations;
    using System.Drawing;
    using ATAS.Indicators.Properties;
    using OFT.Rendering.Context;
    using OFT.Rendering.Tools;
    using Utils.Common.Attributes;
    using Utils.Common.Localization;

    [DisplayName("Maximum Levels")]
    [Category("Clusters, Profiles, Levels")]
    public class MaxLevels : Indicator
    {
        #region Nested types

        public enum MaxLevelType
        {
            Bid,
            Ask,
            PositiveDelta,
            NegativeDelta,
            Volume,
            Tick,
            Time
        }

        #endregion

        #region Private

        private RenderStringFormat _stringRightFormat = new RenderStringFormat
        {
            Alignment = StringAlignment.Far,
            LineAlignment = StringAlignment.Center,
            Trimming = StringTrimming.EllipsisCharacter,
            FormatFlags = StringFormatFlags.NoWrap
        };

        private bool _candleRequested;
        private IndicatorCandle _candle;
        private Color _lineColor = System.Drawing.Color.CornflowerBlue;
        private Color _axisTextColor = System.Drawing.Color.White;
        private Color _textColor = System.Drawing.Color.Black;
        private RenderPen _renderPen = new RenderPen(System.Drawing.Color.CornflowerBlue, 2);
        private int _width = 2;
        private FixedProfilePeriods _period = FixedProfilePeriods.CurrentDay;
        private RenderFont _font = new RenderFont("Arial", 8);
        private string _description = "Current Day";

        #endregion

        #region Properties

        public FixedProfilePeriods Period
        {
            get => _period;
            set
            {
                _period = value;
                _description = GetPeriodDescription(_period);
                RecalculateValues();
            }
        }

        public MaxLevelType Type { get; set; } = MaxLevelType.Volume;

        public System.Windows.Media.Color Color
        {
            get => _lineColor.Convert();
            set
            {
                _lineColor = value.Convert();
                _renderPen = new RenderPen(_lineColor, _width);
            }
        }

        public int Width
        {
            get => _width;
            set
            {
                _width = Math.Max(1, value);
                _renderPen = new RenderPen(_lineColor, _width);
            }
        }

        public System.Windows.Media.Color AxisTextColor
        {
            get => _axisTextColor.Convert();
            set => _axisTextColor = value.Convert();
        }

        public bool ShowText { get; set; } = true;

        public System.Windows.Media.Color TextColor
        {
            get => _textColor.Convert();
            set => _textColor = value.Convert();
        }

        public int FontSize
        {
            get => (int)_font.Size;
            set => _font = new RenderFont("Arial", Math.Max(7, value));
        }

        #endregion

        #region Overrides of BaseIndicator

        public MaxLevels()
            : base(true)
        {
            DataSeries[0].IsHidden = true;
            DenyToChangePanel = true;
            EnableCustomDrawing = true;
            SubscribeToDrawingEvents(DrawingLayouts.LatestBar);
            DrawAbovePrice = true;
            Width = Width;
        }

        protected override void OnCalculate(int bar, decimal value)
        {
            if (bar == 0)
                _candleRequested = false;

            if (!_candleRequested && bar == CurrentBar - 1)
            {
                _candleRequested = true;
                GetFixedProfile(new FixedProfileRequest(Period));
            }
        }

        protected override void OnFixedProfilesResponse(IndicatorCandle fixedProfile, FixedProfilePeriods period)
        {
            _candle = fixedProfile;
            RedrawChart();
        }

        protected override void OnRender(RenderContext context, DrawingLayouts layout)
        {
            if (_candle == null)
                return;

            var priceInfo = GetPriceVolumeInfo(_candle, Type);
            if (priceInfo == null)
                return;

            var y = ChartInfo.GetYByPrice(priceInfo.Price);
            var firstX = ChartInfo.PriceChartContainer.Region.Width / 2;
            var secondX = ChartInfo.PriceChartContainer.Region.Width;

            context.DrawLine(_renderPen, firstX, y, secondX, y);

            if (ShowText)
            {
                var size = context.MeasureString(_description, _font);
                var textRect = new Rectangle(
                    new Point(ChartInfo.PriceChartContainer.Region.Width - (int)size.Width - 20, y - new Size((int)size.Width + 20, (int)size.Height).Height),
                    new Size((int)size.Width + 20, (int)size.Height));

                context.DrawString(_description, _font, _textColor, textRect, _stringRightFormat);
            }
            this.DrawLabelOnPriceAxis(context, string.Format(ChartInfo.StringFormat, priceInfo.Price), y, _font, _lineColor);
        }

        #endregion

        private string GetPeriodDescription(FixedProfilePeriods period)
        {
            switch (period)
            {
                case FixedProfilePeriods.CurrentDay:
                    return "Current Day";
                case FixedProfilePeriods.LastDay:
                    return "Last Day";
                case FixedProfilePeriods.CurrentWeek:
                    return "Current Week";
                case FixedProfilePeriods.LastWeek:
                    return "Last Week";
                case FixedProfilePeriods.CurrentMonth:
                    return "Current Month";
                case FixedProfilePeriods.LastMonth:
                    return "Last Month";
                case FixedProfilePeriods.Contract:
                    return "Contract";
                default:
                    throw new ArgumentOutOfRangeException(nameof(period), period, null);
            }
        }

        private PriceVolumeInfo GetPriceVolumeInfo(IndicatorCandle candle, MaxLevelType levelType)
        {
            switch (levelType)
            {
                case MaxLevelType.Bid:
                    return candle.MaxBidPriceInfo;
                case MaxLevelType.Ask:
                    return candle.MaxAskPriceInfo;
                case MaxLevelType.PositiveDelta:
                    return candle.MaxPositiveDeltaPriceInfo;
                case MaxLevelType.NegativeDelta:
                    return candle.MaxNegativeDeltaPriceInfo;
                case MaxLevelType.Volume:
                    return candle.MaxVolumePriceInfo;
                case MaxLevelType.Tick:
                    return candle.MaxTickPriceInfo;
                case MaxLevelType.Time:
                    return candle.MaxTimePriceInfo;
                default:
                    throw new ArgumentOutOfRangeException();
            }
        }
    }
}
```

#### Описание:

- **Функциональность**: Отображает горизонтальную линию на уровне максимальной цены для выбранного типа данных (Bid, Ask, PositiveDelta, NegativeDelta, Volume, Tick, Time) за указанный период (например, текущий день). Дополнительно отображает текстовое описание периода и метку на ценовой оси.
- **Настройки**:
  - `Period` — период профиля (`CurrentDay`, `LastDay`, `CurrentWeek`, `LastWeek`, `CurrentMonth`, `LastMonth`, `Contract`).
  - `Type` — тип максимального уровня (`Volume` по умолчанию).
  - `Color` — цвет линии (по умолчанию `CornflowerBlue`).
  - `Width` — толщина линии (по умолчанию 2, минимум 1).
  - `AxisTextColor` — цвет текста на ценовой оси (по умолчанию белый).
  - `ShowText` — показывать текстовое описание периода (по умолчанию `true`).
  - `TextColor` — цвет текста описания (по умолчанию черный).
  - `FontSize` — размер шрифта текста (по умолчанию 8, минимум 7).
- **Особенности**:
  - Использует `FixedProfileRequest` для запроса профиля за указанный период.
  - Отрисовка выполняется над ценовым графиком (`DrawAbovePrice = true`) с подпиской на `DrawingLayouts.LatestBar`.
  - Линия рисуется от середины графика до правого края.
  - Текст описания периода выравнивается по правому краю с помощью `RenderStringFormat`.
  - Запрос профиля выполняется в `OnCalculate` для последнего бара, а результат обрабатывается в `OnFixedProfilesResponse`.

## 8. Работа с рыночными данными

Платформа ATAS предоставляет классы и интерфейсы для работы с рыночными данными, такими как глубина рынка, профили и информация об инструменте.

### 8.1. Класс `MarketDepthInfoProvider`

Класс `MarketDepthInfoProvider` (`MarketDepthInfoProvider.cs`) реализует интерфейс `IMarketDepthInfoProvider` и предоставляет доступ к данным глубины рынка (стакана заявок).

#### Основные методы:

- **GetSnapshot(MarketDepthSnapshotRequest request)** — возвращает снимок глубины рынка для указанного временного диапазона.
- **GetCurrentDepth()** — возвращает текущую глубину рынка.

#### Использование:

Используется для получения данных о Bid/Ask уровнях, которые могут быть отображены в индикаторах, таких как `ClusterStatisticSample`.

### 8.2. Класс `MarketDepthSnapshot`

Класс `MarketDepthSnapshot` (`MarketDepthSnapshot.cs`) представляет снимок глубины рынка за определенный период.

#### Основные свойства:

- **Time** — время снимка.
- **Bids** — список уровней спроса (Bid).
- **Asks** — список уровней предложения (Ask).

### 8.3. Класс `MarketDepthSnapshotRequest`

Класс `MarketDepthSnapshotRequest` (`MarketDepthSnapshotRequest.cs`) используется для запроса снимка глубины рынка.

#### Основные свойства:

- **StartTime**, **EndTime** — временной диапазон для снимка.
- **Levels** — количество уровней глубины.

### 8.4. Класс `PriceVolumeInfo`

Класс `PriceVolumeInfo` (`PriceVolumeInfo.cs`) представляет информацию об объемах на определенной цене.

#### Основные свойства:

- **Price** — цена уровня.
- **Volume** — объем на данном уровне.
- **BidVolume**, **AskVolume** — объемы спроса и предложения.
- **Delta** — дельта (разница между Bid и Ask).

#### Использование:

Используется в индикаторах, таких как `MaxLevels`, для получения максимальных уровней (`MaxVolumePriceInfo`, `MaxBidPriceInfo` и др.) из объекта `IndicatorCandle`.

### 8.5. Класс `FixedProfileRequest`

Класс `FixedProfileRequest` (`PriceVolumeInfo.cs`) используется для запроса фиксированного профиля за определенный период.

#### Основные свойства:

- **Period** — период профиля (`FixedProfilePeriods`).

#### Метод:

- **GetFixedProfile(FixedProfileRequest request)** — инициирует запрос профиля, результат обрабатывается в `OnFixedProfilesResponse`.

### 8.6. Класс `ValueArea`

Класс `ValueArea` (`PriceVolumeInfo.cs`) представляет информацию о зоне стоимости (Value Area).

#### Основные свойства:

- **High** — верхняя граница зоны стоимости.
- **Low** — нижняя граница зоны стоимости.

### 8.7. Интерфейс `IKnowFixedProfiles`

Интерфейс `IKnowFixedProfiles` (`PriceVolumeInfo.cs`) используется для объектов, которые могут запрашивать фиксированные профили.

#### Основные методы:

- **GetFixedProfile(FixedProfileRequest request)** — запрашивает профиль.
- **OnFixedProfilesResponse(IndicatorCandle fixedProfile, FixedProfilePeriods period)** — обрабатывает ответ с профилем.







# Методичка по созданию индикаторов для платформы ATAS (.NET 8) — Часть 4



## 10. Рекомендации по настройке и оптимизации индикаторов

Создание эффективных индикаторов требует внимания к настройке пользовательского интерфейса, оптимизации производительности и обеспечению стабильности. Ниже приведены рекомендации, основанные на предоставленных данных.

### 10.1. Настройка пользовательского интерфейса

- **Использование атрибутов** `Display` **и** `Range`:

  - Атрибут `Display` позволяет задавать отображаемое имя, группу и порядок свойств в интерфейсе настроек. Пример:

    ```csharp
    [Display(Name = "Show Text", GroupName = "Settings", Order = 10)]
    public bool ShowText { get; set; } = true;
    ```

  - Атрибут `Range` ограничивает допустимые значения для числовых свойств:

    ```csharp
    [Display(Name = "Font Size", GroupName = "Settings", Order = 20)]
    [Range(8, 20)]
    public int FontSize { get; set; } = 10;
    ```

  - Группируйте свойства по логическим категориям (например, "Settings", "FirstLine", "SecondLine") для удобства пользователей.

- **Поддержка редакторов свойств**:

  - Для сложных типов, таких как шрифты или цвета, используйте специализированные редакторы. Пример для шрифта:

    ```csharp
    [Display(Name = "Font", GroupName = "FirstLine", Order = 70)]
    [Editor(typeof(FontEditor), typeof(FontEditor))]
    public FontSetting Font { get; set; } = new FontSetting { Size = 60, Bold = true };
    ```

  - Это улучшает пользовательский опыт при настройке индикатора.

- **Динамическое обновление**:

  - Подписывайтесь на события изменения свойств, чтобы обновлять график при необходимости:

    ```csharp
    Font.PropertyChanged += (a, b) => RedrawChart();
    ```

  - Используйте метод `RecalculateValues()` для пересчета индикатора при изменении настроек:

    ```csharp
    public bool ShowText
    {
        get => _showText;
        set
        {
            _showText = value;
            RecalculateValues();
        }
    }
    ```

### 10.2. Оптимизация производительности

- **Минимизация расчетов в** `OnCalculate`:

  - Выполняйте только необходимые вычисления для текущего бара. Например, в `MaxLevels` запрос профиля выполняется только для последнего бара:

    ```csharp
    if (!_candleRequested && bar == CurrentBar - 1)
    {
        _candleRequested = true;
        GetFixedProfile(new FixedProfileRequest(Period));
    }
    ```

  - Храните промежуточные результаты в полях класса, чтобы избежать повторных вычислений.

- **Эффективная отрисовка в** `OnRender`:

  - Ограничивайте цикл отрисовки видимыми барами (`FirstVisibleBarNumber` до `LastVisibleBarNumber`):

    ```csharp
    for (int bar = FirstVisibleBarNumber; bar <= LastVisibleBarNumber; bar++)
    ```

  - Проверяйте условия перед отрисовкой, чтобы избежать лишних операций:

    ```csharp
    if (!_showText)
        return;
    ```

  - Кэшируйте объекты, такие как `RenderFont` или `RenderPen`, вместо создания новых на каждом вызове:

    ```csharp
    private RenderFont _font = new RenderFont("Arial", 8);
    ```

- **Работа с данными**:

  - Используйте подходящие типы серий данных (`ValueDataSeries`, `CandleDataSeries` и др.) для минимизации памяти.

  - Скрывайте неиспользуемые серии данных:

    ```csharp
    DataSeries[0].IsHidden = true;
    ```

- **Управление запросами данных**:

  - Запрашивайте рыночные данные, такие как профили или глубину рынка, только при необходимости. Например, в `MaxLevels` запрос профиля выполняется один раз для последнего бара.
  - Обрабатывайте ответы асинхронно через методы, такие как `OnFixedProfilesResponse`.

### 10.3. Обеспечение стабильности

- **Обработка исключений**:

  - Проверяйте входные данные и состояния перед выполнением операций:

    ```csharp
    if (_candle == null)
        return;
    ```

  - Используйте конструкции `switch` с `default` для обработки неизвестных значений:

    ```csharp
    switch (TextLocation)
    {
        case Location.Center: /* ... */
        default: throw new ArgumentOutOfRangeException();
    }
    ```

- **Управление ресурсами**:

  - Избегайте создания множества объектов в циклах. Например, создавайте `Rectangle` один раз и обновляйте его свойства:

    ```csharp
    rectangle.Y += rowHeight;
    ```

- **Тестирование**:

  - Проверяйте индикатор на разных таймфреймах, инструментах и масштабах графика.
  - Убедитесь, что индикатор корректно обрабатывает случаи, когда данные недоступны (например, отсутствие профиля в `MaxLevels`).

## 11. Обзор оставшихся классов и интерфейсов

### 11.1. Класс `InstrumentInfo`

Класс `InstrumentInfo` (`InstrumentInfo.cs`) реализует интерфейс `IInstrumentInfo` и хранит информацию о торговом инструменте.

#### Основные свойства:

- **Instrument** — название инструмента (например, "ES" для фьючерса S&P 500).
- **TickSize** — минимальный шаг цены.
- **Description** — описание инструмента.
- **TradingHours** — часы торгов.

#### Использование:

Используется в индикаторах, таких как `Watermark`, для отображения имени инструмента:

```csharp
if (ShowInstrument)
    firstLine = InstrumentInfo.Instrument;
```

### 11.2. Класс `ChartObject`

Класс `ChartObject` (`ChartObject.cs`) является базовым для графических объектов на графике, таких как линии, уровни и метки.

#### Основные свойства:

- **X**, **Y** — координаты объекта.
- **Width**, **Height** — размеры объекта.
- **Color** — цвет объекта.

#### Использование:

Может использоваться для создания пользовательских графических элементов, таких как линии тренда или уровни.

### 11.3. Класс `TrendLine`

Класс `TrendLine` (`TrendLine.cs`) используется для создания линий тренда на графике.

#### Основные свойства:

- **StartPoint**, **EndPoint** — начальная и конечная точки линии.
- **Color** — цвет линии (по умолчанию из `DefaultColors`).
- **Width** — толщина линии.

#### Использование:

Может быть интегрирован в индикаторы для отображения трендовых линий на основе расчетных данных.

### 11.4. Класс `Candle`

Класс `Candle` (`Candle.cs`) представляет свечу на графике с базовыми ценовыми данными.

#### Основные свойства:

- **Open** — цена открытия.
- **High** — максимальная цена.
- **Low** — минимальная цена.
- **Close** — цена закрытия.
- **Volume** — объем торгов.

#### Использование:

Используется в сериях данных `CandleDataSeries` и как основа для `IndicatorCandle`.

### 11.5. Класс `CumulativeTrade`

Класс `CumulativeTrade` (`CumulativeTrade.cs`) представляет накопительную сделку, объединяющую несколько исполнений.

#### Основные свойства:

- **Price** — цена сделки.
- **Volume** — объем сделки.
- **Direction** — направление сделки (`Buy`, `Sell`, `Between`).

### 11.6. Класс `CumulativeTradesRequest`

Класс `CumulativeTradesRequest` (`CumulativeTradesRequest.cs`) используется для запроса данных накопительных сделок.

#### Основные свойства:

- **StartDate**, **EndDate** — временной диапазон.
- **Instrument** — инструмент для запроса.

### 11.7. Класс `CustomValue`

Класс `CustomValue` (`CustomValue.cs`) представляет пользовательское значение с дополнительными свойствами.

#### Основные свойства:

- **Value** — основное значение.
- **Label** — метка для значения.
- **Properties** — дополнительные параметры.

#### Использование:

Используется в `CustomValueDataSeries` для хранения сложных данных.

### 11.8. Интерфейсы для контейнеров и графиков

- **IChart** (`IIndicatorContainer.cs`):
  - Предоставляет методы для взаимодействия с графиком (масштабирование, прокрутка).
- **IIndicatorContainer** (`IIndicatorContainer.cs`):
  - Хранит данные и методы для работы с индикаторами.
- **IChartCoordinatesManager** (`IIndicatorContainer.cs`):
  - Управляет координатами и масштабированием графика.
- **IContainer** (`IIndicatorContainer.cs`):
  - Определяет прямоугольную область на графике.
- **IChartColorsStore** (`IIndicatorContainer.cs`):
  - Хранит цвета и перья для отрисовки.

### 11.9. Интерфейсы для данных и торговли

- **IOnlineDataProvider** (`IOnlineDataProvider.cs`):
  - Предоставляет доступ к реальным рыночным данным.
- **ITradesCache** (`IOnlineDataProvider.cs`):
  - Хранит кеш сделок.
- **IMarketByOrdersDataProvider** (`IOnlineDataProvider.cs`):
  - Предоставляет данные о рыночных заказах.
- **ITradingManager** (`ITradingManager.cs`):
  - Управляет торговыми операциями (отправка ордеров, управление позициями).

### 11.10. Класс `NotifyPropertyChangedBase`

Класс `NotifyPropertyChangedBase` (`NotifyPropertyChangedBase.cs`) реализует интерфейс `INotifyPropertyChanged` для уведомления об изменении свойств.

#### Использование:

Используется в классах, таких как фильтры и настройки, для динамического обновления интерфейса.

## 12. Дополнительные перечисления

### 12.1. `ObjectType`

Перечисление `ObjectType` (`ObjectType.cs`) определяет типы графических объектов для `PriceSelectionDataSeries`:

- `Ellipse` — эллипс.
- `Triangle` — треугольник.
- `Rectangle` — прямоугольник.
- `Diamond` — ромб.
- `OnlyCluster` — только кластер.

#### Использование:

Применяется для настройки визуализации ценовых уровней в кластерах.

### 12.2. `DrawingLayouts`

Перечисление `DrawingLayouts` (`DrawingLayouts.cs`) определяет режимы размещения графических объектов:

- `None` — отключено.
- `Historical` — отображение исторических данных.
- `LatestBar` — отображение последней свечи.
- `Final` — финальный режим (например, закрытие позиции).

#### Использование:

Используется в методе `SubscribeToDrawingEvents` для выбора режима отрисовки:

```csharp
SubscribeToDrawingEvents(DrawingLayouts.LatestBar);
```










# Методичка по созданию индикаторов для платформы ATAS (.NET 8) — Часть 5



## 14. Практические советы по отладке и тестированию индикаторов

Отладка и тестирование индикаторов критически важны для обеспечения их корректной работы на платформе ATAS. Ниже приведены рекомендации, основанные на предоставленных данных и общих принципах разработки.

### 14.1. Отладка индикаторов

- **Логирование**:

  - Используйте временное логирование для отслеживания значений переменных и хода выполнения. Например, добавьте `Console.WriteLine` в метод `OnCalculate`:

    ```csharp
    protected override void OnCalculate(int bar, decimal value)
    {
        var candle = GetCandle(bar);
        Console.WriteLine($"Bar {bar}: Volume = {candle.Volume}");
        _volumeSeries[bar] = candle.Volume;
    }
    ```

  - Удаляйте логирование перед релизом, чтобы не снижать производительность.

- **Проверка данных**:

  - Убедитесь, что входные данные (например, свечи или профили) доступны и корректны:

    ```csharp
    if (_candle == null)
    {
        Console.WriteLine("Profile data not available");
        return;
    }
    ```

  - Проверяйте индексы баров, чтобы избежать выхода за пределы массива:

    ```csharp
    if (bar >= CurrentBar)
        return;
    ```

- **Тестирование настроек**:

  - Проверяйте поведение индикатора при изменении параметров через интерфейс. Например, для `FontSize` в `VolumeOnChart`:

    ```csharp
    public int FontSize
    {
        get => _fontSize;
        set
        {
            _fontSize = value;
            Console.WriteLine($"FontSize changed to {value}");
            RecalculateValues();
        }
    }
    ```

- **Обработка исключений**:

  - Добавляйте обработку исключений в критических местах, таких как запросы данных или отрисовка:

    ```csharp
    try
    {
        var priceInfo = GetPriceVolumeInfo(_candle, Type);
        if (priceInfo == null)
            return;
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error in OnRender: {ex.Message}");
    }
    ```

### 14.2. Тестирование индикаторов

- **Разные таймфреймы**:

  - Тестируйте индикатор на различных таймфреймах (1 минута, 5 минут, дневной). Например, проверьте, как `ClusterStatisticSample` отображает данные на минутном и дневном графиках.

- **Разные инструменты**:

  - Проверяйте индикатор на разных инструментах (фьючерсы, акции, криптовалюты), используя `InstrumentInfo.Instrument` для адаптации:

    ```csharp
    firstLine = InstrumentInfo.Instrument;
    ```

- **Крайние случаи**:

  - Тестируйте индикатор при отсутствии данных (например, пустой профиль в `MaxLevels`):

    ```csharp
    if (_candle == null)
        return;
    ```

  - Проверяйте поведение при экстремальных значениях параметров (например, минимальный/максимальный `FontSize`).

- **Визуальная проверка**:

  - Убедитесь, что отрисовка корректна при масштабировании графика. Например, в `Watermark` проверьте положение текста при изменении `TextLocation` и масштаба.

- **Производительность**:

  - Замеряйте время выполнения `OnCalculate` и `OnRender` на больших исторических данных. Оптимизируйте циклы и запросы данных, как описано в разделе оптимизации.

### 14.3. Инструменты для тестирования

- **ATAS SDK**:

  - Используйте встроенные инструменты ATAS для тестирования индикаторов, такие как окно предварительного просмотра графика.

- **Unit-тесты**:

  - Создавайте unit-тесты для проверки логики расчетов. Например, тестируйте метод `GetPriceVolumeInfo` в `MaxLevels`:

    ```csharp
    private PriceVolumeInfo GetPriceVolumeInfo(IndicatorCandle candle, MaxLevelType levelType)
    {
        // Логика метода
    }
    ```

- **Мониторинг ошибок**:

  - Настройте логирование ошибок в файл или консоль для анализа проблем в продакшене.

## 15. Обзор оставшихся классов и интерфейсов

### 15.1. Класс `LineSeries`

Класс `LineSeries` (`LineSeries.cs`) представляет серию данных для отображения линий на графике.

#### Основные свойства:

- **Color** — цвет линии (`System.Windows.Media.Color`).
- **Width** — толщина линии.
- **Name** — имя серии для отображения в легенде.

#### Использование:

Используется в индикаторах, таких как `CurrentPrice`, для отображения горизонтальной линии текущей цены:

```csharp
private readonly LineSeries _currentPrice = new("CurrentPrice", "Current Price")
{
    Color = DefaultColors.Blue.Convert(),
    Width = 2
};
```

### 15.2. Класс `RedrawArg`

Класс `RedrawArg` (`RedrawArg.cs`) используется для передачи параметров при вызове перерисовки графика.

#### Основные свойства:

- **Reason** — причина перерисовки (например, изменение данных или масштаба).
- **Layout** — режим отрисовки (`DrawingLayouts`).

#### Использование:

Применяется в методе `RedrawChart` для инициирования перерисовки:

```csharp
RedrawChart();
```

### 15.3. Класс `FontSetting`

Класс `FontSetting` (`FontSetting.cs`) представляет настройки шрифта для отрисовки текста.

#### Основные свойства:

- **Size** — размер шрифта.
- **Bold** — жирный шрифт (`true`/`false`).
- **FontFamily** — семейство шрифта (например, "Arial").

#### Использование:

Используется в индикаторах, таких как `Watermark`, для настройки шрифта:

```csharp
public FontSetting Font { get; set; } = new FontSetting { Size = 60, Bold = true };
```

### 15.4. Класс `RenderFont`

Класс `RenderFont` (`RenderFont.cs`) используется для создания шрифта в контексте отрисовки.

#### Основные свойства:

- **FamilyName** — имя семейства шрифта (например, "Arial").
- **Size** — размер шрифта.

#### Использование:

Применяется в методе `OnRender` для отрисовки текста:

```csharp
var font = new RenderFont("Arial", _fontSize);
context.DrawString(text, font, _textColor.Convert(), textRect);
```

### 15.5. Класс `RenderStringFormat`

Класс `RenderStringFormat` (`RenderStringFormat.cs`) задает форматирование текста при отрисовке.

#### Основные свойства:

- **Alignment** — горизонтальное выравнивание (`StringAlignment.Near`, `Far`, `Center`).
- **LineAlignment** — вертикальное выравнивание.
- **Trimming** — обрезка текста (`StringTrimming.EllipsisCharacter`).
- **FormatFlags** — флаги форматирования (`StringFormatFlags.NoWrap`).

#### Использование:

Используется в `MaxLevels` для выравнивания текста по правому краю:

```csharp
private RenderStringFormat _stringRightFormat = new RenderStringFormat
{
    Alignment = StringAlignment.Far,
    LineAlignment = StringAlignment.Center,
    Trimming = StringTrimming.EllipsisCharacter,
    FormatFlags = StringFormatFlags.NoWrap
};
```

### 15.6. Класс `PriceSelectionValue`

Класс `PriceSelectionValue` (`PriceSelectionValue.cs`) представляет значение выбора цены в кластерах.

#### Основные свойства:

- **Price** — выбранная цена.
- **Type** — тип выбора (`SelectionType.Full`, `Bid`, `Ask`).
- **ObjectType** — тип объекта для визуализации (`Ellipse`, `Triangle`, и др.).

#### Использование:

Используется в `PriceSelectionDataSeries` для хранения данных о выбранных уровнях.

### 15.7. Класс `RangeValue`

Класс `RangeValue` (`RangeValue.cs`) представляет диапазон значений.

#### Основные свойства:

- **Min** — минимальное значение.
- **Max** — максимальное значение.

#### Использование:

Используется в `RangeDataSeries` для хранения диапазонов, таких как максимум и минимум цен.

### 15.8. Класс `MouseButtons`

Класс `MouseButtons` (`MouseButtons.cs`) определяет кнопки мыши для обработки событий.

#### Основные значения:

- `Left` — левая кнопка.
- `Right` — правая кнопка.
- `Middle` — средняя кнопка.

#### Использование:

Может использоваться для обработки пользовательских взаимодействий с графиком.

## 16. Заключительные рекомендации по созданию сложных индикаторов

Создание сложных индикаторов требует интеграции множества компонентов платформы ATAS. Ниже приведены финальные рекомендации, основанные на предоставленных данных.

### 16.1. Проектирование индикатора

- **Определите цель**:
  - Четко сформулируйте, какие данные и как должен отображать индикатор (например, объемы в `VolumeOnChart`, максимальные уровни в `MaxLevels`).
- **Выберите подходящие классы**:
  - Используйте `ValueDataSeries` для простых числовых данных, `CandleDataSeries` для свечей, `PriceSelectionDataSeries` для кластеров.
  - Для сложных данных создавайте `CustomValue` или `ObjectDataSeries`.
- **Планируйте пользовательский интерфейс**:
  - Определите параметры, которые пользователь сможет настраивать, и используйте фильтры (`FilterBool`, `FilterColor`) для удобства.

### 16.2. Интеграция с данными

- **Работа с рыночными данными**:

  - Используйте `MarketDepthInfoProvider` для доступа к глубине рынка, как в `ClusterStatisticSample`.
  - Запрашивайте профили через `FixedProfileRequest` для анализа исторических данных, как в `MaxLevels`.

- **Обработка асинхронных данных**:

  - Реализуйте методы, такие как `OnFixedProfilesResponse`, для обработки ответов на запросы:

    ```csharp
    protected override void OnFixedProfilesResponse(IndicatorCandle fixedProfile, FixedProfilePeriods period)
    {
        _candle = fixedProfile;
        RedrawChart();
    }
    ```

### 16.3. Визуализация

- **Пользовательская отрисовка**:
  - Используйте `OnRender` для сложной графики, как в `Watermark` или `ClusterStatisticSample`.
  - Подписывайтесь на подходящий режим отрисовки (`DrawingLayouts.Historical` или `LatestBar`) в зависимости от задачи.
- **Оптимизация графики**:
  - Кэшируйте объекты `RenderPen`, `RenderFont` и `RenderStringFormat` для повышения производительности.
  - Ограничивайте отрисовку видимыми барами и проверяйте условия перед рисованием.

### 16.4. Тестирование и поддержка

- **Модульное тестирование**:
  - Разделяйте логику на методы, чтобы упростить тестирование. Например, метод `GetPriceVolumeInfo` в `MaxLevels` можно тестировать отдельно.
- **Документирование**:
  - Добавляйте комментарии к коду и описания в атрибутах `DisplayName` для упрощения поддержки.
- **Обратная связь**:
  - Собирайте отзывы пользователей о работе индикатора, чтобы улучшать настройки и функциональность.



