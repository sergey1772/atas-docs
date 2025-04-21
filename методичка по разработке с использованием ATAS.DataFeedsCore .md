Полная методичка по разработке с использованием ATAS.DataFeedsCore Эта методичка описывает пространство имён ATAS.DataFeedsCore, используемое в платформе ATAS для работы с рыночными данными, управления ордерами, портфелями, комиссиями и другими аспектами торговой инфраструктуры. Она включает описание всех классов, интерфейсов, перечислений, структур, делегатов и подмодулей, а также примеры кода и рекомендации для создания технических заданий (ТЗ).

1.  Обзор пространства имён ATAS.DataFeedsCore Пространство имён ATAS.DataFeedsCore предоставляет функциональность для работы с данными биржевой торговли, включая подключение к биржам, управление ордерами, портфелями, позициями, рыночными данными, а также настройку комиссий и прав пользователей. Оно предназначено для интеграции с платформой ATAS и разработки пользовательских решений, таких как индикаторы, стратегии и аналитические инструменты. 1.1. Подмодули пространства имён

Commissions: Управление комиссиями (группы, правила, типы). ConnectorWebsocket: Подключение через веб-сокеты. Database: Работа с базами данных. Dom: Управление глубиной рынка (DOM). Exceptions: Обработка исключений. Rebate: Управление возвратами комиссий. SessionServer: Управление сессиями. Statistics: Сбор статистики. TradeStatistics: Статистика по сделкам.

1.2. Основные задачи

Подключение к биржам через асинхронные коннекторы (AsyncConnector, CryptoAsyncConnector). Управление рыночными данными (MarketDepth, UniversalMarketData). Работа с ордерами и позициями (Order, Position, Portfolio). Настройка комиссий (CommissionGroup, ICommissionRule). Управление пользователями и ролями (User, UserRole, UserGroup). Проверка времени сделок и рабочих сессий (TradesTimeChecker, WorkingTime).

2.  Основные классы и интерфейсы 2.1. Классы 2.1.1. AsyncConnector

Описание: Базовый класс для асинхронного подключения к бирже, поддерживающий await для работы в потоке очереди коннектора. Пример:var connector = new AsyncConnector(); await connector.ConnectAsync();

2.1.2. BaseConnector

Описание: Базовый класс для всех коннекторов, предоставляющий общие методы подключения. Наследники: AsyncConnector, CryptoBaseConnector.

2.1.3. BasketConnector

Описание: Коннектор для работы с несколькими источниками данных. Настройки: BasketConnectorSettings.

2.1.4. MarketDepth

Описание: Представляет данные глубины рынка (DOM). Свойства: Price: Цена уровня. Volume: Объём на уровне. Type: Тип данных (Bid, Ask).

Пример:var depth = new MarketDepth { Price = 100.5m, Volume = 10, Type = MarketDataType.Bid };

2.1.5. Order

Описание: Представляет торговый ордер. Свойства: Direction: Направление (Buy, Sell). Type: Тип (Market, Limit, Stop, StopLimit). QuantityToFill: Объём. Price: Цена (для лимитных ордеров). TriggerPrice: Цена срабатывания (для стоп-ордеров).

Пример:var order = new Order { Direction = OrderDirections.Buy, Type = OrderTypes.Market, QuantityToFill = 1, Portfolio = portfolio, Security = security };

2.1.6. Portfolio

Описание: Представляет торговый портфель. Свойства: Balance: Баланс счёта. PnL: Прибыль/убыток. Positions: Список позиций.

Пример:var portfolio = new Portfolio { Balance = 10000m };

2.1.7. Position

Описание: Представляет торговую позицию. Свойства: Quantity: Объём позиции. AveragePrice: Средняя цена входа. Security: Инструмент.

Пример:var position = new Position { Quantity = 1, AveragePrice = 100.5m, Security = security };

2.1.8. Security

Описание: Представляет торговый инструмент (акция, фьючерс, криптовалюта). Свойства: Symbol: Символ инструмента. Type: Тип (Stock, Future, CryptoFutures).

Пример:var security = new Security { Symbol = "BTCUSD", Type = SecType.CryptoFutures };

2.1.9. Trade

Описание: Представляет тик (сделку) на бирже. Свойства: Price: Цена сделки. Volume: Объём. Time: Время сделки. OrderDirection: Направление (Buy, Sell, Between).

Пример:var trade = new Trade { Price = 100.5m, Volume = 1, Time = DateTime.Now, OrderDirection = TradeDirection.Buy };

2.1.10. UniversalMarketData

Описание: Универсальный класс для представления рыночных данных (тиков, котировок, DOM). Свойства: Price: Цена. Volume: Объём. Type: Тип данных (Bid, Ask, Trade). Direction: Направление (Buy, Sell, Between).

