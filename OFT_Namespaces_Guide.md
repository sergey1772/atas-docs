# Методичка по пространствам имен `OFT.Attributes`, `OFT.Attributes.Editors` и `OFT.Core` для разработки индикаторов ATAS (.NET 8)

Эта методичка предоставляет полное описание пространств имен `OFT.Attributes`, `OFT.Attributes.Editors` и `OFT.Core`, включая классы, интерфейсы, перечисления и их использование для создания индикаторов на платформе ATAS с использованием C# на .NET 8. Все данные основаны исключительно на предоставленном файле `OFT.txt`, без добавления предположений или сторонней информации. Методичка предназначена для разработчиков, желающих настроить пользовательские интерфейсы индикаторов и управлять подключением к рыночным данным.

## 1. Обзор пространств имен

### 1.1. Пространство имен `OFT.Attributes`

Пространство имен `OFT.Attributes` содержит классы и перечисления для работы с атрибутами объектов, используемых в платформе ATAS.

#### Перечисления

- **DataSeriesTypes**:
  - Описание: Определяет типы серий данных для отображения на графике.
  - Значения:
    - `Band` — серия данных типа "полоса".
    - `Line` — серия данных типа "линия".
    - `Candle` — серия данных типа "свеча".
    - `Value` — серия данных типа "значение".

#### Классы

- **Extensions**:
  - Описание: Класс расширений. Назначение не указано в документации.
- **FeatureIdAttribute**:
  - Описание: Атрибут для идентификации функциональных возможностей. Назначение не указано.
- **HelpLinkAttribute**:
  - Описание: Атрибут для ссылки на справку. Назначение не указано.
- **IgnoreCloneAttribute**:
  - Описание: Атрибут для игнорирования клонирования свойства или поля.
- **LogoAttribute**:
  - Описание: Атрибут для логотипа. Назначение не указано.
- **MappingAttribute**:
  - Описание: Атрибут для маппинга. Назначение не указано.
- **ParameterAttribute**:
  - Описание: Атрибут для параметра. Назначение не указано.
- **ReferralLinkAttribute**:
  - Описание: Атрибут для реферальной ссылки.
  - Конструктор: `ReferralLinkAttribute(string url)`
  - Свойства:
    - `Url` (string, get): Ссылка на реферальную страницу.
  - Файл: `ReferralLinkAttribute.cs`

#### Вложенные пространства имен

- **Editors**: Содержит классы для редакторов (см. раздел 1.2).
- **Properties**: Содержит классы для свойств (детали не предоставлены).

### 1.2. Пространство имен `OFT.Attributes.Editors`

Пространство имен `OFT.Attributes.Editors` содержит классы и перечисления для настройки редакторов в панели параметров индикаторов ATAS. Эти редакторы позволяют создавать удобные интерфейсы для ввода и изменения настроек.

#### Перечисления

- **MaskTypes**:
  - Описание: Определяет типы масок для атрибута `MaskAttribute`.
  - Значения:
    - `None`: Без маски.
    - `DateTime`: Маска для даты и времени.
    - `DateTimeAdvancingCaret`: Маска для даты и времени с автоматическим переходом.
    - `Numeric`: Маска для чисел.
    - `RegEx`: Маска на основе регулярного выражения.
    - `Regular`: Стандартная маска.
    - `Simple`: Простая маска.
    - `TimeSpan`: Маска для временного интервала.
    - `TimeSpanAdvancingCaret`: Маска для временного интервала с автоматическим переходом.

- **NumericEditorTypes**:
  - Описание: Определяет типы редакторов для числовых значений.
  - Значения:
    - `Spin`: Редактор типа "счетчик" с кнопками увеличения/уменьшения.
    - `TrackBar`: Редактор типа "ползунок" для выбора значения в диапазоне.

- **PostValueModes**:
  - Описание: Определяет режимы отправки значений из редактора.
  - Значения:
    - `OnChanged`: Значение отправляется при каждом изменении.
    - `OnLostFocus`: Значение отправляется при потере фокуса.
    - `Delayed`: Значение отправляется с задержкой.

