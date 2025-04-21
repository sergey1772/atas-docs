# Полная методичка по разработке с использованием OFT Framework для ATAS

Эта методичка описывает пространства имён `OFT.Attributes`, `OFT.Attributes.Editors`, и `OFT.Core`, используемые в платформе ATAS для настройки пользовательских интерфейсов индикаторов, управления параметрами и конфигурации коннекторов данных. Она включает описание всех классов, интерфейсов, перечислений, примеры кода и рекомендации для интеграции с платформой ATAS.

## 1. Обзор пространства имён `OFT.Attributes`

Пространство имён `OFT.Attributes` предоставляет атрибуты для аннотирования объектов, используемых в платформе ATAS, таких как индикаторы, настройки и коннекторы. Эти атрибуты определяют поведение, отображение и функциональность объектов.

### 1.1. Подмодули
- **Editors**: Содержит атрибуты для настройки редакторов параметров индикаторов.
- **Properties**: Пространство имён для свойств (детали отсутствуют в документации).

### 1.2. Основные задачи
- Настройка визуальных элементов управления для параметров индикаторов (`CheckEditorAttribute`, `ComboBoxEditorAttribute`, и др.).
- Управление метаданными объектов (`FeatureIdAttribute`, `HelpLinkAttribute`).
- Контроль клонирования объектов (`IgnoreCloneAttribute`).

## 2. Обзор пространства имён `OFT.Attributes.Editors`

Пространство имён `OFT.Attributes.Editors` содержит атрибуты для настройки редакторов параметров индикаторов в пользовательском интерфейсе ATAS. Эти атрибуты определяют, как параметры отображаются и редактируются.

### 2.1. Основные задачи
- Создание интерфейсов для ввода параметров (чекбоксы, выпадающие списки, числовые поля).
- Настройка форматов ввода данных (маски, диапазоны значений).
- Управление поведением редакторов (режимы отправки значений, выделение текста).

## 3. Обзор пространства имён `OFT.Core`

Пространство имён `OFT.Core` предоставляет базовые классы и интерфейсы для настройки коннекторов данных, используемых в ATAS для получения рыночных данных и управления торговыми операциями.

### 3.1. Основные задачи
- Создание и настройка коннекторов (`BaseConnectorSettings<T>`, `IConnectorSettings`).
- Управление функциями коннекторов (`ConnectorFeatures`).
- Поддержка инициализации и типизации настроек (`IConnectorSettingsSupportInitialization`, `ITypedConnectorSettings`).

## 4. Основные классы, интерфейсы и перечисления

### 4.1. Пространство имён `OFT.Attributes`

#### 4.1.1. Классы
- **Extensions**: Класс расширений (детали отсутствуют).
- **FeatureIdAttribute**: Атрибут для идентификации функциональных возможностей.
- **HelpLinkAttribute**: Атрибут для ссылки на справку.
- **IgnoreCloneAttribute**: Атрибут для игнорирования клонирования свойства или поля.
  - **Пример**:
    ```csharp
    [IgnoreClone]
    public int NonClonableField;
    ```
- **LogoAttribute**: Атрибут для логотипа.
- **MappingAttribute**: Атрибут для маппинга.
- **ParameterAttribute**: Атрибут для параметров.
- **ReferralLinkAttribute**: Атрибут для реферальной ссылки.
  - **Свойства**:
    - `Url` (`string`, get): URL реферальной ссылки.
  - **Пример**:
    ```csharp
    [ReferralLink("https://example.com/referral")]
    public class MyIndicator { }
    ```

#### 4.1.2. Перечисления
- **DataSeriesTypes**:
  - `Band`: Полоса.
  - `Line`: Линия.
  - `Candle`: Свеча.
  - `Value`: Значение.
  - **Пример**:
    ```csharp
    var seriesType = DataSeriesTypes.Candle;
    ```

### 4.2. Пространство имён `OFT.Attributes.Editors`