Пример:var marketData = new UniversalMarketData { Price = 100.5m, Volume = 10, Type = MarketDataType.Trade, Direction = TradeDirection.Buy };

2.1.11. CommissionGroup (из ATAS.DataFeedsCore.Commissions)

Описание: Управляет группами комиссий. Свойства: Id: Уникальный идентификатор. Name: Название группы. Rules: Список правил (CommissionGroupItem).

Пример:var commissionGroup = new CommissionGroup { Name = "Standard", Rules = new List() };

2.1.12. User

Описание: Представляет пользователя платформы. Свойства: Login: Логин. Password: Пароль. Role: Роль (UserRole). Portfolios: Список портфелей.

Методы: Check(string password): Проверка пароля. Update(string password): Обновление пароля.

Пример:var user = new User("trader", "password") { Role = new UserRole { Name = "Trader" } }; if (user.Check("password")) Console.WriteLine("Password correct");

2.1.13. WorkingTime

Описание: Определяет рабочее время биржи. Свойства: StartDay: День начала (например, Monday). StartTime: Время начала. EndDay: День окончания. EndTime: Время окончания.

Методы: IsWorkingTime(DateTime time): Проверка, является ли время рабочим.

Пример:var workingTime = new WorkingTime { StartDay = DayOfWeek.Monday, StartTime = TimeSpan.FromHours(9), EndDay = DayOfWeek.Friday, EndTime = TimeSpan.FromHours(17) }; if (workingTime.IsWorkingTime(DateTime.Now)) Console.WriteLine("Market is open");

2.2. Интерфейсы 2.2.1. IEntity

Описание: Базовый интерфейс для всех сущностей платформы. Свойства: EntityType: Тип сущности.

Реализуется: Order, Portfolio, Security, Trade, User, и др.

2.2.2. IDataFeedConnector

Описание: Интерфейс для коннекторов данных. Методы: ConnectAsync(): Подключение. DisconnectAsync(): Отключение.

Реализуется: AsyncConnector, CryptoAsyncConnector.

2.2.3. IMarketDepth

Описание: Интерфейс для работы с данными глубины рынка. Свойства: Price, Volume, Type.

2.2.4. ICommissionRule

Описание: Интерфейс для правил расчёта комиссий. Реализуется: PercentCommissionRule, PerContractCommissionRule, PerTradeCommissionRule.

3.  Перечисления 3.1. OrderTypes

Значения: Limit: Лимитный ордер. Market: Рыночный ордер. Stop: Стоп-ордер. StopLimit: Стоп-лимит ордер. Unknown: Неизвестный тип.

3.2. OrderDirections

Значения: Buy: Покупка. Sell: Продажа.

3.3. OrderStates

Значения: None: Нет состояния. Active: Активный. Done: Выполнен. Failed: Ошибка.

3.4. OrderStatus

Значения: None: Нет статуса. Placed: Размещён. Filled: Исполнен. PartlyFilled: Частично исполнен. Canceled: Отменён.

3.5. MarketDataType

Значения: Bid: Предложение (покупка). Ask: Спрос (продажа). Trade: Сделка.

3.6. TradeDirection

Значения: Buy: Покупка. Sell: Продажа. Between: Между (неопределённое направление).

3.7. SecType

Значения: Future: Фьючерс. Forex: Форекс. Stock: Акция. Bitcoin: Криптовалюта. CryptoFutures: Криптофьючерс. Indexes: Индекс. Option: Опцион.

3.8. EntityType

Значения: Security, Portfolio, Position, Order, Trade, MarketDepth, User, и др.

4.  Работа с рыночными данными 4.1. Подписка на данные Используйте SubscriptionManager для управления подписками на рыночные данные. Пример: var subscriptionManager = new SubscriptionManager(); subscriptionManager.Subscribe(new Security { Symbol = "BTCUSD" }, SubscriptionType.Quotes);

4.2. Обработка глубины рынка Используйте MarketDepth для получения данных DOM. Пример: void OnMarketDepthReceived(MarketDepth depth) { Console.WriteLine($"Price: {depth.Price}, Volume: {depth.Volume}, Type: {depth.Type}"); }

4.3. Обработка тиков Используйте Trade для обработки сделок. Пример: void OnTradeReceived(Trade trade) { Console.WriteLine($"Trade at {trade.Time}: {trade.Price} x {trade.Volume}"); }

5.  Управление ордерами 5.1. Создание и отправка ордера Пример: var order = new Order { Portfolio = portfolio, Security = security, Direction = OrderDirections.Buy, Type = OrderTypes.Limit, Price = 100.5m, QuantityToFill = 1 }; await connector.OpenOrderAsync(order);