- **SelectAllOnModes**:
  - Описание: Определяет режимы выделения текста в редакторе при получении фокуса.
  - Значения:
    - `None`: Без автоматического выделения.
    - `GotFocus`: Выделение при получении фокуса.
    - `MouseUp`: Выделение при отпускании кнопки мыши.

- **SelectFileTypes**:
  - Описание: Определяет типы файлов для редактора выбора файла.
  - Значения:
    - `Open`: Режим открытия файла.
    - `Save`: Режим сохранения файла.

#### Классы

- **CheckEditorAttribute**:
  - Описание: Атрибут для создания редактора типа "чекбокс" (checkbox) для логических значений.
  - Наследование: `Attribute`
  - Конструктор: `CheckEditorAttribute(string propertyName)`
    - Параметры:
      - `propertyName` (string): Имя свойства, связанного с чекбоксом.
    - Исключения: `ArgumentNullException` (если `propertyName` — `null`).
  - Свойства:
    - `PropertyName` (string, get): Имя связанного свойства.
  - Файл: `CheckEditorAttribute.cs`

- **ComboBoxEditorAttribute**:
  - Описание: Атрибут для создания редактора типа "выпадающий список" (combobox).
  - Наследование: `Attribute`
  - Конструкторы:
    - `ComboBoxEditorAttribute(params object[] itemsSource)`: Задает элементы для выбора.
    - `ComboBoxEditorAttribute(Type typeSource)`: Задает тип, наследующийся от `IEnumerable`.
      - Исключения:
        - `ArgumentNullException`: Если `typeSource` — `null`.
        - `ArgumentException`: Если `typeSource` не наследуется от `IEnumerable` или имеет сложный конструктор.
  - Методы:
    - `IEnumerable GetItemsSource(object instance)`: Возвращает источник данных.
  - Свойства:
    - `DisplayMember` (string, get, set): Имя члена для отображения.
    - `ValueMember` (string, get, set): Имя члена для значений.
    - `ItemsSource` (object[], get, set): Источник данных.
    - `IsTextEditable` (bool, get, set): Разрешено ли редактирование текста.
    - `AutoComplete` (bool, get, set): Включено ли автодополнение.
    - `SelectItemWithNullValue` (bool, get, set): Поиск `null` при пустом значении.
  - Файл: `ComboBoxEditorAttribute.cs`

- **DataSeriesEditorAttribute**:
  - Описание: Атрибут для редактора, выбирающего серию данных (например, цены, объемы).
  - Наследование: `Attribute`
  - Конструктор: `DataSeriesEditorAttribute(DataSeriesTypes dataSeriesType)`
    - Параметры:
      - `dataSeriesType` (DataSeriesTypes): Тип серии данных.
  - Свойства:
    - `DataSeriesType` (DataSeriesTypes, get): Тип серии данных.
  - Файл: `DataSeriesEditorAttribute.cs`

- **IsExpandedAttribute**:
  - Описание: Атрибут, определяющий, развернут ли раздел настроек по умолчанию.
  - Наследование: `Attribute`
  - Конструктор: `IsExpandedAttribute()`
  - Файл: `IsExpandedAttribute.cs`

- **MaskAttribute**:
  - Описание: Атрибут для задания маски ввода для текстовых редакторов.
  - Наследование: `Attribute`
  - Конструкторы:
    - `MaskAttribute()`: Пустой конструктор.
    - `MaskAttribute(MaskTypes type, string mask)`: Задает тип и маску.
  - Свойства:
    - `Culture` (string, get, set): Культура для форматирования.
    - `Mask` (string, get, set): Строка маски.
    - `MaskAsDisplayFormat` (bool, get, set): Использовать маску только для отображения.
    - `MaskType` (MaskTypes, get, set): Тип маски.
    - `MethodName` (string, get, set): Имя метода для дополнительной обработки.
  - Файл: `MaskAttribute.cs`