#### 4.2.1. Классы
- **CheckEditorAttribute**: Атрибут для создания чекбокса.
  - **Конструктор**: `CheckEditorAttribute(string propertyName)`
  - **Свойства**:
    - `PropertyName` (`string`, get): Имя связанного свойства.
  - **Исключения**: `ArgumentNullException` при `propertyName == null`.
  - **Пример**:
    ```csharp
    [CheckEditor("IsEnabled")]
    public bool IsEnabled { get; set; }
    ```

- **ComboBoxEditorAttribute**: Атрибут для создания выпадающего списка.
  - **Конструкторы**:
    - `ComboBoxEditorAttribute(params object[] itemsSource)`
    - `ComboBoxEditorAttribute(Type typeSource)`
  - **Свойства**:
    - `DisplayMember` (`string`, get/set): Имя поля для отображения.
    - `ValueMember` (`string`, get/set): Имя поля для значения.
    - `ItemsSource` (`object[]`, get/set): Источник данных.
    - `IsTextEditable` (`bool`, get/set): Разрешено ли редактировать текст.
    - `AutoComplete` (`bool`, get/set): Включено ли автодополнение.
    - `SelectItemWithNullValue` (`bool`, get/set): Выбирать элемент при `null`.
  - **Методы**:
    - `IEnumerable GetItemsSource(object instance)`: Получение источника данных.
  - **Исключения**:
    - `ArgumentNullException`: Если `typeSource` — `null`.
    - `ArgumentException`: Если `typeSource` не реализует `IEnumerable`.
  - **Пример**:
    ```csharp
    [ComboBoxEditor(new[] { "Option1", "Option2" }, IsTextEditable = true)]
    public string SelectedOption { get; set; }
    ```

- **DataSeriesEditorAttribute**: Атрибут для выбора серии данных.
  - **Конструктор**: `DataSeriesEditorAttribute(DataSeriesTypes dataSeriesType)`
  - **Свойства**:
    - `DataSeriesType` (`DataSeriesTypes`, get): Тип серии данных.
  - **Пример**:
    ```csharp
    [DataSeriesEditor(DataSeriesTypes.Candle)]
    public DataSeries Series { get; set; }
    ```

- **IsExpandedAttribute**: Атрибут для управления состоянием группы параметров.
  - **Пример**:
    ```csharp
    [IsExpanded]
    public class IndicatorSettings { }
    ```

- **MaskAttribute**: Атрибут для задания маски ввода.
  - **Конструкторы**:
    - `MaskAttribute()`
    - `MaskAttribute(MaskTypes type, string mask)`
  - **Свойства**:
    - `Culture` (`string`, get/set): Культура форматирования.
    - `Mask` (`string`, get/set): Строка маски.
    - `MaskAsDisplayFormat` (`bool`, get/set): Использовать маску только для отображения.
    - `MaskType` (`MaskTypes`, get/set): Тип маски.
    - `MethodName` (`string`, get/set): Имя метода обработки.
  - **Пример**:
    ```csharp
    [Mask(MaskTypes.DateTime, "dd.MM.yyyy")]
    public DateTime StartDate { get; set; }
    ```

- **NumericEditorAttribute**: Атрибут для числового редактора.
  - **Конструкторы**:
    - `NumericEditorAttribute()`
    - `NumericEditorAttribute(NumericEditorTypes editorType, double minValue)`
    - `NumericEditorAttribute(NumericEditorTypes editorType, double minValue, double maxValue)`
    - `NumericEditorAttribute(double minValue, double maxValue)`
    - `NumericEditorAttribute(double minValue)`
  - **Свойства**:
    - `EditorType` (`NumericEditorTypes`, get/set): Тип редактора (`Spin`, `TrackBar`).
    - `MinValue` (`double?`, get): Минимальное значение.
    - `MaxValue` (`double?`, get): Максимальное значение.
    - `Step` (`double`, get/set): Шаг изменения.
    - `DisplayFormat` (`string`, get/set): Формат отображения.
  - **Пример**:
    ```csharp
    [NumericEditor(NumericEditorTypes.Spin, 0, 100)]
    public double Threshold { get; set; }
    ```

