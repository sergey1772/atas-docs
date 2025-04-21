Полная методичка по разработке торговых стратегий в ATAS Эта методичка предназначена для создания автоматизированных торговых стратегий в платформе ATAS с использованием пространства имён ATAS.Strategies. Она охватывает настройку проектов, управление ордерами, работу с данными индикаторов, асинхронные методы, визуализацию и управление рисками. Методичка подходит для пользователей с торговым опытом, но без навыков программирования, предоставляя чёткие инструкции и примеры для реализации идей через технические задания.

1.  Основы разработки стратегий 1.1. Настройка проекта

Создайте проект в Visual Studio (тип: Class Library).

Добавьте ссылку на ATAS.Strategies.dll и ATAS.Indicators.dll из директории установки ATAS (bin или версия).

Создайте класс стратегии, унаследовав его от ChartStrategy: using ATAS.Strategies.Chart; public class CustomStrategy : ChartStrategy { protected override void OnCalculate(int bar, decimal value) { } }

Пояснение: ChartStrategy наследуется от Indicator, обеспечивая доступ к данным индикаторов и торговым функциям.

Реализуйте логику в методах OnCalculate, OnOrderChanged, и других.

Скомпилируйте проект в DLL.

Поместите DLL в Documents/ATAS/Strategies.

Обновите список стратегий в ATAS через мигающую кнопку.

1.2. Структура стратегии Стратегия состоит из:

Свойств: Security, Portfolio, Connector, CurrentPosition, AveragePrice, State, MyTrades, Orders. Публичных методов: OpenOrder, ModifyOrder, CancelOrder, ShrinkPrice, RaiseShowNotification. Виртуальных методов: OnStarted, OnSuspended, OnStopping, OnStopped, OnCurrentPositionChanged, CanProcess.

