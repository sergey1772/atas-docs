

---

# Методичка по созданию АТРИБУТОВ и продвинутых стратегий в ATAS (.NET 8)

*Эта методичка предназначена для разработчиков, работающих с платформой ATAS. В документе собраны подробные инструкции по созданию атрибутов, разработке стратегий, описаны классы, интерфейсы, перечисления, методы, свойства и файловая структура, характерная для ATAS. Язык программирования – .NET 8.*

---

## Содержание

1. [Методичка для создания АТРИБУТОВ в ATAS на основе PDF-файла](#1-методичка-для-создания-атрибутов-в-atas-на-основе-pdf-файла)
2. [Методичка по ATAS.Strategies.ATM Namespace Reference](#2-методичка-по-atasstrategiesatm-namespace-reference)
3. [Методичка по классу `ATAS.Strategies.ATM.ATMStrategy<TStrategy>`](#3-методичка-по-классу-atasstrategiesatmatmstrategytstrategy)
4. [Методичка по классу `ATAS.Strategies.ATM.BaseStopProfitStrategy<TStrategy, TSettings>`](#4-методичка-по-классу-atasstrategiesatm-basestopprofitstrategytstrategy-tsettings)
5. [Методичка по структуре `ATAS.Strategies.ATM.ChangesInfo`](#5-методичка-по-структуре-atasstrategiesatmchangesinfo)
6. [Методичка по интерфейсу `ATAS.Strategies.ATM.IATMStrategy`](#6-методичка-по-интерфейсу-atasstrategiesatmistatstrategy)
7. [Методичка по интерфейсу `ATAS.Strategies.ATM.ISimpleStopProfitStrategy`](#7-методичка-по-интерфейсу-atasstrategiesatmisimplestopprofitstrategy)
8. [Методичка по интерфейсу `ATAS.Strategies.ATM.IStopProfitSettings` (на основе PDF)](#8-методичка-по-интерфейсу-atasstrategiesatmistoppfsettings)
9. [Методичка по интерфейсу `ATAS.Strategies.ATM.IStopProfitStrategy<TSettings>`](#9-методичка-по-интерфейсу-atasstrategiesatmistoppfstrategytsettings)
10. [Методичка: Функция `GetLastPrice`](#10-методичка-функция-getlastprice)
11. [Методичка по интерфейсу `ATAS.Strategies.ATM.ISupportCustomStopOrTake`](#11-методичка-по-интерфейсу-atasstrategiesatmisupportcustomstoportake)
12. [Методичка по классу `ATAS.Strategies.ATM.StopProfit`](#12-методичка-по-классу-atasstrategiesatmstoppf)
13. [Методичка по ATAS.Strategies.Chart Namespace Reference](#13-методичка-по-atasstrategieschart-namespace-reference)
14. [Методичка по классу `ATAS.Strategies.Chart.ChartStrategy`](#14-методичка-по-классу-atasstrategieschartchartstrategy)
15. [Методичка по интерфейсу `ATAS.Strategies.Chart.IChartStrategy`](#15-методичка-по-интерфейсу-atasstrategieschartichartstrategy)
16. [Методичка по классу `ATAS.Strategies.Chart.SmaChartStrategy`](#16-методичка-по-классу-atasstrategieschartsmachartstrategy)
17. [Директории, файловая структура и зависимостями](#17-директории-файловая-структура-и-зависимости)
18. [Методичка для создания продвинутых стратегий (ProtectionSettingsEditor)](#18-методичка-для-создания-продвинутых-стратегий-protectionsettingseditor)
19. [Методичка по интерфейсу `ATAS.Strategies.IStrategy`](#19-методичка-по-интерфейсу-atasstrategiesistrategy)
20. [Методичка по классу `ATAS.Strategies.Strategy`](#20-методичка-по-классу-atasstrategiesstrategy)
21. [Методичка по классу `ATAS.Strategies.StrategyNotificationEventArgs`](#21-методичка-по-классу-atasstrategiesstrategynotificationeventargs)
22. [Методичка по классу `ATAS.Strategies.StrategyStateChangedEventArgs`](#22-методичка-по-классу-atasstrategiesstrategystatechangedeventargs)

---

## 1. Методичка для создания АТРИБУТОВ в ATAS на основе PDF-файла

### Стратегии

#### Разработка стратегии

Для разработки стратегии необходимо:

1. Разработать проект библиотеки классов в Visual Studio.
2. Добавить ссылку на `ATAS Strategies.dll`, которая находится в директории с установленной программой, в проекте.
3. Создать класс стратегии и унаследовать его от класса `ChartStrategy`.
4. Реализовать логику работы стратегии.
5. Скомпилировать библиотеку.
6. Поместить полученный `dll`-файл в директорию `\Documents\ATAS\Strategies`.
7. Нажать на мигающую кнопку для вступления изменений в списке стратегий в силу.

*Изображение интерфейса ATAS с выделенной кнопкой обновления списка стратегий*

#### Стратегии графика (Chart Strategies)

Функциональность стратегий графика позволяет получать и обрабатывать весь набор данных, доступный в индикаторах, и выполнять торговые операции на основе этих данных.

Класс `ChartStrategy` наследуется от класса `Indicator`. Таким образом, стратегии обладают полной функциональностью индикаторов.

В дополнение к функциональности индикаторов, стратегии имеют дополнительные свойства и методы, отвечающие за торговую функциональность:

**Свойства (Properties):**
- `Security` – торговый инструмент.
- `Portfolio` – выбранный портфель.
- `Connector` – торговое соединение.
- `MyTrades` – список сделок.
- `Orders` – список ордеров.
- `CurrentPosition` – объем текущей позиции стратегии.
- `AveragePrice` – средняя цена текущей позиции стратегии.
- `State` – состояние стратегии (может быть Stopped, Started и Suspended).

**Основные публичные методы стратегии:**
- `OpenOrder` – метод открытия нового ордера.
- `ModifyOrder` – метод изменения существующего ордера.
- `CancelOrder` – метод отмены ордера.
- `ShrinkPrice` – округление переданной цены до размера тика торгового инструмента.
- `RaiseShowNotification` – метод, позволяющий визуализировать уведомления в платформе.

**Виртуальные методы стратегии**, которые, при необходимости, должны быть переопределены:
- `OnStarted` – вызывается при запуске стратегии.
- `OnSuspended` – вызывается, когда стратегия приостановлена (например, график закрыт).
- `OnStopping` – вызывается перед остановкой стратегии.
- `OnStopped` – вызывается, когда стратегия остановлена.
- `OnCurrentPositionChanged` – вызывается при изменении текущей позиции.

**Позиции стратегии.**

Каждая стратегия хранит свою позицию отдельно. Общую позицию можно получить через `TradingManager.Position`, а внутреннюю – через свойства `CurrentPosition` и `AveragePrice`.

**Важно!** При остановке стратегии внутренняя позиция сбрасывается в ноль. Рекомендуется переопределить `OnStopping` для закрытия внутренних позиций и отмены ордеров.

Для выбора стратегии через меню:

1. Щёлкните правой кнопкой мыши.
2. Нажмите на иконку списка стратегий.
3. Выберите нужную стратегию в открывшемся окне.
4. Нажмите кнопку `Add`.
5. Для включения/отключения стратегии используйте соответствующую кнопку.

*Изображение окна "Chart strategies" с выделенными элементами управления стратегией*

---

### Дополнительные опции ордеров

При разработке стратегии можно установить определённые опции ордеров через свойство стратегии `TradingManager`.

#### Reduced only

Ордера **Reduced only** гарантируют, что при их исполнении позиция лишь уменьшится. Особенно важны для контроля риска.

```csharp
private void TrySetReduceOnly(Order order)
{
    // Объявляем переменную flags для хранения опций ордера.
    var flags = TradingManager?
        // Получаем интерфейс опций безопасности.
        .GetSecurityTradingOptions()?
        // Создаем расширенные опции ордера на основе типа ордера.
        .CreateExtendedOptions(order.Type);

    // Проверяем, является ли flags типом IOrderOptionReduceOnly.
    if (flags is not IOrderOptionReduceOnly ro)
        // Если нет, выходим из метода.
        return;

    // Устанавливаем ReduceOnly в true.
    ro.ReduceOnly = true;
    // Применяем опции к ордеру.
    order.ExtendedOptions = flags;
}
```

#### Post only

Ордер **Post only** гарантирует, что он будет исполнен как мейкерский (без уплаты торговых комиссий). Если ордер исполнится как тейкерский, он будет отменён.

```csharp
private void TrySetPostOnly(Order order)
{
    var flags = TradingManager?
        .GetSecurityTradingOptions()?
        .CreateExtendedOptions(order.Type);

    if (flags is not IOrderOptionPostOnly po)
        return;

    po.PostOnly = true;
    order.ExtendedOptions = flags;
}
```

#### Close on trigger

Ордер **Close on trigger** исполняется при наступлении заданного триггерного условия – часто используется для управления рисками.

```csharp
private void TrySetCloseOnTrigger(Order order)
{
    var flags = TradingManager?
        .GetSecurityTradingOptions()?
        .CreateExtendedOptions(order.Type);

    if (flags is not IOrderOptionCloseOnTrigger ct)
        return;

    ct.CloseOnTrigger = true;
    order.ExtendedOptions = flags;
}
```

---

### Методичка по управлению ордерами в трейдинге

Основные методы управления ордерами – `OpenOrder`, `ModifyOrder` и `CancelOrder`. Результат отправки ордера можно отслеживать в методах `OnOrderChanged`, `OnOrderRegisterFailed`, `OnOrderModifyFailed` и `OnOrderCancelFailed`.

#### Синхронное размещение ордера

Пример размещения рыночного ордера на покупку 1 лота:

```csharp
var order = new Order
{
    Portfolio = Portfolio,
    Security = Security,
    Direction = OrderDirections.Buy,
    Type = OrderTypes.Market,
    QuantityToFill = 1,
};
OpenOrder(order);
```

Пример размещения лимитного ордера на продажу по цене 100:

```csharp
var order = new Order
{
    Portfolio = Portfolio,
    Security = Security,
    Direction = OrderDirections.Sell,
    Type = OrderTypes.Limit,
    Price = 100,
    QuantityToFill = 1,
};
OpenOrder(order);
```

Пример размещения стоп-ордера на продажу по цене 100:

```csharp
var order = new Order
{
    Portfolio = Portfolio,
    Security = Security,
    Direction = OrderDirections.Sell,
    Type = OrderTypes.Stop,
    TriggerPrice = 100,
    QuantityToFill = 1,
};
OpenOrder(order);
```

Пример размещения стоп-лимит ордера на продажу с триггером 100 и лимитной ценой 95:

```csharp
var order = new Order
{
    Portfolio = Portfolio,
    Security = Security,
    Direction = OrderDirections.Sell,
    Type = OrderTypes.StopLimit,
    TriggerPrice = 100,
    Price = 95,
    QuantityToFill = 1,
};
OpenOrder(order);
```

Метод `OpenOrder` отправляет ордер на биржу – результат отслеживается в методах уведомлений.

#### Изменение ордера

Для изменения ордера создаётся его копия, вносятся изменения и вызывается `ModifyOrder`, передавая старый и новый ордеры.

```csharp
_newOrder = _order.Clone();
_newOrder.Price = 200;
ModifyOrder(_order, _newOrder);
```

#### Отмена ордера

Проверяем состояние ордера и вызываем `CancelOrder`.

```csharp
if (_order.State == OrderStates.Active)
{
    CancelOrder(_order);
}
```

---

### Асинхронные методы управления ордерами

Асинхронные методы (например, `OpenOrderAsync`, `ModifyOrderAsync`, `CancelOrderAsync`) позволяют использовать блоки `try-catch` для обработки успеха или исключений.

Пример размещения ордера с использованием `OpenOrderAsync`:

```csharp
try
{
    await OpenOrderAsync(order);
    switch (order.Status())
    {
        case OrderStatus.Placed:
            // ордер размещён
            break;
        case OrderStatus.Filled:
            // ордер исполнен
            break;
        case OrderStatus.PartlyFilled:
            var unfilled = order.Unfilled;
            break;
    }
}
catch (Exception ex)
{
    this.LogInfo($"Open position error: {ex.Message}.");
}
```

Пример изменения ордера с использованием `ModifyOrderAsync`:

```csharp
_newOrder = _order.Clone();
_newOrder.Price = 200;
try
{
    await ModifyOrderAsync(_order, _newOrder);
    // изменения прошли успешно
}
catch (Exception ex)
{
    // изменения не прошли
}
```

Пример отмены ордера с использованием `CancelOrderAsync`:

```csharp
if (_order.State == OrderStates.Active)
{
    try
    {
        await CancelOrderAsync(_order);
        // ордер отменён
    }
    catch (Exception ex)
    {
        // отмена не прошла
    }
}
```

> **Примечание:** При использовании асинхронных методов важно учитывать, что обновление статусов происходят асинхронно, поэтому необходимо правильно управлять состоянием и временными задержками.

---

## 2. Методичка по ATAS.Strategies.ATM Namespace Reference

### Обзор пространства имен ATAS.Strategies.ATM

Это пространство имен содержит классы, структуры, интерфейсы и перечисления для создания автоматизированных торговых стратегий (ATM) в платформе ATAS, включая реализацию логики стоп-лосс/тейк-профит, трейлинг-стопа и поддержки собственных настроек.

#### Пространства имен

- **ATM**  
  Описание: Автоматизированные стратегии.  
  Аннотация: Содержит классы для автоматической торговли (ATM: Automated Trading Module).

- **Chart**  
  Описание: Интерфейсы и классы для работы с графической визуализацией стратегий.  
  Аннотация: Отвечает за графическое отображение индикаторов и стратегий.

- **Editors**  
  Описание: Классы для создания редакторов настроек стратегий.  
  Аннотация: Позволяет создавать UI для редактирования параметров стратегий.

---

## 3. Методичка по классу `ATAS.Strategies.ATM.ATMStrategy<TStrategy>`

### Общая информация

**Название класса:** `ATAS.Strategies.ATM.ATMStrategy<TStrategy>`  
**Тип:** Шаблон класса (abstract, generic)  
**Наследование:**

```
INotifyPropertyChanged
   └── BaseLoggerSource
         └── IStrategy
               └── Strategy
                     └── IATMStrategy
                           └── ATAS.Strategies.ATM.ATMStrategy<TStrategy>
                                  └── ATAS.Strategies.ATM.BaseStopProfitStrategy<TStrategy, TSettings>
```

### Публичные методы

- `Task RetryAsync()`  
- `Task CancelAsync()`  
- `Task ResetOrdersAsync()`  
- `Task CancelOrdersAsync(IEnumerable<Order> orders)`  
- `bool IsStopLoss(Order order)`  
- `bool IsTakeProfit(Order order)`  
- `IATMStrategy Clone(bool cloneOrders = true)`  
- `void SetSettings(IStopProfitSettings settings)`  
- `IStopProfitSettings GetSettings()`  
- `bool IEnumerable<string> Errors IsValidSettings(IStopProfitSettings settings, decimal? expectedPositionVolume = null, decimal? expectedPositionPrice = null)`

### Наследованные методы от базового класса Strategy

- `Task StartAsync()` – запуск стратегии  
- `Task StopAsync()` – остановка стратегии  

### Дополнительные сведения

Методы и свойства этого класса обеспечивают реализацию автоматизированных стратегий с поддержкой стоп-лосс и тейк-профит, а также обработку ошибок и повторов операций.

---

## 4. Методичка по классу `ATAS.Strategies.ATM.BaseStopProfitStrategy<TStrategy, TSettings>`

### Описание класса

`ATAS.Strategies.ATM.BaseStopProfitStrategy<TStrategy, TSettings>` – абстрактный класс-шаблон, предназначенный для реализации стратегий с функцией стоп-лосс/тейк-профит.

### Наследование

```
INotifyPropertyChanged
   └── BaseLoggerSource
         └── IStrategy
               └── Strategy
                     └── IATMStrategy
                           └── ATMStrategy<TStrategy>
                                  └── BaseStopProfitStrategy<TStrategy, TSettings>
```

### Публичные методы

- `void SetSettings(TSettings settings)`  
- `new TSettings GetSettings()`  
- `bool IEnumerable<string> Errors IsValidSettings(TSettings settings, decimal? expectedPositionVolume = null, decimal? expectedPositionPrice = null)`

### Особенности

Этот класс реализует методы для установки и проверки параметров стоп-профита, позволяя обеспечить безопасность сделок и автоматическое управление рисками.

---

## 5. Методичка по структуре `ATAS.Strategies.ATM.ChangesInfo`

### Описание структуры

Структура `ChangesInfo` предназначена для хранения информации об изменениях, произошедших в ATM стратегии. Она содержит свойства:

- **HasChanges** – `bool`: есть ли изменения.
- **IsPositionChanged** – `bool`: изменилась ли позиция.
- **IsPositionOpened** – `bool`: открыта ли позиция.
- **IsPositionClosed** – `bool`: закрыта ли позиция.
- **IsPositionReverted** – `bool`: позиция возвращена.
- **IsOrdersChanged** – `bool`: изменились ли ордера.
- **IsSettingsChanged** – `bool`: изменились ли настройки.

### Методы

- Переопределённый метод `ToString()` для преобразования структуры в строку.  

> **Примечание:** Данная структура используется для анализа изменений в стратегии, что помогает оптимизировать торговую логику.

---

## 6. Методичка по интерфейсу `ATAS.Strategies.ATM.IATMStrategy`

### Обзор интерфейса

Интерфейс `IATMStrategy` наследуется от `IStrategy` и `INotifyPropertyChanged`. Он предназначен для автоматизированных стратегий в ATAS.

### Публичные методы

| Тип возвращаемого значения | Метод                                                          |
| -------------------------- | -------------------------------------------------------------- |
| `Task`                     | `WatchAsync()`                                                 |
| `Task`                     | `StartFromWatchAsync()`                                          |
| `Task`                     | `RetryAsync()`                                                 |
| `Task`                     | `CancelAsync()`                                                |
| `Task`                     | `ResetOrdersAsync()`                                             |
| `bool`                     | `IsStopLoss(Order order)`                                        |
| `bool`                     | `IsTakeProfit(Order order)`                                      |
| `Task`                     | `OpenOrderAsync(Order order, bool isAutomated = true)`           |
| `Task`                     | `ModifyOrderAsync(Order order, Order newOrder, bool isAutomated = true)` |
| `Task`                     | `CancelOrderAsync(Order order, bool isAutomated = true)`         |
| `Task`                     | `CancelOrdersAsync(IEnumerable<Order> orders)`                  |
| `IATMStrategy`             | `Clone(bool cloneOrders = true)`                                 |
| `void`                     | `SetSettings(IStopProfitSettings settings)`                      |
| `IStopProfitSettings`      | `GetSettings()`                                                 |
| `bool IEnumerable<string> Errors` | `IsValidSettings(IStopProfitSettings settings, decimal? expectedPositionVolume = null, decimal? expectedPositionPrice = null)` |

### Свойства

- `bool HasActiveOrders { get; }`
- `IStrategyMarketDataProvider MarketDataProvider { get; set; }`

### События

- `EventHandler<EventArgs> SettingsChanged`

---

## 7. Методичка по интерфейсу `ATAS.Strategies.ATM.ISimpleStopProfitStrategy`

### Обзор интерфейса

Интерфейс `ISimpleStopProfitStrategy` предназначен для простых стратегий стоп-лосс и тейк-профит. Он наследуется от:
- `IStopProfitStrategy<SimpleStopProfitSettings>`
- `ISupportCustomStopOrTake`
- `IATMStrategy`
- `IStrategy`
- `INotifyPropertyChanged`

### Основные свойства

- **StopLoss**: `StopProfitSettings { get; }`
- **TakeProfit**: `StopProfitSettings { get; }`
- **TrailingStop**: `TrailingStopSettings { get; }`
- **Breakeven**: `BreakevenSettings { get; }`
- **CurrentStop**: `PriceUnit? { get; }`
- **CurrentTake**: `PriceUnit? { get; }`

### Дополнительные члены

Интерфейс наследует методы и свойства от базовых интерфейсов, обеспечивая поддержку пользовательских настроек стоп-лосс/тейк-профит.

---

## 8. Методичка по интерфейсу `ATAS.Strategies.ATM.IStopProfitSettings`

### Описание

Интерфейс `IStopProfitSettings` (на основе PDF) находится в пространстве имен `ATAS.Strategies.ATM`. Он определяет настройки базового стоп-лосс и тейк-профита.

### Свойства

- `TimeInForce` – тип: `TimeInForce` (только чтение).

> **Примечание:** Свойство `TimeInForce` задаёт время действия ордеров (например, Day, GTC).

---

## 9. Методичка по интерфейсу `ATAS.Strategies.ATM.IStopProfitStrategy<TSettings>`

### Описание интерфейса

`IStopProfitStrategy<TSettings>` – интерфейс-шаблон для стратегий стоп-профита, параметризованный типом настроек `TSettings`.

### Наследование

Наследует:
- `INotifyPropertyChanged`
- `IStrategy`
- `IATMStrategy`
- (также связан с `BaseStopProfitStrategy<TStrategy, TSettings>`)

### Основные методы

- `void SetSettings(TSettings settings)`  
  Устанавливает настройки стратегии.

- `new TSettings GetSettings()`  
  Возвращает текущие настройки.

- `bool IEnumerable<string> Errors IsValidSettings(TSettings settings, decimal? expectedPositionVolume = null, decimal? expectedPositionPrice = null)`  
  Проверяет валидность настроек и возвращает список ошибок, если они имеются.

### Атрибуты

- `bool IsValid` – указывает, валидны ли настройки.

> **Примечание:** Методы `GetSettings` и `SetSettings` реализованы в классе `BaseStopProfitStrategy<TStrategy, TSettings>`.

---

## 10. Методичка: Функция `GetLastPrice`

### Описание

Функция `GetLastPrice` является частью интерфейса `IStrategyMarketDataProvider` в пространстве имен `ATAS.Strategies.ATM`.

### Сигнатура функции

```csharp
decimal GetLastPrice(Security security)
```

- **Тип возвращаемого значения:** `decimal` – точное десятичное число, подходящее для финансовых расчётов.
- **Параметры:**  
  - `Security security` – объект, представляющий торговый инструмент.

### Полное определение

```csharp
decimal ATAS.Strategies.ATM.IStrategyMarketDataProvider.GetLastPrice(Security security)
```

> **Примечание:** Реализация находится в файле `IATMStrategy.cs`.

---

## 11. Методичка по интерфейсу `ATAS.Strategies.ATM.ISupportCustomStopOrTake`

### Описание

Интерфейс `ISupportCustomStopOrTake` предназначен для стратегий, поддерживающих установку пользовательских уровней стоп-лосс и тейк-профит.

### Основные методы

- **`CanSetCustomStop()`**  
  - **Возвращаемое значение:** `bool`  
  - **Описание:** Возвращает `true`, если можно установить пользовательский стоп-лосс.

- **`CanSetCustomTake()`**  
  - **Возвращаемое значение:** `bool`  
  - **Описание:** Возвращает `true`, если можно установить пользовательский тейк-профит.

- **`GetSettingsWithStopOrTake(PriceUnit? stop, PriceUnit? take)`**  
  - **Возвращаемое значение:** `IStopProfitSettings`  
  - **Описание:** Возвращает объект настроек, скорректированный с учётом заданных `stop` и `take`.

- **`SetCustomStopOrTake(PriceUnit? stop, PriceUnit? take)`**  
  - **Возвращаемое значение:** `Task`  
  - **Описание:** Асинхронно устанавливает заданные уровни стоп-лосс и тейк-профит.

> **Реализация:** Методы реализованы в классе `ATAS.Strategies.ATM.StopProfit`.

---

## 12. Методичка по классу `ATAS.Strategies.ATM.StopProfit`

### Обзор класса

`ATAS.Strategies.ATM.StopProfit` – этот класс реализует стратегию стоп-лосс/тейк-профит и поддерживает пользовательские настройки через интерфейс `ISupportCustomStopOrTake`.

### Наследование

```
INotifyPropertyChanged
  └── IStrategy
        └── IATMStrategy
              └── ATAS.Strategies.ATM.ATMStrategy<TStrategy>
                    └── ATAS.Strategies.ATM.BaseStopProfitStrategy<TStrategy, TSettings>
                          └── ATAS.Strategies.ATM.StopProfit
```

### Публичные методы

- `StopProfit()` – конструктор класса.
- `bool CanSetCustomStop()` – проверяет возможность установки пользовательского стоп-лосса.
- `bool CanSetCustomTake()` – проверяет возможность установки пользовательского тейк-профита.
- `async Task SetCustomStopOrTake(PriceUnit? stop, PriceUnit? take)` – устанавливает кастомные уровни.
- `IStopProfitSettings GetSettingsWithStopOrTake(PriceUnit? stop, PriceUnit? take)` – возвращает текущие настройки с учётом заданных уровней.

### Наследованные методы и события

Методы и свойства, унаследованные от:
- `BaseStopProfitStrategy<StopProfit, SimpleStopProfitSettings>`
- `IStopProfitStrategy<SimpleStopProfitSettings>`
- `IATMStrategy`
- `IStrategy`

> **Примечание:** Подробная документация по методам, защищённым членам, свойствам и событиям содержится в файле `StopProfit.cs`.

---

## 13. Методичка по ATAS.Strategies.Chart Namespace Reference

### Обзор пространства имен ATAS.Strategies.Chart

Это пространство имен предназначено для разработки стратегий, работающих с графиками (чартовыми стратегиями). Оно содержит классы и интерфейсы для построения, отображения и анализа графических данных.

#### Ключевые классы и интерфейсы

1. **ChartStrategy**  
   Абстрактный класс для чартовых стратегий, расширяющий функциональность индикатора.

2. **IChartStrategy**  
   Интерфейс, определяющий контракт для реализации стратегий на графике.

3. **SmaChartStrategy**  
   Пример конкретной реализации чартовой стратегии, использующей скользящую среднюю (SMA).

#### Typedef

- `using MarketDataType = ATAS.Indicators.MarketDataType`  
  – Псевдоним для типа рыночных данных.

---

## 14. Методичка по классу `ATAS.Strategies.Chart.ChartStrategy`

### Общая информация

`ATAS.Strategies.Chart.ChartStrategy` – абстрактный класс для создания стратегий, работающих на графике. Он расширяет функциональность стандартного индикатора и реализует интерфейсы `IChartStrategy` и `ILoggerSource`.

### Основные публичные методы

- `void Start()` – запускает стратегию.
- `void Suspend()` – приостанавливает стратегию.
- `void Stop()` – останавливает стратегию.
- `void StopWithNotification(string message)` – останавливает стратегию с уведомлением.
- `Task StartAsync()` – асинхронный запуск стратегии.
- `Task SuspendAsync()` – асинхронная приостановка.
- `Task StopAsync()` – асинхронная остановка.
- `async Task StopWithNotificationAsync(string message)` – асинхронное закрытие с уведомлением.
- `async Task OpenOrderAsync(Order order)` – асинхронное открытие ордера.
- `async void OpenOrder(Order order)` – синхронное открытие ордера.
- `async Task ModifyOrderAsync(Order order, Order newOrder)` – асинхронное изменение ордера.
- `async void ModifyOrder(Order order, Order newOrder)` – синхронное изменение ордера.
- `async Task CancelOrderAsync(Order order)` – асинхронная отмена ордера.
- `async void CancelOrder(Order order)` – синхронная отмена ордера.

### Защищённые методы

- Конструктор `ChartStrategy(bool useCandles=false)`.
- `void RaiseShowNotification(string message, string title = null, LoggingLevel level = LoggingLevel.Info)` – запуск уведомления.
- `bool SetProperty<TValue>(ref TValue storage, TValue newValue, string propertyName, Action<TValue, TValue> onChanged = null)` – установка свойства с уведомлением.
- `decimal ShrinkPrice(decimal price)` – округляет цену до размера тика.
- Переопределения:  
  - `OnContainerChanged(IIndicatorContainer container)`  
  - `OnDataProviderChanged(IIndicatorDataProvider oldDataProvider, IIndicatorDataProvider newDataProvider)`  
  - `OnBestBidAskChanged(MarketDataArg depth)`  
  - `OnNewMyTrade(MyTrade myTrade)`  
  - `OnStarted()`, `OnSuspended()`, `OnStopping()`, `OnStopped()`, `OnCurrentPositionChanged()`
- `bool CanProcess(int bar)` – определяет, можно ли обрабатывать бар.

### Свойства (унаследованные или определённые непосредственно)

Включают:
- Коллекции ордеров (`Orders`) и трейдов (`MyTrades`).
- Свойства состояния стратегии: `State`, `CurrentPosition`, `AveragePrice`, `OpenPnL`, `ClosedPnL`.
- Свойства, связанные с инструментом: `Security`, `Portfolio`.
- Коннектор данных: `Connector`.
- Логгирование: `LoggerId`, `LoggerName`, `LoggingLevel`, `ParentLoggerSource`, `ChildLoggerSources`.

---

## 15. Методичка по интерфейсу `ATAS.Strategies.Chart.IChartStrategy`

### Описание

Интерфейс `IChartStrategy` расширяет `IStrategy` дополнительными методами для работы на графике. Он обязан предоставлять методы для управления ордерами и получения данных, связанных с графиком.

### Основные методы

- `void StopWithNotification(string message)` – остановка с уведомлением.
- `void OpenOrder(Order order)` – открытие ордера.
- `void ModifyOrder(Order order, Order newOrder)` – модификация ордера.
- `void CancelOrder(Order order)` – отмена ордера.
- `Task OpenOrderAsync(Order order)` – открытие ордера асинхронно.
- `Task ModifyOrderAsync(Order order, Order newOrder)` – асинхронная модификация.
- `Task CancelOrderAsync(Order order)` – асинхронная отмена.

### Свойства

- `IEnumerable<Order> Orders { get; }` – коллекция ордеров стратегии.
- `IEnumerable<MyTrade> MyTrades { get; }` – коллекция сделок.

> **Примечание:** Члены `IChartStrategy` наследуются от `IStrategy`, поэтому дополнительно доступны базовые функции управления стратегией.

---

## 16. Методичка по классу `ATAS.Strategies.Chart.SmaChartStrategy`

### Введение

`SmaChartStrategy` – конкретная реализация чартовой стратегии, использующая скользящую среднюю (SMA) для принятия торговых решений. Класс наследуется от `ChartStrategy`.

### Пример кода

```csharp
public class SmaChartStrategy : ATAS.Strategies.Chart.ChartStrategy
{
    #region Private fields
    private readonly SMA _shortSma = new SMA();
    private readonly SMA _longSma = new SMA();
    private int _lastBar;
    #endregion

    #region Public properties
    [Display(
        Name = "Short period",
        Order = 10)]
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

    [Display(
        Name = "Long period",
        Order = 20)]
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

    [Display(
        Name = "Volume",
        Order = 30)]
    [Parameter]
    public decimal Volume { get; set; }

    [Display(ResourceType = typeof(Resources),
        Name = "ClosePositionOnStopping",
        Order = 40)]
    [Parameter]
    public bool ClosePositionOnStopping { get; set; }
    #endregion

    #region ctor
    public SmaChartStrategy()
    {
        var firstSeries = (ValueDataSeries)DataSeries[0];
        firstSeries.Name = "Short";
        firstSeries.Color = Colors.Red;
        firstSeries.VisualType = VisualMode.Line;

        DataSeries.Add(new ValueDataSeries("Long")
        {
            VisualType = VisualMode.Line,
            Color = Colors.Green,
        });

        ShortPeriod = 21;
        LongPeriod = 75;
        Volume = 1;
        ClosePositionOnStopping = true;
    }
    #endregion

    #region Overrides of BaseIndicator
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

        if (_shortSma[prevBar - 1] < _longSma[prevBar - 1] && _shortSma[prevBar] >= _longSma[prevBar] )
        {
            // cross up: краткосрочная SMA пересекла долгосрочную снизу вверх
            OpenPosition(OrderDirections.Buy);
        }
        if (_shortSma[prevBar - 1] > _longSma[prevBar - 1] && _shortSma[prevBar] <= _longSma[prevBar] )
        {
            // cross down: краткосрочная SMA пересекла долгосрочную сверху вниз
            OpenPosition(OrderDirections.Sell);
        }
    }

    protected override void OnStopping()
    {
        if (CurrentPosition != 0 && ClosePositionOnStopping)
        {
            RaiseShowNotification($"Closing current position {CurrentPosition} on stopping.", level: NotificationLevel.Alert);
            CloseCurrentPosition();
        }
        base.OnStopping();
    }
    #endregion

    #region Private methods
    private void OpenPosition(OrderDirections direction)
    {
        var order = new Order
        {
            Portfolio = Portfolio,
            Security = Security,
            Direction = direction,
            Type = OrderTypes.Market,
            QuantityToFill = GetOrderVolume(),
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
            QuantityToFill = Math.Abs(CurrentPosition),
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
    #endregion
}
```

### Аннотация

- Класс демонстрирует использование двух объектов _SMA – для краткосрочной и долгосрочной скользящих средних.
- Параметры периодов, объёма и флага закрытия позиций задаются через публичные свойства.
- Метод `OnCalculate` отвечает за вычисление значений SMA, определение пересечений и открытие позиций.
- Методы `OpenPosition`, `CloseCurrentPosition` и `GetOrderVolume` реализуют торговую логику.

---

## 17. Директории, файловая структура и зависимости

### Strategies Directory Reference

#### Directory Dependency Graph
- **Indicators**
- **Strategies**

#### Директории
- **ATM**
- **Chart**
- **Editors**
- **Properties**

#### Файлы
- `IStrategy.cs`
- `Strategy.cs`
- `StrategyLoggingExtensions.cs`
- `StrategyNotificationEventArgs.cs`
- `StrategyStateChangedEventArgs.cs`
- `StrategyStates.cs`

---

### ATM Directory Reference

#### Directory Dependency Graph
- **Strategies**
- **ATM**

#### Файлы
- `ATMStrategy.cs`
- `BaseStopProfitStrategy.cs`
- `Strategies/ATM/Extensions.cs`
- `IATMStrategy.cs`
- `MultipleStopProfit.cs`
- `StopProfit.cs`
- `StopTakeValueTypes.cs`

#### Namespaces
- `namespace ATAS`
- `namespace ATAS.Strategies.ATM`

---

### Chart Directory Reference

#### Directory Dependency Graph
- **Strategies**
- **Chart**

#### Файлы
- `ChartStrategy.cs`
- `IChartStrategy.cs`
- `SmaChartStrategy.cs`

#### Namespaces
- `namespace ATAS`
- `namespace ATAS.Indicators`
- `namespace ATAS.Strategies.Chart`

---

### Editors Directory Reference

#### Directory Dependency Graph
- **Strategies**
- **Editors**

#### Файлы
- `ProtectionSettingsEditor.Common.cs`
- `ProtectionSettingsEditor.xaml.cs`

#### Namespaces
- `namespace ATAS`
- `namespace ATAS.Strategies.Editors`

---

### Properties Directory Reference

#### Directory Dependency Graph
- **Strategies**
- **Properties**

#### Файлы
- `Strategies/Properties/AssemblyInfo.cs`

---

## 18. Методичка для создания продвинутых стратегий на платформе ATAS (ProtectionSettingsEditor)

### ProtectionSettingsEditor

#### Полное название класса:
`ATAS.Strategies.Editors.ProtectionSettingsEditor`

#### Тип:
Class Reference

#### Публичкие функции-члены:
- Конструктор:
  - `ProtectionSettingsEditor()`

#### Документация конструктора:
```csharp
ATAS.Strategies.Editors.ProtectionSettingsEditor.ProtectionSettingsEditor()
```

#### Файлы:
- `ProtectionSettingsEditor.xaml.cs`

---

## 19. Методичка по интерфейсу `ATAS.Strategies.IStrategy`

### Обзор

Интерфейс `ATAS.Strategies.IStrategy` – базовый интерфейс для торговых стратегий в ATAS. Он определяет основные функции, необходимые для интеграции стратегии с системой.

#### Наследование

```
ATAS.Strategies.Strategy
    INotifyPropertyChanged
         ATAS.Strategies.IStrategy
              → ATAS.Strategies.ATM.IATMStrategy
              → ATAS.Strategies.Chart.IChartStrategy
```

#### Публичные методы

- `Task StartAsync()` – запускает стратегию.
- `Task StopAsync()` – останавливает стратегию.

#### Свойства

- `string Name { get; set; }` – имя стратегии.
- `StrategyStates State { get; }` – текущее состояние.
- `decimal CurrentPosition { get; }` – текущий объём позиции.
- `decimal AveragePrice { get; }` – средняя цена сделок.
- `decimal OpenPnL { get; }` – открытая прибыль/убыток.
- `decimal ClosedPnL { get; }` – закрытая прибыль/убыток.
- `Security Security { get; set; }` – торговый инструмент.
- `Portfolio Portfolio { get; set; }` – портфель.
- `TPlusLimits? TPlusLimit { get; set; }` – лимиты T+.
- `IDataFeedConnector Connector { get; set; }` – коннектор данных.

#### События

- `EventHandler<StrategyStateChangedEventArgs> StateChanged`
- `EventHandler<StrategyNotificationEventArgs> ShowNotification`

#### Подробное описание

Интерфейс определяет базовый функционал для работы со стратегией, включая запуск, остановку, управление позициями и ордерами, а также события уведомлений.

*Документация создана из файла `IStrategy.cs`.*

---

## 20. Методичка по классу `ATAS.Strategies.Strategy`

### Общая информация

`ATAS.Strategies.Strategy` – базовый класс для реализации торговых стратегий.

#### Наследование

```
INotifyPropertyChanged
   └── BaseLoggerSource
         └── IStrategy
                └── ATAS.Strategies.Strategy
                      └── ATAS.Strategies.ATM.ATMStrategy<TStrategy>
                            └── ATAS.Strategies.ATM.BaseStopProfitStrategy<TStrategy, TSettings>
```

#### Публичные методы

- `void Start()`
- `void Stop()`
- `Task StartAsync()`
- `Task StopAsync()`
- `Task WatchAsync()`
- `Task StartFromWatchAsync()`
- Методы управления ордерами:
  - `async void OpenOrder(Order order, bool isAutomated = true)`
  - `async Task OpenOrderAsync(Order order, bool isAutomated = true)`
  - `async void ModifyOrder(Order order, Order neworder, bool isAutomated = true)`
  - `async Task ModifyOrderAsync(Order order, Order neworder, bool isAutomated = true)`
  - `async void CancelOrder(Order order, bool isAutomated = true)`
  - `async Task CancelOrderAsync(Order order, bool isAutomated = true)`

#### Защищённые методы

Содержат внутреннюю логику:
- Конструктор `Strategy()`
- `SetState(StrategyStates state)`
- `SetErrorState(StrategyErrorTypes type, string[] errorDescriptions)`
- `ResetErrorState()`
- `RaisePropertyChanged(string propertyName)`
- `RaiseShowNotification(string message, string title = null, bool isError = false)`
- `SetProperty<TValue>(ref TValue storage, TValue newValue, string propertyName, Action<TValue, TValue> onChanged = null)`
- `GetOCOGroup()`
- `CanProcess(Order order)` и `CanProcess()`
- `UpdateCurrentPosition()`
- Переопределения событий жизненного цикла: `OnStarted()`, `OnStartedFromWatch()`, `OnStopping()`, `OnStopped()`
- Методы управления ордерами: `OnOpenOrder`, `OnModifyOrder`, `OnCancelOrder`, `OnOrderRegisterFailed`, `OnOrderModifyFailed`, `OnOrderCancelFailed`
- Методы получения и фильтрации трейдов: `FilterMyTrades()`
- Метод `LogParameters()`

#### Свойства

- `Security Security { get; set; }`
- `Portfolio Portfolio { get; set; }`
- `TPlusLimits? TPlusLimit { get; set; }`
- `IDataFeedConnector Connector { get; set; }`
- `IEnumerable<MyTrade> MyTrades { get; }`
- `IEnumerable<Order> Orders { get; }`
- `Position Position { get; set; }`
- `decimal CurrentPosition { get; }`
- `decimal AveragePrice { get; }`
- `int OpenTicksPnL { get; }`
- `decimal OpenPnL { get; }`
- `decimal ClosedPnL { get; }`
- `MarketDepth BestBid { get; }`
- `MarketDepth BestAsk { get; }`
- `StrategyStates State { get; protected set; }`
- `StrategyStateDescription StateDescription { get; }`
- `string Name { get; set; }`

#### События

- `PropertyChangedEventHandler PropertyChanged`
- `EventHandler<StrategyStateChangedEventArgs> StateChanged`
- `EventHandler<StrategyNotificationEventArgs> ShowNotification`

*Подробнее – см. файл `Strategy.cs`.*

---

## 21. Методичка по классу `ATAS.Strategies.StrategyNotificationEventArgs`

### Описание класса

Класс `StrategyNotificationEventArgs` предоставляет данные для события уведомления стратегии.

#### Наследование

```
System.EventArgs
   └── ATAS.Strategies.StrategyNotificationEventArgs
```

#### Публичие члены

- **Конструктор:**
  ```csharp
  StrategyNotificationEventArgs(
      IStrategy strategy,
      string message,
      string title,
      bool isError)
  ```
  Инициализирует новый экземпляр с заданными параметрами.

#### Свойства

- `IStrategy Strategy { get; }` – стратегия, вызвавшая уведомление.
- `string Message { get; }` – текст уведомления.
- `string Title { get; }` – заголовок уведомления.
- `bool IsError { get; }` – флаг, указывающий, является ли уведомление ошибкой.

*Документация создана из файла `StrategyNotificationEventArgs.cs`.*

---

## 22. Методичка по классу `ATAS.Strategies.StrategyStateChangedEventArgs`

### Описание класса

`StrategyStateChangedEventArgs` предоставляет данные для события изменения состояния стратегии.

#### Наследование

```
System.EventArgs
   └── ATAS.Strategies.StrategyStateChangedEventArgs
```

#### Конструктор

```csharp
ATAS.Strategies.StrategyStateChangedEventArgs.StrategyStateChangedEventArgs(
    IStrategy strategy,
    StrategyStateDescription state)
```

- **Параметры:**
  - `strategy` (IStrategy): стратегия, для которой происходит изменение.
  - `state` (StrategyStateDescription): описание нового состояния стратегии.

#### Свойства

- `IStrategy Strategy { get; }` – возвращает связанную стратегию.
- `StrategyStateDescription State { get; }` – возвращает новое состояние стратегии.

*Документация создана из файла `StrategyStateChangedEventArgs.cs`.*

---

## Дополнительные разделы: Директорий и файловая структура

### Strategies Directory

- **Directories:**  
  - ATM, Chart, Editors, Properties

- **Files:**  
  - `IStrategy.cs`, `Strategy.cs`, `StrategyLoggingExtensions.cs`, `StrategyNotificationEventArgs.cs`, `StrategyStateChangedEventArgs.cs`, `StrategyStates.cs`

### ATM Directory

- **Files:**  
  - `ATMStrategy.cs`, `BaseStopProfitStrategy.cs`, `Extensions.cs`, `IATMStrategy.cs`, `MultipleStopProfit.cs`, `StopProfit.cs`, `StopTakeValueTypes.cs`

- **Namespaces:**  
  - `ATAS.Strategies.ATM`

### Chart Directory

- **Files:**  
  - `ChartStrategy.cs`, `IChartStrategy.cs`, `SmaChartStrategy.cs`

- **Namespaces:**  
  - `ATAS.Strategies.Chart`

### Editors Directory

- **Files:**  
  - `ProtectionSettingsEditor.Common.cs`, `ProtectionSettingsEditor.xaml.cs`

- **Namespaces:**  
  - `ATAS.Strategies.Editors`

### Properties Directory

- **Files:**  
  - `AssemblyInfo.cs` (в папке Strategies/Properties)

---

## Заключение

В этой методичке собрана полная техническая информация по разработке атрибутов и стратегий для платформы ATAS с использованием .NET 8. Документ охватывает:

- Основные принципы разработки стратегий (как синхронных, так и асинхронных) для работы с ордерами, позициями и уведомлениями.
- Подробное описание классов в пространствах имен `ATAS.Strategies.ATM` и `ATAS.Strategies.Chart`, а также базовых интерфейсов и классов `IStrategy`, `Strategy`, `ChartStrategy` и другие.
- Полное описание методов, свойств, событий и конструкций, включая примеры кода, используемые в стратегии Sample SMA (SmaChartStrategy).
- Обзор файловой структуры и директорий проекта для удобства навигации в исходном коде.

Эта методичка предназначена для разработчиков, которые хотят применять глубокие знания ATAS API для создания собственных торговых стратегий с полным контролем над логикой торговли, управлением ордерами, рисками и настройками стратегии.

---