- **PostValueModeAttribute**: Атрибут для режима отправки значений.
  - **Конструкторы**:
    - `PostValueModeAttribute()`
    - `PostValueModeAttribute(PostValueModes mode)`
  - **Свойства**:
    - `Mode` (`PostValueModes`, get): Режим отправки.
    - `DelayMilliseconds` (`int`, get/set): Задержка в миллисекундах.
    - `SelectAllOnMode` (`SelectAllOnModes`, get/set): Режим выделения текста.
  - **Пример**:
    ```csharp
    [PostValueMode(PostValueModes.Delayed, DelayMilliseconds = 500)]
    public string Input { get; set; }
    ```

- **SelectDirectoryEditorAttribute**: Атрибут для выбора директории.
  - **Конструкторы**:
    - `SelectDirectoryEditorAttribute()`
    - `SelectDirectoryEditorAttribute(SpecialFolder initialSpecialFolder)`
  - **Свойства**:
    - `IsTextEditable` (`bool`, get/set): Разрешено ли редактировать текст.
    - `ShowNewFolderButton` (`bool`, get/set): Показывать кнопку создания папки.
    - `InitialSpecialFolder` (`SpecialFolder?`, get): Начальная папка.
  - **Пример**:
    ```csharp
    [SelectDirectoryEditor(SpecialFolder.MyDocuments)]
    public string OutputPath { get; set; }
    ```

- **SelectFileEditorAttribute**: Атрибут для выбора файла.
  - **Конструкторы**:
    - `SelectFileEditorAttribute()`
    - `SelectFileEditorAttribute(SpecialFolder initialSpecialFolder)`
  - **Свойства**:
    - `IsTextEditable` (`bool`, get/set): Разрешено ли редактировать текст.
    - `SelectFileType` (`SelectFileTypes`, get/set): Тип выбора (`Open`, `Save`).
    - `Multiselect` (`bool`, get/set): Разрешён ли множественный выбор.
    - `InitialFolder` (`string`, get/set): Начальная папка.
    - `InitialSpecialFolder` (`SpecialFolder?`, get): Начальная специальная папка.
    - `Filter` (`string`, get/set): Фильтр файлов.
  - **Пример**:
    ```csharp
    [SelectFileEditor(Filter = "CSV Files|*.csv")]
    public string DataFile { get; set; }
    ```

- **TextEditorAttribute**: Атрибут для текстового редактора.
  - **Конструктор**: `TextEditorAttribute(bool isMultiline, int maxHeight = 50)`
  - **Свойства**: (не указаны в документации, предполагается стандартное поведение).
  - **Пример**:
    ```csharp
    [TextEditor(isMultiline = true, maxHeight = 100)]
    public string Notes { get; set; }
    ```

#### 4.2.2. Перечисления
- **MaskTypes**:
  - `None`: Без маски.
  - `DateTime`: Для даты и времени.
  - `DateTimeAdvancingCaret`: Дата/время с автопереходом.
  - `Numeric`: Для чисел.
  - `RegEx`: Регулярное выражение.
  - `Regular`: Стандартная маска.
  - `Simple`: Простая маска.
  - `TimeSpan`: Для временного интервала.
  - `TimeSpanAdvancingCaret`: Временной интервал с автопереходом.

- **NumericEditorTypes**:
  - `Spin`: Счётчик с кнопками.
  - `TrackBar`: Ползунок.

- **PostValueModes**:
  - `OnChanged`: Отправка при изменении.
  - `OnLostFocus`: Отправка при потере фокуса.
  - `Delayed`: Отправка с задержкой.

- **SelectAllOnModes**:
  - `None`: Без выделения.
  - `GotFocus`: Выделение при получении фокуса.
  - `MouseUp`: Выделение при отпускании мыши.

- **SelectFileTypes**:
  - `Open`: Открытие файла.
  - `Save`: Сохранение файла.

### 4.3. Пространство имён `OFT.Core`