5.2. Отслеживание статуса ордера Пример: void OnOrderStatusChanged(Order order) { switch (order.Status()) { case OrderStatus.Placed: Console.WriteLine("Order placed"); break; case OrderStatus.Filled: Console.WriteLine("Order filled"); break; case OrderStatus.Canceled: Console.WriteLine("Order canceled"); break; } }

6.  Управление комиссиями 6.1. Настройка правил комиссий Используйте CommissionGroup и ICommissionRule. Пример: var commissionGroup = new CommissionGroup { Name = "Crypto", Rules = new List { new CommissionGroupItem { Rule = new PercentCommissionRule { Rate = 0.01m }, TypeName = "Percent" } } };
    
7.  Управление пользователями и ролями 7.1. Создание пользователя Пример: var user = new User("trader", "password") { Email = "trader@example.com", Role = new UserRole { Name = "Trader", Rights = new HashSet { EntityAction.OrderRegister } } };
    

7.2. Проверка прав Пример: if (user.Role.Rights.Contains(EntityAction.OrderRegister)) Console.WriteLine("User can place orders");

8.  Проверка времени сделок Используйте TradesTimeChecker для валидации времени сделок. Пример: var checker = new TradesTimeChecker(); var trade = new Trade { Time = DateTime.Now }; checker.Check(trade); checker.Reset();
    
9.  Пример интеграции: Подписка на рыночные данные и размещение ордера Пример: public class MarketDataTrader { private readonly AsyncConnector \_connector; private readonly SubscriptionManager \_subscriptionManager; private readonly Portfolio \_portfolio; private readonly Security \_security;
    
    public MarketDataTrader() { \_connector = new AsyncConnector(); \_subscriptionManager = new SubscriptionManager(); \_portfolio = new Portfolio { Balance = 10000m }; \_security = new Security { Symbol = "BTCUSD", Type = SecType.CryptoFutures }; }
    
    public async Task StartAsync() { await \_connector.ConnectAsync(); \_subscriptionManager.Subscribe(\_security, SubscriptionType.Quotes);
    
        _connector.MarketDepthReceived += OnMarketDepth;
        
    
    }
    
    private void OnMarketDepth(MarketDepth depth) { if (depth.Type == MarketDataType.Bid && depth.Price < 100) { var order = new Order { Portfolio = \_portfolio, Security = \_security, Direction = OrderDirections.Buy, Type = OrderTypes.Market, QuantityToFill = 1 }; \_connector.OpenOrderAsync(order).GetAwaiter().GetResult(); } } }
    
10.  Рекомендации для технических заданий Для создания интеграции с ATAS.DataFeedsCore:
    

Опишите задачу:

Тип данных: тики, DOM, котировки. Инструменты: акции, фьючерсы, криптовалюты. Действия: размещение ордеров, управление позициями.

Укажите параметры:

Типы ордеров (Market, Limit). Правила комиссий (процентные, фиксированные). Ограничения (максимальный объём, просадка).

Задайте сценарии:

Подписка на данные и их обработка. Условия для открытия/закрытия позиций. Логирование и уведомления.

Пример ТЗ:

Подписаться на котировки BTCUSD (тип: CryptoFutures). Открывать лимитный ордер на покупку при цене ниже 50000 (объём: 1 контракт). Установить комиссию 0.1% за сделку. Закрывать позицию при достижении прибыли 1000 USD. Логировать все сделки.

11.  Полный список классов, интерфейсов и перечислений 11.1. Классы

AsyncConnector, BaseConnector, BasketConnector, CryptoAsyncConnector, CryptoBaseConnector. MarketDepth, MarketByOrder, Order, Portfolio, Position, Security, Trade, UniversalMarketData. CommissionGroup, CommissionGroupItem, PercentCommissionRule, PerContractCommissionRule, PerTradeCommissionRule. User, UserRole, UserGroup, UserChange, UserChangeHistory, UserRoleRight. TradesTimeChecker, WorkingTime, TradingOptions, TradingOptionsSecurity.

11.2. Интерфейсы

IEntity, IDataFeedConnector, IMarketDepth, IMarketByOrdersManager, ICommissionRule. ISecurityTradingOptions, IOrderOptionReduceOnly, IOrderOptionPostOnly, IOrderOptionCloseOnTrigger.

11.3. Перечисления

OrderTypes, OrderDirections, OrderStates, OrderStatus, MarketDataType, TradeDirection, SecType. EntityType, EntityAction, MarketByOrderUpdateTypes, Currencies, EcnId, TimeInForce.

11.4. Структуры

Money, PriceUnit, VolumeUnit, OptionSeries, CurrencyInfo.

11.5. Делегаты

ConnectorEventHandler, ConnectorEventHandler, ConnectorEventHandler, ConnectorEventHandler.