- **NumericEditorAttribute**:
  - Описание: Атрибут для редактора числовых значений.
  - Наследование: `Attribute`
  - Конструкторы:
    - `NumericEditorAttribute()`
    - `NumericEditorAttribute(NumericEditorTypes editorType, double minValue)`
    - `NumericEditorAttribute(NumericEditorTypes editorType, double minValue, double maxValue)`
    - `NumericEditorAttribute(double minValue, double maxValue)`
    - `NumericEditorAttribute(double minValue)`
  - Свойства:
    - `EditorType` (NumericEditorTypes, get, set): Тип редактора.
    - `MinValue` (double?, get): Минимальное значение.
    - `MaxValue` (double?, get): Максимальное значение.
    - `Step` (double, get, set): Шаг изменения.
    - `DisplayFormat` (string, get, set): Формат отображения.
  - Файл: `NumericEditorAttribute.cs`

- **PostValueModeAttribute**:
  - Описание: Атрибут, определяющий режим отправки значений из редактора.
  - Наследование: `Attribute`
  - Конструкторы:
    - `PostValueModeAttribute()`
    - `PostValueModeAttribute(PostValueModes mode)`
  - Свойства:
    - `Mode` (PostValueModes, get): Режим отправки.
    - `DelayMilliseconds` (int, get, set): Задержка в миллисекундах.
    - `SelectAllOnMode` (SelectAllOnModes, get, set): Режим выделения текста (по умолчанию `MouseUp`).
  - Файл: `PostValueModeAttribute.cs`

- **SelectDirectoryEditorAttribute**:
  - Описание: Атрибут для редактора выбора директории.
  - Наследование: `Attribute`
  - Конструкторы:
    - `SelectDirectoryEditorAttribute()`
    - `SelectDirectoryEditorAttribute(SpecialFolder initialSpecialFolder)`
  - Свойства:
    - `IsTextEditable` (bool, get, set): Разрешено ли редактирование текста.
    - `ShowNewFolderButton` (bool, get, set): Отображение кнопки "Новая папка".
    - `InitialSpecialFolder` (SpecialFolder?, get): Начальная специальная папка.
  - Файл: `SelectDirectoryEditorAttribute.cs`

- **SelectFileEditorAttribute**:
  - Описание: Атрибут для редактора выбора файла.
  - Наследование: `Attribute`
  - Конструкторы:
    - `SelectFileEditorAttribute()`
    - `SelectFileEditorAttribute(SpecialFolder initialSpecialFolder)`
  - Свойства:
    - `IsTextEditable` (bool, get, set): Разрешено ли редактирование текста.
    - `SelectFileType` (SelectFileTypes, get, set): Тип выбора файла.
    - `Multiselect` (bool, get, set): Разрешить множественный выбор.
    - `InitialFolder` (string, get, set): Начальная папка.
    - `InitialSpecialFolder` (SpecialFolder?, get): Начальная специальная папка.
    - `Filter` (string, get, set): Фильтр файлов.
  - Файл: `SelectFileEditorAttribute.cs`

- **TextEditorAttribute**:
  - Описание: Атрибут для текстового редактора.
  - Наследование: `Attribute`
  - Конструктор: `TextEditorAttribute(bool isMultiline, int maxHeight = 50)`
    - Параметры:
      - `isMultiline` (bool): Разрешить многострочный ввод.
      - `maxHeight` (int): Максимальная высота (по умолчанию 50).
  - Файл: `TextEditorAttribute.cs`

### 1.3. Пространство имен `OFT.Core`

Пространство имен `OFT.Core` содержит классы и интерфейсы для управления настройками коннекторов, обеспечивающих подключение к рыночным данным.

#### Перечисления

- **ConnectorFeatures**:
  - Описание: Определяет функции коннектора.
  - Значения:
    - `None` (0x0): Отсутствие функций.
    - `MarketData` (0x1): Получение рыночных данных.
    - `Executions` (0x2): Обработка исполнений сделок.
    - `StopOrders` (0x4): Поддержка стоп-ордеров.
    - `OcoOrders` (0x8): Поддержка OCO-ордеров.