#### 4.3.1. Классы
- **BaseConnectorSettings<T>**: Абстрактный шаблонный класс для настроек коннекторов.
  - **Методы**:
    - `IDataFeedConnector CreateConnector(string dataPath)`: Создаёт коннектор.
    - `void ApplySettings(IDataFeedConnector connector)`: Применяет настройки.
    - `virtual bool CheckSupported(out string? errorMessage)`: Проверяет поддержку.
    - `override string ToString()`: Строковое представление.
    - `abstract IDataFeedConnector OnCreateConnector(string dataPath)`: Абстрактный метод создания.
    - `abstract void OnApplySettings(IDataFeedConnector connector)`: Абстрактное применение настроек.
    - `void RaisePropertyChanged(string propertyName)`: Уведомление об изменении свойства.
    - `bool SetProperty<TValue>(ref TValue storage, TValue newValue, string propertyName, Action<TValue, TValue> onChanged = null)`: Установка свойства.
  - **Свойства**:
    - `Type` (`string`, get/set): Тип коннектора.
    - `Description` (`string`, get/set): Описание.
    - `Logo` (`Uri`, get): Логотип.
    - `Id` (`Guid`, get/set): Уникальный идентификатор.
    - `DisplayName` (`string`, get/set, virtual): Отображаемое имя.
    - `Name` (`string`, get/set): Имя.
    - `IsMarketDataEnabled` (`bool`, get/set): Включены ли рыночные данные.
    - `IsAutoConnectEnabled` (`bool`, get/set): Включено ли автоподключение.
    - `AllowUpdatePositionsPnL` (`bool`, get/set): Разрешено ли обновление PnL.
    - `RefreshSecuritiesTime` (`TimeOnly?`, get/set): Время обновления инструментов.
    - `Features` (`ConnectorFeatures`, get, abstract): Возможности коннектора.
    - `SettingsTypes` (`ConnectorSettingsTypes`, get, virtual): Типы настроек.
    - `IsDemo` (`bool`, get, virtual): Тестовая среда.
    - `MarketDataDelayPeriod` (`MarketDataDelayPeriods`, get, virtual): Период задержки данных.
  - **События**:
    - `PropertyChanged` (`PropertyChangedEventHandler`): Изменение свойства.
  - **Пример**:
    ```csharp
    public class MyConnectorSettings : BaseConnectorSettings<MyConnector>
    {
        protected override IDataFeedConnector OnCreateConnector(string dataPath)
        {
            return new MyConnector(dataPath);
        }

        protected override void OnApplySettings(IDataFeedConnector connector)
        {
            // Применение настроек
        }
    }
    ```

- **ConnectorCategories**: Класс для категорий коннекторов (детали отсутствуют).

#### 4.3.2. Интерфейсы
- **IConnectorSettings**:
  - **Методы**:
    - `IDataFeedConnector CreateConnector(string dataPath)`
    - `void ApplySettings(IDataFeedConnector connector)`
    - `bool CheckSupported(out string? errorMessage)`
  - **Свойства**:
    - `Type` (`string`, get)
    - `Description` (`string`, get)
    - `Logo` (`Uri`, get)
    - `Id` (`Guid`, get/set)
    - `Name` (`string`, get/set)
    - `IsMarketDataEnabled` (`bool`, get/set)
    - `IsAutoConnectEnabled` (`bool`, get/set)
    - `Features` (`ConnectorFeatures`, get)
    - `SettingsTypes` (`ConnectorSettingsTypes`, get)
    - `IsDemo` (`bool`, get)
    - `MarketDataDelayPeriod` (`MarketDataDelayPeriods`, get)
  - **Пример**:
    ```csharp
    public class MySettings : IConnectorSettings
    {
        public IDataFeedConnector CreateConnector(string dataPath) => new MyConnector(dataPath);
        public void ApplySettings(IDataFeedConnector connector) { }
        public bool CheckSupported(out string? errorMessage) { errorMessage = null; return true; }
        public string Type => "MyConnector";
        public string Description => "Custom Connector";
        public Uri Logo => new Uri("http://example.com/logo.png");
        public Guid Id { get; set; }
        public string Name { get; set; }
        public bool IsMarketDataEnabled { get; set; }
        public bool IsAutoConnectEnabled { get; set; }
        public ConnectorFeatures Features => ConnectorFeatures.MarketData;
        public ConnectorSettingsTypes SettingsTypes => ConnectorSettingsTypes.Properties;
        public bool IsDemo => false;
        public MarketDataDelayPeriods MarketDataDelayPeriod => MarketDataDelayPeriods.None;
    }
    ```