Пример базовой структуры: public class CustomStrategy : ChartStrategy { protected override void OnCalculate(int bar, decimal value) { // Логика расчёта } protected override void OnStarted() { // Действия при запуске } protected override void OnStopping() { // Закрытие позиций и ордеров } }

1.3. Состояния стратегии (StrategyStates) Стратегия может находиться в одном из состояний:

Stopped: Остановлена, не обрабатывает сделки. Started: Запущена, активно торгует. Suspended: Приостановлена (например, график закрыт). Error: Ошибка, ожидает исправления. Watch: Наблюдает за позицией.

Проверка состояния: if (State == StrategyStates.Started) { // Выполнять торговые операции }

1.4. Управление позициями

Внутренняя позиция: Хранится в CurrentPosition и AveragePrice. Общая позиция: Доступна через TradingManager.Position. Сброс позиции: При остановке стратегии внутренняя позиция сбрасывается.

Рекомендация: Переопределите OnStopping для закрытия позиций: protected override void OnStopping() { if (CurrentPosition != 0) CloseCurrentPosition(); base.OnStopping(); }

2.  Управление ордерами 2.1. Основные методы

OpenOrder: Открытие нового ордера. ModifyOrder: Изменение существующего ордера. CancelOrder: Отмена ордера. ShrinkPrice: Округление цены до размера тика. RaiseShowNotification: Отображение уведомления.

2.2. Синхронные методы 2.2.1. Размещение ордера Создайте и отправьте ордер: var order = new Order { Portfolio = Portfolio, Security = Security, Direction = OrderDirections.Buy, Type = OrderTypes.Market, QuantityToFill = 1 }; OpenOrder(order);

Пояснение: OpenOrder отправляет ордер на биржу. Отслеживайте результат в OnOrderChanged и OnOrderRegisterFailed. Пример обработки: protected override void OnOrderChanged(Order order) { switch (order.Status()) { case OrderStatus.Placed: LogInfo("Order placed"); break; case OrderStatus.Filled: LogInfo("Order filled"); break; case OrderStatus.Canceled: LogInfo("Order canceled"); break; } } protected override void OnOrderRegisterFailed(Order order, string message) { LogError($"Order failed: {message}"); }

2.2.2. Изменение ордера Создайте копию ордера и измените параметры: var newOrder = order.Clone(); newOrder.Price = 200; ModifyOrder(order, newOrder);

Пояснение: Отслеживайте результат в OnOrderChanged и OnOrderModifyFailed. 2.2.3. Отмена ордера Проверьте состояние и отмените: if (order.State == OrderStates.Active) CancelOrder(order);

Пояснение: Отслеживайте результат в OnOrderChanged и OnOrderCancelFailed. 2.3. Асинхронные методы Асинхронные методы (OpenOrderAsync, ModifyOrderAsync, CancelOrderAsync) упрощают обработку результатов. 2.3.1. Размещение ордера private async Task OpenPositionAsync(Order order) { try { await OpenOrderAsync(order); LogInfo($"Order placed: {order.Status()}"); } catch (Exception ex) { LogError($"Open failed: {ex.Message}"); } }

Пояснение: Используйте try-catch для обработки ошибок. 2.3.2. Изменение ордера private async Task ChangeOrderAsync(Order order) { var newOrder = order.Clone(); newOrder.Price = 200; try { await ModifyOrderAsync(order, newOrder); LogInfo("Order modified"); } catch (Exception ex) { LogError($"Modify failed: {ex.Message}"); } }

2.3.3. Отмена ордера private async Task TryCancelOrderAsync(Order order) { try { await CancelOrderAsync(order); LogInfo("Order canceled"); } catch (Exception ex) { LogError($"Cancel failed: {ex.Message}"); } }

2.4. Дополнительные опции ордеров Используйте TradingManager для настройки:

ReduceOnly: Только уменьшает позицию. private void TrySetReduceOnly(Order order) { var flags = TradingManager?.GetSecurityTradingOptions()?.CreateExtendedOptions(order.Type); if (flags is IOrderOptionReduceOnly ro) { ro.ReduceOnly = true; order.ExtendedOptions = flags; } }

PostOnly: Только мейкерский ордер. private void TrySetPostOnly(Order order) { var flags = TradingManager?.GetSecurityTradingOptions()?.CreateExtendedOptions(order.Type); if (flags is IOrderOptionPostOnly po) { po.PostOnly = true; order.ExtendedOptions = flags; } }

CloseOnTrigger: Закрытие при триггере. private void TrySetCloseOnTrigger(Order order) { var flags = TradingManager?.GetSecurityTradingOptions()?.CreateExtendedOptions(order.Type); if (flags is IOrderOptionCloseOnTrigger ct) { ct.CloseOnTrigger = true; order.ExtendedOptions = flags; } }

2.5. Типы ордеров

Market: Рыночный ордер. Limit: Лимитный ордер. Stop: Стоп-ордер. StopLimit: Стоп-лимит ордер.

Пример стоп-лимит ордера: var order = new Order { Portfolio = Portfolio, Security = Security, Direction = OrderDirections.Sell, Type = OrderTypes.StopLimit, TriggerPrice = 100, Price = 95, QuantityToFill = 1 }; OpenOrder(order);

3.  Работа с индикаторами Стратегии наследуют функциональность индикаторов, позволяя использовать DataSeries и методы OnCalculate. 3.1. Добавление индикаторов private readonly SMA \_shortSma = new SMA(); private readonly SMA \_longSma = new SMA();

public CustomStrategy() { \_shortSma.Period = 21; \_longSma.Period = 75; }

3.2. Визуализация Настройте DataSeries для отображения: public CustomStrategy() { var shortSeries = (ValueDataSeries)DataSeries\[0\]; shortSeries.Name = "Short SMA"; shortSeries.Color = Colors.Red; shortSeries.VisualType = VisualMode.Line;

    DataSeries.Add(new ValueDataSeries("Long SMA")
    {
        VisualType = VisualMode.Line,
        Color = Colors.Green
    });
    

}

3.3. Расчёты Обрабатывайте данные в OnCalculate: protected override void OnCalculate(int bar, decimal value) { var shortSma = \_shortSma.Calculate(bar, value); var longSma = \_longSma.Calculate(bar, value); DataSeries\[0\]\[bar\] = shortSma; DataSeries\[1\]\[bar\] = longSma; }

4.  Управление рисками (ATM) Пространство имён ATAS.Strategies.ATM предоставляет инструменты для управления стоп-лоссами и тейк-профитами. 4.1. Классы и интерфейсы

ATMStrategy: Базовый класс для ATM-стратегий. BaseStopProfitStrategy: Для стратегий со стоп-лоссами и тейк-профитами. IStopProfitStrategy: Интерфейс для таких стратегий. StopProfit: Класс для стоп-лосса/тейк-профита.

4.2. Настройка стоп-лосса public class StopLossStrategy : BaseStopProfitStrategy { protected override void OnCalculate(int bar, decimal value) { var settings = new SimpleStopProfitSettings( new StopProfitSettings { Value = 50, ValueType = StopTakeValueTypes.Ticks }, new StopProfitSettings { Value = 100, ValueType = StopTakeValueTypes.Ticks }, null, null); ApplyStopProfitSettings(settings); } }

5.  Атрибуты и параметры Добавляйте настраиваемые параметры с атрибутами: \[Display(Name = "Period", GroupName = "Settings", Order = 10)\] \[Parameter\] public int Period { get => \_sma.Period; set { \_sma.Period = Math.Max(1, value); RecalculateValues(); } }

Пояснение:

Display: Задаёт имя, группу и порядок в интерфейсе. Parameter: Делает свойство настраиваемым.

6.  Уведомления и логирование

Уведомления: Используйте RaiseShowNotification: RaiseShowNotification("Position opened", "Trade Alert", false);

Логирование: Используйте LogInfo, LogError: LogInfo($"Position: {CurrentPosition}");

7.  Пример стратегии: SMA Crossover Стратегия открывает позиции при пересечении скользящих средних. using ATAS.Strategies.Chart; using ATAS.Indicators.Technical; using System.ComponentModel.DataAnnotations; using System.Drawing;

public class SmaChartStrategy : ChartStrategy { private readonly SMA \_shortSma = new SMA(); private readonly SMA \_longSma = new SMA(); private int \_lastBar;

    [Display(Name = "Short Period", Order = 10)]
    [Parameter]
    public int ShortPeriod
    {
        get => _shortSma.Period;
        set
        {
            _shortSma.Period = Math.Max(1, value);
            RecalculateValues();
        }
    }
    
    [Display(Name = "Long Period", Order = 20)]
    [Parameter]
    public int LongPeriod
    {
        get => _longSma.Period;
        set
        {
            _longSma.Period = Math.Max(1, value);
            RecalculateValues();
        }
    }
    
    [Display(Name = "Volume", Order = 30)]
    [Parameter]
    public decimal Volume { get; set; }
    
    [Display(Name = "Close Position on Stopping", Order = 40)]
    [Parameter]
    public bool ClosePositionOnStopping { get; set; }
    
    public SmaChartStrategy()
    {
        var firstSeries = (ValueDataSeries)DataSeries[0];
        firstSeries.Name = "Short";
        firstSeries.Color = Colors.Red;
        firstSeries.VisualType = VisualMode.Line;
    
        DataSeries.Add(new ValueDataSeries("Long")
        {
            VisualType = VisualMode.Line,
            Color = Colors.Green
        });
    
        ShortPeriod = 21;
        LongPeriod = 75;
        Volume = 1;
        ClosePositionOnStopping = true;
    }
    
    protected override void OnCalculate(int bar, decimal value)
    {
        var shortSma = _shortSma.Calculate(bar, value);
        var longSma = _longSma.Calculate(bar, value);
    
        DataSeries[0][bar] = shortSma;
        DataSeries[1][bar] = longSma;
    
        var prevBar = _lastBar;
        _lastBar = bar;
    
        if (!CanProcess(bar) || prevBar == bar)
            return;
    
        if (_shortSma[prevBar - 1] < _longSma[prevBar - 1] && _shortSma[prevBar] >= _longSma[prevBar])
            OpenPosition(OrderDirections.Buy);
    
        if (_shortSma[prevBar - 1] > _longSma[prevBar - 1] && _shortSma[prevBar] <= _longSma[prevBar])
            OpenPosition(OrderDirections.Sell);
    }
    
    protected override void OnStopping()
    {
        if (CurrentPosition != 0 && ClosePositionOnStopping)
        {
            RaiseShowNotification($"Closing position {CurrentPosition} on stopping.", level: NotificationLevel.Alert);
            CloseCurrentPosition();
        }
        base.OnStopping();
    }
    
    private void OpenPosition(OrderDirections direction)
    {
        var order = new Order
        {
            Portfolio = Portfolio,
            Security = Security,
            Direction = direction,
            Type = OrderTypes.Market,
            QuantityToFill = GetOrderVolume()
        };
        OpenOrder(order);
    }
    
    private void CloseCurrentPosition()
    {
        var order = new Order
        {
            Portfolio = Portfolio,
            Security = Security,
            Direction = CurrentPosition > 0 ? OrderDirections.Sell : OrderDirections.Buy,
            Type = OrderTypes.Market,
            QuantityToFill = Math.Abs(CurrentPosition)
        };
        OpenOrder(order);
    }
    
    private decimal GetOrderVolume()
    {
        if (CurrentPosition == 0)
            return Volume;
        if (CurrentPosition > 0)
            return Volume + CurrentPosition;
        return Volume + Math.Abs(CurrentPosition);
    }
    

}

8.  Оптимизация и обработка ошибок 8.1. Асинхронные проблемы

Дублирование ордеров: Проверяйте состояние перед повторной отправкой. if (State == StrategyStates.Started && !IsOrderPending) OpenOrder(order);

Позиции: Проверяйте CurrentPosition перед открытием. if (CurrentPosition == 0) OpenPosition(OrderDirections.Buy);

8.2. Обработка ошибок Используйте try-catch и логирование: try { await OpenOrderAsync(order); } catch (Exception ex) { LogError($"Error: {ex.Message}"); SetErrorState(StrategyErrorTypes.MaxNumberOfErrorsExceeded, new\[\] { ex.Message }); }

8.3. Типы ошибок (StrategyErrorTypes)

MaxNumberOfErrorsExceeded: Превышено количество ошибок. MaxNumberOfConnectionErrorsExceeded: Проблемы с соединением. PreviousOrdersNotFound: Ордера не найдены.

9.  Рекомендации для технических заданий Чтобы создать стратегию:

Опишите логику:

Условия входа/выхода (например, пересечение SMA). Типы ордеров (рыночные, лимитные).

Укажите параметры:

Настраиваемые значения (период, объём). Визуализация (линии, уведомления).

Задайте риски:

Стоп-лосс, тейк-профит. Опции (ReduceOnly, PostOnly).

Пример ТЗ:

Стратегия открывает покупку при пересечении 20-периодной SMA с 50-периодной снизу вверх. Объём: 1 лот. Стоп-лосс: 50 тиков, тейк-профит: 100 тиков. Закрывать позицию при остановке. Отображать SMA на графике (красная и зелёная линии).

10.  Полный список классов и перечислений 10.1. Классы

Strategy: Базовый класс для стратегий. ChartStrategy: Для стратегий на графике. ATMStrategy: Для автоматических стратегий. BaseStopProfitStrategy: Для стоп-лосса/тейк-профита. StrategyNotificationEventArgs: Данные для уведомлений. StrategyStateChangedEventArgs: Данные для изменения состояния.

10.2. Интерфейсы

IStrategy: Контракт для стратегий. IATMStrategy: Для ATM-стратегий. IStopProfitStrategy: Для стоп-лосса/тейк-профита.

10.3. Перечисления

StrategyStates: Stopped, Started, Suspended, Error, Watch. StrategyErrorTypes: MaxNumberOfErrorsExceeded, MaxNumberOfConnectionErrorsExceeded, PreviousOrdersNotFound. StopTakeValueTypes: Ticks, Percent. OrderTypes: Market, Limit, Stop, StopLimit. OrderDirections: Buy, Sell.