- **ConnectorSettingsTypes**:
  - Описание: Определяет типы настроек коннектора.
  - Значения:
    - `None` (0x00): Отсутствие настроек.
    - `Description` (0x01): Настройки для описания.
    - `Properties` (0x02）： Настройки для свойств.

- **ConnectorSettingsPostActions**:
  - Описание: Определяет действия после настройки коннектора.
  - Значения:
    - `None` (0x0): Отсутствие действий.
    - `ShowMessage` (0x1): Показать сообщение.
    - `SaveSettings` (0x2): Сохранить настройки.
    - `UpdateTitle` (0x4): Обновить заголовок.

#### Классы

- **BaseConnectorSettings<T>**:
  - Описание: Абстрактный шаблонный класс для настроек коннектора.
  - Наследование: `IConnectorSettings`, `INotifyPropertyChanged`
  - Конструктор: `protected BaseConnectorSettings()`
  - Публичные методы:
    - `IDataFeedConnector CreateConnector(string dataPath)`: Создает коннектор.
    - `void ApplySettings(IDataFeedConnector connector)`: Применяет настройки.
    - `virtual bool CheckSupported(out string? errorMessage)`: Проверяет поддержку коннектора.
    - `override string ToString()`: Возвращает строковое представление.
  - Защищенные методы:
    - `abstract IDataFeedConnector OnCreateConnector(string dataPath)`: Абстрактный метод создания коннектора.
    - `abstract void OnApplySettings(IDataFeedConnector connector)`: Абстрактный метод применения настроек.
    - `abstract void OnApplySettings(T connector)`: Абстрактный метод применения настроек для типа `T`.
    - `void RaisePropertyChanged(string propertyName)`: Вызывает событие изменения свойства.
    - `bool SetProperty<TValue>(ref TValue storage, TValue newValue, string propertyName, Action<TValue, TValue> onChanged = null)`: Устанавливает свойство с уведомлением.
  - Свойства:
    - `string Type` (get, set): Тип коннектора.
    - `string Description` (get, set): Описание коннектора.
    - `Uri Logo` (get): URI логотипа.
    - `Guid Id` (get, set): Уникальный идентификатор.
    - `virtual string DisplayName` (get, set): Отображаемое имя.
    - `string Name` (get, set): Имя коннектора.
    - `bool IsMarketDataEnabled` (get, set): Включены ли рыночные данные.
    - `bool IsAutoConnectEnabled` (get, set): Включено ли автоподключение.
    - `bool AllowUpdatePositionsPnL` (get, set): Разрешено ли обновление позиций.
    - `TimeOnly? RefreshSecuritiesTime` (get, set): Время обновления инструментов.
    - `abstract ConnectorFeatures Features` (get): Возможности коннектора.
    - `virtual ConnectorSettingsTypes SettingsTypes` (get): Типы настроек.
    - `virtual bool IsDemo` (get): Используется ли тестовая среда.
    - `virtual MarketDataDelayPeriods MarketDataDelayPeriod` (get): Период задержки данных.
  - События:
    - `PropertyChangedEventHandler PropertyChanged`: Событие изменения свойства.
  - Файл: `IConnectorSettings.cs`

- **ConnectorCategories**:
  - Описание: Класс для категорий коннекторов (детали не предоставлены).

#### Интерфейсы

- **IConnectorSettings**:
  - Описание: Интерфейс для настроек коннектора.
  - Методы:
    - `IDataFeedConnector CreateConnector(string dataPath)`: Создает коннектор.
    - `void ApplySettings(IDataFeedConnector connector)`: Применяет настройки.
    - `bool CheckSupported(out string? errorMessage)`: Проверяет поддержку.
  - Свойства:
    - `string Type` (get): Тип коннектора.
    - `string Description` (get): Описание.
    - `Uri Logo` (get): URI логотипа.
    - `Guid Id` (get, set): Уникальный идентификатор.
    - `string Name` (get, set): Имя коннектора.
    - `bool IsMarketDataEnabled` (get, set): Включены ли рыночные данные.
    - `bool IsAutoConnectEnabled` (get, set): Включено ли автоподключение.
    - `ConnectorFeatures Features` (get): Возможности коннектора.
    - `ConnectorSettingsTypes SettingsTypes` (get): Типы настроек.
    - `bool IsDemo` (get): Тестовая среда.
    - `MarketDataDelayPeriods MarketDataDelayPeriod` (get): Период задержки данных.
  - Файл: `IConnectorSettings.cs`

- **IConnectorSettingsAction**:
  - Описание: Интерфейс для действий с настройками коннектора.
  - Методы:
    - `bool? string Do(ConnectorSettingsPostActions postActions)`: Выполняет действие.
  - Свойства:
    - `string Title` (get): Заголовок действия.
  - Атрибуты:
    - `bool isSuccess`: Успех выполнения.
    - `bool? string message`: Сообщение о результате.
  - Файл: `IConnectorSettings.cs`

- **IConnectorSettingsSupportInitialization**:
  - Описание: Интерфейс для поддержки инициализации настроек.
  - Наследование: `IConnectorSettings`
  - Методы:
    - `void Initialize(string dataPath, bool throwErrors)`: Инициализирует настройки.
  - Файл: `IConnectorSettings.cs`

- **ILoginPasswordConnectorSettings**:
  - Описание: Интерфейс для настроек с логином и паролем (детали не предоставлены).
  - Наследование: `IConnectorSettings`

- **ITypedConnectorSettings**:
  - Описание: Интерфейс для типизированных настроек.
  - Наследование: `IConnectorSettings`
  - Свойства:
    - `string ConnectionType` (get, set): Тип соединения.
  - Файл: `IConnectorSettings.cs`

## 2. Использование в разработке индикаторов ATAS

### 2.1. Настройка пользовательского интерфейса индикаторов

Пространство имен `OFT.Attributes.Editors` предоставляет инструменты для создания удобных интерфейсов настроек индикаторов. Вот пример использования атрибутов для настройки параметров индикатора:

```csharp
using OFT.Attributes.Editors;

public class MyIndicator : Indicator
{
    [Display(Name = "Enable Volume", GroupName = "Settings")]
    [CheckEditor]
    public bool EnableVolume { get; set; } = true;

    [Display(Name = "Period", GroupName = "Settings")]
    [NumericEditor(NumericEditorTypes.Spin, 1, 100)]
    public int Period { get; set; } = 14;

    [Display(Name = "Chart Type", GroupName = "Settings")]
    [ComboBoxEditor(new object[] { "Line", "Candle", "Bar" })]
    public string ChartType { get; set; } = "Line";

    [Display(Name = "Start Date", GroupName = "Settings")]
    [Mask(MaskTypes.DateTime, "dd.MM.yyyy")]
    public DateTime StartDate { get; set; } = DateTime.Now;

    protected override void OnCalculate(int bar, decimal value)
    {
        if (EnableVolume)
        {
            var candle = GetCandle(bar);
            // Логика расчета с учетом объема
        }
    }
}
```

**Пояснения**:
- `CheckEditor`: Добавляет чекбокс для свойства `EnableVolume`.
- `NumericEditor`: Создает счетчик для `Period` с диапазоном 1–100.
- `ComboBoxEditor`: Создает выпадающий список для `ChartType`.
- `Mask`: Задает формат даты для `StartDate`.

### 2.2. Работа с коннекторами

Пространство имен `OFT.Core` используется для настройки коннекторов, обеспечивающих доступ к рыночным данным. Пример реализации пользовательского коннектора:

```csharp
using OFT.Core;

public class CustomConnectorSettings : BaseConnectorSettings<CustomConnector>
{
    public override string DisplayName { get; set; } = "Custom Connector";

    public override ConnectorFeatures Features => ConnectorFeatures.MarketData;

    protected override IDataFeedConnector OnCreateConnector(string dataPath)
    {
        return new CustomConnector(dataPath);
    }

    protected override void OnApplySettings(IDataFeedConnector connector)
    {
        if (connector is CustomConnector customConnector)
        {
            customConnector.Configure(this);
        }
    }

    protected override void OnApplySettings(CustomConnector connector)
    {
        connector.Configure(this);
    }
}

public class CustomConnector : IDataFeedConnector
{
    public CustomConnector(string dataPath) { }

    public void Configure(CustomConnectorSettings settings) { }
}
```

**Пояснения**:
- `BaseConnectorSettings<T>`: Шаблонный класс для настроек коннектора.
- `ConnectorFeatures`: Указывает, что коннектор поддерживает рыночные данные.
- `OnCreateConnector` и `OnApplySettings`: Реализуют логику создания и настройки коннектора.

### 2.3. Интеграция с индикаторами

Для использования коннектора в индикаторе:

```csharp
public class MyIndicator : Indicator
{
    private readonly CustomConnectorSettings _connectorSettings;

    public MyIndicator()
    {
        _connectorSettings = new CustomConnectorSettings();
    }

    protected override void OnCalculate(int bar, decimal value)
    {
        // Получение данных через коннектор
        var connector = _connectorSettings.CreateConnector("path/to/data");
        // Логика обработки данных
    }
}
```

## 3. Рекомендации по разработке

### 3.1. Использование атрибутов редакторов

- **Выбор подходящего редактора**:
  - Используйте `CheckEditorAttribute` для булевых параметров.
  - Применяйте `NumericEditorAttribute` для числовых параметров с заданным диапазоном.
  - Используйте `ComboBoxEditorAttribute` для выбора из списка опций.
  - Применяйте `MaskAttribute` для форматированного ввода (даты, числа).

- **Оптимизация интерфейса**:
  - Используйте `IsExpandedAttribute` для управления видимостью групп настроек.
  - Задавайте `DisplayFormat` в `NumericEditorAttribute` для читаемого отображения чисел.

- **Обработка пользовательского ввода**:
  - Используйте `PostValueModeAttribute` с режимом `Delayed` для снижения нагрузки при частых изменениях.
  - Настройте `SelectAllOnMode` для удобного редактирования текста.

### 3.2. Работа с коннекторами

- **Проверка поддержки**:
  - Реализуйте метод `CheckSupported` для проверки совместимости коннектора с системой:

    ```csharp
    public override bool CheckSupported(out string? errorMessage)
    {
        errorMessage = null;
        if (!IsSupportedPlatform())
        {
            errorMessage = "Platform not supported";
            return false;
        }
        return true;
    }
    ```

- **Инициализация**:
  - Используйте `IConnectorSettingsSupportInitialization` для инициализации настроек:

    ```csharp
    public void Initialize(string dataPath, bool throwErrors)
    {
        try
        {
            // Инициализация
        }
        catch (Exception ex)
        {
            if (throwErrors)
                throw;
        }
    }
    ```

- **Обработка ошибок**:
  - Проверяйте возвращаемые значения методов `Do` в `IConnectorSettingsAction` для обработки ошибок.

### 3.3. Тестирование и отладка

- **Тестирование интерфейса**:
  - Проверяйте поведение редакторов при крайних значениях (например, минимальный/максимальный `Period` в `NumericEditorAttribute`).
  - Убедитесь, что маски (`MaskAttribute`) корректно ограничивают ввод.

- **Логирование**:
  - Добавляйте временное логирование для отслеживания изменений настроек:

    ```csharp
    protected override void OnApplySettings(IDataFeedConnector connector)
    {
        Console.WriteLine($"Applying settings: {this}");
        base.OnApplySettings(connector);
    }
    ```

- **Тестирование коннекторов**:
  - Проверяйте поддержку коннектора на разных платформах.
  - Тестируйте инициализацию с различными значениями `dataPath`.

## 4. Заключение

Эта методичка охватывает все аспекты работы с пространствами имен `OFT.Attributes`, `OFT.Attributes.Editors` и `OFT.Core` для разработки индикаторов на платформе ATAS. Она включает:

- Полное описание классов, интерфейсов и перечислений.
- Примеры использования атрибутов для настройки интерфейса индикаторов.
- Примеры реализации и интеграции коннекторов для доступа к рыночным данным.
- Рекомендации по тестированию и отладке.

Для дальнейшей работы рекомендуется:

- Изучить примеры кода и адаптировать их под конкретные задачи.
- Использовать ATAS SDK для тестирования интерфейсов и коннекторов.
- Проверять актуальность документации в случае обновлений платформы.