- **IConnectorSettingsAction**:
  - **Методы**:
    - `bool? string Do(ConnectorSettingsPostActions postActions)`: Выполняет действие.
  - **Свойства**:
    - `Title` (`string`, get): Заголовок действия.
  - **Атрибуты**:
    - `isSuccess` (`bool`): Успех выполнения.
    - `message` (`bool? string`): Сообщение результата.
  - **Пример**:
    ```csharp
    public class MyAction : IConnectorSettingsAction
    {
        public bool isSuccess { get; set; }
        public bool? string message { get; set; }
        public string Title => "My Action";
        public bool? string Do(ConnectorSettingsPostActions postActions)
        {
            isSuccess = true;
            message = "Success";
            return (true, "Action completed");
        }
    }
    ```

- **IConnectorSettingsSupportInitialization**:
  - **Методы**:
    - `void Initialize(string dataPath, bool throwErrors)`: Инициализация настроек.
  - **Пример**:
    ```csharp
    public class MyInitSettings : IConnectorSettingsSupportInitialization
    {
        public void Initialize(string dataPath, bool throwErrors)
        {
            if (throwErrors && string.IsNullOrEmpty(dataPath))
                throw new ArgumentException("Invalid data path");
        }
    }
    ```

- **ILoginPasswordConnectorSettings**: Интерфейс для настроек с логином и паролем (детали отсутствуют).
- **ITypedConnectorSettings**:
  - **Свойства**:
    - `ConnectionType` (`string`, get/set): Тип соединения.
  - **Пример**:
    ```csharp
    public class MyTypedSettings : ITypedConnectorSettings
    {
        public string ConnectionType { get; set; } = "WebSocket";
    }
    ```

#### 4.3.3. Перечисления
- **ConnectorFeatures**:
  - `None` (0x0): Без функций.
  - `MarketData` (0x1): Рыночные данные.
  - `Executions` (0x2): Исполнение сделок.
  - `StopOrders` (0x4): Стоп-ордера.
  - `OcoOrders` (0x8): OCO-ордера.
  - **Пример**:
    ```csharp
    var features = ConnectorFeatures.MarketData | ConnectorFeatures.Executions;
    ```

- **ConnectorSettingsTypes**:
  - `None` (0x00): Без настроек.
  - `Description` (0x01): Описание.
  - `Properties` (0x02): Свойства.
  - **Пример**:
    ```csharp
    var settingsType = ConnectorSettingsTypes.Properties;
    ```

- **ConnectorSettingsPostActions**:
  - `None` (0x0): Без действий.
  - `ShowMessage` (0x1): Показать сообщение.
  - `SaveSettings` (0x2): Сохранить настройки.
  - `UpdateTitle` (0x4): Обновить заголовок.
  - **Пример**:
    ```csharp
    var postAction = ConnectorSettingsPostActions.SaveSettings;
    ```

## 5. Примеры интеграции

### 5.1. Создание индикатора с настройками

**Пример** (в предположении использования StimulScript или C# для ATAS):
```csharp
using OFT.Attributes;
using OFT.Attributes.Editors;

public class MyIndicator : Indicator
{
    [NumericEditor(NumericEditorTypes.Spin, 1, 100, Step = 1)]
    public int Period { get; set; } = 14;

    [ComboBoxEditor(new[] { "SMA", "EMA" }, DisplayMember = "Name", ValueMember = "Value")]
    public string MaType { get; set; } = "SMA";

    [CheckEditor("Enable Alerts")]
    public bool EnableAlerts { get; set; }

    [Mask(MaskTypes.DateTime, "dd.MM.yyyy")]
    public DateTime StartDate { get; set; } = DateTime.Now;

    protected override void OnCalculate(int barIndex)
    {
        // Логика индикатора
        if (EnableAlerts)
        {
            // Отправка уведомления
        }
    }
}
```

### 5.2. Настройка коннектора

**Пример**:
```csharp
using OFT.Core;
using ATAS.DataFeedsCore;

public class MyConnectorSettings : BaseConnectorSettings<MyConnector>
{
    public MyConnectorSettings()
    {
        Id = Guid.NewGuid();
        Name = "My Connector";
        IsMarketDataEnabled = true;
    }

    protected override IDataFeedConnector OnCreateConnector(string dataPath)
    {
        return new MyConnector(dataPath);
    }

    protected override void OnApplySettings(IDataFeedConnector connector)
    {
        // Применение настроек
        ((MyConnector)connector).Configure(this);
    }

    public override bool CheckSupported(out string? errorMessage)
    {
        errorMessage = null;
        return true;
    }
}

public class MyConnector : AsyncConnector
{
    public MyConnector(string dataPath) : base() { }
    public void Configure(MyConnectorSettings settings) { }
}
```

## 6. Рекомендации для технических заданий

Для создания индикаторов или коннекторов с использованием OFT Framework:

1. **Опишите задачу**:
   - Тип индикатора: трендовый, осциллятор, объёмный.
   - Параметры: числовые, текстовые, выбор из списка.
   - Источник данных: рыночные данные, сделки, DOM.

2. **Укажите параметры**:
   - Тип редактора: чекбокс, выпадающий список, числовой.
   - Ограничения: минимальные/максимальные значения, формат ввода.
   - Поведение: задержка отправки, автодополнение.

3. **Задайте сценарии**:
   - Условия расчёта индикатора.
   - Логирование и уведомления.
   - Подключение к данным через коннектор.

4. **Пример ТЗ**:
   > Создать индикатор скользящей средней (SMA/EMA). Параметры: период (числовой, 1–100, шаг 1), тип MA (выпадающий список: SMA, EMA), включение уведомлений (чекбокс). Использовать рыночные данные через коннектор с поддержкой `MarketData`. Формат даты для фильтра данных: `dd.MM.yyyy`.

## 7. Полный список классов, интерфейсов и перечислений

### 7.1. Пространство имён `OFT.Attributes`
- **Классы**: `Extensions`, `FeatureIdAttribute`, `HelpLinkAttribute`, `IgnoreCloneAttribute`, `LogoAttribute`, `MappingAttribute`, `ParameterAttribute`, `ReferralLinkAttribute`.
- **Перечисления**: `DataSeriesTypes`.

### 7.2. Пространство имён `OFT.Attributes.Editors`
- **Классы**: `CheckEditorAttribute`, `ComboBoxEditorAttribute`, `DataSeriesEditorAttribute`, `IsExpandedAttribute`, `MaskAttribute`, `NumericEditorAttribute`, `PostValueModeAttribute`, `SelectDirectoryEditorAttribute`, `SelectFileEditorAttribute`, `TextEditorAttribute`.
- **Перечисления**: `MaskTypes`, `NumericEditorTypes`, `PostValueModes`, `SelectAllOnModes`, `SelectFileTypes`.

### 7.3. Пространство имён `OFT.Core`
- **Классы**: `BaseConnectorSettings<T>`, `ConnectorCategories`.
- **Интерфейсы**: `IConnectorSettings`, `IConnectorSettingsAction`, `IConnectorSettingsSupportInitialization`, `ILoginPasswordConnectorSettings`, `ITypedConnectorSettings`.
- **Перечисления**: `ConnectorFeatures`, `ConnectorSettingsTypes`, `ConnectorSettingsPostActions`.

