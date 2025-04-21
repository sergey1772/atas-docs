

## Методичка по разработке пользовательских индикаторов ATAS

**Основы**

Для разработки пользовательского индикатора необходимо выполнить следующие шаги:

1.  **Создать проект библиотеки классов в Visual Studio.**
    > *Комментарий:*  Первый шаг - это создание основы для вашего индикатора в среде разработки Visual Studio. Выберите тип проекта "Class Library" (Библиотека классов).

2.  **Добавить ссылку на `ATAS.Indicators.dll`, который находится в папке с установленной программой, в проект.**
    > *Комментарий:*  Чтобы ваш индикатор мог взаимодействовать с платформой ATAS, необходимо подключить библиотеку `ATAS.Indicators.dll`.  Эта библиотека содержит все необходимые классы и функции для создания индикаторов.  Найдите папку, куда установлен ATAS, и в ней найдите `ATAS.Indicators.dll` (обычно в подпапке `bin` или `версия`). В Visual Studio в вашем проекте добавьте ссылку на этот DLL-файл.

3.  **Переопределить класс `Indicator`.**
    > *Комментарий:*  Ваш индикатор должен быть классом, который наследуется от базового класса `Indicator` из библиотеки `ATAS.Indicators.dll`.  Это позволит вашему классу использовать функциональность индикаторов ATAS.
    ```csharp
    using ATAS.Indicators;

    public class MyCustomIndicator : Indicator
    {
        protected override void OnCalculate(int bar, decimal value)
        {
        }
    }
    ```
    > *Комментарий:*  Пример кода показывает создание класса `MyCustomIndicator`, который наследуется от `Indicator`.  Метод `OnCalculate` будет содержать основную логику расчета индикатора.

4.  **Добавить необходимые `DataSeries` - `ValueDataSeries`, `RangeDataSeries`, `CandleDataSeries`, `CandlePartSeries`, `IndicatorSeries`, `PriceSelectionDataSeries`, `PaintbarsDataSeries` или `ObjectDataSeries` - в конструкторе.**
    > *Комментарий:* `DataSeries` используются для хранения и обработки данных, необходимых для индикатора.  В конструкторе вашего индикатора вы должны добавить те типы `DataSeries`, которые вам понадобятся.  Например, `ValueDataSeries` для хранения простых значений, `CandleDataSeries` для работы со свечами и т.д.
    ```csharp
    using ATAS.Indicators;

    public class MyCustomIndicator() : Indicator
    {
        DataSeries.Add(new ValueDataSeries("Values"));
    }
    ```
    > *Комментарий:*  В примере кода в конструкторе индикатора добавляется `ValueDataSeries` с именем "Values". Это создаст набор данных, который можно будет использовать для отображения значений индикатора.

5.  **Описать логику работы индикатора в функции `OnCalculate()`.**
    > *Комментарий:*  Функция `OnCalculate()` - это сердце вашего индикатора. В ней вы должны написать код, который будет рассчитывать значения индикатора для каждого бара.  Эта функция вызывается для каждого нового бара и для каждого тика.
    ```csharp
    using ATAS.Indicators;

    public class MyCustomIndicator() : Indicator
    {
        protected override void OnCalculate (int bar, decimal value)
        {
            var period = 10;
            var start = Math.Max(0, bar - period + 1);
            var count = Math.Min(bar + 1, period);
            var max = (decimal) SourceDataSeries[start];
            for (var i = start + 1; i < start + count; i++)
            {
                max = Math.Max(max, (decimal)SourceDataSeries[i]);
            }
            this[bar] = max;
        }
    }
    ```
    > *Комментарий:*  Пример кода показывает расчет максимального значения `DataSeries` за последние 10 баров.  `SourceDataSeries` - это входной `DataSeries`, который пользователь может выбрать в настройках индикатора. `this[bar] = max;`  присваивает рассчитанное максимальное значение текущему бару в выходной `DataSeries` индикатора.

6.  **Скомпилировать библиотеку.**
    > *Комментарий:*  После написания кода индикатора необходимо скомпилировать проект в Visual Studio. В результате компиляции будет создан DLL-файл.

7.  **Поместить полученный DLL-файл в папку `Documents/ATAS/Indicators`.**
    > *Комментарий:*  Чтобы ATAS мог использовать ваш индикатор, DLL-файл нужно скопировать в специальную папку `Indicators` в папке `ATAS` в ваших документах.

8.  **После добавления файла с индикатором на нижней панели появится мигающая кнопка, указывающая на то, что список индикаторов был обновлен. Разработанный индикатор появится в списке индикаторов платформы после нажатия этой кнопки.**
    > *Комментарий:*  После копирования DLL-файла в нужную папку, ATAS обнаружит новый индикатор при следующем запуске или при обновлении списка индикаторов (через мигающую кнопку).  Ваш индикатор станет доступен для добавления на график.

**Обновление списка индикаторов**

> *Комментарий:*  Если вы вносите изменения в код индикатора и перекомпилируете DLL, вам нужно обновить список индикаторов в ATAS, чтобы изменения вступили в силу.

Вы можете получить примеры из нашего репозитория GitHub [здесь](ссылка на GitHub репозиторий). Вам нужно добавить текущую версию `ATAS.Indicators.dll` в `references` этого проекта.

**Пример разработки индикатора**

**Создайте класс `MyCustomIndicator` и унаследуйте его от `Indicator`:**

```csharp
using ATAS.Indicators;

public class MyCustomIndicator : Indicator
{
    protected override void OnCalculate(int bar, decimal value)
    {
    }
}
```

**Определите конструктор. Добавьте необходимое количество `DataSeries`, если требуется. Каждый индикатор по умолчанию имеет один `ValueDataSeries`. Добавьте еще один `ValueDataSeries` в этом примере.**

```csharp
using ATAS.Indicators;

public class MyCustomIndicator() : Indicator
{
    DataSeries.Add(new ValueDataSeries("Values"));
}
```

**Определите логику работы индикатора. Например, нам нужен индикатор, который показывает максимальные значения входящего `DataSeries` за последние 10 баров:**

```csharp
using ATAS.Indicators;

public class MyCustomIndicator() : Indicator
{
    protected override void OnCalculate (int bar, decimal value)
    {
        var period = 10;
        var start = Math.Max(0, bar - period + 1);
        var count = Math.Min(bar + 1, period);
        var max = (decimal) SourceDataSeries[start];
        for (var i = start + 1; i < start + count; i++)
        {
            max = Math.Max(max, (decimal)SourceDataSeries[i]);
        }
        this[bar] = max;
    }
}
```

Метод `OnCalculate` вызывается для каждого бара в истории и далее на каждом тике. Все бары и значения соответствующих `DataSeries` имеют порядковые номера. Номер 0 относится к самому раннему бару на графике, следующий бар имеет номер 1 и так далее. Значение, которое передается в этот метод, зависит от Источника (Source), который пользователь может выбрать. В качестве источника можно использовать `Open`, `High`, `Low` и `Close` свечей, а также значения других индикаторов.

**Значение источника**

> *Комментарий:*  Раздел "Источник" в настройках индикатора позволяет пользователю выбирать, какие данные будут использоваться для расчета индикатора (например, цена закрытия, цена открытия, High, Low, Volume или значения другого индикатора).

![Изображение настроек источника индикатора](изображение_источника_индикатора.png)
> *Комментарий:*  На изображении показан пример настроек индикатора, где в разделе "Источник" выбран "Close(ESZ9)". Это означает, что индикатор будет использовать цену закрытия инструмента ESZ9 для своих расчетов.

Могут быть ситуации, когда индикатору не нужно предоставлять возможность выбора источника (например, это индикаторы, которые используют только данные свечей в своих расчетах). В таком случае раздел "Источник" можно скрыть. Чтобы сделать это, необходимо вызвать базовый конструктор с параметром `true`.

```csharp
public class MyCustomIndicator(): base(true)
{
}
```
> *Комментарий:*  Вызов `base(true)` в конструкторе индикатора скрывает раздел "Источник" в настройках индикатора.  Это полезно, когда индикатор всегда использует один и тот же источник данных и нет необходимости давать пользователю возможность его менять.







## Методичка по созданию индикаторов ATAS на основе PDF-файла

### Настройка индикатора

#### Внешние параметры индикатора

Для того чтобы иметь возможность изменять тот или иной параметр индикатора, его следует реализовать в виде публичного свойства:

```csharp
public int Period { get; set; }
```
> **Аннотация:** Это объявление публичного свойства `Period` типа `int`. Ключевое слово `public` делает свойство доступным извне класса. `{ get; set; }`  обеспечивает автоматическую реализацию методов `get` (получение значения) и `set` (установка значения) для этого свойства.

Для того чтобы индикатор пересчитывался при изменении свойства, необходимо вызвать метод `RecalculateValues()`.

```csharp
private int _size = 10;
public int Size
{
    get { return _size; }
    set
    {
        _size = value;
        RecalculateValues();
    }
}
```
> **Аннотация:** Здесь показан пример свойства `Size`.
> - `private int _size = 10;` -  приватное поле `_size` для хранения значения размера, инициализированное значением 10. `private` означает, что это поле доступно только внутри класса.
> - `public int Size` - публичное свойство `Size` типа `int`.
> - `get { return _size; }` - метод `get` возвращает текущее значение приватного поля `_size`.
> - `set { ... }` - метод `set` устанавливает новое значение.
>     - `_size = value;` -  присваивает входящее значение (`value`) приватному полю `_size`.
>     - `RecalculateValues();` - вызывает метод `RecalculateValues()`, который инициирует пересчет индикатора при изменении значения свойства `Size`.

Атрибут `Display`, в котором указывается отображаемое имя, категория и порядковый номер свойства, может быть установлен для каждого свойства. Этот атрибут находится в пространстве имен `System.ComponentModel.DataAnnotations`.

```csharp
using System.ComponentModel.DataAnnotations;

public class SampleIndicator : ATAS.Indicators.Indicator
{
    [Display (GroupName = "GroupName", Name = "PropertyName", Order = 10)]
    public int Type { get; set; }

    protected override void OnCalculate(int bar, decimal value)
    {
    }
}
```
> **Аннотация:**
> - `using System.ComponentModel.DataAnnotations;` -  добавление пространства имен `System.ComponentModel.DataAnnotations` для использования атрибута `Display`.
> - `[Display (GroupName = "GroupName", Name = "PropertyName", Order = 10)]` - атрибут `Display` используется для настройки отображения свойства в настройках индикатора.
>     - `GroupName = "GroupName"` - задает имя группы, к которой будет принадлежать свойство в настройках.
>     - `Name = "PropertyName"` -  задает отображаемое имя свойства для пользователя.
>     - `Order = 10` -  устанавливает порядковый номер свойства в списке настроек.
> - `public int Type { get; set; }` -  публичное свойство `Type` типа `int`, к которому применен атрибут `Display`.
> - `protected override void OnCalculate(int bar, decimal value)` -  переопределенный метод `OnCalculate`, который будет содержать основную логику расчета индикатора. Пока пустой.

#### Типы параметров

Примеры использования различных типов свойств индикатора приведены в индикаторе `Properties`.

#### Типы значений

Типы значений, такие как `int`, `decimal`, `boolean`, `enum`, `string` и т.д.

Вот несколько примеров параметров этих типов:

```csharp
[Display (Name = "Integer", GroupName = "Examples")]
public int Integer { get; set; }

[Display (Name = "Decimal", GroupName = "Examples")]
public decimal Decimal { get; set; }

[Display (Name = "Boolean", GroupName = "Examples")]
public bool Boolean { get; set; }

[Display (Name = "Heatmap", GroupName = "Examples")]
public HeatmapTypes HeatmapType { get; set; }

[Display (Name = "Date", GroupName = "Examples")]
public DateTime DateTime { get; set; } = new (2020, 01, 01);

[Display (Name = "Label", GroupName = "Examples")]
public string Label { get; set; } = "Some Label";
```
> **Аннотация:** Примеры различных типов свойств с атрибутом `Display`.
> - `public int Integer { get; set; }` - свойство целого числа.
> - `public decimal Decimal { get; set; }` - свойство десятичного числа.
> - `public bool Boolean { get; set; }` - свойство логического типа (истина/ложь).
> - `public HeatmapTypes HeatmapType { get; set; }` - свойство перечисления `HeatmapTypes` (предполагается, что `HeatmapTypes` определен где-то еще).
> - `public DateTime DateTime { get; set; } = new (2020, 01, 01);` - свойство даты и времени, инициализированное значением 1 января 2020 года.
> - `public string Label { get; set; } = "Some Label";` - строковое свойство, инициализированное значением "Some Label".

### Фильтры

Производные типы от класса `Filter`. Такие типы позволяют реализовать в одном свойстве возможность хранения данных и контроля доступа к этим данным.

Эти свойства должны быть инициализированы немедленно. Объекты, производные от класса `Filter`, имеют событие `PropertyChanged`, подписываясь на которое можно отслеживать изменения значений в этом свойстве.

```csharp
public class SampleIndicator : ATAS.Indicators.Indicator
{
    [Display (Name = "Filter Decimal", GroupName = "Examples")]
    public Filter FilterDecimal { get; set; } = new();

    [Display (Name = "Filter Integer", GroupName = "Examples")]
    public FilterInt FilterInt { get; set; } = new();

    [Display (Name = "Filter Text", GroupName = "Examples")]
    public FilterString FilterText { get; set; } = new() { Enabled = true, Value = "Value" };

    public SampleIndicator() : base(true)
    {
        DenyToChangePanel = true;
        DataSeries[0].IsHidden = true;

        FilterDecimal.PropertyChanged += OnFilterPropertyChanged;
        FilterInt.PropertyChanged += OnFilterPropertyChanged;
        FilterText.PropertyChanged += OnFilterPropertyChanged;
    }

    private void OnFilterPropertyChanged(object sender, PropertyChangedEventArgs e)
    {
    }

    protected override void OnCalculate(int bar, decimal value)
    {
    }
}
```
> **Аннотация:**
> - `public Filter FilterDecimal { get; set; } = new();` -  свойство `FilterDecimal` типа `Filter` (предполагается, что `Filter`, `FilterInt`, `FilterString` и `PropertyChangedEventArgs` определены в ATAS API). Инициализация новым экземпляром `new()`.
> - `public FilterInt FilterInt { get; set; } = new();` - свойство `FilterInt` типа `FilterInt`, аналогично `FilterDecimal`.
> - `public FilterString FilterText { get; set; } = new() { Enabled = true, Value = "Value" };` - свойство `FilterText` типа `FilterString`, инициализированное с настройками `Enabled = true` и `Value = "Value"`.
> - `public SampleIndicator() : base(true) { ... }` - конструктор класса `SampleIndicator`.
>     - `DenyToChangePanel = true;` -  запрещает изменение панели индикатора пользователем.
>     - `DataSeries[0].IsHidden = true;` - скрывает первую серию данных индикатора (по умолчанию).
>     - `FilterDecimal.PropertyChanged += OnFilterPropertyChanged;` -  подписка на событие `PropertyChanged` свойства `FilterDecimal`. Когда значение `FilterDecimal` изменяется, будет вызван метод `OnFilterPropertyChanged`. Аналогично для `FilterInt` и `FilterText`.
> - `private void OnFilterPropertyChanged(object sender, PropertyChangedEventArgs e) { ... }` -  метод-обработчик события `PropertyChanged`. В данном примере пустой, но здесь можно добавить логику, которая должна выполняться при изменении свойств фильтра.

### Коллекции

Для хранения коллекций в свойствах индикатора рекомендуется использовать `ObservableCollection`. Для корректной сериализации таких свойств необходимо использовать атрибут `JsonPropertyAttribute`.

Вы можете добавлять и удалять элементы из свойств типа коллекции.

```csharp
using System.Collections.ObjectModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;

public class SampleIndicator : ATAS.Indicators.Indicator
{
    [Display (Name = "Numbers", GroupName = "Examples")]
    [JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public ObservableCollection<decimal> Numbers { get; set; } = new()
    { 1.0m, 2.0m, 3.0m };

    [Display (Name = "Filters", GroupName = "Examples")]
    [JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public ObservableCollection<Filter> Filters { get; set; } = new();

    [Display (Name = "Colors", GroupName = "Examples")]
    [JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public ObservableCollection<Color> ColorsSource { get; set; } = new()
    { Color.Red, Color.Green, Color.Blue };

    public SampleIndicator() : base(true)
    {
        DenyToChangePanel = true;
        DataSeries[0].IsHidden = true;
    }

    protected override void OnCalculate(int bar, decimal value)
    {
    }
}
```
> **Аннотация:**
> - `using System.Collections.ObjectModel;` - добавление пространства имен для `ObservableCollection`.
> - `using Newtonsoft.Json;` и `using Newtonsoft.Json.Converters;` - добавление пространств имен для работы с JSON и атрибутом `JsonProperty`.
> - `[JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]` - атрибут `JsonProperty` используется для управления сериализацией JSON. `ObjectCreationHandling.Replace` указывает, что при десериализации существующая коллекция должна быть заменена новой.
> - `public ObservableCollection<decimal> Numbers { get; set; } = new() { 1.0m, 2.0m, 3.0m };` - свойство `Numbers` типа `ObservableCollection<decimal>`, инициализированное коллекцией с тремя десятичными числами. `ObservableCollection` позволяет отслеживать изменения в коллекции (добавление, удаление, изменение элементов).
> - `public ObservableCollection<Filter> Filters { get; set; } = new();` - свойство `Filters` типа `ObservableCollection<Filter>`, инициализированное пустой коллекцией.
> - `public ObservableCollection<Color> ColorsSource { get; set; } = new() { Color.Red, Color.Green, Color.Blue };` - свойство `ColorsSource` типа `ObservableCollection<Color>`, инициализированное коллекцией с тремя предопределенными цветами `Color.Red`, `Color.Green`, `Color.Blue` (предполагается, что `Color` определен в ATAS API).

### Расширенные настройки параметров

Дополнительные атрибуты могут быть использованы для более тонкой настройки параметров. Имена атрибутов можно писать без суффикса `Attribute`. Например: `DisplayAttribute = Display`.

Некоторые из атрибутов описаны здесь.

#### DisplayAttribute

Используя этот атрибут, вы можете установить отображаемое имя параметра, имя группы, порядковый номер и многое другое.

#### NumericEditorAttribute

Используется для настроек числовых параметров. В конструкторе этого атрибута можно указать минимальное значение, максимальное значение, шаг цены и формат отображения.

#### ComboBoxEditorAttribute

Свойство с этим атрибутом выглядит как выпадающее меню.

```csharp
using OFT.Attributes.Editors; // Пространство имен для атрибутов редакторов ATAS

public class Entity
{
    #region Properties
    public int Value { get; set; }
    public string Name { get; set; }
    #endregion
}

[DisplayName("Simple Indicator")] // Атрибут для задания имени индикатора в списке индикаторов
public class SimpleInd : ATAS.Indicators.Indicator
{
    private class EntitiesSource : Collection<Entity>
    {
        #region ctor
        public EntitiesSource()
            : base(new[]
            {
                new Entity { Value = 1, Name = "Entity 1" },
                new Entity { Value = 2, Name = "Entity 2" },
                new Entity { Value = 3, Name = "Entity 3" },
                new Entity { Value = 4, Name = "Entity 4" },
                new Entity { Value = 5, Name = "Entity 5" }
            })
        {
        }
        #endregion
    }

    [Display (Name = "Selector", GroupName = "Examples")]
    [ComboBoxEditor(typeof(EntitiesSource), DisplayMember = nameof(Entity.Name), ValueMember = nameof(Entity.Value))] // Атрибут ComboBoxEditor
    public int? Selector { get; set; } // Свойство для выпадающего списка

    [Display (Name = "Decimal", GroupName = "Examples")]
    [NumericEditor(0.0, 100.0, Step = 0.5, DisplayFormat = "F2")] // Атрибут NumericEditor
    public decimal Decimal { get; set; } // Десятичное свойство с настройками редактора

    public SimpleInd() : base(true)
    {
        DenyToChangePanel = true;
        DataSeries[0].IsHidden = true;
    }

    protected override void OnCalculate(int bar, decimal value)
    {
    }
}
```
> **Аннотация:**
> - `using OFT.Attributes.Editors;` - добавление пространства имен `OFT.Attributes.Editors` для использования атрибутов редакторов ATAS, таких как `ComboBoxEditorAttribute` и `NumericEditorAttribute`.
> - `public class Entity { ... }` -  вспомогательный класс `Entity` с двумя свойствами: `Value` (int) и `Name` (string). Используется для примера с `ComboBoxEditorAttribute`.
> - `[DisplayName("Simple Indicator")]` - атрибут `DisplayName` задает имя индикатора, которое будет отображаться в списке индикаторов ATAS.
> - `private class EntitiesSource : Collection<Entity> { ... }` - приватный класс `EntitiesSource`, производный от `Collection<Entity>`. Используется как источник данных для `ComboBoxEditorAttribute`. Содержит предопределенный набор объектов `Entity`.
> - `[ComboBoxEditor(typeof(EntitiesSource), DisplayMember = nameof(Entity.Name), ValueMember = nameof(Entity.Value))]` - атрибут `ComboBoxEditor` превращает свойство `Selector` в выпадающий список в настройках индикатора.
>     - `typeof(EntitiesSource)` - указывает тип источника данных для выпадающего списка.
>     - `DisplayMember = nameof(Entity.Name)` -  указывает, какое свойство класса `Entity` будет отображаться в выпадающем списке (в данном случае `Name`).
>     - `ValueMember = nameof(Entity.Value)` -  указывает, какое свойство класса `Entity` будет использоваться как значение выбранного элемента (в данном случае `Value`).
> - `public int? Selector { get; set; }` - свойство `Selector` типа `int?` (nullable int), которое будет отображаться как выпадающий список.
> - `[NumericEditor(0.0, 100.0, Step = 0.5, DisplayFormat = "F2")]` - атрибут `NumericEditor` настраивает редактор для свойства `Decimal` как числовой редактор с определенными параметрами.
>     - `0.0` - минимальное значение.
>     - `100.0` - максимальное значение.
>     - `Step = 0.5` - шаг изменения значения.
>     - `DisplayFormat = "F2"` - формат отображения числа (два знака после запятой).
> - `public decimal Decimal { get; set; }` - свойство `Decimal` типа `decimal`, которое будет отображаться как числовой редактор с заданными настройками.

#### RangeAttribute

Используется для числовых параметров и задает минимальное и максимальное значения.

#### DisplayFormat

Указывает, как отображаются поля данных и форматируются.

```csharp
[Display (Name = "Filter Decimal", GroupName = "Examples")]
[Range(-100, 100)] // Атрибут Range, устанавливает диапазон значений
[DisplayFormat(DataFormatString = "##0.0##")] // Атрибут DisplayFormat, задает формат отображения
public Filter FilterDecimal { get; set; } = new();
```
> **Аннотация:**
> - `[Range(-100, 100)]` - атрибут `Range` устанавливает допустимый диапазон значений для свойства `FilterDecimal` от -100 до 100.
> - `[DisplayFormat(DataFormatString = "##0.0##")]` - атрибут `DisplayFormat` задает формат отображения значения свойства `FilterDecimal`. `"##0.0##"` - пользовательский формат, который отображает число с одним знаком после запятой, подавляя незначащие нули в начале и конце.

#### RegularExpressionAttribute

Указывает, что значение поля данных должно соответствовать заданному регулярному выражению.

#### MaskAttribute

При вводе значения в поля свойств с этим атрибутом редактор проверяет текст на соответствие маске и разрешает ввод только данных, удовлетворяющих выражению маски.

```csharp
[Display (Name = "E-mail", GroupName = "Examples")]
[RegularExpression(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$")] // Атрибут RegularExpression для валидации email
public string Email { get; set; }

[Display (Name = "Time Span", GroupName = "Examples")]
[Mask (MaskTypes.DateTimeAdvancingCaret, "HH:mm:ss")] // Атрибут Mask для задания маски времени
public TimeSpan TimeSpan { get; set; } = new(1, 0, 0);
```
> **Аннотация:**
> - `[RegularExpression(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$")]` - атрибут `RegularExpression` задает регулярное выражение для проверки формата электронной почты для свойства `Email`.
> - `public string Email { get; set; }` - строковое свойство `Email`, которое должно соответствовать формату электронной почты.
> - `[Mask (MaskTypes.DateTimeAdvancingCaret, "HH:mm:ss")]` - атрибут `Mask` задает маску ввода для свойства `TimeSpan` в формате "HH:mm:ss" (часы:минуты:секунды). `MaskTypes.DateTimeAdvancingCaret` указывает тип маски.
> - `public TimeSpan TimeSpan { get; set; } = new(1, 0, 0);` - свойство `TimeSpan` типа `TimeSpan`, инициализированное значением 1 час 0 минут 0 секунд.

#### BrowsableAttribute

С параметром `false` позволяет не отображать свойство с этим атрибутом в меню настроек.

```csharp
[Browsable(false)] // Атрибут Browsable(false) скрывает свойство в настройках
public int NoBrowsable { get; set; }
```
> **Аннотация:**
> - `[Browsable(false)]` - атрибут `Browsable` с параметром `false` делает свойство `NoBrowsable` невидимым в настройках индикатора.
> - `public int NoBrowsable { get; set; }` - свойство `NoBrowsable` типа `int`, которое не будет отображаться в настройках индикатора благодаря атрибуту `Browsable(false)`.

### Расположение индикатора в отдельной панели

Индикатор может быть отображен в области графика или в отдельной панели. По умолчанию индикатор отображается в области графика. Если вы хотите, чтобы индикатор отображался в новой панели по умолчанию, необходимо указать это в конструкторе.

```csharp
public MyCustomIndicator()
{
    Panel = IndicatorDataProvider.NewPanel; // Создание новой панели для индикатора
}
```
> **Аннотация:**
> - `Panel = IndicatorDataProvider.NewPanel;` -  в конструкторе индикатора `MyCustomIndicator` свойству `Panel` присваивается результат вызова `IndicatorDataProvider.NewPanel`. Это создает новую панель для индикатора, и он будет отображаться в отдельной панели под графиком.

Также можно запретить пользователю изменять расположение графика из интерфейса. Для этого необходимо установить флаг `DenyToChangePanel` в конструкторе.

```csharp
public MyCustomIndicator()
{
    DenyToChangePanel = true; // Запрет изменения панели пользователем
    Panel = IndicatorDataProvider.NewPanel; // Создание новой панели для индикатора
}
```
> **Аннотация:**
> - `DenyToChangePanel = true;` -  установка свойства `DenyToChangePanel` в `true` в конструкторе индикатора запрещает пользователю перетаскивать панель индикатора в область графика или в другую панель через интерфейс ATAS.

### Унифицированный стиль

Каждый примененный индикатор может демонстрировать результаты своей работы на графике, отображая линии, метки, гистограммы и другие визуальные элементы определенных цветов. При разработке индивидуального индикатора можно выбрать широкий спектр цветов. Однако для обеспечения согласованного стиля графиков с пользовательскими настройками рекомендуется использовать предопределенные цвета, содержащиеся в объекте `ColorsStore`. В этом случае разработчику не нужно знать, какой цвет следует установить для конкретного свойства. Это цвета, которые установлены в настройках графика.

Вы можете получить объект `ColorsStore` из свойства `ChartInfo` индикатора.

Давайте приведем пример индикатора, который будет отображать дельту свечей в виде гистограммы.

```csharp
public class SampleIndicator : ATAS.Indicators.Indicator
{
    public SampleIndicator() : base(true)
    {
        Panel = IndicatorDataProvider.NewPanel;
        DataSeries[0].IsHidden = true;
        ((ValueDataSeries)DataSeries[0]).VisualType = VisualMode.Histogram; // Установка типа визуализации в гистограмму
    }

    protected override void OnCalculate(int bar, decimal value)
    {
        var candle = GetCandle(bar);
        DataSeries[0][bar] = candle.Delta; // Запись дельты свечи в серию данных

        if (ChartInfo is not null) // Проверка на null для ChartInfo
        {
            ((ValueDataSeries)DataSeries[0]).Colors[bar] = candle.Delta < 0
                ? ChartInfo.ColorsStore.DownCandleColor // Цвет для отрицательной дельты (медвежья свеча)
                : ChartInfo.ColorsStore.UpCandleColor; // Цвет для положительной дельты (бычья свеча)
        }
    }
}
```
> **Аннотация:**
> - `((ValueDataSeries)DataSeries[0]).VisualType = VisualMode.Histogram;` - устанавливает тип визуализации первой серии данных индикатора (`DataSeries[0]`) в гистограмму (`VisualMode.Histogram`).
> - `var candle = GetCandle(bar);` - получает объект `CandleData` для текущего бара (`bar`) с помощью метода `GetCandle(bar)`.
> - `DataSeries[0][bar] = candle.Delta;` -  записывает значение дельты свечи (`candle.Delta`) в первую серию данных индикатора для текущего бара.
> - `if (ChartInfo is not null)` - проверяет, что свойство `ChartInfo` не равно `null` перед использованием. `ChartInfo` содержит информацию о графике, к которому привязан индикатор, и может быть `null` в некоторых ситуациях.
> - `((ValueDataSeries)DataSeries[0]).Colors[bar] = candle.Delta < 0 ? ChartInfo.ColorsStore.DownCandleColor : ChartInfo.ColorsStore.UpCandleColor;` - устанавливает цвет столбца гистограммы в зависимости от знака дельты.
>     - `candle.Delta < 0` - условие: если дельта отрицательная (медвежья свеча).
>     - `ChartInfo.ColorsStore.DownCandleColor` - цвет для медвежьей свечи, берется из настроек графика ATAS.
>     - `ChartInfo.ColorsStore.UpCandleColor` - цвет для бычьей свечи, берется из настроек графика ATAS.
>     - `?:` - тернарный оператор, выбирает цвет в зависимости от условия.

### Идентификация момента новой сессии, новой недели и нового месяца

Чтобы определить, началась ли новая сессия на определенном баре, необходимо вызвать функцию `IsNewSession(int bar)` и передать ей номер бара. Аналогично существуют функции `IsNewWeek(int bar)` и `IsNewMonth(int bar)`.

### Индикаторные алерты

Можно использовать следующую функцию для вызова алерта из индикатора:

```csharp
AddAlert(string soundFile, string instrument, string message, Color background, Color foreground)
```

> **Аннотация:** Функция `AddAlert` для вызова уведомлений в ATAS.
> - `soundFile` - путь к .wav файлу, который будет воспроизведен при срабатывании алерта.
> - `instrument` - имя инструмента, которое будет показано в окне алертов.
> - `message` - сообщение, которое будет показано в окне алертов.
> - `background` - цвет фона сообщения алерта.
> - `foreground` - цвет текста сообщения алерта.

- `soundFile` - путь к .wav файлу, который должен быть воспроизведен при возникновении события.
- `instrument` - имя инструмента, которое будет показано в окне с алертами.
- `message` - сообщение в окне алертов.
- `background foreground` - цвет текста сообщения.

Существует также упрощенный механизм вызова алертов, где цвета фона и текста используются по умолчанию.

```csharp
AddAlert(string soundFile, string message)
```
> **Аннотация:** Упрощенная версия функции `AddAlert` с использованием цветов по умолчанию.
> - `soundFile` - путь к .wav файлу.
> - `message` - сообщение алерта.






## Методичка для создания индикаторов ATAS на основе PDF-файла

### Получение и обработка данных

#### Получение и использование данных свечей

Для получения и использования данных свечей необходимо получить объект класса `IndicatorCandle`. Это можно сделать с помощью метода `GetCandle(int bar)`, указав номер бара.

**Важно помнить:**

*   `bar` - это целое число, представляющее номер бара.
*   Нумерация баров начинается с `0` и заканчивается `CurrentBar - 1`, где `CurrentBar` - общее количество загруженных баров.

Значение бара можно получить из параметров переопределенного метода `OnCalculate`.

При вызове метода `GetCandle(bar)` с номером бара меньше `0` или больше `CurrentBar - 1` возникнет ошибка, и вычисления будут прерваны.

Объект `IndicatorCandle` содержит данные, которые можно получить через его свойства или методы.

**Некоторые важные свойства:**

*   `LastTime` - время последней сделки на этом баре.
*   `MaxDelta` - наибольшее значение дельты на этом баре.
*   `MinDelta` - наименьшее значение дельты на этом баре.
*   `MaxOI` - наибольшее значение открытого интереса на этом баре.
*   `MinOI` - наименьшее значение открытого интереса на этом баре.

**Пример кода для получения данных свечи в индикаторе:**

```csharp
public class SimpleInd: ATAS.Indicators.Indicator
{
    protected override void OnCalculate(int bar, decimal value)
    {
        // Получаем объект свечи для текущего бара
        var candle = GetCandle(bar);

        // Получаем различные параметры свечи
        var open = candle.Open;   // Цена открытия
        var close = candle.Close; // Цена закрытия
        var high = candle.High;   // Максимальная цена
        var low = candle.Low;     // Минимальная цена

        var volume = candle.Volume; // Объем
        var ask = candle.Ask;     // Цена Ask
        var bid = candle.Bid;     // Цена Bid
        var delta = candle.Delta;   // Дельта

        // TODO: ваш код
    }
}
```

#### Данные футпринта

Важным типом данных являются данные бара по определенной цене (данные футпринта), которые хранятся в объекте `PriceVolumeInfo`.

`PriceVolumeInfo` хранит такие данные, как: цена, количество объемов, проторгованных по цене Bid, цене Ask и т.д.

**Способы получения объектов `PriceVolumeInfo` из объекта `IndicatorCandle`:**

*   `GetPriceVolumeInfo(decimal price)` - данные кластера по определенной цене.
*   `GetAllPriceLevels()` - кластерные данные по всем ценам. В этом случае мы получим коллекцию объектов `PriceVolumeInfo`.
*   `MaxVolumePriceInfo` - кластерные данные по цене максимального объема.
*   `MaxTickPriceInfo` - кластерные данные по цене с наибольшим количеством тиков.
*   `MaxPositiveDeltaPriceInfo` - кластерные данные по цене с максимальной положительной дельтой.
*   `MaxNegativeDeltaPriceInfo` - кластерные данные по цене с минимальной отрицательной дельтой.

**Пример кода для работы с данными футпринта в индикаторе:**

```csharp
public class SimpleInd: ATAS.Indicators.Indicator
{
    protected override void OnCalculate(int bar, decimal value)
    {
        var candle = GetCandle(bar);
        var high = candle.High;

        // Получаем PriceVolumeInfo для максимальной цены бара
        var highData = candle.GetPriceVolumeInfo(high);
        if (highData != null) // Проверяем, что данные получены
        {
            var highVolume = highData.Volume; // Получаем объем для цены High
            // TODO: ваш код
        }

        // Получаем PriceVolumeInfo для цены максимального объема
        var maxVolumeData = candle.MaxVolumePriceInfo;
        if (maxVolumeData != null) // Проверяем, что данные получены
        {
            var maxVolume = maxVolumeData.Volume; // Получаем максимальный объем
            // TODO: ваш код
        }

        // Получаем все уровни цен PriceVolumeInfo
        var allPriceLevels = candle.GetAllPriceLevels();
        foreach (var level in allPriceLevels) // Перебираем все уровни цен
        {
            if (level == null) // Проверяем уровень на null
                continue;

            var price = level.Price;   // Цена уровня
            var volume = level.Volume;  // Объем уровня
            var delta = level.Ask - level.Bid; // Дельта уровня
            // TODO: ваш код
        }
    }
}
```

### Получение онлайн тиков и агрегированных сделок

Для получения данных онлайн-тиков необходимо переопределить метод `OnNewTrade(MarketDataArg arg)`.

**Пример кода для получения онлайн тиков:**

```csharp
public class SampleTick: Indicator
{
    protected override void OnCalculate(int bar, decimal value)
    {
    }

    protected override void OnNewTrade(MarketDataArg arg)
    {
    }
}
```

API также позволяет получать агрегированные сделки. Для этого необходимо переопределить метод `OnCumulativeTrade(CumulativeTrade arg)`.

Этот метод будет автоматически вызываться при появлении новой кумулятивной сделки. Такие сделки могут быть впоследствии изменены. Чтобы отслеживать эти изменения, необходимо переопределить метод `OnUpdateCumulativeTrade(CumulativeTrade trade)`.

**Пример индикатора, который выводит дельту кумулятивных сделок объемом более 3 лотов:**

```csharp
public class SampleCumulativeTrades: ATAS.Indicators.Indicator
{
    private CumulativeTrade _lastTrade;
    private decimal _lastCumulativeTradeVolume;

    protected override void OnCalculate(int bar, decimal value)
    {
    }

    protected override void OnCumulativeTrade(CumulativeTrade trade)
    {
        AddCumulativeTrade(trade);
    }

    protected override void OnUpdateCumulativeTrade(CumulativeTrade trade)
    {
        AddCumulativeTrade(trade);
    }

    private void AddCumulativeTrade(CumulativeTrade trade)
    {
        if (trade.Volume < 3) // Проверяем объем сделки
            return;

        if (_lastTrade != trade) // Проверяем, новая ли это сделка
        {
            _lastTrade = trade;
            this[CurrentBar - 1] += GetVolumeByDirection(trade.Volume, trade.Direction);
        }
        else
        {
            this[CurrentBar - 1] += GetVolumeByDirection(trade.Volume - _lastCumulativeTradeVolume, trade.Direction);
        }
        _lastCumulativeTradeVolume = trade.Volume;
    }

    // Метод для определения направления объема (покупка или продажа)
    private decimal GetVolumeByDirection(decimal volume, TradeDirection direction)
    {
        return volume * (direction == TradeDirection.Buy ? 1 : -1); // Если покупка, то 1, иначе -1
    }
}
```

Для получения исторических кумулятивных сделок необходимо вызвать метод `RequestForCumulativeTrades`, передать запрос `CumulativeTradesRequest` в параметры и переопределить метод `OnCumulativeTradesResponse`. В запросе необходимо указать начало и конец временного периода, за который вы хотите получить кумулятивные сделки, также можно установить фильтры по минимальному и максимальному объему сделок. Если указать 0 для минимального и максимального объемов, то фильтры по объему применяться не будут.

**Пример индикатора, который получает данные о кумулятивных сделках с первого до последнего бара:**

```csharp
public class SampleCumulativeTrades: ATAS.Indicators.Indicator
{
    private List<CumulativeTrade> _trades;
    private CumulativeTradesRequest _request;

    protected override void OnCalculate(int bar, decimal value)
    {
    }

    protected override void OnFinishRecalculate()
    {
        var startTime = GetCandle(0).Time; // Время первого бара
        var lastTime = GetCandle(CurrentBar - 1).LastTime; // Время последнего бара
        _request = new CumulativeTradesRequest(startTime, lastTime, 0, 0); // Создаем запрос на кумулятивные сделки
        RequestForCumulativeTrades(_request); // Отправляем запрос
    }

    protected override void OnCumulativeTradesResponse(CumulativeTradesRequest request, IEnumerable<CumulativeTrade> cumulativeTrades)
    {
        if (request != _request) // Проверяем, что ответ соответствует нашему запросу
            return;

        _trades = cumulativeTrades.ToList(); // Сохраняем полученные сделки в список
    }
}
```

### Работа с Market Depth

API позволяет получать данные об обновлениях Market Depth. Для этого метод `MarketDepthChanged(MarketDataArg arg)` должен быть переопределен.

**Пример кода для обработки изменений Market Depth:**

```csharp
public class SampleMD: ATAS.Indicators.Indicator
{
    protected override void OnCalculate(int bar, decimal value)
    {
    }

    protected override void MarketDepthChanged(MarketDataArg arg)
    {
    }
}
```

Также можно получить общий объем всех Bid и общий объем всех Ask с помощью свойств объекта `MarketDepthInfo`: `CumulativeDomAsks` и `CumulativeDomBids`.

**Пример индикатора, который отображает историю суммарных объемов Ask и Bid:**

```csharp
public class DomPower: ATAS.Indicators.Indicator
{
    private ValueDataSeries_asks;
    private ValueDataSeries_bids = new ValueDataSeries("Bids");
    private int _lastCalculatedBar = 0;
    private bool _first = true;

    public DomPower() : base(true)
    {
        Panel = IndicatorDataProvider.NewPanel; // Создаем новую панель для индикатора
        _asks = (ValueDataSeries)DataSeries[0]; // Получаем ValueDataSeries для Ask
        _asks.Name = "Asks"; // Название линии Ask
        _asks.Color = Colors.Red; // Цвет линии Ask
        _bids.Color = Colors.Green; // Цвет линии Bid
        DataSeries.Add(_bids); // Добавляем ValueDataSeries для Bid
    }

    protected override void OnCalculate(int bar, decimal value)
    {
    }

    protected override void MarketDepthChanged(MarketDataArg arg)
    {
        if (_first) // Первый вызов MarketDepthChanged
        {
            _first = false;
            _lastCalculatedBar = CurrentBar - 1; // Запоминаем последний бар
        }

        var lastCandle = CurrentBar - 1;
        var cumAsks = MarketDepthInfo.CumulativeDomAsks; // Суммарный объем Ask
        var cumBids = MarketDepthInfo.CumulativeDomBids; // Суммарный объем Bid

        for (int i = _lastCalculatedBar; i <= lastCandle; i++) // Заполняем данные для ValueDataSeries
        {
            _asks[i] = -cumAsks; // Записываем объем Ask с минусом для отображения ниже нуля
            _bids[i] = cumBids;  // Записываем объем Bid
        }
        _lastCalculatedBar = lastCandle; // Обновляем последний бар
    }
}
```

В некоторых случаях может потребоваться получить снимок данных Market Depth. Для этого можно использовать функцию `MarketDepthInfo.GetMarketDepthSnapshot()`. Функция возвращает список всех уровней в DOM.

### MBO (Market By Orders)

На данный момент эти данные предоставляются только провайдером Rhithmic.

Чтобы получить и проанализировать данные MBO (Market By Orders), необходимо получить экземпляр объекта, реализующего интерфейс `IMarketByOrdersManager`, с помощью метода `SubscribeMarketByOrderData`. Этот объект содержит свойство `MarketByOrders`, которое представляет собой коллекцию объектов типа `MarketByOrder`. Чтобы отслеживать изменения в этой коллекции в режиме реального времени, необходимо подписаться на событие `Changed` объекта `IMarketByOrdersManager`.

**Объекты `MarketByOrder` имеют следующие свойства:**

*   `Security` - инструмент, связанный с заявкой.
*   `Time` - дата и время поступления заявки.
*   `Type` - тип обновления заявки:
    *   `Snapshot` - данные MBO из кэша, а не из потока данных в реальном времени.
    *   `New` - данные MBO представляют новую заявку, добавленную в книгу заявок инструмента.
    *   `Change` - данные MBO представляют изменение существующей заявки.
    *   `Delete` - данные MBO представляют удаление существующей заявки.
*   `Side` - тип рыночных данных:
    *   `Bid` - заявка на покупку.
    *   `Ask` - заявка на продажу.
    *   `Trade` - сделка.
*   `Priority` - приоритет этой заявки в очереди механизма сопоставления биржи.
*   `ExchangeOrderld` - идентификатор заявки на бирже.
*   `Price` - цена, связанная с этой заявкой.
*   `Volume` - объем, связанный с этой заявкой.

**Пример индикатора, использующего `IMarketByOrdersManager`:**

```csharp
public class SampleMBODataIndicator: ATAS.Indicators.Indicator
{
    private IMarketByOrdersManager _manager;

    protected override async void OnInitialize()
    {
        _manager = await SubscribeMarketByOrderData(); // Подписываемся на данные MBO
        _manager.Changed += Manager_Changed; // Подписываемся на событие Changed
        Manager_Changed(_manager.MarketByOrders); // Обрабатываем начальные данные
    }

    private void Manager_Changed(System.Collections.Generic.IEnumerable<DataFeedsCore.MarketByOrder> marketByOrders)
    {
        this.LogInfo("new MBO data");
        foreach (var marketByOrder in marketByOrders) // Перебираем новые MBO данные
        {
            this.LogInfo("{0}", marketByOrder); // Выводим информацию о каждой заявке
        }
    }

    #region Overrides of Extended Indicator
    protected override void OnNewTrade(MarketDataArg trade)
    {
        this.LogInfo($"ExchangeOrderId: {trade.ExchangeOrderId}, AggressorExchangeOrderId: {trade.AggressorExchangeOrderId}");
    }
    #endregion

    protected override void OnDispose()
    {
        base.OnDispose();
        if (_manager != null)
        {
            _manager.Changed -= Manager_Changed; // Отписываемся от события Changed при удалении индикатора
        }
    }

    protected override void OnCalculate(int bar, decimal value)
    {
    }
}
```

### Статистические данные

Вы можете получить статистические данные, используя свойство индикатора `TradingStatisticsProvider`. Статистические данные хранятся в объекте, реализующем `ITradingStatistics`, который можно получить через:

*   `TradingStatisticsProvider.Realtime` - для получения статистики в реальном времени.
*   Асинхронный метод `TradingStatisticsProvider.LoadAsync(DateTime from, DateTime to, ICollection<string>? accounts = null, ICollection<string>? securities = null)` - для загрузки исторической статистики.

Чтобы получить историю статистики, необходимо вызвать асинхронный метод `LoadAsync`, передав время начала и окончания желаемого периода в качестве параметров. Вы также можете передать параметры для счетов и инструментов, по которым вам нужны данные. Статистические данные, полученные с помощью этого метода, отображают статистику за определенный период и не обновляются в реальном времени.

**Пример индикатора, который регистрирует данные об эквити за период с начала графика до текущего бара:**

```csharp
using ATAS.DataFeedsCore;
using ATAS.DataFeedsCore.Statistics;
using ATAS.Indicators;
using Utils.Common.Collections;
using Utils.Common.Logging;

public class SampleStatistics: Indicator
{
    private ITradingStatistics_tradingStatistics;

    protected override void OnCalculate(int bar, decimal value)
    {
    }

    protected override void OnFinishRecalculate()
    {
        if (_tradingStatistics is null) // Проверяем, загружена ли статистика
            return;

        foreach (var equity in _tradingStatistics.Equity) // Перебираем данные по эквити
        {
            this.LogInfo($"Time: {equity.Time} | equity: {equity.Equity} | total equity: {equity.TotalEquity}"); // Выводим данные в лог
        }
    }

    protected override async void OnInitialize()
    {
        var start = GetCandle(0).Time; // Время первого бара
        var end = GetCandle(CurrentBar - 1).LastTime; // Время последнего бара
        _tradingStatistics = await TradingStatisticsProvider.LoadAsync(start, end); // Загружаем историческую статистику
    }
}
```

Объект `ITradingStatistics` содержит следующие типы статистики:

*   `Equity` - коллекция `EquityValue` (данные по эквити).
*   `HistoryMyTrades` - коллекция `HistoryMyTrade` (история моих сделок).
*   `Orders` - коллекция `Order` (мои ордера).
*   `MyTrades` - коллекция `MyTrade` (мои сделки).
*   `Statistics` - коллекция `IStatisticsParameterGroup` (различные статистические параметры).

Если вы получаете объект `ITradingStatistics` с помощью `TradingStatisticsProvider.Realtime`, то вы можете отслеживать изменения в этих свойствах в режиме реального времени, подписавшись на их события:

*   `Added` - добавляется новый элемент в коллекцию.
*   `Changed` - элемент в коллекции изменяется.
*   `Removed` - элемент удаляется из коллекции.
*   `Cleared` - коллекция очищается.

**Пример индикатора, который выводит эти данные в лог при появлении новых статистических данных:**

```csharp
using ATAS.DataFeedsCore;
using ATAS.DataFeedsCore.Statistics;
using ATAS.Indicators;
using Utils.Common.Collections;
using Utils.Common.Logging;

public class SampleStatistics: Indicator
{
    private IMutableEnumerable<HistoryMyTrade> _historyMyTrades;
    private IMutableEnumerable<EquityValue> _equity;
    private IMutableEnumerable<Order> _orders;
    private IMutableEnumerable<MyTrade> _myTrades;
    private IMutableEnumerable<IStatisticsParameterGroup> _statistics;

    protected override void OnCalculate(int bar, decimal value)
    {
    }

    protected override void OnInitialize()
    {
        var tradingStatistics = TradingStatisticsProvider.Realtime; // Получаем статистику в реальном времени

        _historyMyTrades = tradingStatistics.HistoryMyTrades;
        _historyMyTrades.Added += HistoryMyTrades_Added; // Подписываемся на событие добавления HistoryMyTrade

        _equity = tradingStatistics.Equity;
        _equity.Added += Equity_Added; // Подписываемся на событие добавления EquityValue

        _orders = tradingStatistics.Orders;
        _orders.Added += Orders_Added; ; // Подписываемся на событие добавления Order

        _myTrades = tradingStatistics.MyTrades;
        _myTrades.Added += MyTrades_Added; // Подписываемся на событие добавления MyTrade

        _statistics = tradingStatistics.Statistics;
        _statistics.Added += Statistics_Added; // Подписываемся на событие добавления Statistics
    }

    private void Statistics_Added(IStatisticsParameterGroup spGroup)
    {
        this.LogInfo($"New {spGroup.Name} data."); // Выводим имя группы статистики
    }

    private void MyTrades_Added(MyTrade trade)
    {
        this.LogInfo($"New MyTrade {trade.AccountID} added."); // Выводим AccountID новой сделки
    }

    private void Orders_Added(Order order)
    {
        this.LogInfo($"New Order {order.AccountID} added."); // Выводим AccountID нового ордера
    }

    private void Equity_Added(EquityValue equity)
    {
        this.LogInfo($"Time: {equity.Time} | equity: {equity.Equity} | total equity: {equity.TotalEquity}"); // Выводим данные по эквити
    }

    private void HistoryMyTrades_Added(HistoryMyTrade trade)
    {
        this.LogInfo($"New HistoryMyTrade {trade.AccountID} added."); // Выводим AccountID новой исторической сделки
    }

    protected override void OnDispose()
    {
        base.OnDispose();
        if (_historyMyTrades != null)
            _historyMyTrades.Added -= HistoryMyTrades_Added; // Отписываемся от события

        if (_equity != null)
            _equity.Added -= Equity_Added; // Отписываемся от события

        if (_orders != null)
            _orders.Added -= Orders_Added; // Отписываемся от события

        if (_myTrades != null)
            _myTrades.Added -= MyTrades_Added; // Отписываемся от события

        if (_statistics != null)
            _statistics.Added -= Statistics_Added; // Отписываемся от события
    }
}
```






## Методичка для создания индикаторов ATAS по PDF-файлу "Working with trading events"

**Рабочие события торговли**

Необходимо обратиться к свойству `TradingInfo` для получения данных о торговых сущностях.

Это свойство включает в себя:

*   **Security** - выбранный инструмент
*   **Portfolio** - выбранный портфель
*   **Position** - текущая позиция
*   **MyTrade** - выполненные сделки
*   **Order** - размещенные ордера

Кроме того, можно переопределить следующие методы для получения обновлений:

*   `void OnNewOrder(Order order)` - новый ордер
*   `void OnOrderChanged(Order order)` - изменение ордера
*   `void OnNewMyTrade(MyTrade myTrade)` - новая сделка
*   `void OnPortfolioChanged(Portfolio portfolio)` - изменение портфеля
*   `void OnPositionChanged(Position position)` - изменение позиции

Ниже приведен пример индикатора, который отображает данные портфеля, позиции, ордера и сделки. Индикатор также добавляет записи в журнал при получении новых ордеров и новых сделок, когда изменяется портфель или позиция.

```csharp
using ATAS.Indicators; // Пространство имен ATAS.Indicators, необходимое для создания индикаторов

public class SampleTradingInfo : Indicator // Объявление публичного класса SampleTradingInfo, наследующегося от класса Indicator (базовый класс для индикаторов ATAS)
{
    public SampleTradingInfo() // Публичный конструктор класса SampleTradingInfo (вызывается при создании экземпляра индикатора)
    {
        EnableCustomDrawing = true; // Включение пользовательской прорисовки индикатора. Необходимо для ручной отрисовки элементов индикатора на графике.
        SubscribeToDrawingEvents (DrawingLayouts.Final); // Подписка на событие отрисовки Draw в финальном слое графика. DrawingLayouts.Final указывает, что отрисовка будет происходить после отрисовки всех стандартных элементов графика.
    }

    protected override void OnCalculate(int bar, decimal value) // Переопределение метода OnCalculate. Этот метод вызывается для каждого нового бара и предназначен для основных расчетов индикатора (в данном примере пока пустой).
    {
    }

    protected override void OnNewOrder(Order orders) // Переопределение метода OnNewOrder. Этот метод вызывается при поступлении нового ордера.
    {
        this.LogWarn(orders.ToString()); // Вывод информации о новом ордере в журнал предупреждений ATAS. LogWarn используется для записи предупреждающих сообщений. orders.ToString() преобразует объект ордера в строковое представление для вывода.
    }

    protected override void OnOrderChanged(Order order) // Переопределение метода OnOrderChanged. Этот метод вызывается при изменении существующего ордера.
    {
        this.LogWarn(order.ToString()); // Вывод информации об измененном ордере в журнал предупреждений ATAS.
    }
```

```csharp
    protected override void OnNewMyTrade(MyTrade myTrade) // Переопределение метода OnNewMyTrade. Этот метод вызывается при совершении новой сделки.
    {
        this.LogWarn(myTrade.ToString()); // Вывод информации о новой сделке в журнал предупреждений ATAS.
    }

    protected override void OnPortfolioChanged(Portfolio portfolio) // Переопределение метода OnPortfolioChanged. Этот метод вызывается при изменении портфеля.
    {
        this.LogWarn(portfolio.ToString()); // Вывод информации об изменениях портфеля в журнал предупреждений ATAS.
    }

    protected override void OnPositionChanged(Position position) // Переопределение метода OnPositionChanged. Этот метод вызывается при изменении позиции.
    {
        this.LogWarn(position.ToString()); // Вывод информации об изменениях позиции в журнал предупреждений ATAS.
    }

    protected override void OnRender(RenderContext context, DrawingLayouts layout) // Переопределение метода OnRender. Этот метод вызывается для отрисовки индикатора на графике. RenderContext предоставляет инструменты для рисования. DrawingLayouts указывает слой отрисовки.
    {
        var label = ""; // Объявление строковой переменной label для хранения текста, который будет отображаться на графике. Инициализация пустой строкой.

        if (TradingManager.Security != null) // Проверка, что TradingManager.Security не равен null. TradingManager предоставляет доступ к информации о текущем торгуемом инструменте. Проверка на null необходима, чтобы избежать ошибок, если инструмент не выбран.
            label += $"Security: {TradingManager.Security}{Environment.NewLine}"; // Если инструмент выбран, добавляем в label строку "Security: " и название инструмента (TradingManager.Security). Environment.NewLine добавляет символ новой строки для переноса текста на следующую строку.

        if (TradingManager.Portfolio != null) // Проверка, что TradingManager.Portfolio не равен null. TradingManager предоставляет доступ к информации о портфеле.
            label += $"Portfolio: {TradingManager.Portfolio}{Environment.NewLine}"; // Если портфель доступен, добавляем в label строку "Portfolio: " и информацию о портфеле (TradingManager.Portfolio).

        if (TradingManager.Position != null) // Проверка, что TradingManager.Position не равен null. TradingManager предоставляет доступ к информации о текущей позиции.
            label += $"Position: {TradingManager.Position}{Environment.NewLine}"; // Если позиция есть, добавляем в label строку "Position: " и информацию о позиции (TradingManager.Position).

        var orders = TradingManager.Orders.Where(t => t.State == OrderStates.Active); // Получение списка активных ордеров из TradingManager.Orders. Используется LINQ-запрос Where для фильтрации ордеров, у которых состояние (t.State) равно OrderStates.Active.

        if (orders.Any()) // Проверка, есть ли активные ордера в списке orders. Any() возвращает true, если в списке есть хотя бы один элемент.
        {
            label += $"{Environment.NewLine}-----Active orders:-----{Environment.NewLine}"; // Добавляем разделительную строку "----Active orders:----" в label для визуального отделения блоков информации.
            foreach (var order in orders) // Цикл foreach для перебора каждого ордера в списке activeOrders.
            {
                label += $"{order}{Environment.NewLine}"; // Для каждого ордера добавляем его строковое представление (order.ToString()) и символ новой строки в label.
            }
        }

        var myTrades = TradingManager.MyTrades; // Получение списка всех сделок из TradingManager.MyTrades.

        if (myTrades.Any()) // Проверка, есть ли сделки в списке myTrades.
        {
            label += $"{Environment.NewLine}----MyTrades:----{Environment.NewLine}"; // Добавляем разделительную строку "----MyTrades:----" в label.
            foreach (var myTrade in myTrades) // Цикл foreach для перебора каждой сделки в списке myTrades.
            {
                label += $"{myTrade}{Environment.NewLine}"; // Для каждой сделки добавляем ее строковое представление (myTrade.ToString()) и символ новой строки в label.
            }
        }

        var font = new RenderFont("Arial", 10); // Создание нового объекта шрифта RenderFont с именем "Arial" и размером 10. Используется для задания шрифта текста, отображаемого на графике.
        var size = context.MeasureString(label, font); // Измерение размера текста label в пикселях с использованием заданного шрифта font. Результат сохраняется в переменной size.
        context.FillRectangle(Color.DarkRed, new Rectangle(25, 25, (int)size.Width + 50, (int)size.Height + 50)); // Отрисовка прямоугольника. FillRectangle заливает прямоугольник заданным цветом.
        // Color.DarkRed - цвет заливки прямоугольника (темно-красный).
        // new Rectangle(25, 25, (int)size.Width + 50, (int)size.Height + 50) - создание объекта Rectangle, определяющего положение и размер прямоугольника.
        // (25, 25) - координаты верхнего левого угла прямоугольника (отступ от левого и верхнего края области рисования).
        // (int)size.Width + 50 - ширина прямоугольника. Берется ширина текста size.Width и добавляется 50 пикселей для отступа по бокам.
        // (int)size.Height + 50 - высота прямоугольника. Берется высота текста size.Height и добавляется 50 пикселей для отступа сверху и снизу.
        context.DrawString(label, font, Color.Azure, 50, 50); // Отрисовка текста label на графике. DrawString рисует строку текста.
        // label - текст для отрисовки.
        // font - шрифт для текста.
        // Color.Azure - цвет текста (светло-голубой).
        // (50, 50) - координаты верхнего левого угла текста относительно области рисования.
    }
}
```

**Отображает данные портфеля, позиции, ордера и торговые данные**

![Скриншот графика с индикатором](изображение_графика_из_pdf.png)







## Методичка по созданию композитных индикаторов в ATAS

**Создание композитных индикаторов**

Для построения индикатора на основе другого индикатора необходимо разработать и рассчитать индикатор, на основе которого будет производиться расчет, после чего его значение будет использовано в собственных расчетах.

**Пример реализации индикатора MACD:**

```csharp
using ATAS.Indicators;

public class MACD : Indicator
{
    private readonly EMA _long = new EMA(); // Объявляем приватную переменную _long типа EMA и инициализируем новым экземпляром EMA. Это будет длинная скользящая средняя.
    private readonly EMA _short = new EMA(); // Объявляем приватную переменную _short типа EMA и инициализируем новым экземпляром EMA. Это будет короткая скользящая средняя.
    private readonly SMA _sma = new SMA(); // Объявляем приватную переменную _sma типа SMA и инициализируем новым экземпляром SMA. Это будет сигнальная линия, скользящая средняя от MACD.

    public int LongPeriod // Объявляем публичное свойство LongPeriod типа int для доступа к периоду длинной EMA.
    {
        get { return _long.Period; } //  Getter для возврата текущего значения периода длинной EMA.
        set // Setter для установки нового значения периода длинной EMA.
        {
            if (value <= 0) // Проверяем, что новое значение периода больше нуля. Если нет, выходим из setter.
                return;

            _long.Period = value; // Устанавливаем новое значение периода для длинной EMA.
            RecalculateValues(); // Вызываем метод RecalculateValues(), чтобы пересчитать значения индикатора с новым периодом.
        }
    }

    public int ShortPeriod // Объявляем публичное свойство ShortPeriod типа int для доступа к периоду короткой EMA.
    {
        get { return _short.Period; } // Getter для возврата текущего значения периода короткой EMA.
        set // Setter для установки нового значения периода короткой EMA.
        {
            if (value <= 0) // Проверяем, что новое значение периода больше нуля. Если нет, выходим из setter.
                return;

            _short.Period = value; // Устанавливаем новое значение периода для короткой EMA.
            RecalculateValues(); // Вызываем метод RecalculateValues(), чтобы пересчитать значения индикатора с новым периодом.
        }
    }

    public int SignalPeriod // Объявляем публичное свойство SignalPeriod типа int для доступа к периоду SMA сигнальной линии.
    {
        get { return _sma.Period; } // Getter для возврата текущего значения периода SMA сигнальной линии.
        set // Setter для установки нового значения периода SMA сигнальной линии.
        {
            if (value <= 0) // Проверяем, что новое значение периода больше нуля. Если нет, выходим из setter.
                return;

            _sma.Period = value; // Устанавливаем новое значение периода для SMA сигнальной линии.
            RecalculateValues(); // Вызываем метод RecalculateValues(), чтобы пересчитать значения индикатора с новым периодом.
        }
    }

    public MACD() // Конструктор класса MACD. Вызывается при создании экземпляра индикатора MACD.
    {
        Panel = IndicatorDataProvider.NewPanel; // Указываем, что индикатор будет отображаться на новой панели графика.
        ((ValueDataSeries)DataSeries[0]).VisualType = VisualMode.Histogram; // Настраиваем визуальное отображение первой линии данных (индекса 0) как гистограмму.
        ((ValueDataSeries)DataSeries[0]).Color = Colors.CadetBlue; // Устанавливаем цвет гистограммы в CadetBlue.
        DataSeries.Add(new ValueDataSeries("Signal") // Добавляем новую линию данных с именем "Signal". Это будет сигнальная линия MACD.
        {
            VisualType = VisualMode.Line, // Указываем, что сигнальная линия будет отображаться как линия.
            LineDashStyle = LineDashStyle.Dash // Устанавливаем стиль линии как пунктир.
        });
    }

    LongPeriod = 26; // Инициализируем период длинной EMA значением 26 по умолчанию.
    ShortPeriod = 12; // Инициализируем период короткой EMA значением 12 по умолчанию.
    SignalPeriod = 9; // Инициализируем период сигнальной SMA значением 9 по умолчанию.

    protected override void OnCalculate(int bar, decimal value) // Переопределяем метод OnCalculate, который вызывается на каждом новом баре для расчета значений индикатора.
    {
        var macd = _short.Calculate(bar, value) - _long.Calculate(bar, value); // Рассчитываем значение MACD как разницу между короткой и длинной EMA для текущего бара.
        var signal = _sma.Calculate(bar, macd); // Рассчитываем сигнальную линию как SMA от значения MACD для текущего бара.
        this[bar] = macd; // Присваиваем рассчитанное значение MACD первой линии данных индикатора (индекс 0) для текущего бара.
        DataSeries[1][bar] = signal; // Присваиваем рассчитанное значение сигнальной линии второй линии данных индикатора (индекс 1) для текущего бара.
    }
}
```

Методы `Calculate`, которые рассчитывали эти индикаторы и возвращали значения каждого индикатора для определенного бара, были вызваны в этом примере для сторонних индикаторов (EMA, SMA и т.д.).

Если используемые индикаторы должны быть рассчитаны на том же входящем источнике, что и разрабатываемый индикатор, их можно просто добавить как «внутренние» через метод `Add` в конструкторе.

Ниже приведен пример измененного индикатора MACD, где индикаторы `_long` и `_short` добавлены как «внутренние», и их значения получаются просто через запрос данных для определенного бара.

```csharp
using ATAS.Indicators;

public class MACD : Indicator
{
    private readonly EMA _long = new EMA(); // Объявляем приватную переменную _long типа EMA и инициализируем новым экземпляром EMA.
    private readonly EMA _short = new EMA(); // Объявляем приватную переменную _short типа EMA и инициализируем новым экземпляром EMA.
    private readonly SMA _sma = new SMA(); // Объявляем приватную переменную _sma типа SMA и инициализируем новым экземпляром SMA.

    public MACD() // Конструктор класса MACD.
    {
        Panel = IndicatorDataProvider.NewPanel; // Указываем, что индикатор будет отображаться на новой панели.
        ((ValueDataSeries)DataSeries[0]).VisualType = VisualMode.Histogram; // Настраиваем визуальное отображение первой линии данных как гистограмму.
        ((ValueDataSeries)DataSeries[0]).Color = Colors.CadetBlue; // Устанавливаем цвет гистограммы в CadetBlue.
        DataSeries.Add(new ValueDataSeries("Signal") // Добавляем новую линию данных "Signal".
        {
            VisualType = VisualMode.Line, // Указываем, что сигнальная линия будет линией.
            LineDashStyle = LineDashStyle.Dash // Устанавливаем стиль линии как пунктир.
        });
    }

    LongPeriod = 26; // Инициализация периода длинной EMA.
    ShortPeriod = 12; // Инициализация периода короткой EMA.
    SignalPeriod = 9; // Инициализация периода сигнальной SMA.

    Add(_short); // Добавляем _short (короткую EMA) как внутренний индикатор.
    Add(_long); // Добавляем _long (длинную EMA) как внутренний индикатор.
}

protected override void OnCalculate(int bar, decimal value) // Переопределенный метод OnCalculate для расчета значений индикатора.
{
    var macd = _short[bar] - _long[bar]; // Рассчитываем MACD, используя значения _short и _long для текущего бара, полученные как внутренние индикаторы.
    var signal = _sma.Calculate(bar, macd); // Рассчитываем сигнальную линию как SMA от MACD.
    this[bar] = macd; // Присваиваем значение MACD первой линии данных.
    DataSeries[1][bar] = signal; // Присваиваем значение сигнальной линии второй линии данных.
}
```

Если используемый внутренний индикатор имеет несколько `DataSeries`, они также могут быть использованы в разработанном индикаторе. Пример получения данных из `Signal DataSeries` индикатора MACD из другого индикатора:

```csharp
using ATAS.Indicators;

public class SampleIndicator : Indicator
{
    private readonly MACD _macd = new MACD(); // Объявляем приватную переменную _macd типа MACD и создаем новый экземпляр MACD.

    public SampleIndicator() // Конструктор класса SampleIndicator.
    {
        Add(_macd); // Добавляем индикатор MACD как внутренний индикатор.
    }

    protected override void OnCalculate(int bar, decimal value) // Переопределенный метод OnCalculate для расчета значений индикатора.
    {
        var macdSignalSeries = (ValueDataSeries)_macd.DataSeries[1]; // Получаем DataSeries с индексом 1 (сигнальная линия) из внутреннего индикатора MACD.
        var macdSignal = macdSignalSeries[bar]; // Получаем значение сигнальной линии MACD для текущего бара.
        this[bar] = macdSignal * 2; // Присваиваем значение сигнальной линии MACD, умноженное на 2, первой линии данных текущего индикатора.
    }
}
```

`DataSeries` используемых индикаторов могут быть отображены в разработанном индикаторе. Для этого `DataSeries` из используемых индикаторов, которые должны быть отображены, должны быть добавлены в коллекцию `DataSeries` текущего индикатора.

```csharp
using ATAS.Indicators;

public class SampleIndicator : Indicator
{
    private readonly MACD _macd = new MACD(); // Объявляем приватную переменную _macd типа MACD и создаем новый экземпляр MACD.

    public SampleIndicator() // Конструктор класса SampleIndicator.
    {
        Add(_macd); // Добавляем индикатор MACD как внутренний индикатор.
        DataSeries.Add(_macd.DataSeries[1]); // Добавляем DataSeries с индексом 1 (сигнальная линия) из MACD в DataSeries текущего индикатора. Это позволит отобразить сигнальную линию MACD на графике текущего индикатора.
    }

    protected override void OnCalculate(int bar, decimal value) // Переопределенный метод OnCalculate. В данном примере расчеты не требуются, так как DataSeries уже добавлены.
    {
    }
}
```







## Методичка по DataSeries для платформы ATAS

### DataSeries

**ValueDataSeries - числовые данные**

`ValueDataSeries` предназначен для хранения десятичных данных для каждого бара.
Он добавляется к каждому индикатору по умолчанию.

Он имеет следующие свойства:

*   **VisualType** - визуальное отображение. Может быть, например, линией, столбчатой диаграммой, стрелкой или чем-то еще.
*   **Width** - ширина линии, точки, столбчатой диаграммы и так далее.
*   **Color** - цвет.
*   **LineDashStyle** - стиль линии.
*   **ScaleIt** - флаг, который регулирует автоматическое масштабирование индикатора.
*   **ShowCurrentValue** - флаг, который регулирует отображение самого последнего (текущего) значения.
*   **ShowZeroValue** - флаг, который регулирует отображение нулевых значений.
*   **Digits** - количество знаков после запятой для отображения значений DataSeries в ценовой шкале.

Этот DataSeries имеет следующие методы:

*   **SetPointOfEndLine(int bar)** - позволяет установить точку прерывания линии.
*   **RemovePointOfEndLine(int bar)** - удаляет указанный индекс бара как точку конца линии в ряду данных значения.
*   **IsThisPointOfStartBar(int bar)** - проверяет, является ли указанный индекс бара точкой начала линии в ряду данных значения.

**RangeDataSeries - диапазоны**

`RangeDataSeries`, каждый элемент которого представляет `RangeValue`, где устанавливаются минимальное и максимальное значения.

Основные свойства:

*   **RangeColor** - цвет области диапазона.
*   **ScaleIt** - флаг, который регулирует автоматическое масштабирование индикатора.
*   **Visible** - флаг, который регулирует видимость DataSeries.

Пример индикатора Bollinger Bands, который использует `RangeDataSeries`.

```csharp
using ATAS.Indicators;

public class BollingerBands : Indicator
{
    private readonly RangeDataSeries _band = new RangeDataSeries("BackGround");
    private readonly StdDev _dev = new StdDev();
    private readonly SMA _sma = new SMA();
    private decimal _width;

    public int Period
    {
        get => _sma.Period;
        set
        {
            if (value <= 0)
                return;

            _sma.Period = _dev.Period = value;
            RecalculateValues();
        }
    }

    public decimal Width
    {
        get => _width;
        set
        {
            if (value <= 0)
                return;

            _width = value;
            RecalculateValues();
        }
    }

    public BollingerBands()
    {
        ((ValueDataSeries)DataSeries[0]).Color = Colors.Green;

        DataSeries.Add(new ValueDataSeries("Up")
        {
            VisualType = VisualMode.Line
        });

        DataSeries.Add(new ValueDataSeries("Down")
        {
            VisualType = VisualMode.Line
        });

        DataSeries.Add(_band);

        Period = 10;
        Width = 1;
    }

    protected override void OnCalculate(int bar, decimal value)
    {
        var sma = _sma.Calculate(bar, value);
        var dev = _dev.Calculate(bar, value);

        this[bar] = sma;

        DataSeries[1][bar] = sma + dev * Width;
        DataSeries[2][bar] = sma - dev * Width;

        _band[bar].Upper = sma + dev * Width;
        _band[bar].Lower = sma - dev * Width;
    }
}
```

**PaintbarsDataSeries - цвета баров**

`PaintbarsDataSeries`, который позволяет устанавливать цвет для каждого бара. Каждый элемент этого DataSeries является допускающим null `Color`.

Имеет одно свойство `Visible`, которое регулирует видимость DataSeries.

Пример индикатора HeikenAshi, который использует этот `DataSeries`. `PaintbarsDataSeries` выполняет здесь функцию скрытия стандартных баров с помощью установки прозрачного цвета.

```csharp
using ATAS.Indicators;

public class HeikenAshi : Indicator
{
    readonly CandleDataSeries _candles = new CandleDataSeries("Heiken Ashi");
    readonly PaintbarsDataSeries _bars = new PaintbarsDataSeries("Bars"){ IsHidden = true };

    public HeikenAshi(): base(true)
    {
        DenyToChangePanel = true;
        DataSeries[0] = _bars;
        DataSeries.Add(_candles);
    }

    protected override void OnCalculate(int bar, decimal value)
    {
        var candle = GetCandle(bar);
        _bars[bar] = Colors.Transparent;

        if (bar == 0)
        {
            _candles[bar] = new Candle()
            {
                Close = candle.Close,
                High = candle.High,
                Low = candle.Low,
                Open = candle.Open
            };
        }
        else
        {
            var prevCandle = _candles[bar - 1];
            var close = (candle.Open + candle.Close + candle.High + candle.Low) * 0.25m;
            var open = (prevCandle.Open + prevCandle.Close) * 0.5m;
            var high = Math.Max(Math.Max(close, open), candle.High);
            var low = Math.Min(Math.Min(close, open), candle.Low);

            _candles[bar] = new Candle()
            {
                Close = close,
                High = high,
                Low = low,
                Open = open,
            };
        }
    }
}
```

Пример индикатора "Bar's volume filter", который раскрашивает бары в зависимости от их объемов.

```csharp
using ATAS.Indicators;

[DisplayName("Bar's volume filter")]
public class BarVolumeFilter : Indicator
{
    #region Nested types
    public enum VolumeType
    {
        Volume,
        Ticks,
        Delta,
        Bid,
        Ask
    }
    #endregion

    private readonly PaintbarsDataSeries _paintBars = new PaintbarsDataSeries("Paint bars");
    private int _minFilter;
    private int _maxFilter = 100;
    private System.Windows.Media.Color _color = System.Windows.Media.Colors.Orange;
    private VolumeType _volumeType;

    [Display(Name = "Type", Order = 5)]
    public VolumeType Type
    {
        get => _volumeType;
        set { _volumeType = value; RecalculateValues(); }
    }

    [Display(Name = "Minimum", Order = 10)]
    public int MinFilter
    {
        get => _minFilter;
        set { _minFilter = value; RecalculateValues(); }
    }

    [Display(Name = "Maximum", Order = 20)]
    public int MaxFilter
    {
        get => _maxFilter;
        set { _maxFilter = value; RecalculateValues(); }
    }

    [Name = "Color", Order = 30)]
    public System.Windows.Media.Color FilterColor
    {
        get => _color;
        set { _color = value; RecalculateValues(); }
    }

    public BarVolumeFilter() : base(true)
    {
        DataSeries[0] = _paintBars;
        _paintBars.IsHidden = true;
        DenyToChangePanel = true;
    }

    #region Overrides of BaseIndicator
    protected override void OnCalculate(int bar, decimal value)
    {
        var candle = GetCandle(bar);
        decimal volume = 0;
        switch (Type)
        {
            case VolumeType.Volume:
                {
                    volume = candle.Volume;
                    break;
                }
            case VolumeType.Ticks:
                {
                    volume = candle.Ticks;
                    break;
                }
            case VolumeType.Delta:
                {
                    volume = candle.Delta;
                    break;
                }
            case VolumeType.Bid:
                {
                    volume = candle.Bid;
                    break;
                }
            case VolumeType.Ask:
                {
                    volume = candle.Ask;
                    break;
                }
            default:
                throw new ArgumentOutOfRangeException();
        }

        if (volume > _minFilter && volume <= _maxFilter)
        {
            _paintBars[bar] = _color;
        }
        else
        {
            _paintBars[bar] = null;
        }
    }
    #endregion
}
```

**CandleDataSeries - свечи**

`CandleDataSeries` предназначен для визуализации свечей. Каждый элемент представляет собой `Candle`.

Пример индикатора, который отображает инвертированный график:

```csharp
using ATAS.Indicators;

public class ReversalChart : Indicator
{
    readonly CandleDataSeries _reversalCandles = new CandleDataSeries ("Candles");

    public ReversalChart()
    {
        ((ValueDataSeries)DataSeries[0]).VisualType = VisualMode.Hide;
        DataSeries.Add(_reversalCandles);
        Panel = IndicatorDataProvider.NewPanel;
    }

    protected override void OnCalculate(int bar, decimal value)
    {
        var candle = GetCandle(bar);
        _reversalCandles[bar].High = -candle.High;
        _reversalCandles[bar].Low = -candle.Low;
        _reversalCandles[bar].Open = -candle.Open;
        _reversalCandles[bar].Close = -candle.Close;
    }
}
```

**CandlePartSeries**

`CandlePartSeries` представляет ряд данных десятичных значений, полученных из определенных частей `IndicatorCandle`, созданного с помощью `ICandleCreator`.

Этот DataSeries позволяет создавать ряд данных для извлечения определенного параметра свечи. Другими словами, он позволяет генерировать набор данных, содержащий значения определенного параметра (такого как `Open`) на основе набора `IndicatorCandle`. Возможные типы параметров представлены в перечислении `DataSeriesType`.

**IndicatorSeries**

`IndicatorSeries` представляет пользовательский ряд данных для индикатора, производный от `BaseDataSeries<decimal>`.
Этот DataSeries служит оберткой для индикатора, позволяя извлекать данные из указанного ряда данных индикатора через интерфейс `IDataSeries`. Эта функция используется для использования одного индикатора в качестве источника данных для другого, облегчая интеграцию индикаторов.

**PriceSelectionDataSeries - выбор цены**

`PriceSelectionDataSeries`, который представляет список `PriceSelectionValue` для каждого бара, следует использовать для выбора ценовых уровней в кластерах и барах (аналогично выбору индикатора Cluster Search).

Пример использования (выбор кластеров, объем которых выше установленного фильтром):

```csharp
using ATAS.Indicators;

public class SampleClusterSearch : Indicator
{
    private int _filter = 10;
    readonly PriceSelectionDataSeries _priceSelectionSeries = new PriceSelectionDataSeries ("Clust");

    public int Filter
    {
        get { return _filter; }
        set
        {
            _filter = value;
            RecalculateValues();
        }
    }

    public SampleClusterSearch()
    {
        DataSeries.Add(_priceSelectionSeries);
    }

    protected override void OnCalculate(int bar, decimal value)
    {
        var candle = GetCandle(bar);
        for (decimal price = candle.High; price >= candle.Low; price -= TickSize)
        {
            var volumeinfo = candle.GetPriceVolumeInfo(price);
            if (volumeinfo == null)
                continue;

            if (volumeinfo.Volume > _filter)
            {
                var values = _priceSelectionSeries[bar];
                var priceSelection = values.FirstOrDefault(t => t.MinimumPrice == volumeinfo.Price);
                if (priceSelection == null)
                    values.Add(new PriceSelectionValue(volumeinfo.Price)
                    {
                        VisualObject = ObjectType.Rectangle,
                        Size = 10
                    });
            }
        }
    }
}
```

**ObjectDataSeries - любой объект**

`ObjectDataSeries` предназначен для хранения любого объекта.

Его можно использовать для удобной привязки различных объектов к барам. Его также можно использовать для обмена сложными объектами между индикаторами.








## Методичка по созданию графических элементов в индикаторах ATAS

### Вложенные коллекции графических фигур

#### Добавление горизонтальных линий

Для добавления горизонтальных линий необходимо создать объект `LineSeries` и добавить `LineSeries` в его коллекцию. Пример использования в RSI:

```csharp
public class RSI : ATAS.Indicators.Indicator
{
    private readonly SMMA _negative;
    private readonly SMMA _positive;
    public int Period
    {
        get => _positive.Period;
        set
        {
            if (value <= 0)
                return;
            _positive.Period = _negative.Period = value;
            RecalculateValues();
        }
    }

    public RSI()
    {
        Panel = IndicatorDataProvider.NewPanel;

        LineSeries.Add(new LineSeries("Down") // Добавляем новую линию с именем "Down"
        {
            Color = Colors.Orange, // Устанавливаем цвет линии оранжевым
            LineDashStyle = LineDashStyle.Dash, // Устанавливаем стиль линии как пунктир
            Value = 30, // Устанавливаем значение уровня линии на 30
            Width = 1 // Устанавливаем толщину линии равной 1
        });

        LineSeries.Add(new LineSeries("Up") // Добавляем еще одну линию с именем "Up"
        {
            Color = Colors.Orange, // Цвет линии также оранжевый
            LineDashStyle = LineDashStyle.Dash, // Стиль линии пунктирный
            Value = 70, // Значение уровня линии на 70
            Width = 1 // Толщина линии 1
        });

        _positive = new SMMA(); // Инициализация объекта SMMA для положительных значений
        _negative = new SMMA(); // Инициализация объекта SMMA для отрицательных значений
        Period = 10; // Установка периода для SMMA равным 10
    }

    protected override void OnCalculate(int bar, decimal value)
    {
        if (bar == 0)
        {
            this[bar] = 0;
        }
        else
        {
            var diff = (decimal)SourceDataSeries[bar] - (decimal)SourceDataSeries[bar - 1]; // Вычисляем разницу между текущим и предыдущим значениями SourceDataSeries
            var pos = _positive.Calculate(bar, diff > 0 ? diff : 0); // Вычисляем SMMA для положительных разниц
            var neg = _negative.Calculate(bar, diff < 0 ? -diff : 0); // Вычисляем SMMA для отрицательных разниц

            if (neg != 0)
            {
                var div = pos / neg; // Вычисляем отношение положительного SMMA к отрицательному
                this[bar] = div == 1 // Если отношение равно 1
                    ? 0m // Устанавливаем значение индикатора в 0
                    : 100m - 100m / (1m + div); // Иначе вычисляем значение по формуле
            }
            else
            {
                this[bar] = 100m; // Если отрицательный SMMA равен 0, устанавливаем значение индикатора в 100
            }
        }
    }
}
```

#### Добавление линий тренда

Можно добавить линии тренда на график с помощью коллекции `TrendLines`.

Для этого необходимо создать объект `TrendLine`, указав начальный номер бара, начальную цену точки, конечный номер бара и конечную цену точки. Флаг `IsRay` также можно установить, чтобы преобразовать линию тренда в полулинию.

После создания объекта его следует просто добавить в коллекцию `TrendLines`.

#### Рисование прямоугольных областей

Прямоугольные области можно добавить на график с помощью коллекции `Rectangles`.

Для этого следует создать объект `DrawingRectangle`, указав начальный номер бара, начальную цену точки, конечный номер бара, конечную цену точки, цвет заливки и цвет границ.

После создания объекта его следует просто добавить в коллекцию `Rectangles`.

#### Горизонтальные линии до первого касания

Можно создавать горизонтальные линии в индикаторе, которые будут продлены до первого касания, с помощью коллекции `HorizontalLinesTillTouch`.

Для этого необходимо создать объект `LineTillTouch` и добавить его в коллекцию.

`LineTillTouch` имеет 2 конструктора:

*   **1.** Создает горизонтальную линию, которая начинается на указанном баре по указанной цене.

    ```csharp
    public LineTillTouch(int bar, decimal price, Pen pen)
    ```

*   **2.** Аналогично создает горизонтальную линию, для которой количество баров, по истечении которых линия будет считаться завершенной, устанавливается дополнительно.

    ```csharp
    public LineTillTouch(int bar, decimal price, Pen pen, int fixedBarsCount)
    ```

#### Добавление текстовых меток

Текстовые метки можно добавлять, используя следующие перегрузки метода `AddText`:

```csharp
AddText(string tag, string text, bool isAbovePrice, int bar, decimal price,
        int yOffset, int xOffset, Color textColor, Color outlineColor, Color fillColor,
        float fontSize, DrawingText.TextAlign align, bool autoSize = false)
```

```csharp
AddText(string tag, string text, bool isAbovePrice, int bar, decimal price,
        Color textColor, Color outlineColor, Color fillColor,
        float fontSize, DrawingText.TextAlign align, bool autoSize = false)
```

```csharp
AddText(string tag, string text, bool isAbovePrice, int bar, decimal price,
        Color textColor, Color fillColor, float fontSize, DrawingText.TextAlign align,
        bool autoSize = false)
```

Передаваемые параметры:

*   `tag` - уникальный текстовый идентификатор, который связан с текстовой меткой.
*   `text` - отображаемый текст.
*   `isAbovePrice` - определяет, отображать текст над ценой или нет.
*   `bar` - номер бара, на котором должен отображаться текст.
*   `price` - цена, по которой должен отображаться текст.
*   `yOffset` - смещение в пикселях по оси Y.
*   `xOffset` - смещение в пикселях по оси X.
*   `textColor` - цвет текста.
*   `outlineColor` - цвет рамки вокруг текста.
*   `fillColor` - цвет фона текста.
*   `fontSize` - размер шрифта.
*   `align` - выравнивание текста (слева, справа или по центру).
*   `autoSize` - флаг, отвечающий за активацию динамического размера текста в зависимости от масштаба графика.

Все три метода возвращают объект `DrawingText`, в котором параметры можно изменять динамически.






## Методичка по рисованию индикаторов в ATAS

### Рисование

#### Основы

В индикаторе можно переопределить метод `OnRender` и реализовать собственную логику отображения данных.

Для того чтобы этот метод начал вызываться, необходимо установить флаг `EnableCustomDrawing` в значение `true`.

Также необходимо задать список слоев, на которых будет выполняться рендеринг.

Список слоев задается через метод `SubscribeToDrawingEvents`, в который передаются флаги `DrawingLayouts`.

Флаги `DrawingLayouts` могут быть следующими:

*   `None` — в этом случае ничего не будет отображаться.
*   `Historical` — в этом случае рендеринг будет вызываться при каждой новой свече, при сжатии и перемещении графика.
*   `LatestBar` — рендеринг вызывается при изменении самой последней свечи. Как правило, это происходит при каждом новом тике.
*   `Final` — финальный слой, который отображается при каждом рендеринге графика. Например, при перемещении мыши.

Если метод `SubscribeToDrawingEvents` не вызывается, индикатор будет отображаться только при изменении самой последней свечи.

Пример вызова `SubscribeToDrawingEvents`, после которого метод `OnRender` будет вызываться при каждом новом тике и после финального рендеринга:

```csharp
SubscribeToDrawingEvents(DrawingLayouts.Final | DrawingLayouts.LatestBar);
```

Следующие объекты передаются в метод `OnRender`:

*   `RenderContext context` — контекст, в котором будет происходить рендеринг.
*   `DrawingLayouts layout` — слой, который отображается в текущий момент.

#### RenderContext

Рендеринг с использованием принципов, подобных GDI+, фактически осуществляется с помощью этого контекста.

Несколько примеров:

```csharp
using ATAS.Indicators;

protected virtual void OnRender(RenderContext context, DrawingLayouts layout)
{
    // Нарисовать прямоугольник (ширина = 100; высота = 200) от точки (x = 5; y = 10)
    context.DrawRectangle(RenderPens.Blue, new Rectangle(5, 10, 100, 200));

    // Залить прямоугольник (ширина = 100; высота = 100) от точки (x = 0; y = 0)
    context.FillRectangle(Color.DarkSalmon, new Rectangle(0, 0, 100, 100));

    // Нарисовать линию от точки (x = 10; y = 20) до точки (x = 50; y = 60)
    context.DrawLine(RenderPens.AliceBlue, 10, 20, 50, 60);

    // Нарисовать строку в точке (x = 50; y = 60)
    context.DrawString("Sample string", new RenderFont("Arial", 15), Color.Black, 50, 60);

    // Нарисовать эллипс внутри прямоугольника
    context.DrawEllipse(new RenderPen(Color.Bisque), new Rectangle(10, 10, 100, 100));
}
```

#### Система координат

Начало координат графика находится в верхнем левом углу.

Система координат показана на рисунке ниже.

![Система координат](image_coordinates_system.png)  \*Замените 'image_coordinates_system.png' на путь к изображению из PDF, если оно есть\*

Каждый индикатор имеет контейнер (свойство `Container`), который содержит информацию об области рендеринга (свойство `Region`). Все основные свойства графика, мыши и клавиатуры можно получить через свойство `ChartInfo`.

Свойство `ChartArea`, которое возвращает `Region`, было добавлено для удобства доступа к области графика.

Свойство `MouseLocationInfo`, которое возвращает `ChartInfo.MouseLocationInfo`, было добавлено для удобства доступа к данным мыши.

Также существуют расширения `ChartInfo`, которые позволяют работать с координатами:

*   `GetXByBar(int bar, bool isStartOfBar)` — метод возвращает X-координату для переданного номера бара. Если `isStartOfBar = true`, возвращается координата начала бара, в противном случае — координата середины бара.
*   `GetYByPrice(decimal price, bool isStartOnPriceLevel)` — метод возвращает Y-координату для переданной цены. Если `isStartOnPriceLevel = true`, возвращается координата начала ценового уровня, в противном случае — координата середины ценового уровня.

Пример индикатора, который рисует пересечение и показывает объем бара, над которым находится указатель мыши.

```csharp
using ATAS.Indicators;

public class SampleRendering : Indicator
{
    public SampleRendering()
    {
        EnableCustomDrawing = true;

        // Подписка только на рисование на финальном слое
        SubscribeToDrawingEvents(DrawingLayouts.Final);
    }

    protected override void OnRender(RenderContext context, DrawingLayouts layout)
    {
        // создание пера, ширина 4px
        var pen = new RenderPen(Color.BlueViolet, 4);

        // рисование горизонтальной линии
        context.DrawLine(pen, 0, MouseLocationInfo.LastPosition.Y, ChartArea.Width, MouseLocationInfo.LastPosition.Y);

        // рисование вертикальной линии
        context.DrawLine(pen, MouseLocationInfo.LastPosition.X, 0, MouseLocationInfo.LastPosition.X, ChartArea.Height);

        var candle = GetCandle(MouseLocationInfo.BarBelowMouse);
        if (candle != null)
        {
            var font = new RenderFont("Arial", 14);
            var text = $"Total candle volume={candle.Volume}";
            var textSize = context.MeasureString(text, font);
            var textRectangle = new Rectangle(MouseLocationInfo.LastPosition.X + 10, MouseLocationInfo.LastPosition.Y - textSize.Height, textSize.Width, textSize.Height);

            context.FillRectangle(Color.CornflowerBlue, textRectangle);
            context.DrawString(text, font, Color.AliceBlue, textRectangle);
        }
    }

    protected override void OnCalculate(int bar, decimal value)
    {
    }
}
```

![Индикатор объема](image_volume_indicator.png) \*Замените 'image_volume_indicator.png' на путь к изображению из PDF, если оно есть\*
*Показывает объем бара*

#### Рендеринг индикатора выше и ниже графика

Свойство индикатора `DrawAbovePrice` может использоваться для регулировки порядка рендеринга индикатора.

Если `DrawAbovePrice = true`, индикатор отображается над графиком, иначе — под графиком.






## Методичка по работе с клавиатурой и мышью в ATAS Indicators

### Основы

Индикаторы в ATAS могут реагировать на события клавиатуры и мыши.

Для обработки этих событий в индикаторе предусмотрены следующие методы, которые можно переопределить:

*   **ProcessMouseClick(RenderControlMouseEventArgs e)** - обработка кликов мыши.
*   **ProcessMouseDown(RenderControlMouseEventArgs e)** - обработка нажатия кнопки мыши.
*   **ProcessMouseUp(RenderControlMouseEventArgs e)** - обработка отпускания кнопки мыши.
*   **ProcessMouseMove(RenderControlMouseEventArgs e)** - обработка движения мыши.
*   **ProcessMouseDoubleClick(RenderControlMouseEventArgs e)** - обработка двойного клика мыши.
*   **ProcessKeyDown(KeyEventArgs e)** - обработка нажатия клавиши на клавиатуре.
*   **ProcessKeyUp(KeyEventArgs e)** - обработка отпускания клавиши на клавиатуре.

Каждый из этих методов должен возвращать значение `true` или `false`.

Если метод возвращает `true`, это означает, что индикатор обработал это событие, и нет необходимости вызывать этот же метод в следующих индикаторах.

Эта логика необходима для ситуаций, когда на графике присутствует несколько индикаторов, и только один из них (первый, кто обработал событие) должен это делать.

Также, в некоторых случаях, в зависимости от положения мыши, может потребоваться возможность изменить курсор. Для этого предусмотрен метод **GetCursor(RenderControlMouseEventArgs e)**.

Переопределив этот метод, можно вернуть курсор в зависимости от положения мыши.

### Пример работы с событиями

```csharp
using ATAS.Indicators; // Пространство имен ATAS Indicators

public class SampleKeyboardAndMouseProcessor : Indicator // Объявление класса индикатора SampleKeyboardAndMouseProcessor, наследующегося от Indicator
{
    private bool _selected; // Приватная переменная типа bool для хранения состояния "выбран"
    private Rectangle _rectangle = new Rectangle(30, 30, 150, 50); // Приватная переменная типа Rectangle для хранения прямоугольника, заданного координатами (x=30, y=30, ширина=150, высота=50)
    private Point _pressedPoint; // Приватная переменная типа Point для хранения координат точки нажатия мыши
    private Point _lastPoint; // Приватная переменная типа Point для хранения последних координат мыши
    private bool _pressed; // Приватная переменная типа bool для хранения состояния "нажата кнопка мыши"

    public SampleKeyboardAndMouseProcessor() // Конструктор класса SampleKeyboardAndMouseProcessor
    : base(true) // Вызов конструктора базового класса Indicator с параметром true (указывает, что индикатор будет использовать собственные отрисовки)
    {
        EnableCustomDrawing = true; // Разрешить пользовательскую отрисовку индикатора
        DenyToChangePanel = true; // Запретить изменение панели индикатора пользователем
        DataSeries[0].IsHidden = true; // Скрыть первую (и единственную в данном случае) DataSeries индикатора на графике
        SubscribeToDrawingEvents(DrawingLayouts.Final); // Подписаться на событие отрисовки DrawingLayouts.Final, которое происходит в самом конце отрисовки графика
    }

    #region Overrides of BaseIndicator // Регион кода для переопределения методов базового класса BaseIndicator
    protected override void OnCalculate(int bar, decimal value) // Переопределение метода OnCalculate, вызываемого для каждого бара графика (в данном примере пустой, т.к. индикатор не выполняет расчетов на основе данных графика)
    {
    }
    #endregion

    protected override void OnRender (RenderContext context, DrawingLayouts layout) // Переопределение метода OnRender, вызываемого для отрисовки индикатора на графике
    {
        context.FillRectangle(System.Drawing.Color.Coral, _rectangle); // Заполнить прямоугольник _rectangle коралловым цветом

        if (_selected) // Если индикатор выбран (_selected == true)
            context.DrawRectangle(new RenderPen(System.Drawing.Color.Blue, 3), _rectangle); // Нарисовать контур прямоугольника _rectangle синим цветом толщиной 3 пикселя

        context.DrawString(DateTime.Now.ToString("T"), new RenderFont("Arial", 20), System.Drawing.Color.White, _rectangle.Location); // Отобразить текущее время (только часы и минуты) белым цветом шрифтом Arial размером 20 в левом верхнем углу прямоугольника _rectangle
    }

    public override bool ProcessMouseClick(RenderControlMouseEventArgs e) // Переопределение метода ProcessMouseClick для обработки клика мыши
    {
        this.LogInfo("Mouse click"); // Записать в лог сообщение "Mouse click"
        return false; // Вернуть false, чтобы событие могло быть обработано другими индикаторами
    }

    public override bool ProcessMouseDown(RenderControlMouseEventArgs e) // Переопределение метода ProcessMouseDown для обработки нажатия кнопки мыши
    {
        this.LogInfo("Mouse down"); // Записать в лог сообщение "Mouse down"
        if (e.Button == RenderControlMouseButtons.Left && IsPointInsideRectangle(_rectangle, e.Location)) // Если нажата левая кнопка мыши И точка клика (e.Location) находится внутри прямоугольника _rectangle
        {
            _pressedPoint = _lastPoint = e.Location; // Запомнить точку нажатия мыши в _pressedPoint и _lastPoint
            _pressed = true; // Установить флаг _pressed в true, указывая, что кнопка мыши нажата
            return true; // Вернуть true, чтобы указать, что индикатор обработал событие
        }
        return false; // Вернуть false, если событие не обработано
    }

    public override bool ProcessMouseUp(RenderControlMouseEventArgs e) // Переопределение метода ProcessMouseUp для обработки отпускания кнопки мыши
    {
        this.LogInfo("Mouse up"); // Записать в лог сообщение "Mouse up"
        if (Math.Abs(e.Location.X - _pressedPoint.X) < 4 && Math.Abs(e.Location.Y - _pressedPoint.Y) < 4) // Если смещение мыши между нажатием и отпусканием кнопки мыши по X и Y координатам меньше 4 пикселей (то есть клик был почти на месте нажатия)
        {
            _selected = !_selected; // Инвертировать состояние _selected (если было true, станет false, и наоборот)
        }
        _pressed = false; // Сбросить флаг _pressed в false, указывая, что кнопка мыши отпущена
        return true; // Вернуть true, чтобы указать, что индикатор обработал событие
    }

    public override bool ProcessMouseMove(RenderControlMouseEventArgs e) // Переопределение метода ProcessMouseMove для обработки движения мыши
    {
        if (_pressed && _selected) // Если кнопка мыши нажата (_pressed == true) И индикатор выбран (_selected == true)
        {
            var dx = e.Location.X - _lastPoint.X; // Вычислить смещение мыши по X координате (delta X)
            var dy = e.Location.Y - _lastPoint.Y; // Вычислить смещение мыши по Y координате (delta Y)
            _lastPoint = e.Location; // Обновить _lastPoint текущими координатами мыши
            _rectangle.X += dx; // Изменить X координату прямоугольника _rectangle на величину dx
            _rectangle.Y += dy; // Изменить Y координату прямоугольника _rectangle на величину dy
            return true; // Вернуть true, чтобы указать, что индикатор обработал событие
        }
        return false; // Вернуть false, если событие не обработано
    }

    public override bool ProcessMouseDoubleClick(RenderControlMouseEventArgs e) // Переопределение метода ProcessMouseDoubleClick для обработки двойного клика мыши
    {
        this.LogInfo("Mouse double click"); // Записать в лог сообщение "Mouse double click"
        return false; // Вернуть false, чтобы событие могло быть обработано другими индикаторами
    }

    public override bool ProcessKeyDown(KeyEventArgs e) // Переопределение метода ProcessKeyDown для обработки нажатия клавиши на клавиатуре
    {
        this.LogInfo("Key down {0}", e.Key); // Записать в лог сообщение "Key down" и код нажатой клавиши (e.Key)
        return false; // Вернуть false, чтобы событие могло быть обработано другими индикаторами
    }

    public override bool ProcessKeyUp(KeyEventArgs e) // Переопределение метода ProcessKeyUp для обработки отпускания клавиши на клавиатуре
    {
        this.LogInfo("Key up {0}", e.Key); // Записать в лог сообщение "Key up" и код отпущенной клавиши (e.Key)
        return false; // Вернуть false, чтобы событие могло быть обработано другими индикаторами
    }

    public override StdCursor GetCursor(RenderControlMouseEventArgs e) // Переопределение метода GetCursor для изменения курсора мыши
    {
        if (_selected && IsPointInsideRectangle(_rectangle, e.Location)) // Если индикатор выбран (_selected == true) И точка, где находится курсор (e.Location), находится внутри прямоугольника _rectangle
            return StdCursor.Hand; // Вернуть курсор в виде "руки" (StdCursor.Hand)

        return StdCursor.NULL; // В противном случае вернуть стандартный курсор (StdCursor.NULL)
    }

    private bool IsPointInsideRectangle(Rectangle rectangle, Point point) // Приватный метод IsPointInsideRectangle для проверки, находится ли точка point внутри прямоугольника rectangle
    {
        return point.X >= rectangle.X && point.X <= rectangle.X + rectangle.Width && point.Y >= rectangle.Y && point.Y <= rectangle.Y + rectangle.Height; // Вернуть true, если X координата точки больше или равна X координате прямоугольника И X координата точки меньше или равна X координате прямоугольника + ширина прямоугольника И Y координата точки больше или равна Y координате прямоугольника И Y координата точки меньше или равна Y координате прямоугольника + высота прямоугольника (то есть точка находится внутри прямоугольника), иначе вернуть false
    }
}
```

### Работа с методом "GetCursor"

*(Здесь должна быть вставлена картинка из PDF, показывающая пример работы индикатора на графике)*

**Описание:**

Этот индикатор демонстрирует базовую обработку событий мыши и клавиатуры. Он рисует прямоугольник кораллового цвета на графике.

*   При клике мышью в лог выводится сообщение "Mouse click".
*   При нажатии левой кнопки мыши внутри прямоугольника, прямоугольник становится "выбранным", и его контур отрисовывается синим цветом. Также в лог выводится сообщение "Mouse down".
*   При отпускании кнопки мыши, если клик был почти на месте нажатия, состояние "выбран" инвертируется (выбирается или снимается выделение). В лог выводится сообщение "Mouse up".
*   При перетаскивании мыши с нажатой левой кнопкой и выбранном прямоугольнике, прямоугольник перемещается вслед за курсором мыши.
*   При двойном клике мышью в лог выводится сообщение "Mouse double click".
*   При нажатии и отпускании клавиш на клавиатуре в лог выводятся сообщения "Key down" и "Key up" с кодом клавиши.
*   Когда курсор мыши находится внутри выбранного прямоугольника, курсор меняется на "руку".

Этот пример показывает, как можно использовать методы обработки событий мыши и клавиатуры для создания интерактивных индикаторов в ATAS.






## Методичка по управлению графиком в ATAS Indicator

**Управление графиком** осуществляется как через меню настроек графика, так и путем ввода соответствующих параметров непосредственно в индикатор. В последнем случае индикатор может быть снабжен полями для настройки графических параметров.

Основные параметры графика доступны через объект `ChartInfo`.

Рассмотрим некоторые свойства этого объекта:

*   **FootprintContentMode** - Режим отображения текстового содержимого внутри кластеров.
*   **FootprintVisualMode** - Режим визуализации кластеров (гистограммы, "bid" и "ask" ячейки и т.д.).
*   **FootprintColorScheme** - Цветовая схема для кластеров.
*   **ChartVisualMode** - Это свойство облегчает как получение значений, так и определение типа графика.
*   **PriceChartContainer**:
    *   **SetCustomBarsSpacing(decimal? value)** - Этот метод позволяет установить пользовательское расстояние между барами.
    *   **SetCustomBarsWidth(decimal value, bool freezeValue)** - Этот метод позволяет установить пользовательскую ширину бара и указать, могут ли пользователи вручную регулировать ширину через интерфейс.
*   **CoordinatesManager**:
    *   **MoveChartUpAndDown(int offset)** - Этот метод сдвигает график вверх или вниз на указанное расстояние.
    *   **ChangeRowsHeight(decimal newHeight)** - Этот метод изменяет высоту строк ценовых уровней.
    *   **EnableAutoScale()** - Этот метод включает автоматическое масштабирование графика.
    *   **ScrollPrice(int ticksCount)** - Этот метод прокручивает график вдоль ценовой оси на указанное количество тиков.

```csharp
public class SampleChartManaging : ATAS.Indicators.Indicator
{
    // Объявляем приватные переменные для хранения параметров графика.
    private decimal _barsSpacing = 1; // Расстояние между барами по умолчанию 1
    private decimal _barsWidth = 1;   // Ширина баров по умолчанию 1
    private ChartVisualModes _modes;    // Режим визуализации графика (enum)
    private FootprintVisualModes _visualMode; // Режим визуализации футпринта (enum)
    private FootprintContentModes _contentMode; // Режим содержимого футпринта (enum)
    private FootprintColorSchemes _colorScheme; // Цветовая схема футпринта (enum)
    private int _moveChartUpAndDown; // Смещение графика вверх/вниз
    private decimal _changeRowsHeight = 3; // Высота строк цены по умолчанию 3
    private int _scrollPrice;        // Прокрутка графика по цене

    // Свойство BarsSpacing для настройки расстояния между барами через интерфейс индикатора.
    [Range (1, 1000)] // Атрибут Range задает допустимый диапазон значений от 1 до 1000
    public decimal BarsSpacing
    {
        get => _barsSpacing; // get возвращает текущее значение _barsSpacing
        set                  // set устанавливает новое значение и обновляет график
        {
            _barsSpacing = value; // Присваиваем новое значение приватной переменной _barsSpacing
            ChartInfo?.PriceChartContainer?.SetCustomBarsSpacing(_barsSpacing); // Используем метод SetCustomBarsSpacing объекта PriceChartContainer для изменения расстояния между барами на графике. '?' - оператор условного доступа, чтобы избежать ошибок, если ChartInfo или PriceChartContainer null.
            RedrawChart(); // Вызываем метод RedrawChart() для перерисовки графика с новыми параметрами.
        }
    }
```

```csharp
    // Свойство BarsWidth для настройки ширины баров через интерфейс индикатора.
    [Range (1, 1000)] // Атрибут Range задает допустимый диапазон значений от 1 до 1000
    public decimal BarsWidth
    {
        get => _barsWidth; // get возвращает текущее значение _barsWidth
        set                  // set устанавливает новое значение и обновляет график
        {
            _barsWidth = value; // Присваиваем новое значение приватной переменной _barsWidth
            ChartInfo?.PriceChartContainer?.SetCustomBarsWidth(_barsWidth, false); // Используем метод SetCustomBarsWidth объекта PriceChartContainer для изменения ширины баров на графике. Второй параметр 'false' указывает, что пользователь не может вручную изменять ширину.
            RedrawChart(); // Вызываем метод RedrawChart() для перерисовки графика с новыми параметрами.
        }
    }

    // Свойство ChartMode для настройки режима визуализации графика (например, Clusters, стандартные бары и т.д.).
    public ChartVisualModes ChartMode
    {
        get => _modes; // get возвращает текущее значение _modes
        set             // set устанавливает новое значение и обновляет график
        {
            _modes = value; // Присваиваем новое значение приватной переменной _modes
            if (ChartInfo != null) // Проверяем, что объект ChartInfo не null
                ChartInfo.ChartVisualMode = value; // Устанавливаем режим визуализации графика через свойство ChartVisualMode объекта ChartInfo.
            RedrawChart(); // Вызываем метод RedrawChart() для перерисовки графика с новыми параметрами.
        }
    }

    // Свойство FootprintContentMode для настройки режима отображения содержимого футпринта (например, Volume, Delta и т.д.).
    public FootprintContentModes FootprintContentMode
    {
        get => _contentMode; // get возвращает текущее значение _contentMode
        set                 // set устанавливает новое значение и обновляет график
        {
            _contentMode = value; // Присваиваем новое значение приватной переменной _contentMode
            if (ChartInfo != null) // Проверяем, что объект ChartInfo не null
                ChartInfo.FootprintContentMode = value; // Устанавливаем режим содержимого футпринта через свойство FootprintContentMode объекта ChartInfo.
        }
    }
```

```csharp
    // Свойство FootprintVisualMode для настройки режима визуализации футпринта (например, FullRow, Clusters и т.д.).
    public FootprintVisualModes FootprintVisualMode
    {
        get => _visualMode; // get возвращает текущее значение _visualMode
        set                // set устанавливает новое значение и обновляет график
        {
            _visualMode = value; // Присваиваем новое значение приватной переменной _visualMode
            if (ChartInfo != null) // Проверяем, что объект ChartInfo не null
                ChartInfo.FootprintVisualMode = value; // Устанавливаем режим визуализации футпринта через свойство FootprintVisualMode объекта ChartInfo.
        }
    }

    // Свойство FootprintColorScheme для настройки цветовой схемы футпринта (например, Delta, Volume и т.д.).
    public FootprintColorSchemes FootprintColorScheme
    {
        get => _colorScheme; // get возвращает текущее значение _colorScheme
        set                 // set устанавливает новое значение и обновляет график
        {
            _colorScheme = value; // Присваиваем новое значение приватной переменной _colorScheme
            if (ChartInfo != null) // Проверяем, что объект ChartInfo не null
                ChartInfo.FootprintColorScheme = value; // Устанавливаем цветовую схему футпринта через свойство FootprintColorScheme объекта ChartInfo.
        }
    }

    // Свойство MoveChartUpAndDown для смещения графика вверх и вниз.
    public int MoveChartUpAndDown
    {
        get => _moveChartUpAndDown; // get возвращает текущее значение _moveChartUpAndDown
        set                         // set устанавливает новое значение и обновляет график
        {
            var diff = _moveChartUpAndDown - value; // Вычисляем разницу между текущим и новым значением смещения.
            ChartInfo?.CoordinatesManager?.MoveChartUpAndDown(diff); // Используем метод MoveChartUpAndDown объекта CoordinatesManager для смещения графика на величину 'diff'.
            _moveChartUpAndDown = value; // Обновляем значение приватной переменной _moveChartUpAndDown.
        }
    }

    // Свойство ChangeRowsHeight для изменения высоты строк цены.
    [Range(1, 100)] // Атрибут Range задает допустимый диапазон значений от 1 до 100
    public decimal ChangeRowsHeight
    {
        get => _changeRowsHeight; // get возвращает текущее значение _changeRowsHeight
        set                      // set устанавливает новое значение и обновляет график
        {
            _changeRowsHeight = value; // Присваиваем новое значение приватной переменной _changeRowsHeight
            ChartInfo?.CoordinatesManager?.ChangeRowsHeight(value); // Используем метод ChangeRowsHeight объекта CoordinatesManager для изменения высоты строк цены.
        }
    }

    // Свойство ScrollPrice для прокрутки графика по цене.
    public int ScrollPrice
    {
        get => _scrollPrice; // get возвращает текущее значение _scrollPrice
        set                 // set устанавливает новое значение и обновляет график
        {
            var diff = _scrollPrice - value; // Вычисляем разницу между текущим и новым значением прокрутки.
            ChartInfo?.CoordinatesManager?.ScrollPrice(diff); // Используем метод ScrollPrice объекта CoordinatesManager для прокрутки графика на величину 'diff'.
            _scrollPrice = value; // Обновляем значение приватной переменной _scrollPrice.
        }
    }

    // Переопределенный метод OnCalculate - вызывается при каждом расчете индикатора (на каждом баре). В данном примере пустой, так как индикатор управляет только параметрами графика.
    protected override void OnCalculate(int bar, decimal value)
    {
    }

    // Переопределенный метод OnDispose - вызывается при удалении индикатора с графика.
    protected override void OnDispose()
    {
        // При удалении индикатора, возвращаем стандартные значения расстояния и ширины баров, чтобы не оставались пользовательские настройки.
        ChartInfo?.PriceChartContainer?.SetCustomBarsSpacing(null); // Устанавливаем расстояние между барами в значение по умолчанию (null).
        ChartInfo?.PriceChartContainer?.SetCustomBarsWidth(ChartInfo.PriceChartContainer.BarsWidth, false); // Возвращаем стандартную ширину баров. Используем текущую ширину графика BarsWidth и 'false', чтобы запретить ручное изменение.
    }
}
```

**Описание кода:**

*   **`public class SampleChartManaging : ATAS.Indicators.Indicator`**: Объявление класса индикатора `SampleChartManaging`, наследующегося от базового класса `ATAS.Indicators.Indicator`.
*   **`private decimal _barsSpacing = 1;` и т.д.** : Объявление приватных переменных для хранения значений параметров графика.
*   **`[Range(1, 1000)]`**: Атрибуты `Range` используются для задания допустимого диапазона значений для свойств, отображаемых в настройках индикатора.
*   **`public decimal BarsSpacing { get; set; }` и т.д.** : Объявление публичных свойств с блоками `get` и `set`. Блок `get` возвращает текущее значение, блок `set` устанавливает новое значение и выполняет действия по изменению параметров графика через объект `ChartInfo`.
*   **`ChartInfo?.PriceChartContainer?.SetCustomBarsSpacing(_barsSpacing);`**:  Использование операторов условного доступа `?.` для безопасного доступа к свойствам и методам объекта `ChartInfo`. Это предотвращает ошибки, если `ChartInfo` или `PriceChartContainer` окажутся `null`.
*   **`RedrawChart();`**: Метод `RedrawChart()` вызывает перерисовку графика после изменения параметров.
*   **`protected override void OnDispose()`**: Метод `OnDispose()` вызывается при удалении индикатора и используется для возврата стандартных настроек графика.

**Практическое применение:**

Данный индикатор `SampleChartManaging` позволяет управлять различными параметрами графика непосредственно из настроек индикатора, такими как расстояние между барами, ширина баров, режим отображения графика и футпринта, смещение и прокрутка графика. Это демонстрирует, как можно использовать объект `ChartInfo` для программного управления графиком в ATAS.

**Chart managing from indicator** (Подпись под скриншотом) - Управление графиком из индикатора.








## Методичка по созданию индикатора ATAS для управления категориями и свойствами

**Название:** Управление категориями и свойствами в настройках индикатора

**Введение:**

Этот индикатор демонстрирует, как контролировать сворачивание и разворачивание категорий и свойств, чтобы предоставить пользователю гибкий и удобный интерфейс для управления объектами.

**Основные функции:**

*   **Управление категориями:** Свойство `CategoryManager` управляет состоянием сворачивания/разворачивания для категории с именем `Managed category`.
    *   *Аннотация:* Это свойство позволяет пользователю сворачивать или разворачивать группу настроек индикатора, что делает интерфейс более организованным.

*   **Управление свойствами:** Свойство `PropertyManager` управляет состоянием сворачивания/разворачивания свойства `ExpandableProperty`, которое представлено типом `PenSettings`.
    *   *Аннотация:* Это свойство позволяет пользователю сворачивать или разворачивать отдельные свойства внутри группы настроек, обеспечивая дополнительную гибкость.

**Основные свойства:**

*   `CategoryManager` - контролирует, развернута ли категория `Managed category` в редакторе.
    *   *Аннотация:* Логическое свойство, определяющее видимость группы настроек 'Managed category'.

*   `PropertyManager` - контролирует, развернуто ли свойство `ExpandableProperty` в редакторе.
    *   *Аннотация:* Логическое свойство, определяющее видимость настроек свойства 'ExpandableProperty'.

*   `ExpandableProperty` - управляемое свойство.
    *   *Аннотация:*  Свойство типа `PenSettings`, чьи настройки могут быть скрыты или показаны.

*   `ManagedProperty1` и `ManagedProperty2` - свойства, принадлежащие категории `Managed category`.
    *   *Аннотация:* Пример свойств, которые будут сгруппированы под категорией 'Managed category'.

**Шаги для создания индикатора:**

1.  **Наследуйте ваш индикатор от класса `Indicator` и реализуйте интерфейс `IPropertiesEditorOwner`, чтобы ваш индикатор мог взаимодействовать с редактором свойств:**

```csharp
using ATAS.Indicators; // Пространство имен для базовых классов индикаторов ATAS
using OFT.Rendering.Settings; // Пространство имен для настроек рендеринга OFT
using System.ComponentModel; // Пространство имен для атрибутов ComponentModel, таких как DisplayName
using System.ComponentModel.DataAnnotations; // Пространство имен для атрибутов DataAnnotations
using System.Drawing; // Пространство имен для работы с графикой и цветами

[DisplayName("Expand and collapse categories or properties")] // Атрибут DisplayName задает отображаемое имя индикатора в ATAS
public class ExpandAndCollapseCategoriesOrPropertiesIndicator : Indicator, IPropertiesEditorOwner // Объявление класса индикатора, наследуемся от Indicator и реализуем IPropertiesEditorOwner
{
    // Тело класса индикатора будет добавлено на следующих шагах
}
```
    *   *Аннотация:* Этот код создает основу индикатора, подключает необходимые пространства имен и объявляет класс `ExpandAndCollapseCategoriesOrPropertiesIndicator`, который будет нашим индикатором. Атрибут `DisplayName` задает имя, которое пользователь увидит в списке индикаторов ATAS. Интерфейс `IPropertiesEditorOwner` необходим для управления свойствами индикатора в редакторе настроек.

2.  **Определите приватные поля для хранения состояния категорий и свойств, а также для работы с объектом редактора свойств:**

```csharp
private IPropertiesEditor _propertiesEditor; // Приватное поле для хранения объекта редактора свойств, интерфейс IPropertiesEditor
private bool _categoryManager; // Приватное поле для хранения состояния категории (свернута/развернута)
private bool _propertyManager; // Приватное поле для хранения состояния свойства (свернуто/развернуто)
```
    *   *Аннотация:*  Эти приватные поля используются внутри индикатора для хранения и управления состоянием свойств и категорий. `_propertiesEditor` нужен для взаимодействия с редактором свойств ATAS, а `_categoryManager` и `_propertyManager` хранят текущее состояние видимости категорий и свойств.

3.  **Добавьте публичные свойства для группировки и отображения в редакторе свойств:**

```csharp
[Display (Name = "Category", GroupName = "Management")] // Атрибут Display задает имя свойства и группу в редакторе свойств
public bool CategoryManager // Публичное свойство CategoryManager типа bool
{
    get => _categoryManager; // Геттер возвращает текущее значение приватного поля _categoryManager
    set => SetProperty(ref _categoryManager, value, () => _propertiesEditor?.SetIsExpandedCategory(_managedCategoryCategoryName, value)); // Сеттер устанавливает значение и вызывает SetProperty для уведомления об изменениях и управления состоянием категории
}

[Display (Name = "Property", GroupName = "Management")] // Атрибут Display задает имя свойства и группу в редакторе свойств
public bool PropertyManager // Публичное свойство PropertyManager типа bool
{
    get => _propertyManager; // Геттер возвращает текущее значение приватного поля _propertyManager
    set => SetProperty(ref _propertyManager, value, () => _propertiesEditor?.SetIsExpandedProperty(nameof(ExpandableProperty), value)); // Сеттер устанавливает значение и вызывает SetProperty для уведомления об изменениях и управления состоянием свойства
}
```
    *   *Аннотация:* Эти публичные свойства `CategoryManager` и `PropertyManager` отображаются в редакторе настроек индикатора. Атрибуты `[Display]` определяют, как эти свойства будут выглядеть в интерфейсе пользователя ATAS. `GroupName = "Management"` группирует эти свойства в категорию "Management". `SetProperty` используется для уведомления системы об изменении свойства и выполнения дополнительных действий, таких как сворачивание/разворачивание категорий и свойств в редакторе.

4.  **Используйте `SetProperty` для уведомления об изменениях и выполнения определенных действий, когда состояние свойства меняется.**

    *   *Аннотация:*  Метод `SetProperty` является ключевым для обновления интерфейса редактора свойств ATAS при изменении значений свойств индикатора. Он гарантирует, что изменения в коде индикатора отражаются в пользовательском интерфейсе редактора настроек.

    *   **В этом примере, используя свойство `CategoryManager`, мы будем управлять группой свойств, таких как `ManagedProperty1` и `ManagedProperty2`, а используя свойство `PropertyManager`, мы будем управлять свойством `ExpandableProperty` типа `PenSettings`, который имеет несколько свойств для конфигурации.**

```csharp
[Display (Name = "Expandable property", GroupName = "Custom class properties")] // Атрибут Display задает имя свойства и группу в редакторе свойств
public PenSettings ExpandableProperty { get; set; } = new(); // Публичное свойство ExpandableProperty типа PenSettings, инициализируется новым объектом PenSettings

[Display (Name = "Managed Property 1", GroupName = _managedCategoryCategoryName)] // Атрибут Display задает имя свойства и группу, GroupName берется из приватной переменной _managedCategoryCategoryName
public int ManagedProperty1 { get; set; } // Публичное свойство ManagedProperty1 типа int

[Display (Name = "Managed Property 2", GroupName = _managedCategoryCategoryName)] // Атрибут Display задает имя свойства и группу, GroupName берется из приватной переменной _managedCategoryCategoryName
public int ManagetProperty2 { get; set; } // Публичное свойство ManagetProperty2 типа int
```
    *   *Аннотация:*  Здесь определены свойства, которыми мы будем управлять. `ExpandableProperty` - это свойство типа `PenSettings`, которое будет сворачиваться/разворачиваться свойством `PropertyManager`. `ManagedProperty1` и `ManagedProperty2` - это примеры свойств, которые будут сгруппированы под категорией "Managed category" и управляться свойством `CategoryManager`. `GroupName = _managedCategoryCategoryName` указывает, что эти свойства принадлежат к категории, имя которой хранится в `_managedCategoryCategoryName`.

5.  **Реализуйте свойство `IPropertiesEditorOwner.PropertiesEditor`, которое будет хранить объект редактора и обрабатывать его изменения:**

```csharp
[Browsable(false)] // Атрибут Browsable(false) скрывает свойство из редактора свойств
IPropertiesEditor IPropertiesEditorOwner.PropertiesEditor // Явная реализация свойства PropertiesEditor интерфейса IPropertiesEditorOwner
{
    get => _propertiesEditor; // Геттер возвращает текущее значение приватного поля _propertiesEditor
    set
    {
        if (_propertiesEditor == value) // Проверка на то, что новое значение редактора не совпадает с текущим
            return; // Если значения совпадают, выходим из сеттера

        _propertiesEditor = value; // Устанавливаем новое значение редактора свойств
        PropertiesEditorOnChanged(value); // Вызываем метод PropertiesEditorOnChanged для обработки изменения редактора
    }
}
```
    *   *Аннотация:*  Это свойство является частью реализации интерфейса `IPropertiesEditorOwner`. Атрибут `[Browsable(false)]` скрывает это свойство от отображения в редакторе свойств, так как оно предназначено для внутреннего использования. Сеттер свойства `PropertiesEditor` сохраняет переданный объект редактора свойств и вызывает метод `PropertiesEditorOnChanged` для выполнения действий при изменении редактора.

6.  **Реализуйте метод `PropertiesEditorOnChanged`, который будет обновлять состояние редактора свойств при его изменении:**

```csharp
private void PropertiesEditorOnChanged(IPropertiesEditor? newValue) // Приватный метод PropertiesEditorOnChanged, принимающий nullable IPropertiesEditor в качестве параметра
{
    newValue?.BeginInit(); // Вызов BeginInit() для редактора свойств, начало блока изменений

    newValue?.SetIsExpandedCategory(_managedCategoryCategoryName, CategoryManager); // Вызов SetIsExpandedCategory для управления состоянием развернутости категории "Managed category" в зависимости от значения свойства CategoryManager
    newValue?.SetIsExpandedProperty(nameof(ExpandableProperty), PropertyManager); // Вызов SetIsExpandedProperty для управления состоянием развернутости свойства ExpandableProperty в зависимости от значения свойства PropertyManager

    newValue?.EndInit(); // Вызов EndInit() для редактора свойств, завершение блока изменений и обновление интерфейса
}
```
    *   *Аннотация:* Этот метод вызывается при изменении редактора свойств. Он использует методы `SetIsExpandedCategory` и `SetIsExpandedProperty` объекта редактора свойств (`newValue`) для установки состояния развернутости категорий и свойств в зависимости от значений свойств `CategoryManager` и `PropertyManager`. Методы `BeginInit()` и `EndInit()` используются для оптимизации процесса обновления редактора свойств, группируя изменения.

**Управление категориями и свойствами осуществляется в этом методе между вызовами методов редактора `BeginInit` и `EndInit`.**

*   *Аннотация:* Важно отметить, что все операции по изменению состояния редактора свойств должны выполняться между вызовами `BeginInit()` и `EndInit()`. Это обеспечивает корректное и эффективное обновление интерфейса редактора.

**Для управления категориями мы вызываем метод редактора `SetIsExpandedCategory` и передаем имя управляемой категории в качестве первого аргумента и управляющее свойство `CategoryManager` в качестве второго аргумента.**

*   *Аннотация:* Метод `SetIsExpandedCategory` отвечает за сворачивание/разворачивание категории в редакторе свойств. Первым аргументом указывается имя категории, а вторым - логическое свойство, которое определяет, должна ли категория быть развернута (true) или свернута (false).

**Для управления свойством мы вызываем метод редактора `SetIsExpandedProperty` и передаем имя управляемого свойства в качестве первого аргумента и управляющее свойство `PropertyManager` в качестве второго аргумента.**

*   *Аннотация:* Метод `SetIsExpandedProperty` аналогичен `SetIsExpandedCategory`, но предназначен для управления видимостью отдельных свойств. Первым аргументом передается имя свойства, а вторым - логическое свойство, определяющее его состояние развернутости. `nameof(ExpandableProperty)` используется для получения имени свойства `ExpandableProperty` в виде строки.

**Полный код для этого индикатора:**

```csharp
using ATAS.Indicators;
using OFT.Rendering.Settings;
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;
using System.Drawing;

[DisplayName("Expand and collapse categories or properties")]
public class ExpandAndCollapseCategoriesOrPropertiesIndicator : Indicator, IPropertiesEditorOwner
{
    #region Fields // Область Fields - приватные поля класса

    private const string _managedCategoryCategoryName = "Managed category"; // Приватная константа для имени управляемой категории
    private IPropertiesEditor _propertiesEditor; // Приватное поле для объекта редактора свойств
    private bool _categoryManager; // Приватное поле для состояния CategoryManager
    private bool _propertyManager; // Приватное поле для состояния PropertyManager

    #endregion

    #region Properties // Область Properties - публичные свойства индикатора

    [Display (Name = "Category", GroupName = "Management")] // Атрибут Display для свойства CategoryManager
    public bool CategoryManager
    {
        get => _categoryManager;
        set => SetProperty(ref _categoryManager, value, () => _propertiesEditor?.SetIsExpandedCategory(_managedCategoryCategoryName, value));
    }

    [Display (Name = "Property", GroupName = "Management")] // Атрибут Display для свойства PropertyManager
    public bool PropertyManager
    {
        get => _propertyManager;
        set => SetProperty(ref _propertyManager, value, () => _propertiesEditor?.SetIsExpandedProperty(nameof(ExpandableProperty), value));
    }

    [Display (Name = "Expandable property", GroupName = "Custom class properties")] // Атрибут Display для свойства ExpandableProperty
    public PenSettings ExpandableProperty { get; set; } = new(); // Инициализация свойства ExpandableProperty новым объектом PenSettings

    [Display (Name = "Managed Property 1", GroupName = _managedCategoryCategoryName)] // Атрибут Display для свойства ManagedProperty1
    public int ManagedProperty1 { get; set; }

    [Display (Name = "Managed Property 2", GroupName = _managedCategoryCategoryName)] // Атрибут Display для свойства ManagedProperty2
    public int ManagetProperty2 { get; set; }

    #endregion

    #region ctor // Область ctor - конструктор класса

    public ExpandAndCollapseCategoriesOrPropertiesIndicator() : base(true) // Конструктор класса, вызывающий базовый конструктор Indicator(true)
    {
        DenyToChangePanel = true; // Запрет на изменение панели индикатора пользователем
        DataSeries[0].IsHidden = true; // Скрытие первой серии данных индикатора
        ExpandableProperty.Color = Color.Red.Convert(); // Установка цвета для ExpandableProperty
    }

    #endregion

    #region Protected methods // Область Protected methods - защищенные методы класса

    protected override void OnCalculate(int bar, decimal value) // Переопределенный метод OnCalculate, вызывается при расчете индикатора
    {
        // Пустая реализация, так как индикатор не выполняет расчетов на основе данных графика
    }

    #endregion

    #region Implementation of IPropertiesEditorOwner // Область Implementation of IPropertiesEditorOwner - реализация интерфейса IPropertiesEditorOwner

    [Browsable(false)] // Атрибут Browsable(false) для свойства PropertiesEditor
    IPropertiesEditor IPropertiesEditorOwner.PropertiesEditor
    {
        get => _propertiesEditor;
        set
        {
            if (_propertiesEditor == value)
                return;

            _propertiesEditor = value;
            PropertiesEditorOnChanged(value);
        }
    }

    private void PropertiesEditorOnChanged(IPropertiesEditor? newValue) // Приватный метод PropertiesEditorOnChanged
    {
        newValue?.BeginInit(); // Начало блока изменений редактора свойств

        newValue?.SetIsExpandedCategory(_managedCategoryCategoryName, CategoryManager); // Установка состояния развернутости категории
        newValue?.SetIsExpandedProperty(nameof(ExpandableProperty), PropertyManager); // Установка состояния развернутости свойства

        newValue?.EndInit(); // Завершение блока изменений редактора свойств
    }

    #endregion
}
```
    *   *Аннотация:* Это полный код индикатора, объединяющий все шаги, описанные выше. Он включает в себя приватные поля, публичные свойства, конструктор, пустой метод `OnCalculate` и реализацию интерфейса `IPropertiesEditorOwner` с методом `PropertiesEditorOnChanged`. Код разделен на регионы для лучшей организации и читаемости.






## Методичка: Добавление логирования в индикаторы ATAS

**1. Введение в механизм логирования платформы ATAS**

Для использования стандартного механизма логирования платформы ATAS необходимо выполнить следующие шаги:

*   **Шаг 1:** Убедитесь, что файл `Utils.Common.dll` находится в папке с программой ATAS.  (Аннотация: `Utils.Common.dll` - это динамическая библиотека, содержащая общие утилиты, включая функционал логирования. Она должна быть доступна для вашего индикатора.)
*   **Шаг 2:** Добавьте `using Utils.Common.Logging;` в начало вашего проекта индикатора или стратегии. (Аннотация: `using Utils.Common.Logging;` -  эта строка импортирует пространство имен `Utils.Common.Logging`, которое содержит классы и методы для логирования. Это позволяет вам использовать функции логирования в вашем коде.)

**2. Использование расширений библиотеки логирования**

После выполнения шагов, описанных выше, становятся доступны следующие расширения библиотеки логирования: `LogDebug`, `LogInfo`, `LogWarn` и `LogError`. (Аннотация: Это основные методы для записи логов разных уровней важности.)

*   `LogDebug`:  Используется для отладочных сообщений, полезных при разработке и поиске ошибок. (Аннотация: `Debug` - самый подробный уровень логирования, обычно используется разработчиками.)
*   `LogInfo`:  Используется для информационных сообщений о работе индикатора. (Аннотация: `Info` - уровень для записи общей информации о работе программы.)
*   `LogWarn`: Используется для предупреждений о потенциальных проблемах, которые не приводят к ошибкам, но требуют внимания. (Аннотация: `Warn` - уровень для предупреждений о необычных ситуациях, которые могут потребовать внимания.)
*   `LogError`: Используется для сообщений об ошибках, которые влияют на работу индикатора. (Аннотация: `Error` - уровень для записи сообщений об ошибках, которые могут нарушить работу программы.)

**3. Уровень значимости логов**

Каждое расширение (метод) создает запись лога с определенным уровнем значимости:

*   `LogDebug` - уровень "Debug"
*   `LogInfo` - уровень "Info"
*   `LogWarn` - уровень "Warning"
*   `LogError` - уровень "Error"

**4. Просмотр логов**

Записанные логи сохраняются в логах приложения и отображаются в окне логов приложения ATAS. (Аннотация: Логи можно просмотреть в специальном окне в ATAS, что помогает отслеживать работу индикатора и искать ошибки.)

**5. Пример использования логирования**

Ниже приведен пример использования методов логирования в индикаторе ATAS:

```csharp
using ATAS.Indicators; //  Подключаем пространство имен ATAS.Indicators, необходимое для создания индикаторов.

public class SampleTick : Indicator // Объявляем класс SampleTick, который наследуется от класса Indicator. Это базовый класс для всех индикаторов в ATAS.
{
    protected override void OnCalculate(int bar, decimal value) //  Переопределяем метод OnCalculate, который вызывается на каждом тике или баре для расчета значения индикатора.
    {
        this.LogDebug("Debug message"); //  Вызываем метод LogDebug для записи отладочного сообщения в лог. "Debug message" - это текст сообщения.
        this.LogInfo("Info message");   //  Вызываем метод LogInfo для записи информационного сообщения в лог. "Info message" - это текст сообщения.
        this.LogWarn("Warn message");   //  Вызываем метод LogWarn для записи предупреждающего сообщения в лог. "Warn message" - это текст сообщения.

        try //  Начало блока try-catch для обработки возможных исключений (ошибок).
        {
            //your code //  В этом месте должен быть ваш код индикатора.
        }
        catch (Exception e) //  Блок catch для перехвата исключений типа Exception. 'e' - переменная, содержащая информацию об исключении.
        {
            this.LogError("Error message", e); //  В случае возникновения исключения, вызывается метод LogError для записи сообщения об ошибке и информации об исключении 'e' в лог. "Error message" - это текст сообщения об ошибке.
        }
    }
}
```






## Методичка для ИИ по отладке индикаторов ATAS

**Режим отладки**

**Основы**

Режим отладки — хороший помощник в разработке индикаторов. В этом разделе мы расскажем о подключении к работающему процессу платформы ATAS с помощью отладчика Visual Studio.

Работая в этом режиме, разработчик может последовательно проходить код, анализировать данные, хранящиеся в переменных, устанавливать мониторинг переменных для отслеживания изменений, определять, где возникают исключения, и так далее.

Перед использованием этого режима следует ознакомиться с принципами его работы [здесь](здесь - *в оригинальном тексте ссылка, которую нужно заменить на актуальную ссылку на документацию по отладке в Visual Studio или ATAS*).

**Чтобы подключить процесс, необходимо выполнить следующие шаги:**

*   скомпилируйте файл с индикатором и поместите его в папку `Documents/ATAS/Indicators`, (*аннотация: убедитесь, что путь к папке индикаторов в вашей системе соответствует указанному, или измените его при необходимости.*)
*   в Visual Studio нажмите комбинацию клавиш `CTRL + ALT + P`, (*аннотация: это сочетание клавиш открывает окно "Attach to Process" в Visual Studio.*)
*   в появившемся окне со списком процессов, запущенных на компьютере, введите имя нужного процесса в поле поиска, в данном случае это `platform`, и нажмите кнопку `Attach` или дважды щелкните нужный процесс.

**Присоединение к процессу**

![Изображение окна "Attach to Process" из Visual Studio с выделенным процессом platform и кнопкой Attach](изображение окна "Attach to Process" - *в методичке для ИИ нужно указать путь к изображению или описать его структуру, чтобы ИИ мог его интерпретировать или найти похожее изображение*)

**Точки останова**

Используя такой инструмент отладки, как **точка останова**, вы можете приостановить выполнение процесса в нужной строке кода. Используя клавиши `F10` и `F11`, можно построчно проходить код и отслеживать путь процесса, одновременно выясняя значения переменных.

*   `F11` (**Шаг с заходом**) - перемещение по строкам кода с заходом в методы. (*аннотация: если текущая строка кода вызывает метод, F11 перейдет внутрь этого метода.*)
*   `F10` (**Шаг с обходом**) - перемещение по строкам кода без захода в методы. (*аннотация: если текущая строка кода вызывает метод, F10 выполнит метод целиком и перейдет к следующей строке кода после вызова метода.*)

Если вам нужно продолжить процесс - вам нужно нажать кнопку `Continue`, в этом случае процесс продолжится до следующей точки останова. Если вам какое-то время больше не нужно останавливать процесс, вам нужно удалить точки останова, снова нажав на них. Чтобы выйти из режима отладки, нажмите кнопку `Stop`.

**Ошибки**

Появление ошибок можно найти в окне журнала платформы. Ошибки вызывают **Исключения** (*аннотация: Exceptions - особый тип ошибок в программировании, указывающий на нештатную ситуацию.*) во время выполнения кода.

Обнаружив исключение в отладчике, вы можете выяснить причину этого исключения, возможно, значения в некоторых переменных неверны для расчета.

**Несколько советов по устранению неполадок:**

*   **если ошибки повторяются при каждом тике** (*аннотация: тик - обновление данных на графике, обычно происходит с определенной периодичностью.*) - при подключении к процессу платформы отладчик часто останавливается в месте возникновения исключения,
*   **если ошибка произошла один или несколько раз** - чтобы воспроизвести это исключение в отладчике, обновите график, чтобы индикатор был пересчитан. (*аннотация: обновление графика может заставить индикатор снова выполнить расчеты, и ошибка может повториться в режиме отладки.*)

На скриншоте ниже вы можете увидеть пример возникновения исключения, когда мы пытаемся вызвать метод `GetCandle` (*аннотация: GetCandle - предположительно метод для получения данных свечи с графика.*) и передать отрицательное значение в параметры. Наведя курсор на переменную, вы можете увидеть ее текущее значение. В этом случае курсор наведен на переменную `prevBar`, и в подсказке видно, что ее значение `-1`.

![Изображение окна отладчика Visual Studio с примером исключения ArgumentOutOfRangeException](изображение окна отладчика с исключением - *в методичке для ИИ нужно указать путь к изображению или описать его структуру, чтобы ИИ мог его интерпретировать или найти похожее изображение*)

**Пример возникновения исключения**








## Методичка по распространению индикаторов и стратегий в ATAS

**Распространение индикаторов и стратегий**

Вы можете предоставить доступ к разработанным индикаторам или стратегиям (модулям) для любого пользователя ATAS, установить дату истечения доступа и отключить доступ в любой момент времени.

Для этого необходимо:

*   **Активировать опцию загрузки модулей в Личном кабинете**
*   **Подготовить модуль**
*   **Загрузить его в сервис**

**Активация опции загрузки модуля**

Чтобы активировать опцию загрузки модуля, вам необходимо:

1.  **Зарегистрироваться в Личном кабинете.**  *(Это первый шаг, убедитесь, что у вас есть аккаунт в личном кабинете ATAS)*
2.  **Активировать опцию "Раздел модулей" в разделе Профиль -> Для разработчиков.** *(В настройках вашего профиля найдите раздел для разработчиков и активируйте там опцию модулей)*
3.  **После этого в левом меню появится раздел "Мои модули".** *(После активации, в меню программы появится новый раздел для управления вашими модулями)* **В этом разделе вы можете добавить новый модуль, назначить пользователей, у которых есть доступ к этим модулям, и установить срок действия доступа.** *(Здесь вы сможете загружать свои индикаторы, указывать кому они доступны и на какой срок)*

**Подготовка модуля**

Каждый модуль, который будет распространяться среди других пользователей, должен иметь уникальный идентификатор. **Атрибут `FeatureId`** предназначен для этого. *(`FeatureId` - это специальный атрибут, который позволяет идентифицировать ваш модуль)* **Этот атрибут должен быть присвоен каждому модулю, который включен в распространяемый пакет, с указанием уникального строкового идентификатора.** *(Каждый ваш индикатор должен иметь этот атрибут с уникальным кодом)*

**Пример:**

```csharp
[FeatureId("DFD43423-6645-4490-B5F7-45579FF940EE")] // Атрибут FeatureId с уникальным идентификатором
public class MyIndicator : Indicator // Объявление класса вашего индикатора, наследующегося от Indicator
{
    //ваш код // Здесь должен быть код вашего индикатора
}
```

**Один и тот же идентификатор может быть присвоен нескольким модулям.** *(Вы можете использовать один и тот же `FeatureId` для группы модулей, если хотите управлять доступом к ним как к единому целому)* **В таком случае вы сможете контролировать доступ ко всей группе модулей из Личного кабинета.** *(Управление доступом будет применяться ко всем модулям с одинаковым `FeatureId`)*

**После того, как модуль готов, необходимо его скомпилировать.** *(Прежде чем загружать модуль в ATAS, его нужно преобразовать в исполняемый файл - скомпилировать)* **Мы также рекомендуем обфусцировать модуль для его защиты.** *(Обфускация - это процесс усложнения кода для защиты от несанкционированного анализа и копирования)*

**Загрузка модуля в сервис**

1.  **Перейдите в раздел "Модули".** *(В программе ATAS найдите раздел "Модули", который появился после активации опции)*
2.  **Нажмите кнопку "Добавить модуль".** *(В разделе "Модули" нажмите кнопку для добавления нового модуля)*
3.  **Заполните все поля в появившейся форме:** *(Откроется форма, которую нужно заполнить информацией о вашем модуле)*
    *   **a. Имя модуля - имя** *(Укажите название вашего индикатора или стратегии)*
    *   **b. Module UID - идентификатор, указанный в атрибуте `FeatureId`** *(Введите тот же уникальный идентификатор, который вы использовали в атрибуте `FeatureId` в коде)*
    *   **c. Описание - произвольное описание** *(Добавьте описание вашего модуля, это может быть любая информация)*
    *   **d. Файл - выберите скомпилированный dll файл** *(Выберите скомпилированный файл вашего индикатора с расширением .dll)*

**После создания модуля вы получите ссылку на dll файл, загруженный в сервис.** *(После загрузки модуля, система предоставит вам ссылку на загруженный файл)*

**Вы можете отправить эту ссылку своим пользователям для загрузки модуля.** *(Эту ссылку можно передать пользователям, чтобы они могли установить ваш модуль)*

**Также для каждого такого модуля вы можете указать пользователей, у которых есть к нему доступ, и дату истечения доступа.** *(В настройках модуля вы можете указать, кто именно и до какого времени имеет доступ к вашему индикатору)*

**Если у пользователя есть загруженный модуль, но нет доступа или доступ истек, пользователь не увидит этот модуль в платформе и не сможет его использовать.** *(Если пользователь не имеет активного доступа, ваш индикатор не будет виден и доступен для использования в платформе ATAS)*









## Методичка: Управление доступом к пользовательским индикаторам в ATAS

**Введение**

Благодаря специально разработанному публичному API, вы можете контролировать доступ других пользователей к вашим индивидуальным пользовательским индикаторам из любого внешнего интерфейса. Для этого вам не нужно входить в свою личную учетную запись в ATAS.

*Аннотация: Введение объясняет, что с помощью API можно управлять доступом к индикаторам без необходимости входа в ATAS.*

**Подготовка к использованию**

Перед использованием поместите dll-файл с разработанным пользовательским индикатором в папку Documents/ATAS/Indicators. Обратитесь к Базе знаний, чтобы найти подробные требования к разработке индикаторов, информацию о возможностях специализированного API для создания индикаторов и правилах подключения пользовательских индикаторов к платформе ATAS.

*Аннотация: Перед началом работы необходимо поместить файл индикатора в нужную папку и ознакомиться с документацией API.*

**Предоставление доступа к новому индикатору через API**

Чтобы предоставить другим пользователям доступ к новому индикатору с помощью специализированного API, выполните следующие шаги:

1.  **Используйте метод POST /seller/modules/upload** для загрузки файла индикатора в формате строки ($binary). Максимальный размер файла составляет 20 МБ. Метод вернет `uploadFileKey` в качестве ответа.

    *Аннотация: Первый шаг - загрузка файла индикатора на сервер ATAS с использованием POST запроса к указанному адресу. В ответе будет получен ключ `uploadFileKey`.*

2.  **В течение 5 минут после загрузки файла** с индикатором с помощью метода POST /seller/modules, необходимо добавить описание загруженного индикатора в раздел "Мои модули". При добавлении описания необходимо указать:
    *   Название индикатора
    *   Уникальный UID
    *   Краткое описание
    *   `uploadFileKey`, полученный на предыдущем шаге.

    Если индикатор и его описание уже были добавлены в список ваших модулей, вы можете пропустить первые два шага.

    *Аннотация: Второй шаг - добавление описания индикатора в "Мои модули" через API. Необходимо указать название, UID, описание и `uploadFileKey`. Если описание уже есть, шаги можно пропустить.*

3.  **Используйте метод GET /seller/modules**, чтобы получить список всех опубликованных вами модулей и выбрать внутренний ID индикатора, к которому вы хотите предоставить доступ другим пользователям.

    *Аннотация: Третий шаг - получение списка всех модулей для выбора нужного индикатора по его внутреннему ID. Используется GET запрос к `/seller/modules`.*

4.  **Используя метод POST /seller/modules/{id}/subscriptions**, вы можете добавить пользователя, которому планируете предоставить доступ к индикатору. На этом этапе вам нужно ввести:
    *   Email пользователя, которому предоставляется доступ.
    *   Период доступа, указывающий дату истечения срока действия.
    *   ID модуля, к которому будет предоставлен доступ.

    *Аннотация: Четвертый шаг - добавление пользователя для доступа к индикатору с использованием POST запроса к `/seller/modules/{id}/subscriptions`. Необходимо указать email пользователя, срок доступа и ID модуля.*

**Публичный API для управления доступом к пользовательским индикаторам содержит следующие методы:**

*   **Get all modules** – получение всех опубликованных вами индикаторов.
*   **Create a module** – добавление описания пользовательского индикатора в раздел "Мои модули".
*   **Get a module by ID** – получение индикатора с использованием его внутреннего ID.
*   **Update a module** – обновление атрибутов пользовательского индикатора.
*   **Delete a module** – удаление пользовательского индикатора из списка.
*   **Check UID for uniqueness** – проверка UID индикатора на уникальность.
*   **Upload a module and get "uploadFileKey" for a module creating** – загрузка файла с пользовательским индикатором и получение `uploadFileKey`.
*   **Add subscribers to a module** - предоставление пользователю доступа к вашему индивидуальному индикатору.
*   **Change the expiration date of module subscriptions** – изменение даты истечения срока действия доступа пользователя к индикатору.
*   **Get a list of subscribers for a module** – получение списка пользователей, имеющих доступ к индикатору.
*   **Block a subscriber for a module** – ограничение доступа к индикатору для пользователя.
*   **Get all subscriber's modules** – получение списка индикаторов и дат истечения срока действия для пользователя.

*Аннотация: Перечень всех доступных методов API для управления индикаторами и доступом к ним.*

Более подробное описание методов, примеры их использования и возможные ошибки можно найти в разделе "Мои модули" - "API documentation" в вашем личном кабинете на сайте ATAS.

*Аннотация: Информация о том, где найти более подробную документацию по API.*





































## Методичка для ИИ по ATAS.Indicators Namespace Reference

### Namespaces (Пространства имен)

**1. Attributies (Атрибуты)** - *Содержит классы для работы с атрибутами индикаторов.*
**2. Drawing (Рисование)** - *Содержит классы для рисования графических элементов на графике.*
**3. Filters (Фильтры)** - *Содержит классы для создания и управления фильтрами.*

### Classes (Классы)

**1. BaseDataSeries (Базовый ряд данных)**
   - Base generic data series class providing common functionality. More... - *Базовый класс для создания рядов данных, предоставляет общие функции.*

**2. BaseIndicator (Базовый индикатор)**
   - Base class for custom indicators in a chart. More... - *Базовый класс для создания пользовательских индикаторов на графике.*

**3. Candle (Свеча)**
   - Represents a candle in trading, which includes open, high, low, and close prices. More... - *Представляет собой торговую свечу, включая цены открытия, максимум, минимум и закрытия.*

**4. CandleDataSeries (Ряд данных свечей)**
   - Represents a data series of candles. Each element in the series is a Candle. More... - *Представляет собой ряд данных, состоящий из свечей. Каждый элемент в ряде - это объект типа Candle.*

**5. CandlePartSeries (Ряд частей свечи)**
   - Represents a data series of decimal values derived from specific parts of an IndicatorCandle created by an ICandleCreator. More... - *Представляет собой ряд данных десятичных значений, полученных из определенных частей IndicatorCandle, созданного с помощью ICandleCreator.*

**6. ChartObject (Объект графика)**
   - Base class for objects in a chart. More... - *Базовый класс для различных объектов, отображаемых на графике.*

**7. Container (Контейнер)**
   - Represents a container with a defined region on the chart. More... - *Представляет собой контейнер с определенной областью на графике.*

**8. CumulativeTrade (Кумулятивная сделка)**
   - Represents a cumulative trade, which is a trade that includes multiple prints or executions. More... - *Представляет собой кумулятивную сделку, включающую несколько принтов или исполнений.*

**9. CumulativeTradesRequest (Запрос кумулятивных сделок)**
   - Represents a request to retrieve cumulative trade data within a specified time range or for a particular date. More... - *Представляет собой запрос на получение кумулятивных торговых данных за определенный период времени или на конкретную дату.*

**10. CustomValue (Пользовательское значение)**
   - Represents a custom value with associated properties. More... - *Представляет собой пользовательское значение с заданными свойствами.*

**11. CustomValueDataSeries (Ряд данных пользовательских значений)**
   - Represents a custom data series that holds CustomValue objects. More... - *Представляет собой ряд данных, содержащий объекты типа CustomValue.*

**12. ExtendedIndicator (Расширенный индикатор)**
   - An extended base class for custom indicators that provide additional functionality for drawing, alerts, market data handling, etc. More... - *Расширенный базовый класс для пользовательских индикаторов, предоставляющий дополнительные функции для рисования, оповещений, обработки рыночных данных и т.д.*

**13. Extensions (Расширения)**
   - Contains extension methods for various types. - *Содержит методы расширения для различных типов данных.*

**14. Filter (Фильтр)**
   - Generic filter class that implements the IFilterValue interface. More... - *Общий класс фильтров, реализующий интерфейс IFilterValue.*

**15. FilterBase (Базовый фильтр)**
   - Base class for filters implementing the IFilter interface. More... - *Базовый класс для фильтров, реализующих интерфейс IFilter.*

**16. FilterBool (Булевский фильтр)**
   - Represents a filter with a boolean value type. Inherits from Filter<TValue, TFilter> where TValue is set to bool and TFilter is set to FilterBool. More... - *Представляет собой фильтр с булевским типом значения (истина/ложь). Наследуется от Filter<TValue, TFilter>, где TValue установлен как bool, а TFilter установлен как FilterBool.*

**17. FilterColor (Цветовой фильтр)**
   - Represents a filter with a value type of CrossColor. Inherits from Filter<TValue, TFilter> where TValue is set to CrossColor and TFilter is set to FilterColor. More... - *Представляет собой фильтр со значением типа CrossColor (цвет). Наследуется от Filter<TValue, TFilter>, где TValue установлен как CrossColor, а TFilter установлен как FilterColor.*

**18. FilterEnum (Фильтр перечисления)** - *Класс для фильтрации значений перечислений.*
**19. FilterExtensions (Расширения фильтров)**
   - Provides extension methods for working with filters. - *Предоставляет методы расширения для работы с фильтрами.*

**20. FilterHeatmapTypes (Фильтр типов тепловых карт)**
   - Represents a filter for heatmap types with custom JSON serialization/deserialization. More... - *Представляет собой фильтр для типов тепловых карт с пользовательской сериализацией/десериализацией JSON.*

**21. FilterInt (Целочисленный фильтр)**
   - Represents a filter for integer values with custom JSON serialization/deserialization. More... - *Представляет собой фильтр для целочисленных значений с пользовательской сериализацией/десериализацией JSON.*

**22. Filterkey (Фильтр ключей)**
   - Represents a filter for key values with custom JSON serialization/deserialization. More... - *Представляет собой фильтр для значений ключей с пользовательской сериализацией/десериализацией JSON.*

**23. FilterRangeBase (Базовый фильтр диапазона)**
   - Represents an abstract base class for filters that represent a range of values with custom JSON serialization/deserialization. More... - *Представляет собой абстрактный базовый класс для фильтров, представляющих диапазон значений с пользовательской сериализацией/десериализацией JSON.*

**24. FilterRangeInt (Целочисленный фильтр диапазона)**
   - Represents a filter that represents a range of integer values with custom JSON serialization/deserialization. More... - *Представляет собой фильтр, представляющий диапазон целочисленных значений с пользовательской сериализацией/десериализацией JSON.*

**25. FilterRangeValue (Фильтр диапазона значений)**
   - Represents a range of values of type TValue with support for property change notifications. More... - *Представляет собой диапазон значений типа TValue с поддержкой уведомлений об изменении свойств.*

**26. FilterRenderPen (Фильтр пера для рисования)**
   - Represents a filter for PenSettings objects with support for property change notifications. More... - *Представляет собой фильтр для объектов PenSettings (настроек пера для рисования) с поддержкой уведомлений об изменении свойств.*

**27. FilterString (Строковый фильтр)**
   - Represents a filter for string values with support for property change notifications. More... - *Представляет собой фильтр для строковых значений с поддержкой уведомлений об изменении свойств.*

**28. FilterTimeSpan (Фильтр временного интервала)**
   - Represents a filter for TimeSpan values with custom JSON serialization/deserialization. More... - *Представляет собой фильтр для значений TimeSpan (временных интервалов) с пользовательской сериализацией/десериализацией JSON.*

**29. FixedProfileRequest (Запрос фиксированного профиля)**
   - Represents a request for a fixed profile with a specific period. More... - *Представляет собой запрос на получение фиксированного профиля за определенный период.*

**30. Indicator (Индикатор)**
   - Base class for custom indicators. More... - *Базовый класс для создания пользовательских индикаторов.*

**31. IndicatorCandle (Свеча индикатора)**
   - Represents an Indicator Candle. More... - *Представляет собой свечу индикатора - специальный тип свечи для индикаторов.*

**32. IndicatorCategories (Категории индикаторов)** - *Класс для работы с категориями индикаторов.*
**33. IndicatorDataProvider (Провайдер данных индикатора)**
   - Implementation of the IIndicatorDataProvider interface that provides access to various data and services related to an indicator. More... - *Реализация интерфейса IIndicatorDataProvider, предоставляющего доступ к различным данным и сервисам, связанным с индикатором.*

**34. IndicatorSeries (Ряд данных индикатора)**
   - Represents a custom data series for an indicator, derived from BaseDataSeries<decimal>. More... - *Представляет собой пользовательский ряд данных для индикатора, производный от BaseDataSeries<decimal> (базового ряда данных для десятичных чисел).*

**35. InstrumentInfo (Информация об инструменте)**
   - Implementation of the IInstrumentInfo interface representing instrument information. More... - *Реализация интерфейса IInstrumentInfo, представляющего информацию о торговом инструменте.*

**36. LineSeries (Линейный ряд)**
   - Represents a horizontal line with a single value. More... - *Представляет собой горизонтальную линию с одним значением.*

**37. MarketDataArg (Аргумент рыночных данных)**
   - Represents a data point in the market. More... - *Представляет собой точку данных на рынке.*

**38. MarketDepthInfoProvider (Провайдер информации о глубине рынка)**
   - A class that implements the IMarketDepthInfoProvider interface to provide market depth information. More... - *Класс, реализующий интерфейс IMarketDepthInfoProvider для предоставления информации о глубине рынка (стакане цен).*

**39. MarketDepthSnapshot (Снимок глубины рынка)**
   - Represents the end state of market depth over a specified time period. More... - *Представляет собой конечное состояние глубины рынка за определенный период времени (снимок стакана на конец периода).*

**40. MarketDepthSnapshotRequest (Запрос снимка глубины рынка)**
   - Represents a request to retrieve a snapshot of the market depth for a specified time range. More... - *Представляет собой запрос на получение снимка глубины рынка за указанный временной диапазон.*

**41. NotifyPropertyChangedBase (Базовый класс уведомлений об изменении свойств)**
   - Base class for implementing the INotifyPropertyChanged interface. More... - *Базовый класс для реализации интерфейса INotifyPropertyChanged, используемого для уведомления об изменении свойств.*

**42. ObjectDataSeries (Ряд данных объектов)**
   - Represents a data series of objects, allowing storing any type of data elements. More... - *Представляет собой ряд данных, содержащий объекты любого типа.*

**43. PaintbarsDataSeries (Ряд данных раскрашенных баров)**
   - Represents a data series of paintbars, each element is a nullable Color value. More... - *Представляет собой ряд данных раскрашенных баров, каждый элемент - это значение типа Color (цвет), допускающее null (пустое значение).*

**44. ParameterAttribute (Атрибут параметра)** - *Атрибут для определения параметров индикатора.*
**45. PriceSelectionDataSeries (Ряд данных выбора цены)**
   - Represents a data series of price selection values, each element is a synchronized list of PriceSelectionValue. More... - *Представляет собой ряд данных значений выбора цены, каждый элемент - это синхронизированный список объектов PriceSelectionValue.*

**46. PriceSelectionValue (Значение выбора цены)**
   - Represents a class for defining price level selection in clusters and bars. Using in PriceSelectionDataSeries. More... - *Представляет собой класс для определения уровня выбора цены в кластерах и барах. Используется в PriceSelectionDataSeries.*

**47. PriceVolumeInfo (Информация о цене и объеме)**
   - Represents information on volumes at a specific price. More... - *Представляет собой информацию об объемах на определенной цене.*

**48. RangeDataSeries (Ряд данных диапазонов)**
   - Represents a data series of range values, each element is a RangeValue. More... - *Представляет собой ряд данных диапазонов значений, каждый элемент - это объект типа RangeValue.*

**49. RangeValue (Значение диапазона)**
   - RangeDataSeries element. More... - *Элемент ряда данных RangeDataSeries, представляющий диапазон.*

**50. RedrawArg (Аргумент перерисовки)**
   - Represents the arguments for requesting a redraw of a chart. More... - *Представляет собой аргументы для запроса перерисовки графика.*

**51. ValueArea (Область стоимости)**
   - Represents information on Value area high/low. More... - *Представляет собой информацию о верхней и нижней границах области стоимости.*

**52. ValueChangingEventArgs (Аргументы события изменения значения)**
   - Provides event arguments for a value changing event. More... - *Предоставляет аргументы события для события изменения значения.*

**53. ValueDataSeries (Ряд данных значений)**
   - Represents a data series of decimal values, each element is a decimal. More... - *Представляет собой ряд данных десятичных значений, каждый элемент - это десятичное число.*


### Interfaces (Интерфейсы)

**1. ICandleCreator (Создатель свечей)**
   - Represents an interface for creating and managing indicator candles. More... - *Интерфейс для создания и управления свечами индикаторов.*

**2. IChart (График)**
   - Interface for a chart containing various chart-related information and methods. More... - *Интерфейс для графика, содержащий различную информацию и методы, связанные с графиком.*

**3. IChartColorsStore (Хранилище цветов графика)**
   - Interface for accessing colors and pens used in a chart's rendering. More... - *Интерфейс для доступа к цветам и перьям, используемым для отрисовки графика.*

**4. IChartContainer (Контейнер графика)**
   - Interface for a chart container that holds chart-related information and methods. More... - *Интерфейс для контейнера графика, который хранит информацию и методы, связанные с графиком.*

**5. IChartCoordinatesManager (Менеджер координат графика)**
   - Interface for managing chart coordinates and scaling. More... - *Интерфейс для управления координатами и масштабированием графика.*

**6. IContainer (Контейнер)**
   - Interface for defining a container that represents a rectangular region. More... - *Интерфейс для определения контейнера, представляющего прямоугольную область.*

**7. IDataSeries (Ряд данных)**
   - Interface for data series, providing essential properties and methods. More... - *Интерфейс для рядов данных, предоставляющий основные свойства и методы.*

**8. IDrawingObjectsListInfo (Информация о списке графических объектов)**
   - Interface for providing information about the list of drawing objects on the chart. More... - *Интерфейс для предоставления информации о списке графических объектов на графике.*

**9. IFilter (Фильтр)**
   - Represents a filter with common properties and functionality. More... - *Интерфейс, представляющий фильтр с общими свойствами и функциональностью.*

**10. IFilterEnum (Фильтр перечисления)** - *Интерфейс для фильтров, работающих с перечислениями.*
**11. IFilterValue (Значение фильтра)**
   - Represents a filter with a flexible value that can hold data of any type. Inherits the properties of IFilter. More... - *Интерфейс, представляющий фильтр с гибким значением, которое может хранить данные любого типа. Наследует свойства интерфейса IFilter.*

**12. IIndicatorContainer (Контейнер индикатора)**
   - Interface for an indicator container that holds indicator-related information and methods. More... - *Интерфейс для контейнера индикатора, который хранит информацию и методы, связанные с индикатором.*

**13. IIndicatorDataProvider (Провайдер данных индикатора)**
   - Represents a data provider for an indicator, providing access to various data and services related to the indicator. More... - *Интерфейс, представляющий провайдера данных для индикатора, предоставляющего доступ к различным данным и сервисам, связанным с индикатором.*

**14. IInstrumentInfo (Информация об инструменте)**
   - Interface representing instrument information. More... - *Интерфейс, представляющий информацию о торговом инструменте.*

**15. IIntCandle (Целочисленная свеча)**
   - Represents an interface for an integer-based candle. More... - *Интерфейс для целочисленной свечи (свечи, где цены представлены целыми числами).*

**16. IKeyboardInfo (Информация о клавиатуре)**
   - Interface for providing information about the keyboard state. More... - *Интерфейс для предоставления информации о состоянии клавиатуры.*

**17. IKnowFixedProfiles (Знает фиксированные профили)**
   - Represents an interface for objects that know fixed profiles and can request them. More... - *Интерфейс для объектов, которые "знают" о фиксированных профилях и могут запрашивать их.*

**18. IMarketByOrdersCache (Кэш заявок)**
   - Interface for manager that provides access to market by orders cache. More... - *Интерфейс для менеджера, предоставляющего доступ к кэшу заявок (market by orders - MBO).*

**19. IMarketByOrdersDataProvider (Провайдер данных заявок)**
   - Interface for manager that provides access to market by order data. More... - *Интерфейс для менеджера, предоставляющего доступ к данным заявок (market by order - MBO).*

**20. IMarketByOrdersWithTradesCache (Кэш заявок и сделок)**
   - Interface for manager that provides access to market by orders and trades cache. More... - *Интерфейс для менеджера, предоставляющего доступ к кэшу заявок и сделок (market by orders and trades - MBOT).*

**21. IMarketDepthInfoProvider (Провайдер информации о глубине рынка)**
   - Interface for providing market depth information. More... - *Интерфейс для предоставления информации о глубине рынка (стакане цен).*

**22. IMarketTimeProvider (Провайдер рыночного времени)** - *Интерфейс для предоставления текущего рыночного времени.*
**23. IMouseLocationInfo (Информация о положении мыши)**
   - Interface for providing information about the mouse location within the chart. More... - *Интерфейс для предоставления информации о положении мыши внутри графика.*

**24. INotifyPanelPropertyChanged (Уведомление об изменении свойства панели)**
   - Notifies clients that a panel property value has changed. More... - *Уведомляет клиентов об изменении значения свойства панели индикатора.*

**25. IOnlineDataProvider (Онлайн провайдер данных)**
   - Interface for an online data provider that provides access to real-time market data. More... - *Интерфейс для онлайн-провайдера данных, предоставляющего доступ к рыночным данным в реальном времени.*

**26. IPlatformSettings (Настройки платформы)**
   - Interface for accessing platform settings. More... - *Интерфейс для доступа к настройкам платформы ATAS.*

**27. IPropertiesEditor (Редактор свойств)** - *Интерфейс для редактора свойств индикатора.*
**28. IPropertiesEditorOwner (Владелец редактора свойств)** - *Интерфейс для объекта, владеющего редактором свойств.*
**29. ISupportedPriceInfo (Поддержка информации о цене)**
   - Represents an interface for supporting price information. More... - *Интерфейс для поддержки информации о цене.*

**30. ITimeMarketDataCache (Кэш рыночных данных по времени)**
   - Cache that holds recent items based on a specified amount of time. More... - *Кэш, который хранит последние элементы данных за определенный период времени.*

**31. ITradesCache (Кэш сделок)**
   - Interface for manager that provides access to trades cache. More... - *Интерфейс для менеджера, предоставляющего доступ к кэшу сделок.*

**32. ITradingManager (Менеджер торговли)**
   - Interface representing a trading manager for handling trading-related operations. More... - *Интерфейс, представляющий менеджер торговли для обработки торговых операций.*

**33. ITradingVolumeInfo (Информация об объеме торговли)**
   - Interface for providing information about the volume selector. More... - *Интерфейс для предоставления информации о селекторе объема.*

**34. IVolumeSelectorItem (Элемент селектора объема)**
   - Interface representing the volume template. More... - *Интерфейс, представляющий шаблон объема.*


### Typedefs (Определения типов)

**1. TimerSubscriptionKey (Ключ подписки на таймер)**
   - `using TimerSubscriptionkey = (System.TimeSpan period, System.Action callback)` - *Определение типа для ключа подписки на таймер. Используется для хранения периода времени и функции обратного вызова (callback).*

**2. BindingFlags (Флаги связывания)**
   - `using BindingFlags = System.Reflection.BindingFlags` - *Определение типа для флагов связывания, используемых в System.Reflection (для работы с отражением - introspection).*


### Enumerations (Перечисления)

**1. DataSeriesType (Тип ряда данных)**
   ```csharp
   enum DataSeriesType {
       Bars,         // Бары
       Open,         // Цена открытия
       High,         // Максимальная цена
       Low,          // Минимальная цена
       Close,        // Цена закрытия
       Volume,       // Объем
       HL2,          // Среднее значение (High + Low) / 2
       HLC3,         // Среднее значение (High + Low + Close) / 3
       OHLC4,        // Среднее значение (Open + High + Low + Close) / 4
       HLCC4,        // Среднее значение (High + Low + Close + Close) / 4
       Indicator,    // Данные из индикатора
       Line,         // Линейный ряд
       Band,         // Полосовой ряд
       Value,        // Ряд значений
       Candle,       // Свечи
       PriceSelection, // Выбор цены
       PaintBars,     // Раскрашенные бары
       CustomValue,  // Пользовательское значение
       Object        // Объекты
   }
   ```
   - Enum representing different types of data series available. More... - *Перечисление, представляющее различные типы доступных рядов данных.*

**2. CandleVisualMode (Режим отображения свечей)**
   ```csharp
   enum CandleVisualMode { Candles = 0, Bars = 1 }
   ```
   - Visualization mode in CandleDataSeries. More... - *Перечисление, определяющее режим визуализации для ряда данных CandleDataSeries.*

**3. ChartVisualModes (Режимы отображения графика)**
   ```csharp
   enum ChartVisualModes {
       Candles = 0,          // Свечи
       Clusters = 1,         // Кластеры
       TransparentCandles = 2, // Прозрачные свечи
       Line = 3,             // Линия
       Bars = 4,             // Бары
       Hidden = 5            // Скрыто
   }
   ```
   - Enumerates the visual modes available for displaying price chart data on a chart. More... - *Перечисление, определяющее визуальные режимы для отображения ценовых данных на графике.*

**4. DrawingLayouts (Схемы рисования)**
   ```csharp
   enum DrawingLayouts { None = 1, Historical = 2, LatestBar = 4, Final = 8 }
   ```
   - Enumerates the different drawing layouts available for chart drawings. More... - *Перечисление, определяющее различные схемы рисования для графических объектов на графике.*

**5. FootprintColorSchemes (Цветовые схемы футпринта)**
   ```csharp
   enum FootprintColorSchemes {
       Delta = 0,             // Дельта
       Solid = 10,            // Сплошной цвет
       VolumeProportion = 20,  // Пропорция объема
       TradesProportion = 30,  // Пропорция сделок
       VolumeBasedBidAsk = 50, // Объем на Bid/Ask
       HeatMapVolume = 60,     // Тепловая карта объема
       HeatMapTrades = 70,     // Тепловая карта сделок
       HeatMapDelta = 80,      // Тепловая карта дельты
       None = 90              // Нет схемы
   }
   ```
   - *Перечисление, определяющее различные цветовые схемы для отображения футпринта (объемов в ценовых уровнях).*

**6. FootprintContentModes (Режимы содержимого футпринта)**
   ```csharp
   enum FootprintContentModes {
       Volume = 0,           // Объем
       Trades = 10,          // Сделки
       VolumeTrades = 30,     // Объем и сделки
       VolumeDelta = 40,      // Объем и дельта
       Delta = 50,            // Дельта
       DeltaCentered = 51,    // Центрированная дельта
       BidXAsk = 60,          // Bid x Ask
       BidAskCentered = 70,   // Центрированный Bid/Ask
       BidAsk = 80,           // Bid и Ask
       None = 100             // Нет содержимого
   }
   ```
   - *Перечисление, определяющее режимы отображения содержимого в футпринте (что именно отображать в каждом ценовом уровне).*

**7. FootprintVisualModes (Режимы визуализации футпринта)**
   ```csharp
   enum FootprintVisualModes {
       FullRow,                   // Полная строка
       BidAskHistogram,           // Гистограмма Bid/Ask
       VolumeHistogram,           // Гистограмма объема
       TradesHistogram,           // Гистограмма сделок
       DeltaHistogram,            // Гистограмма дельты
       BidAskLadder,              // Лестница Bid/Ask
       PositiveNegativeDeltaProfile // Позитивно-негативный профиль дельты
   }
   ```
   - *Перечисление, определяющее режимы визуального представления футпринта.*

**8. MarketDataType (Тип рыночных данных)**
   ```csharp
   enum MarketDataType { Bid = 0, Ask = 1, Trade = 2 }
   ```
   - Specifies the type of market data. More... - *Перечисление, определяющее тип рыночных данных (Bid, Ask, Trade - сделки).*

**9. ObjectType (Тип объекта)**
   ```csharp
   enum ObjectType {
       Ellipse,     // Эллипс
       Triangle,    // Треугольник
       Rectangle,   // Прямоугольник
       Diamond,     // Ромб
       OnlyCluster  // Только кластер
   }
   ```
   - Enumeration representing different types of graphic objects for PriceSelectionDataSeries. More... - *Перечисление, представляющее различные типы графических объектов для PriceSelectionDataSeries (ряда данных выбора цены).*

**10. FixedProfilePeriods (Периоды фиксированного профиля)**
    ```csharp
    enum FixedProfilePeriods {
        CurrentDay = 0,   // Текущий день
        LastDay = 1,      // Прошлый день
        CurrentWeek = 2,  // Текущая неделя
        LastWeek = 3,     // Прошлая неделя
        CurrentMonth = 4, // Текущий месяц
        LastMonth = 5,    // Прошлый месяц
        Contract = 6      // Контракт
    }
    ```
    - Enumeration representing fixed profile periods. More... - *Перечисление, определяющее периоды для фиксированного профиля (например, профиль за день, неделю, месяц).*

**11. SelectionType (Тип выбора)**
    ```csharp
    enum SelectionType { Full, Bid, Ask }
    ```
    - Enumeration representing different types of selection for a price level. More... - *Перечисление, определяющее типы выбора для ценового уровня (полный уровень, сторона Bid, сторона Ask).*

**12. TradeDirection (Направление сделки)**
    ```csharp
    enum TradeDirection { Between = 0, Buy = 1, Sell = 2 }
    ```
    - Represents the possible trade directions. More... - *Перечисление, представляющее возможные направления сделок (между Bid и Ask, покупка, продажа).*

**13. VisualMode (Режим визуализации)**
    ```csharp
    enum VisualMode {
        Line,           // Линия
        Histogram,      // Гистограмма
        Hash,           // Штрих
        Block,          // Блок
        Cross,          // Крест
        Square,         // Квадрат
        Dots,           // Точки
        UpArrow,        // Стрелка вверх
        DownArrow,      // Стрелка вниз
        OnlyValueOnAxis, // Только значение на оси
        Hide            // Скрыть
    }
    ```
    - Represents the visual modes available for displaying data series on a chart. More... - *Перечисление, определяющее режимы визуализации для отображения рядов данных на графике.*


### Functions (Функции)

**1. TradingSessionDescription (Описание торговой сессии)**
   ```csharp
   record TradingSessionDescription (long? Id, string Name)
   ```
   - Representing a trading session description. - *Функция, представляющая описание торговой сессии (например, имя и ID сессии).*

**2. FixedProfileResponse (Ответ фиксированного профиля)**
   ```csharp
   record struct FixedProfileResponse (IndicatorCandle Scaled, IndicatorCandle Original)
   ```
   - *Структура данных, представляющая ответ на запрос фиксированного профиля. Содержит масштабированную и оригинальную свечи индикатора (IndicatorCandle).*


### Typedef Documentation (Документация Typedef)

**1. BindingFlags**
   - `using ATAS.Indicators.BindingFlags = typedef System.Reflection.BindingFlags` - *Описание typedef для BindingFlags. Указывает, что ATAS.Indicators.BindingFlags - это псевдоним для System.Reflection.BindingFlags.*

**2. TimerSubscriptionKey**
   - `using ATAS.Indicators.TimerSubscriptionKey = typedef (System.TimeSpan period, System.Action callback)` - *Описание typedef для TimerSubscriptionKey. Указывает, что ATAS.Indicators.TimerSubscriptionKey - это псевдоним для типа, представляющего пару: TimeSpan (период времени) и Action (делегат функции обратного вызова).*


### Enumeration Type Documentation (Документация типов перечислений)

**1. CandleVisualMode**
   - `enum ATAS.Indicators.CandleVisualMode`
   - Visualization mode in CandleDataSeries. - *Документация для перечисления CandleVisualMode. Описывает режимы визуализации в ряде данных CandleDataSeries.*
     - **Enumerator:** (Перечислители)
       - `Candles` - Candle visual mode where candles are displayed. - *Режим отображения свечами.*
       - `Bars` - Candle visual mode where bars are displayed. - *Режим отображения барами.*

**2. ChartVisualModes**
   - `enum ATAS.Indicators.ChartVisualModes`
   - Enumerates the visual modes available for displaying price chart data on a chart. - *Документация для перечисления ChartVisualModes. Описывает визуальные режимы для отображения ценовых данных на графике.*
     - **Enumerator:** (Перечислители)
       - `Candles` - Represents the candles visual mode where price data is displayed using candlestick charts. - *Режим отображения свечами (японские свечи).*
       - `Clusters` - Represents the clusters visual mode where price data is displayed using clusters. - *Режим отображения кластерами (цифровые объемы внутри баров).*
       - `TransparentCandles` - Represents the transparent candles visual mode where price data is displayed using transparent candlestick charts. - *Режим отображения прозрачными свечами.*
       - `Line` - Represents the line visual mode where price data is displayed using line charts. - *Режим отображения линией.*
       - `Bars` - Represents the bars visual mode where price data is displayed using bar charts. - *Режим отображения барами (столбиками).*
       - `Hidden` - Represents the hidden visual mode where price data is hidden and not displayed on the chart. - *Режим скрытого отображения (данные не показываются на графике).*

**3. DataSeriesType**
   - `enum ATAS.Indicators.DataSeriesType`
   - Enum representing different types of data series available. - *Документация для перечисления DataSeriesType. Описывает различные типы рядов данных, доступных в ATAS.*
     - **Enumerator:** (Перечислители) - *Далее идет перечисление и описание каждого типа рядов данных, как было описано выше.*

**4. DrawingLayouts**
   - `enum ATAS.Indicators.DrawingLayouts`
   - Enumerates the different drawing layouts available for chart drawings. - *Документация для перечисления DrawingLayouts. Описывает различные схемы рисования графических объектов на графике.*
     - **Enumerator:** (Перечислители) - *Далее идет перечисление и описание каждой схемы рисования, как было описано выше.*

**5. FixedProfilePeriods**
   - `enum ATAS.Indicators.FixedProfilePeriods`
   - Enumeration representing fixed profile periods. - *Документация для перечисления FixedProfilePeriods. Описывает периоды для расчета фиксированных профилей.*
     - **Enumerator:** (Перечислители) - *Далее идет перечисление и описание каждого периода, как было описано выше.*

**6. FootprintColorSchemes**
   - `enum ATAS.Indicators.FootprintColorSchemes`
     - **Enumerator:** (Перечислители) - *Документация для перечисления FootprintColorSchemes.  Далее идет перечисление и описание каждой цветовой схемы, как было описано выше.*

**7. FootprintContentModes**
   - `enum ATAS.Indicators.FootprintContentModes`
     - **Enumerator:** (Перечислители) - *Документация для перечисления FootprintContentModes. Далее идет перечисление и описание каждого режима содержимого, как было описано выше.*

**8. FootprintVisualModes**
   - `enum ATAS.Indicators.FootprintVisualModes`
     - **Enumerator:** (Перечислители) - *Документация для перечисления FootprintVisualModes. Далее идет перечисление и описание каждого режима визуализации, как было описано выше.*

**9. MarketDataType**
   - `enum ATAS.Indicators.MarketDataType`
   - Specifies the type of market data. - *Документация для перечисления MarketDataType. Описывает типы рыночных данных.*
     - **Enumerator:** (Перечислители) - *Далее идет перечисление и описание каждого типа данных, как было описано выше.*

**10. ObjectType**
    - `enum ATAS.Indicators.ObjectType`
    - Enumeration representing different types of graphic objects for PriceSelectionDataSeries. - *Документация для перечисления ObjectType. Описывает типы графических объектов для PriceSelectionDataSeries.*
      - **Enumerator:** (Перечислители) - *Далее идет перечисление и описание каждого типа объекта, как было описано выше.*

**11. SelectionType**
    - `enum ATAS.Indicators.SelectionType`
    - Enumeration representing different types of selection for a price level. - *Документация для перечисления SelectionType. Описывает типы выбора для ценового уровня.*
      - **Enumerator:** (Перечислители) - *Далее идет перечисление и описание каждого типа выбора, как было описано выше.*

**12. TradeDirection**
    - `enum ATAS.Indicators.TradeDirection`
    - Represents the possible trade directions. - *Документация для перечисления TradeDirection. Описывает направления сделок.*
      - **Enumerator:** (Перечислители) - *Далее идет перечисление и описание каждого направления сделки, как было описано выше.*

**13. VisualMode**
    - `enum ATAS.Indicators.VisualMode`
    - Represents the visual modes available for displaying data series on a chart. - *Документация для перечисления VisualMode. Описывает режимы визуализации для рядов данных.*
      - **Enumerator:** (Перечислители) - *Далее идет перечисление и описание каждого режима визуализации, как было описано выше.*


### Function Documentation (Документация функций)

**1. FixedProfileResponse()**
   - `record struct ATAS.Indicators.FixedProfileResponse (IndicatorCandle Scaled, IndicatorCandle Original)` - *Документация для структуры FixedProfileResponse. Описывает структуру ответа фиксированного профиля.*

**2. TradingSessionDescription()**
   - `record ATAS.Indicators.TradingSessionDescription (long? Id, string Name)`
   - Representing a trading session description. - *Документация для функции TradingSessionDescription. Описывает функцию для представления описания торговой сессии.*

---





## Методичка по классу ATAS.Indicators.Attributies.FeatureId

### Введение

Класс `ATAS.Indicators.Attributies.FeatureId` является атрибутом, используемым в индикаторах ATAS. Атрибуты в программировании — это способ добавления метаданных к коду, в данном случае, к индикаторам. `FeatureId` вероятно используется для идентификации или категоризации функциональности индикатора.

### Диаграмма наследования и взаимодействия

**Диаграмма наследования:**

Класс `ATAS.Indicators.Attributies.FeatureId` наследуется от:

*   `FeatureIdAttribute`: Этот класс, вероятно, является базовым классом атрибутов для идентификации функциональности.
*   `Utils::Common::Attributes::FeatureIdAttribute`:  Возможно, это более общий базовый класс, находящийся в общей библиотеке `Utils.Common.Attributes`.

**Диаграмма взаимодействия:**

Диаграмма взаимодействия показывает те же классы, что и диаграмма наследования, что логично, так как наследование является формой взаимодействия "родитель-потомок".

Эти диаграммы показывают, что `FeatureId` построен на основе иерархии атрибутов, обеспечивая структурированный способ для добавления информации о функциональности индикаторов.

### Публичные функции-члены (Public Member Functions)

#### `FeatureId(string id)`

```csharp
FeatureId (string id)
```

*   **Описание:** Конструктор класса `FeatureId`, принимающий строковый параметр `id`.
*   **Параметры:**
    *   `id`: Строка (`string`), представляющая идентификатор функциональности (Feature ID).
*   **Аннотация:** Этот конструктор создает экземпляр атрибута `FeatureId`. Параметр `id` позволяет задать уникальный идентификатор для функциональности, которую описывает этот атрибут. Когда вы используете `@FeatureId("SomeID")` над вашим индикатором, вы вызываете этот конструктор, передавая `"SomeID"` в качестве значения `id`.

### Конструктор и Деструктор (Constructor & Destructor Documentation)

#### `FeatureId()`

```csharp
FeatureId ()
```

*   **Описание:**  Конструктор по умолчанию для класса `FeatureId`.
*   **Аннотация:**  Это конструктор без параметров.  Хотя он указан в документации, из контекста `FeatureId(string id)` кажется, что основной способ использования `FeatureId` - с предоставлением идентификатора. Возможно, конструктор по умолчанию существует для определенных сценариев или для внутреннего использования, но в явном виде в коде индикатора, скорее всего, будет использоваться `FeatureId(string id)`.

#### `FeatureId(string id)`

```csharp
ATAS.Indicators.Attributies.FeatureId.FeatureId (string id)
```

*   **Описание:**  Полное описание конструктора с параметром `id`. Повторение описания из раздела "Публичные функции-члены" для ясности.
*   **Параметры:**
    *   `id`: Строка (`string`), идентификатор функциональности.
*   **Аннотация:**  Еще раз подчеркивается, что этот конструктор инициализирует атрибут `FeatureId` с заданным идентификатором.

### Файл документации

Документация для этого класса была сгенерирована из файла: `FeatureId.cs`. Это указывает на то, что исходный код класса `FeatureId` находится в файле `FeatureId.cs`.





## Методичка по пространству имен ATAS.Indicators.Drawing

### Введение

Пространство имен `ATAS.Indicators.Drawing` в ATAS предназначено для классов, связанных с рисованием графических элементов непосредственно на графике индикатора. Оно предоставляет инструменты для добавления текста, фигур и линий, чтобы визуализировать информацию и сделать индикаторы более наглядными.

### Классы (Classes)

#### `BaseDrawingText`

```csharp
class BaseDrawingText
```

*   **Описание:** Представляет базовый класс для рисования текста на графике. [More...]
*   **Аннотация:** `BaseDrawingText` является основой для всех текстовых элементов, которые можно добавить на график индикатора. Он содержит основные свойства и методы, необходимые для отображения текста, такие как положение, цвет и шрифт.  Ссылка "More..." указывает на то, что в документации есть более подробная информация об этом классе.

#### `DefaultColors`

```csharp
class DefaultColors
```

*   **Описание:** Предоставляет коллекцию стандартных цветов, обычно используемых на графике.
*   **Аннотация:** Класс `DefaultColors` содержит набор предопределенных цветов, которые удобно использовать при рисовании элементов на графике. Это упрощает выбор цветовой палитры и обеспечивает визуальную согласованность элементов индикатора. Вы можете использовать эти цвета для линий, текста, фигур и других графических элементов.

#### `DrawingRectangle`

```csharp
class DrawingRectangle
```

*   **Описание:** Представляет прямоугольник, нарисованный на графике. [More...]
*   **Аннотация:** `DrawingRectangle` позволяет добавлять прямоугольные фигуры на график индикатора. Это может быть полезно для выделения областей графика, обозначения уровней или создания визуальных зон.  Ссылка "More..." указывает на наличие дополнительных деталей об этом классе.

#### `DrawingText`

```csharp
class DrawingText
```

*   **Описание:** Представляет класс для рисования текста на графике с дополнительными параметрами выравнивания. [More...]
*   **Аннотация:** `DrawingText` расширяет функциональность `BaseDrawingText`, добавляя возможности для более точного выравнивания текста. Это важно, когда нужно расположить текст относительно определенных точек на графике, например, над или под свечой, или по центру области. Ссылка "More..." ведет к более подробной информации.

#### `HorizontalChannel`

```csharp
class HorizontalChannel
```

*   **Описание:** Представляет горизонтальную линию на графике. [More...]
*   **Аннотация:**  `HorizontalChannel` предназначен для рисования горизонтальных линий на графике. Несмотря на название "Channel" (канал), описание указывает на *линию*. Возможно, название не совсем точно отражает функциональность или в "More..." содержится информация о каналах.  Горизонтальные линии часто используются для обозначения уровней поддержки и сопротивления. Ссылка "More..." может дать больше информации о возможностях этого класса.

#### `HorizontalLine`

```csharp
class HorizontalLine
```

*   **Описание:** Представляет горизонтальную линию на графике. [More...]
*   **Аннотация:** `HorizontalLine`, как и `HorizontalChannel`, предназначен для рисования горизонтальных линий. Вероятно, `HorizontalLine` является более базовым классом, а `HorizontalChannel` может иметь дополнительные свойства или методы, связанные с каналами (если название все же имеет отношение к каналам).  Ссылка "More..." поможет понять различия и детали.

#### `LineTillTouch`

```csharp
class LineTillTouch
```

*   **Описание:** Представляет линию тренда, которая продолжается до тех пор, пока не коснется цены. [More...]
*   **Аннотация:** `LineTillTouch` создает линию тренда, особенность которой в том, что она автоматически продлевается до момента пересечения с ценой. Это может быть полезно для визуализации динамических уровней тренда, которые меняются вместе с движением цены. Ссылка "More..." предоставит детали реализации и настройки.

#### `TrendLine`

```csharp
class TrendLine
```

*   **Описание:** Представляет линию тренда на графике. [More...]
*   **Аннотация:** `TrendLine` является базовым классом для рисования линий тренда. Он предоставляет основные возможности для создания и настройки линий тренда на графике.  Ссылка "More..." вероятно содержит информацию о настройках стиля, цвета, толщины и других параметров линии тренда.

### Определения типов (Typedefs)

#### `DashStyle`

```csharp
using DashStyle = System.Drawing.Drawing2D.DashStyle
```

*   **Описание:**  `DashStyle` = `System.Drawing.Drawing2D.DashStyle`
*   **Аннотация:**  `using DashStyle = System.Drawing.Drawing2D.DashStyle` — это объявление *typedef* (определение типа).  В C# `using` в данном контексте создает *псевдоним типа*.  Здесь, `ATAS.Indicators.Drawing.DashStyle` становится синонимом для типа `System.Drawing.Drawing2D.DashStyle`.

    `System.Drawing.Drawing2D.DashStyle` — это тип из .NET Framework, который определяет стиль пунктирных линий. Он позволяет выбирать различные виды пунктиров (сплошная линия, пунктирная линия, штрих-пунктирная линия и т.д.) для графических элементов, таких как линии тренда, горизонтальные линии и другие фигуры, рисуемые на графике индикатора.

    Таким образом, `DashStyle` в пространстве имен `ATAS.Indicators.Drawing` используется для задания стиля линий при рисовании графических элементов на графике индикатора, используя стандартные стили пунктиров из .NET.

### Документация по определениям типов (Typedef Documentation)

#### `DashStyle`

```csharp
using ATAS.Indicators.Drawing.DashStyle = typedef System.Drawing.Drawing2D.DashStyle
```

*   **Описание:**  Повторение определения `DashStyle` для ясности.
*   **Аннотация:**  Подчеркивается, что `ATAS.Indicators.Drawing.DashStyle` — это псевдоним для `System.Drawing.Drawing2D.DashStyle`, и используется для управления стилем пунктирных линий в графике индикатора.







``` методичка ATAS.Indicators.Drawing.BaseDrawingText

## Класс ATAS.Indicators.Drawing.BaseDrawingText
_Аннотация: Имя базового класса для рисования текста._

Представляет базовый класс для рисования текста на графике.
_Аннотация: Описание базового класса, представляющего текст на графике._

**Диаграмма наследования**
_Аннотация: Диаграмма наследования показывает, что DrawingText наследуется от BaseDrawingText._
```
ATAS.Indicators.Drawing.BaseDrawingText
↑
ATAS.Indicators.Drawing.DrawingText
```

## Публичные методы
_Аннотация: Публичные методы класса._

### override string ToString()
_Аннотация: Метод ToString: возвращает строковое представление объекта._
Возвращает строковое представление объекта.

## Свойства
_Аннотация: Свойства класса._

### int XOffset  [get, set]
_Аннотация: Свойство XOffset: смещение текста по оси X._
Получает или устанавливает смещение текста по оси X.

### string Tag  [get, set]
_Аннотация: Свойство Tag: тег, связанный с текстом._
Получает или устанавливает тег, связанный с текстом.

### string Text  [get, set]
_Аннотация: Свойство Text: текст для отображения._
Получает или устанавливает текст для отображения.

### int Bar  [get, set]
_Аннотация: Свойство Bar: индекс бара, на котором отображается текст._
Получает или устанавливает индекс бара, на котором отображается текст.

### int YOffset  [get, set]
_Аннотация: Свойство YOffset: смещение текста по оси Y._
Получает или устанавливает смещение текста по оси Y.

### bool IsAbovePrice  [get, set]
_Аннотация: Свойство IsAbovePrice: определяет, отображается ли текст выше цены._
Получает или устанавливает значение, определяющее, отображается ли текст выше цены.

### Color Textcolor  [get, set]
_Аннотация: Свойство Textcolor: цвет текста._
Получает или устанавливает цвет текста.

### Color Outlinecolor  [get, set]
_Аннотация: Свойство Outlinecolor: цвет обводки текста._
Получает или устанавливает цвет обводки текста.

### Color FillColor  [get, set]
_Аннотация: Свойство FillColor: цвет заливки текста._
Получает или устанавливает цвет заливки текста.

### bool AutoSize  [get, set]
_Аннотация: Свойство AutoSize: определяет, автоматически ли регулируется размер текста._
Получает или устанавливает значение, определяющее, автоматически ли регулируется размер текста.

### float FontSize  [get, set]
_Аннотация: Свойство FontSize: размер шрифта текста._
Получает или устанавливает размер шрифта текста.

### Font TextFont  [get, set]
_Аннотация: Свойство TextFont: шрифт текста._
Получает или устанавливает шрифт, используемый для текста.
```






## Методичка по классу `DrawingRectangle` для ATAS

### Класс `ATAS.Indicators.Drawing.DrawingRectangle`

**Описание:** Представляет прямоугольник, нарисованный на графике.

### Конструкторы

#### `DrawingRectangle()` [1/2]

```csharp
ATAS.Indicators.Drawing.DrawingRectangle.DrawingRectangle(
    int firstBar,
    decimal firstPrice,
    int secondBar,
    decimal secondPrice,
    Pen outlinePen,
    Brush fillBrush
)
```

**Описание:** Инициализирует новый экземпляр класса `DrawingRectangle` с указанными параметрами.

**Параметры:**

*   `firstBar`: `int` - Индекс первого бара.
*   `firstPrice`: `decimal` - Ценовое значение первой точки.
*   `secondBar`: `int` - Индекс второго бара.
*   `secondPrice`: `decimal` - Ценовое значение второй точки.
*   `outlinePen`: `Pen` - Перо, используемое для рисования контура прямоугольника.
*   `fillBrush`: `Brush` - Кисть, используемая для заливки прямоугольника.

#### `DrawingRectangle()` [2/2]

```csharp
ATAS.Indicators.Drawing.DrawingRectangle.DrawingRectangle(
    int firstBar,
    decimal firstPrice,
    int secondBar,
    decimal secondPrice,
    Pen outlinePen,
    Brush fillBrush,
    Pen midPen
)
```

**Описание:** Инициализирует новый экземпляр класса `DrawingRectangle` с указанными параметрами.

**Параметры:**

*   `firstBar`: `int` - Индекс первого бара.
*   `firstPrice`: `decimal` - Ценовое значение первой точки.
*   `secondBar`: `int` - Индекс второго бара.
*   `secondPrice`: `decimal` - Ценовое значение второй точки.
*   `outlinePen`: `Pen` - Перо, используемое для рисования контура прямоугольника.
*   `fillBrush`: `Brush` - Кисть, используемая для заливки прямоугольника.
*   `midPen`: `Pen` - Перо, используемое для рисования средней горизонтальной линии прямоугольника.

### Свойства

#### `Brush`

```csharp
Brush ATAS.Indicators.Drawing.DrawingRectangle.Brush { get; set; }
```

**Описание:**  Получает или устанавливает кисть, используемую для заливки прямоугольника.

#### `ExtendLeft`

```csharp
bool ATAS.Indicators.Drawing.DrawingRectangle.ExtendLeft { get; set; }
```

**Описание:** Получает или устанавливает расширение прямоугольника влево.

#### `ExtendRight`

```csharp
bool ATAS.Indicators.Drawing.DrawingRectangle.ExtendRight { get; set; }
```

**Описание:** Получает или устанавливает расширение прямоугольника вправо.

#### `FirstBar`

```csharp
int ATAS.Indicators.Drawing.DrawingRectangle.FirstBar { get; set; }
```

**Описание:** Получает или устанавливает индекс первого бара.

#### `FirstPrice`

```csharp
decimal ATAS.Indicators.Drawing.DrawingRectangle.FirstPrice { get; set; }
```

**Описание:** Получает или устанавливает ценовое значение первой точки.

#### `MiddlePen`

```csharp
Pen ATAS.Indicators.Drawing.DrawingRectangle.MiddlePen = new(DefaultColors.Gray) { get; set; }
```

**Описание:** Получает или устанавливает перо, используемое для рисования средней горизонтальной линии прямоугольника. По умолчанию используется серый цвет (`DefaultColors.Gray`).

#### `MidLineEnabled`

```csharp
bool ATAS.Indicators.Drawing.DrawingRectangle.MidLineEnabled { get; set; }
```

**Описание:** Получает или устанавливает рисование средней горизонтальной линии прямоугольника.

#### `Pen`

```csharp
Pen ATAS.Indicators.Drawing.DrawingRectangle.Pen { get; set; }
```

**Описание:** Получает или устанавливает перо, используемое для рисования контура прямоугольника.

#### `SecondBar`

```csharp
int ATAS.Indicators.Drawing.DrawingRectangle.SecondBar { get; set; }
```

**Описание:** Получает или устанавливает индекс второго бара.

#### `SecondPrice`

```csharp
decimal ATAS.Indicators.Drawing.DrawingRectangle.SecondPrice { get; set; }
```

**Описание:** Получает или устанавливает ценовое значение второй точки.







## Методичка по классу `ATAS.Indicators.Drawing.DrawingText` для создания индикаторов ATAS

### Описание класса

Класс `ATAS.Indicators.Drawing.DrawingText` представляет собой класс для рисования текста на графике с дополнительными опциями выравнивания.

### Диаграмма наследования

```
BaseDrawingText
    ^
    |
ATAS.Indicators.Drawing.DrawingText
    [legend]
```

### Диаграмма коллаборации

```
BaseDrawingText
    ^
    |
ATAS.Indicators.Drawing.DrawingText
    [legend]
```

### Публичные типы (Public Types)

#### enum `TextAlign`

```csharp
enum TextAlign { Left, Right, Center }
```

Получает или устанавливает выравнивание текста.

**Элементы перечисления:**

| Элемент  | Описание                                   |
| :------- | :----------------------------------------- |
| `Left`   | Текст выравнивается по левой стороне.      |
| `Right`  | Текст выравнивается по правой стороне.     |
| `Center` | Текст выравнивается по центру.            |

### Публичные функции-члены (Public Member Functions)

#### `DrawingText(decimal tickSize)`

```csharp
DrawingText(decimal tickSize)
```

Инициализирует новый экземпляр класса `DrawingText` с указанным размером тика (`tickSize`).

**Параметры:**

*   `tickSize`: Размер тика, используемый для расчетов.

### Публичные свойства (Properties)

#### `Align`

```csharp
TextAlign Align = TextAlign.Center [get, set]
```

Получает или устанавливает выравнивание текста. По умолчанию установлено значение `TextAlign.Center`.

#### `Price`

```csharp
int Price [get, set]
```

Получает или устанавливает ценовое значение, связанное с текстом.

#### `TickSize`

```csharp
decimal TickSize [get]
```

Получает значение размера тика, используемое для расчетов.

#### `TextPrice`

```csharp
decimal TextPrice [get, set]
```

Получает или устанавливает ценовое значение, связанное с текстом (в десятичном формате).

### Подробное описание

Класс представляет собой класс для рисования текста на графике с дополнительными опциями выравнивания.

### Расположение в файле

Документация для этого класса была сгенерирована из файла:

*   `TrendLine.cs`






## Методичка для ИИ по классу `ATAS.Indicators.Drawing.HorizontalChannel`

**Описание класса:** `ATAS.Indicators.Drawing.HorizontalChannel` представляет горизонтальную линию на графике.

**Свойства класса:**

1.  **`FirstPrice`**
    *   Тип: `required decimal`
    *   Описание:  Устанавливает или возвращает верхний ценовой уровень горизонтального канала.

2.  **`SecondPrice`**
    *   Тип: `required decimal`
    *   Описание: Устанавливает или возвращает нижний ценовой уровень горизонтального канала.

3.  **`Pen`**
    *   Тип: `required RenderPen`
    *   Значение по умолчанию: `new(DefaultColors.Gray)`
    *   Описание: Устанавливает или возвращает стиль линий границы.

4.  **`Background`**
    *   Тип: `Color`
    *   Значение по умолчанию: `Color.Transparent`
    *   Описание: Устанавливает или возвращает цвет фона.

5.  **`MiddlePen`**
    *   Тип: `RenderPen`
    *   Значение по умолчанию: `new(DefaultColors.Gray)`
    *   Описание: Устанавливает или возвращает стиль средней горизонтальной линии.

6.  **`EnableMiddleLine`**
    *   Тип: `bool`
    *   Описание: Устанавливает или возвращает рисование средней горизонтальной линии.

**Подробное описание свойств:**

*   **`Background`**:
    *   Тип: `Color` `ATAS.Indicators.Drawing.HorizontalChannel.Background = Color.Transparent`
    *   Описание: Устанавливает или возвращает цвет фона.

*   **`EnableMiddleLine`**:
    *   Тип: `bool` `ATAS.Indicators.Drawing.HorizontalChannel.EnableMiddleLine`
    *   Описание: Устанавливает или возвращает рисование средней горизонтальной линии.

*   **`FirstPrice`**:
    *   Тип: `required decimal` `ATAS.Indicators.Drawing.HorizontalChannel.FirstPrice`
    *   Описание: Устанавливает или возвращает верхний ценовой уровень горизонтального канала.

*   **`MiddlePen`**:
    *   Тип: `RenderPen` `ATAS.Indicators.Drawing.HorizontalChannel.MiddlePen = new(DefaultColors.Gray)`
    *   Описание: Устанавливает или возвращает стиль средней горизонтальной линии.

*   **`Pen`**:
    *   Тип: `required RenderPen` `ATAS.Indicators.Drawing.HorizontalChannel.Pen = new (DefaultColors.Gray)`
    *   Описание: Устанавливает или возвращает стиль линий границы.

*   **`SecondPrice`**:
    *   Тип: `required decimal` `ATAS.Indicators.Drawing.HorizontalChannel.SecondPrice`
    *   Описание: Устанавливает или возвращает нижний ценовой уровень горизонтального канала.

**Файл с документацией класса:** `TrendLine.cs`





## Методичка по классу `ATAS.Indicators.Drawing.HorizontalLine`

### Описание класса

`ATAS.Indicators.Drawing.HorizontalLine` - класс, представляющий горизонтальную линию на графике.

### Свойства

*   **`Price`**

    *   Тип: `decimal`
    *   Доступ: `[get, set]`
    *   Описание:  Получает или устанавливает ценовой уровень горизонтальной линии.

*   **`Width`**

    *   Тип: `int`
    *   Доступ: `[get, set]`
    *   Описание: Получает или устанавливает толщину горизонтальной линии.

*   **`Style`**

    *   Тип: `DashStyle`
    *   Доступ: `[get, set]`
    *   Описание: Получает или устанавливает стиль штриховки горизонтальной линии.

*   **`Color`**

    *   Тип: `Color`
    *   Доступ: `[get, set]`
    *   Описание: Получает или устанавливает цвет горизонтальной линии.

### Детальное описание

Класс `ATAS.Indicators.Drawing.HorizontalLine` используется для отображения горизонтальных линий на ценовом графике в платформе ATAS.  Линия характеризуется ценовым уровнем, толщиной, стилем отображения (сплошная, пунктирная и т.д.) и цветом.

### Документация свойств

*   **`Color`**

    *   Тип: `Color` `ATAS.Indicators.Drawing.HorizontalLine.Color`
    *   Доступ: `get`, `set`
    *   Описание: Получает или устанавливает цвет горизонтальной линии.

*   **`Price`**

    *   Тип: `decimal` `ATAS.Indicators.Drawing.HorizontalLine.Price`
    *   Доступ: `get`, `set`
    *   Описание: Получает или устанавливает ценовой уровень горизонтальной линии.

*   **`Style`**

    *   Тип: `DashStyle` `ATAS.Indicators.Drawing.HorizontalLine.Style`
    *   Доступ: `get`, `set`
    *   Описание: Получает или устанавливает стиль штриховки горизонтальной линии.

*   **`Width`**

    *   Тип: `int` `ATAS.Indicators.Drawing.HorizontalLine.Width`
    *   Доступ: `get`, `set`
    *   Описание: Получает или устанавливает толщину горизонтальной линии.

### Файл документации

Документация для этого класса была сгенерирована из файла:

*   `TrendLine.cs`






## Методичка по классу `ATAS.Indicators.Drawing.LineTillTouch`

**Описание класса:**

`ATAS.Indicators.Drawing.LineTillTouch` представляет линию тренда, которая продолжается до тех пор, пока цена не коснется ее.

**Диаграмма наследования:**

```
TrendLine
    ↑
ATAS.Indicators.Drawing.LineTillTouch
```

**Диаграмма взаимодействия:**

```
TrendLine
    ↑
ATAS.Indicators.Drawing.LineTillTouch
```

**Публичные члены класса:**

**Конструкторы:**

1.  `LineTillTouch(int bar, decimal price, Pen pen)`
    *   **Описание:** Инициализирует новый экземпляр класса `LineTillTouch` с одной точкой.
    *   **Параметры:**
        *   `bar`: `int` - Индекс бара.
        *   `price`: `decimal` - Ценовое значение точки.
        *   `pen`: `Pen` - Перо, используемое для рисования линии тренда.

2.  `LineTillTouch(int bar, decimal price, Pen pen, int fixedBarsCount)`
    *   **Описание:** Инициализирует новый экземпляр класса `LineTillTouch` с фиксированным количеством баров.
    *   **Параметры:**
        *   `bar`: `int` - Индекс бара.
        *   `price`: `decimal` - Ценовое значение начальной точки.
        *   `pen`: `Pen` - Перо, используемое для рисования линии тренда.
        *   `fixedBarsCount`: `int` - Фиксированное количество баров для линии.

**Методы:**

1.  `void CheckIfTouched(decimal high, decimal low, int bar, int lastBar)`
    *   **Описание:** Проверяет, была ли линия тренда затронута ценой в пределах указанных высоких и низких значений.
    *   **Параметры:**
        *   `high`: `decimal` - Самая высокая цена в диапазоне для проверки.
        *   `low`: `decimal` - Самая низкая цена в диапазоне для проверки.
        *   `bar`: `int` - Текущий индекс бара.
        *   `lastBar`: `int` - Индекс последнего бара.

**Свойства:**

1.  `Finished`: `bool` \[get]
    *   **Описание:** Возвращает значение, указывающее, была ли линия тренда завершена (затронута).

2.  `Context`: `object` \[get, set]
    *   **Описание:** Пользовательский контекст объекта.

**Файл документации:**

*   TrendLine.cs






## Методичка по классу `ATAS.Indicators.Drawing.TrendLine`

### Описание класса

Класс `ATAS.Indicators.Drawing.TrendLine` представляет линию тренда на графике.

### Диаграмма наследования

```
ATAS.Indicators.Drawing.TrendLine
    ↑
ATAS.Indicators.Drawing.LineTillTouch
```

### Публичные функции-члены

#### `TrendLine` (конструктор)

```csharp
TrendLine (int firstBar, decimal firstPrice, int secondBar, decimal secondPrice, Pen pen)
```

**Описание:**

Инициализирует новый экземпляр класса `TrendLine`.

**Параметры:**

*   `firstBar`: `int` - Индекс первого бара.
*   `firstPrice`: `decimal` - Ценовое значение первой точки.
*   `secondBar`: `int` - Индекс второго бара.
*   `secondPrice`: `decimal` - Ценовое значение второй точки.
*   `pen`: `Pen` - Перо, используемое для рисования линии тренда.

### Свойства

#### `Pen`

```csharp
Pen Pen { get; set; }
```

**Описание:**

Возвращает или устанавливает перо, используемое для рисования линии тренда.

#### `FirstBar`

```csharp
int FirstBar { get; set; }
```

**Описание:**

Возвращает или устанавливает индекс первого бара.

#### `SecondBar`

```csharp
int SecondBar { get; set; }
```

**Описание:**

Возвращает или устанавливает индекс второго бара.

#### `FirstPrice`

```csharp
decimal FirstPrice { get; set; }
```

**Описание:**

Возвращает или устанавливает ценовое значение первой точки.

#### `SecondPrice`

```csharp
decimal SecondPrice { get; set; }
```

**Описание:**

Возвращает или устанавливает ценовое значение второй точки.

#### `IsRay`

```csharp
bool IsRay { get; set; }
```

**Описание:**

Возвращает или устанавливает значение, указывающее, отображается ли линия тренда как луч.

### Подробное описание

Представляет линию тренда на графике.

### Документация конструктора и деструктора

#### `TrendLine()`

```csharp
ATAS.Indicators.Drawing.TrendLine.TrendLine (
    int firstBar,
    decimal firstPrice,
    int secondBar,
    decimal secondPrice,
    Pen pen
)
```

**Описание:**

Инициализирует новый экземпляр класса `TrendLine`.

**Параметры:**

*   `firstBar`: Индекс первого бара.
*   `firstPrice`: Ценовое значение первой точки.
*   `secondBar`: Индекс второго бара.
*   `secondPrice`: Ценовое значение второй точки.
*   `pen`: Перо, используемое для рисования линии тренда.

### Документация свойств

#### `FirstBar`

```csharp
int ATAS.Indicators.Drawing.TrendLine.FirstBar { get; set; }
```

**Описание:**

Возвращает или устанавливает индекс первого бара.

#### `FirstPrice`

```csharp
decimal ATAS.Indicators.Drawing.TrendLine.FirstPrice { get; set; }
```

**Описание:**

Возвращает или устанавливает ценовое значение первой точки.

#### `IsRay`

```csharp
bool ATAS.Indicators.Drawing.TrendLine.IsRay { get; set; }
```

**Описание:**

Возвращает или устанавливает значение, указывающее, отображается ли линия тренда как луч.

#### `Pen`

```csharp
Pen ATAS.Indicators.Drawing.TrendLine.Pen { get; set; }
```

**Описание:**

Возвращает или устанавливает перо, используемое для рисования линии тренда.

#### `SecondBar`

```csharp
int ATAS.Indicators.Drawing.TrendLine.SecondBar { get; set; }
```

**Описание:**

Возвращает или устанавливает индекс второго бара.

#### `SecondPrice`

```csharp
decimal ATAS.Indicators.Drawing.TrendLine.SecondPrice { get; set; }
```

**Описание:**

Возвращает или устанавливает ценовое значение второй точки.

### Файл документации класса

*   `TrendLine.cs`




## Методичка по классу `ATAS.Indicators.Filters.Converters.FilterBoolJsonConvert`

### Общая информация

**Название класса:** `ATAS.Indicators.Filters.Converters.FilterBoolJsonConvert`

**Родительский класс:** `FilterJsonConverterBase<FilterBool, bool>`

**Описание:** Класс `FilterBoolJsonConvert` предназначен для преобразования (конвертации) данных типа `FilterBool` в формат JSON. Он наследуется от класса `FilterJsonConverterBase`, который, вероятно, обеспечивает базовую функциональность для конвертации фильтров в JSON.

**Диаграмма наследования:**

```
FilterJsonConverterBase<FilterBool, bool>
    ↑
ATAS.Indicators.Filters.Converters.FilterBoolJsonConvert
```

**Диаграмма коллаборации:**

```
FilterJsonConverterBase<FilterBool, bool>  <--  ATAS.Indicators.Filters.Converters.FilterBoolJsonConvert
```

### Защищенные члены класса (Protected Member Functions)

**Переопределенный метод:** `Create()`

**Сигнатура метода:** `override FilterBool Create (bool enabledVisible, bool enabled)`

**Описание метода:**

*   `override FilterBool`:  Указывает, что метод `Create` переопределяет метод с таким же именем из родительского класса `FilterJsonConverterBase`. Возвращаемый тип `FilterBool` предполагает, что метод создает и возвращает объект типа `FilterBool`.
*   `Create`: Название метода, вероятно, предназначенного для создания экземпляра `FilterBool`.
*   `(bool enabledVisible, bool enabled)`:  Список параметров метода.
    *   `bool enabledVisible`:  Булевый параметр с именем `enabledVisible`. Возможно, определяет, должен ли фильтр быть видим (отображаться).
    *   `bool enabled`: Булевый параметр с именем `enabled`. Вероятно, контролирует, включен ли фильтр или нет.

**Реализация:**

*   **Файл реализации:** `FilterBoolJsonConvert.cs`

**Имплементирует интерфейс:**

*   `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterBool, bool>`

### Наследованные члены

Класс `FilterBoolJsonConvert` также наследует члены от своего родительского класса `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterBool, bool>`.  К ним относятся:

*   **Защищенные методы (Protected Member Functions)** унаследованные от `FilterJsonConverterBase<FilterBool, bool>`.
*   **Публичные методы (Public Member Functions)** унаследованные от `FilterJsonConverterBase<FilterBool, bool>`.
*   **Свойства (Properties)** унаследованные от `FilterJsonConverterBase<FilterBool, bool>`.

**Примечание:**  Для полного понимания функциональности класса `FilterBoolJsonConvert` необходимо также изучить документацию класса `FilterJsonConverterBase<FilterBool, bool>`, чтобы узнать о его унаследованных членах и базовой логике конвертации.




## Методичка по классу `FilterColorJsonConverter` в ATAS

### Общая информация

**Класс:** `ATAS.Indicators.Filters.Converters.FilterColorJsonConverter`

**Описание:**  Класс `FilterColorJsonConverter` предназначен для преобразования объектов `FilterColor` в формат JSON и обратно, вероятно, в контексте фильтров индикаторов в платформе ATAS.

**Иерархия наследования:**

```
FilterJsonConverterBase<FilterColor, CrossColor>
  └─ ATAS.Indicators.Filters.Converters.FilterColorJsonConverter
```

*   `FilterColorJsonConverter` наследуется от `FilterJsonConverterBase<FilterColor, CrossColor>`.
*   Это указывает на то, что класс специализируется на работе с типами `FilterColor` и `CrossColor` в рамках общей логики преобразования JSON, предоставляемой базовым классом `FilterJsonConverterBase`.

**Диаграмма коллаборации:**

```
FilterJsonConverterBase<FilterColor, CrossColor>
  └─ ATAS.Indicators.Filters.Converters.FilterColorJsonConverter
```

*   Диаграмма коллаборации дублирует иерархию наследования, подчеркивая взаимосвязь между `FilterColorJsonConverter` и его базовым классом `FilterJsonConverterBase<FilterColor, CrossColor>`.

### Защищенные члены класса (Protected Member Functions)

**Метод:** `Create()`

**Модификаторы:** `override`, `protected`, `virtual`

**Возвращаемый тип:** `FilterColor`

**Сигнатура метода:**

```csharp
override FilterColor Create (bool enabledVisible, bool enabled)
```

**Параметры метода:**

*   `bool enabledVisible`:  Булев параметр, предположительно, определяющий видимость или активность фильтра.
*   `bool enabled`: Булев параметр, вероятно, указывающий на общее состояние включения/выключения фильтра.

**Реализация:**

*   Метод `Create()` является переопределенным (`override`), что означает, что он изменяет поведение метода `Create()` из базового класса `FilterJsonConverterBase<FilterColor, CrossColor>`.
*   Метод объявлен как защищенный (`protected`) и виртуальный (`virtual`), что позволяет производным классам переопределять или вызывать его.
*   Метод возвращает объект типа `FilterColor`, вероятно, создавая и настраивая экземпляр `FilterColor` на основе входных параметров `enabledVisible` и `enabled`.

**Реализация интерфейса/базового класса:**

*   Метод `Create()` реализует метод из `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase< FilterColor, CrossColor >`.

### Дополнительные унаследованные члены (Additional Inherited Members)

*   **Публичные члены-функции (Public Member Functions):** Унаследованы от `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterColor, CrossColor>`.
*   **Свойства (Properties):** Унаследованы от `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterColor, CrossColor>`.

### Расположение в файле

*   Класс `FilterColorJsonConverter` определен в файле `FilterColorJsonConverter.cs`.





## Методичка для ИИ по классу `ATAS.Indicators.Filters.Converters.FilterHeatmapTypesJsonConverter`

### 1. Заголовок Класса

```csharp
namespace ATAS.Indicators.Filters.Converters
{
    public class FilterHeatmapTypesJsonConverter : FilterJsonConverterBase<FilterHeatmapTypes, HeatmapTypes>
    {
        // Тело класса будет описано далее
    }
}
```

**Аннотация:**

*   `namespace ATAS.Indicators.Filters.Converters`:  Указывает на пространство имен, к которому относится класс. Пространства имен используются для организации кода и предотвращения конфликтов имен. В данном случае класс `FilterHeatmapTypesJsonConverter` находится в пространстве имен `ATAS.Indicators.Filters.Converters`, что говорит о его принадлежности к ATAS индикаторам, фильтрам и конвертерам.
*   `public class FilterHeatmapTypesJsonConverter`:  Объявление публичного класса с именем `FilterHeatmapTypesJsonConverter`. `public` означает, что класс доступен из любого места в коде. `class` ключевое слово для объявления класса.
*   `: FilterJsonConverterBase<FilterHeatmapTypes, HeatmapTypes>`:  Указывает на наследование класса `FilterHeatmapTypesJsonConverter` от класса `FilterJsonConverterBase`.  Наследование - это механизм, позволяющий классу `FilterHeatmapTypesJsonConverter`  перенимать свойства и методы класса `FilterJsonConverterBase`. `<FilterHeatmapTypes, HeatmapTypes>`  является указанием на типы, используемые в базовом классе `FilterJsonConverterBase` как параметры типа (generics).

### 2.  Методы Класса

#### 2.1. Метод `Create()`

```csharp
protected override FilterHeatmapTypes Create (bool enabledVisible, bool enabled)
{
    // Реализация метода
    throw new NotImplementedException(); // Заглушка, реальная реализация не предоставлена в документации
}
```

**Аннотация:**

*   `protected override FilterHeatmapTypes Create (bool enabledVisible, bool enabled)`:  Объявление защищенного, переопределенного метода `Create`.
    *   `protected`:  Модификатор доступа `protected` означает, что метод доступен внутри текущего класса и в классах-наследниках.
    *   `override`:  Ключевое слово `override` указывает, что этот метод переопределяет метод с таким же именем, объявленный в базовом классе `FilterJsonConverterBase`.
    *   `FilterHeatmapTypes`:  Тип возвращаемого значения метода. Метод `Create` должен возвращать объект типа `FilterHeatmapTypes`.
    *   `Create`:  Имя метода. Предположительно, метод предназначен для создания экземпляра типа `FilterHeatmapTypes`.
    *   `(bool enabledVisible, bool enabled)`:  Список параметров метода.
        *   `bool enabledVisible`:  Первый параметр типа `bool` (логическое значение), названный `enabledVisible`. Вероятно, отвечает за видимость чего-либо при включении.
        *   `bool enabled`:  Второй параметр типа `bool`, названный `enabled`.  Вероятно, отвечает за общее включение или активацию функциональности.
*   `throw new NotImplementedException();`:  В данном примере показана заглушка реализации метода. `NotImplementedException`  выбрасывается, когда метод не имеет реальной реализации. В реальном коде здесь должен быть код, создающий и возвращающий объект типа `FilterHeatmapTypes` на основе параметров `enabledVisible` и `enabled`.


### 3.  Расположение файла с кодом класса

*   Файл: `FilterHeatmapTypesJsonConverter.cs`

**Аннотация:**

*   `FilterHeatmapTypesJsonConverter.cs`:  Указывает имя файла, в котором находится исходный код класса `FilterHeatmapTypesJsonConverter`. Расширение `.cs`  говорит о том, что это файл с кодом на языке C#.

**Заключение:**

Данная методичка предоставляет подробное описание структуры и членов класса `ATAS.Indicators.Filters.Converters.FilterHeatmapTypesJsonConverter` на основе предоставленной PDF-документации.  Она включает заголовок класса с указанием пространства имен и наследования, а также подробное описание метода `Create()` с аннотациями для лучшего понимания структуры кода.  Эта информация должна быть достаточной для ИИ, чтобы понять структуру класса и его основные компоненты для дальнейшей разработки индикаторов ATAS.





## Методичка для создания индикаторов ATAS на основе PDF-документации

### Страница 1

**Название:** `ATAS.Indicators.Filters.Converters.FilterIntJsonConverter Class Reference` (Описание класса `FilterIntJsonConverter`)

**Разделы страницы:**

1.  **Protected Member Functions | List of all members** (Защищенные члены-функции | Список всех членов) - Заголовок страницы, указывающий на тип документации.
2.  **ATAS.Indicators.Filters.Converters.FilterIntJsonConverter Class Reference** (Ссылка на класс `FilterIntJsonConverter`) - Название класса, которому посвящена документация.
3.  **Inheritance diagram for ATAS.Indicators.Filters.Converters.FilterIntJsonConverter:** (Диаграмма наследования для `FilterIntJsonConverter`):
    *   Изображение диаграммы наследования.
    *   **FilterJsonConverterBase < FilterInt, int >** - Базовый класс, от которого наследуется `FilterIntJsonConverter`.
        *   *Аннотация:* `FilterJsonConverterBase < FilterInt, int >` - это родительский класс, от которого данный класс берет свои свойства и методы. `< FilterInt, int >` указывает на типы данных, используемые в базовом классе.
    *   **ATAS.Indicators.Filters.Converters.FilterIntJsonConverter** -  Производный класс `FilterIntJsonConverter`.
        *   *Аннотация:* `ATAS.Indicators.Filters.Converters.FilterIntJsonConverter` - это текущий класс, который расширяет функциональность `FilterJsonConverterBase`.
    *   **[legend]** -  Указание на легенду диаграммы (не представлена на изображении).
4.  **Collaboration diagram for ATAS.Indicators.Filters.Converters.FilterIntJsonConverter:** (Диаграмма взаимодействия для `FilterIntJsonConverter`):
    *   Изображение диаграммы взаимодействия.
    *   **FilterJsonConverterBase < FilterInt, int >** - Базовый класс.
        *   *Аннотация:*  Аналогично диаграмме наследования, показывает взаимодействие с базовым классом.
    *   **ATAS.Indicators.Filters.Converters.FilterIntJsonConverter** - Производный класс.
        *   *Аннотация:*  Показывает, что класс `FilterIntJsonConverter` использует `FilterJsonConverterBase`.
    *   **[legend]** - Указание на легенду диаграммы.
5.  **Protected Member Functions** (Защищенные члены-функции):
    *   **override FilterInt Create (bool enabledVisible, bool enabled)** - Объявление защищенной переопределенной функции `Create`.
        *   *Аннотация:* `override FilterInt Create` -  Функция `Create` переопределяется в этом классе. `FilterInt` -  вероятно, тип возвращаемого значения. `(bool enabledVisible, bool enabled)` - параметры функции: `enabledVisible` и `enabled`, оба типа `bool` (логический тип).
    *   **► Protected Member Functions inherited from ATAS.Indicators.Filters.Converters.FilterJsonConverterBase< FilterInt, int >** -  Указание на наличие защищенных членов-функций, унаследованных от базового класса `FilterJsonConverterBase< FilterInt, int >`.
        *   *Аннотация:*  Этот пункт сообщает, что класс `FilterIntJsonConverter` также имеет защищенные функции, полученные в наследство от `FilterJsonConverterBase`.
6.  **Additional Inherited Members** (Дополнительные унаследованные члены):
    *   **► Public Member Functions inherited from ATAS.Indicators.Filters.Converters.FilterJsonConverterBase< FilterInt, int >** - Указание на наличие публичных членов-функций, унаследованных от базового класса.
        *   *Аннотация:*  Аналогично предыдущему пункту, но для публичных функций, унаследованных от `FilterJsonConverterBase`.
    *   **► Properties inherited from ATAS.Indicators.Filters.Converters.FilterJsonConverterBase< FilterInt, int >** - Указание на наличие свойств, унаследованных от базового класса.
        *   *Аннотация:*  Указывает на унаследованные свойства (характеристики данных) от `FilterJsonConverterBase`.
7.  **Member Function Documentation** (Документация члена-функции):
    *   **Create()** -  Ссылка на документацию функции `Create`.
        *   *Аннотация:*  Это ссылка на подробное описание функции `Create`, представленной выше в разделе "Protected Member Functions".

### Страница 2

**Разделы страницы:**

1.  **override FilterInt** -  Указание на переопределение функции и тип возвращаемого значения `FilterInt`.
    *   *Аннотация:* `override FilterInt` -  Подтверждение, что функция `Create` переопределяется и возвращает тип `FilterInt`.
2.  **ATAS.Indicators.Filters.Converters.FilterIntJsonConverter.Create** - Полное имя функции `Create` с указанием класса и пространства имен.
    *   *Аннотация:*  Полный путь к функции `Create`, показывающий ее принадлежность к классу `FilterIntJsonConverter` и соответствующим пространствам имен.
3.  **(bool enabledVisible, bool enabled)** - Список параметров функции `Create` с указанием типов.
    *   *Аннотация:* `(bool enabledVisible, bool enabled)` -  Перечисление параметров функции `Create`: `enabledVisible` и `enabled`, оба логического типа `bool`.
4.  **protected virtual** - Модификаторы доступа и виртуальности функции `Create`.
    *   *Аннотация:* `protected` -  Функция `Create` имеет защищенный доступ, то есть доступна внутри класса и в его наследниках. `virtual` -  Функция `Create` является виртуальной, что позволяет ее переопределять в производных классах (что и происходит, так как указано `override`).
5.  **Implements ATAS.Indicators.Filters.Converters.FilterJsonConverterBase< FilterInt, int >.** - Указание на интерфейс или базовый класс, который реализует данная функция.
    *   *Аннотация:* `Implements ATAS.Indicators.Filters.Converters.FilterJsonConverterBase< FilterInt, int >.` -  Функция `Create` реализует функцию с таким же именем и сигнатурой, объявленную в базовом классе `FilterJsonConverterBase< FilterInt, int >`.
6.  **The documentation for this class was generated from the following file:** (Документация для этого класса была сгенерирована из следующего файла:) -  Информация об источнике документации.
    *   *Аннотация:*  Сообщает, что документация была создана на основе файла.
7.  **• FilterIntJsonConverter.cs** -  Название файла, из которого сгенерирована документация.
    *   *Аннотация:* `FilterIntJsonConverter.cs` -  Имя файла с исходным кодом, содержащим класс `FilterIntJsonConverter`.







``` методичка
## ATAS.Indicators.Filters.Converters.FilterJsonConverter Class Reference

**Описание:** Класс `ATAS.Indicators.Filters.Converters.FilterJsonConverter` предназначен для преобразования фильтров в формат JSON.

**Наследование:**

*   `FilterJsonConverterBase<Filter, decimal>`  ->  `ATAS.Indicators.Filters.Converters.FilterJsonConverter`

    *Аннотация: `ATAS.Indicators.Filters.Converters.FilterJsonConverter` наследуется от `FilterJsonConverterBase`, который параметризован типами `Filter` и `decimal`.*

**Защищенные функции-члены:**

*   `override Filter Create (bool enabledVisible, bool enabled)`

    *Аннотация: Функция `Create` является переопределением (override) функции из базового класса. Она создает экземпляр `Filter`. Функция является защищенной (protected) и виртуальной (virtual) в базовом классе, что позволяет переопределять ее в производных классах.*

    *   **Параметры:**
        *   `bool enabledVisible`
            *Аннотация: Логический параметр `enabledVisible`, вероятно, контролирует видимость фильтра при его создании.*
        *   `bool enabled`
            *Аннотация: Логический параметр `enabled`, вероятно, включает или отключает функциональность фильтра.*

    *   **Возвращаемое значение:** `Filter`
        *Аннотация: Функция возвращает объект типа `Filter`, который является созданным экземпляром фильтра.*

    *   **Реализует:** `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase< Filter, decimal >`
        *Аннотация: Указывает, что функция `Create` реализует абстрактный или виртуальный метод, определенный в базовом классе `FilterJsonConverterBase< Filter, decimal >`.*

**Дополнительные унаследованные члены:**

*   **Публичные функции-члены, унаследованные от `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase< Filter, decimal >`**
    *Аннотация: Класс `ATAS.Indicators.Filters.Converters.FilterJsonConverter` также наследует публичные функции-члены от своего базового класса `FilterJsonConverterBase< Filter, decimal >`. Детали этих функций не приведены в данном документе.*

*   **Свойства, унаследованные от `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase< Filter, decimal >`**
    *Аннотация:  Аналогично, класс наследует свойства от `FilterJsonConverterBase< Filter, decimal >`. Детали свойств также не указаны в данном документе.*

**Расположение файла:**

*   `FilterJsonConverter.cs`
    *Аннотация: Исходный код класса `FilterJsonConverter` находится в файле `FilterJsonConverter.cs`.*
```






## Методичка по классу `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<TFilter, TValue>`

### Описание класса

```csharp
ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<TFilter, TValue>
```
**Описание:** Абстрактный класс-шаблон `FilterJsonConverterBase<TFilter, TValue>` предназначен для преобразования фильтров (`TFilter`) в формат JSON и обратно. Он является базовым классом для конкретных реализаций конвертеров фильтров в ATAS.

**Наследование:**

```
JsonConverter
    ↑
    ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<TFilter, TValue>
```
**Описание:** `FilterJsonConverterBase` наследуется от класса `JsonConverter`, что указывает на его роль в процессе сериализации и десериализации JSON.

**Коллаборация:**

```
JsonConverter
    ↑
    ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<TFilter, TValue>
```
**Описание:**  `FilterJsonConverterBase` взаимодействует с `JsonConverter` для выполнения задач преобразования данных в JSON.

### Публичные члены класса (Public Member Functions)

#### `WriteJson()`

```csharp
override void WriteJson (JsonWriter writer, object value, JsonSerializer serializer)
```
**Описание:**
*   **`override void WriteJson`**: Переопределенный метод для записи JSON представления объекта.
*   **`(JsonWriter writer, object value, JsonSerializer serializer)`**:
    *   `JsonWriter writer`:  Объект `JsonWriter` для записи JSON данных.
    *   `object value`: Объект фильтра (`TFilter`), который нужно сериализовать в JSON.
    *   `JsonSerializer serializer`:  Сериализатор JSON, используемый для процесса сериализации.

#### `ReadJson()`

```csharp
override object ReadJson (JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer)
```
**Описание:**
*   **`override object ReadJson`**: Переопределенный метод для чтения JSON представления объекта и его десериализации. Возвращает десериализованный объект фильтра.
*   **`(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer)`**:
    *   `JsonReader reader`: Объект `JsonReader` для чтения JSON данных.
    *   `Type objectType`: Тип объекта (`TFilter`), который нужно десериализовать.
    *   `object existingValue`:  Существующий объект, который можно использовать повторно (опционально).
    *   `JsonSerializer serializer`: Десериализатор JSON, используемый для процесса десериализации.

#### `CanConvert()`

```csharp
override bool CanConvert (Type objectType)
```
**Описание:**
*   **`override bool CanConvert`**: Переопределенный метод, определяющий, может ли конвертер обработать указанный тип объекта. Возвращает `true`, если конвертер может конвертировать `objectType`, и `false` в противном случае.
*   **`(Type objectType)`**:
    *   `Type objectType`: Тип объекта, для которого проверяется возможность конвертации.

### Защищенные члены класса (Protected Member Functions)

#### `Create()`

```csharp
abstract TFilter Create (bool enabledVisible, bool enabled)
```
**Описание:**
*   **`abstract TFilter Create`**: Абстрактный метод для создания экземпляра фильтра (`TFilter`). Должен быть реализован в производных классах.
*   **`(bool enabledVisible, bool enabled)`**:
    *   `bool enabledVisible`:  Флаг видимости фильтра (может быть включен и видим).
    *   `bool enabled`: Флаг включения фильтра (активен или нет).
*   **`protected pure virtual`**: Защищенный и чисто виртуальный метод, предназначен для реализации в дочерних классах.

**Реализован в:**

*   `ATAS.Indicators.Filters.Converters.FilterBoolJsonConvert`
*   `ATAS.Indicators.Filters.Converters.FilterColorJsonConverter`
*   `ATAS.Indicators.Filters.Converters.FilterHeatmapTypesJsonConverter`
*   `ATAS.Indicators.Filters.Converters.FilterIntJsonConverter`
*   `ATAS.Indicators.Filters.Converters.FilterJsonConverter`
*   `ATAS.Indicators.Filters.Converters.FilterKeyJsonConvert`
*   `ATAS.Indicators.Filters.Converters.FilterRenderPenJsonConverter`
*   `ATAS.Indicators.Filters.Converters.FilterStringJsonConverter`
*   `ATAS.Indicators.Filters.Converters.FilterTimeSpanJsonConvert`

#### `CreateFromValue()`

```csharp
TFilter CreateFromValue (TFilter filter, TValue value)
```
**Описание:**
*   **`TFilter CreateFromValue`**: Метод для создания нового фильтра (`TFilter`) на основе существующего фильтра и значения (`TValue`).
*   **`(TFilter filter, TValue value)`**:
    *   `TFilter filter`:  Существующий фильтр, на основе которого создается новый.
    *   `TValue value`:  Значение, которое нужно установить для нового фильтра.
*   **`protected`**: Защищенный метод.

#### `ReadFromObject()`

```csharp
virtual TFilter ReadFromObject (JsonReader reader, TFilter filter)
```
**Описание:**
*   **`virtual TFilter ReadFromObject`**: Виртуальный метод для чтения свойств объекта фильтра из JSON ридера и применения их к существующему фильтру. Может быть переопределен в производных классах.
*   **`(JsonReader reader, TFilter filter)`**:
    *   `JsonReader reader`:  Объект `JsonReader` для чтения JSON данных.
    *   `TFilter filter`: Существующий фильтр, свойства которого нужно обновить из JSON.
*   **`protected virtual`**: Защищенный виртуальный метод.

**Переопределен в:**

*   `ATAS.Indicators.Filters.Converters.FilterRangeJsonConverterBase<TFilter, TValue>`

#### `ReadValue()`

```csharp
virtual TValue ReadValue (JsonReader reader)
```
**Описание:**
*   **`virtual TValue ReadValue`**: Виртуальный метод для чтения значения (`TValue`) из JSON ридера. Может быть переопределен в производных классах.
*   **`(JsonReader reader)`**:
    *   `JsonReader reader`: Объект `JsonReader` для чтения JSON данных.
*   **`protected virtual`**: Защищенный виртуальный метод.

**Переопределен в:**

*   `ATAS.Indicators.Filters.Converters.FilterRenderPenJsonConverter`

#### `CreateStoredValue()`

```csharp
virtual object CreateStoredValue (TValue value)
```
**Описание:**
*   **`virtual object CreateStoredValue`**: Виртуальный метод для создания сохраненного значения (object) на основе значения (`TValue`). Может быть переопределен в производных классах для специфической обработки значений.
*   **`(TValue value)`**:
    *   `TValue value`: Значение, на основе которого создается сохраненное значение.
*   **`protected virtual`**: Защищенный виртуальный метод.

#### `WriteValue()`

```csharp
virtual void WriteValue (JsonWriter writer, TValue value)
```
**Описание:**
*   **`virtual void WriteValue`**: Виртуальный метод для записи значения (`TValue`) в JSON writer. Может быть переопределен в производных классах для специфического форматирования значений в JSON.
*   **`(JsonWriter writer, TValue value)`**:
    *   `JsonWriter writer`: Объект `JsonWriter` для записи JSON данных.
    *   `TValue value`: Значение, которое нужно записать в JSON.
*   **`protected virtual`**: Защищенный виртуальный метод.

### Свойства (Properties)

#### `ValuePropertyName`

```csharp
string ValuePropertyName = nameof(Filter.Value).ToLower() [get]
```
**Описание:**
*   **`string ValuePropertyName`**:  Свойство, возвращающее имя свойства "Value" фильтра в нижнем регистре. Используется для сериализации/десериализации JSON.
*   **`nameof(Filter.Value).ToLower()`**:  Получает имя свойства `Value` из класса `Filter` и преобразует его в нижний регистр.
*   **`[get] protected`**:  Доступно только для чтения и защищено для доступа из производных классов.

#### `EnabledPropertyName`

```csharp
string EnabledPropertyName = nameof(Filter.Enabled).ToLower() [get]
```
**Описание:**
*   **`string EnabledPropertyName`**: Свойство, возвращающее имя свойства "Enabled" фильтра в нижнем регистре. Используется для сериализации/десериализации JSON.
*   **`nameof(Filter.Enabled).ToLower()`**: Получает имя свойства `Enabled` из класса `Filter` и преобразует его в нижний регистр.
*   **`[get] protected`**: Доступно только для чтения и защищено для доступа из производных классов.

#### `EnabledVisiblePropertyName`

```csharp
string EnabledVisiblePropertyName = nameof(Filter.EnabledVisible).ToLower() [get]
```
**Описание:**
*   **`string EnabledVisiblePropertyName`**: Свойство, возвращающее имя свойства "EnabledVisible" фильтра в нижнем регистре. Используется для сериализации/десериализации JSON.
*   **`nameof(Filter.EnabledVisible).ToLower()`**: Получает имя свойства `EnabledVisible` из класса `Filter` и преобразует его в нижний регистр.
*   **`[get] protected`**: Доступно только для чтения и защищено для доступа из производных классов.

### Расположение файла

*   `FilterJsonConverterBase.cs`

**Описание:** Исходный код класса `FilterJsonConverterBase` находится в файле `FilterJsonConverterBase.cs`.






## Методичка: Класс FilterKeyJsonConvert для ATAS Indicators

**1. Определение Класса:**

*   **Полное Имя Класса:** `ATAS.Indicators.Filters.Converters.FilterKeyJsonConvert`
    *   *Аннотация:*  Это полное пространство имен и имя класса. В .NET, пространство имен (`ATAS.Indicators.Filters.Converters`) используется для организации классов и предотвращения конфликтов имен. `FilterKeyJsonConvert` - имя самого класса.
*   **Наследование:** Класс наследуется от `FilterJsonConverterBase<FilterKey, CrossKey>`.
    *   *Аннотация:* Наследование означает, что `FilterKeyJsonConvert` получает свойства и методы класса `FilterJsonConverterBase`.  `<FilterKey, CrossKey>` - это параметры типа, указывающие на конкретные типы данных, используемые в базовом классе.

**2. Члены Класса:**

*   **Защищенная Переопределенная Функция:**
    ```csharp
    override FilterKey Create(bool enabledVisible, bool enabled)
    ```
    *   *Аннотация:*  Это определение функции внутри класса `FilterKeyJsonConvert`.
        *   `override`: Ключевое слово `override` означает, что эта функция переопределяет (изменяет поведение) функции `Create` из базового класса `FilterJsonConverterBase`.
        *   `FilterKey`:  Это тип данных, который функция возвращает после выполнения.
        *   `Create`:  Имя функции. Вероятно, она предназначена для создания или инициализации чего-либо типа `FilterKey`.
        *   `protected virtual`:  Из контекста базового класса можно предположить, что в базовом классе функция `Create` была объявлена как `virtual` и `protected`. `protected` означает, что функция доступна внутри этого класса и в его наследниках. `virtual` позволяет переопределение в производных классах.
    *   **Возвращаемый Тип:** `FilterKey`
        *   *Аннотация:* Функция `Create` возвращает объект типа `FilterKey`.
    *   **Имя Функции:** `Create`
        *   *Аннотация:* Имя функции, используемое для вызова этой функциональности.
    *   **Параметры:**
        *   `bool enabledVisible`:  Булев параметр `enabledVisible`.
            *   *Аннотация:* `bool` - это тип данных "булево", который может принимать значения `true` (истина) или `false` (ложь).  `enabledVisible`, вероятно, указывает, должен ли быть элемент "видимым" и "включенным".
        *   `bool enabled`: Булев параметр `enabled`.
            *   *Аннотация:*  `enabled`, вероятно, указывает, должен ли элемент быть "включенным" или нет.

*   **Унаследованные Члены:**
    *   Класс `FilterKeyJsonConvert` наследует члены от базового класса `FilterJsonConverterBase<FilterKey, CrossKey>`.  (Подробности об унаследованных членах можно найти в документации к `FilterJsonConverterBase`).
        *   *Аннотация:*  Это означает, что `FilterKeyJsonConvert` автоматически получает все публичные и защищенные функции, свойства и другие члены, определенные в `FilterJsonConverterBase`.

**3. Расположение в Файле:**

*   Класс определен в файле: `FilterKeyJsonConvert.cs`
    *   *Аннотация:*  Это имя файла с исходным кодом на языке C# (`.cs` расширение), где определен класс `FilterKeyJsonConvert`.







## Методичка по классу `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter`

### Описание класса

Класс `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter` предназначен для работы с фильтрами диапазона целых чисел (FilterRangeInt) в формате JSON в платформе ATAS.

**Полное название класса:** `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter`

**Назначение:**  Преобразование и работа с настройками фильтра целых чисел при сериализации и десериализации JSON.

### Диаграмма наследования

```
FilterRangeJsonConverterBase< FilterRangeInt, int >
    ↑
FilterRangeJsonConverter
    ↑
ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter
```

**Пояснение:**

*   `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter` является наследником класса `FilterRangeJsonConverter`, который, в свою очередь, наследуется от `FilterRangeJsonConverterBase< FilterRangeInt, int >`.
*   Это означает, что класс `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter`  унаследовал функциональность от своих базовых классов и специализируется на работе с типом `FilterRangeInt` и целочисленными значениями (`int`).

### Диаграмма взаимодействия

```
FilterRangeJsonConverterBase< FilterRangeInt, int >
    ↑
FilterRangeJsonConverter
    ↑
ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter
```

**Пояснение:**

*   Диаграмма взаимодействия показывает ту же структуру наследования, что и диаграмма наследования.
*   Она подчеркивает взаимосвязь между классами и то, как `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter` использует и расширяет функциональность базовых классов.

### Защищенные методы класса

#### `Create()`

```csharp
override FilterRangeInt Create (bool enabledVisible, bool enabled)
```

**Описание:**

*   Это переопределенный метод `Create`, который создает экземпляр класса `FilterRangeInt`.
*   Метод принимает два булевых параметра:
    *   `enabledVisible`:  Управляет видимостью фильтра в интерфейсе пользователя.
    *   `enabled`:  Управляет состоянием активности фильтра (включен или выключен).
*   Метод возвращает объект типа `FilterRangeInt`, представляющий созданный фильтр диапазона целых чисел.

**Расположение метода:**

*   `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter.Create`

**Уровень доступа:**

*   `protected` (защищенный) - метод доступен для использования внутри класса `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter` и его наследников.

### Унаследованные защищенные методы

*   Класс `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter` наследует защищенные методы от базового класса `ATAS.Indicators.Filters.Converters.FilterRangeJsonConverterBase< FilterRangeInt, int >`.  (Детали этих унаследованных методов не представлены в данном документе).

### Расположение файла с кодом класса

*   Документация для класса `ATAS.Indicators.Filters.Converters.FilterRangeIntJsonConverter` была сгенерирована из файла:
    *   `FilterRangeIntJsonConverter.cs`





## Методичка по классу `ATAS.Indicators.Filters.Converters.FilterRangeJsonConverterBase<TFilter, TValue>`

### Описание класса

Класс `ATAS.Indicators.Filters.Converters.FilterRangeJsonConverterBase<TFilter, TValue>` является шаблоном класса, предназначенным для преобразования (конвертации) данных фильтров диапазона в формат JSON и обратно в контексте платформы ATAS. Этот класс, вероятно, используется для сериализации и десериализации настроек фильтров, которые работают с диапазонами значений.

**Наследование:**

Класс `FilterRangeJsonConverterBase<TFilter, TValue>` наследуется от класса `FilterJsonConverterBase<TFilter, FilterRangeValue<TValue>>`.  Это означает, что `FilterRangeJsonConverterBase` уже обладает функциональностью для работы с JSON-конвертацией базовых фильтров (`FilterJsonConverterBase`) и специализируется на обработке фильтров, которые оперируют диапазонами значений (`FilterRangeValue<TValue>`).

**Диаграммы:**

*   **Диаграмма наследования:** Показывает, что `FilterRangeJsonConverterBase` является подклассом `FilterJsonConverterBase`.
*   **Диаграмма сотрудничества:**  Также демонстрирует связь между `FilterRangeJsonConverterBase` и `FilterJsonConverterBase`, подчеркивая их взаимодействие в процессе конвертации.

### Protected Member Functions (Защищенные члены-функции)

#### `override TFilter ReadFromObject(JsonReader reader, TFilter filter)`

Эта функция является переопределенной (ключевое слово `override`) функцией, унаследованной от базового класса `FilterJsonConverterBase`.

*   **Назначение:** Функция `ReadFromObject` отвечает за чтение данных фильтра из объекта JSON, представленного `JsonReader`, и заполнение переданного объекта фильтра (`filter`) этими данными.
*   **Параметры:**
    *   `JsonReader reader`:  Объект `JsonReader`, предоставляющий интерфейс для чтения данных из JSON-потока.
    *   `TFilter filter`: Объект фильтра типа `TFilter`, который необходимо заполнить данными из JSON. Тип `TFilter` является параметром шаблона класса, и представляет конкретный тип фильтра, с которым работает конвертер.
*   **Возвращаемое значение:** Функция возвращает объект типа `TFilter`, что, вероятно, является заполненным или обновленным объектом фильтра, который был передан в качестве параметра.
*   **Модификаторы доступа:**
    *   `override`:  Указывает, что функция переопределяет реализацию одноименной функции в базовом классе.
    *   `protected`:  Означает, что функция доступна для вызова из самого класса, его подклассов и классов, находящихся в той же сборке (assembly).
    *   `virtual`:  Указывает, что функция является виртуальной, что позволяет подклассам переопределять ее поведение.

**Реализация:**

Функция `ReadFromObject` переопределена из `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<TFilter, FilterRangeValue<TValue>>`. Это означает, что логика чтения и десериализации JSON для фильтров диапазона значений специфична и отличается от общей логики для базовых фильтров.

**Расположение в файле:**

Документация указывает, что код для этого класса находится в файле `FilterRangeJsonConverterBase.cs`.

### Additional Inherited Members (Дополнительные унаследованные члены)

Раздел "Additional Inherited Members" (Дополнительные унаследованные члены)  ссылается на члены, унаследованные от базового класса `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<TFilter, FilterRangeValue<TValue>>`.  В документации упоминаются следующие типы унаследованных членов:

*   **Public Member Functions (Публичные члены-функции):**  Публичные функции, доступные для использования из любого места, где виден класс `FilterRangeJsonConverterBase`.
*   **Properties (Свойства):**  Свойства, предоставляющие доступ к данным объекта через методы `get` и `set`.

Чтобы узнать конкретные унаследованные члены, необходимо обратиться к документации класса `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<TFilter, FilterRangeValue<TValue>>`.







## Методичка по классу `ATAS.Indicators.Filters.Converters.FilterRenderPenJsonConverter` для создания индикаторов ATAS

### Общая информация

**Название класса:** `ATAS.Indicators.Filters.Converters.FilterRenderPenJsonConverter`

**Описание:** Данный класс предназначен для конвертации настроек отображения (`FilterRenderPen`) в формат JSON и обратно. Это необходимо для сохранения и загрузки настроек фильтров в платформе ATAS.

**Назначение для индикаторов ATAS:**  При разработке индикаторов, особенно фильтров, часто требуется сохранять и восстанавливать визуальные настройки, такие как цвет, стиль линии и толщина. Этот класс обеспечивает механизм для сериализации и десериализации этих настроек, используя JSON формат.

### Диаграмма наследования

```
FilterJsonConverterBase
< FilterRenderPen, PenSettings >
    ↑
ATAS.Indicators.Filters.
Converters. FilterRenderPenJson
Converter
```

**Аннотация:**
*   `FilterJsonConverterBase<FilterRenderPen, PenSettings>`:  Базовый класс, предоставляющий общую функциональность для JSON конвертеров, работающих с настройками отображения (`PenSettings`). Он параметризован типами `FilterRenderPen` (тип настроек отображения для фильтров) и `PenSettings` (тип настроек пера/отображения).
*   `ATAS.Indicators.Filters.Converters.FilterRenderPenJsonConverter`:  Класс `FilterRenderPenJsonConverter` наследуется от `FilterJsonConverterBase` и специализируется на конвертации именно `FilterRenderPen` настроек.

### Диаграмма коллаборации

```
FilterJsonConverterBase
< FilterRenderPen, PenSettings >
    ↑
ATAS.Indicators.Filters.
Converters. FilterRenderPenJson
Converter
```

**Аннотация:**
*   Диаграмма коллаборации показывает те же отношения, что и диаграмма наследования, подчеркивая, что `FilterRenderPenJsonConverter` *является* специализированным типом `FilterJsonConverterBase`.

### Protected Member Functions (Защищенные члены-функции)

#### `override FilterRenderPen Create(bool enabledVisible, bool enabled)`

```csharp
override FilterRenderPen Create (bool enabledVisible, bool enabled)
```

**Аннотация:**
*   `override`: Ключевое слово `override` указывает, что эта функция переопределяет функцию с тем же именем из базового класса `FilterJsonConverterBase`.
*   `FilterRenderPen`:  Тип возвращаемого значения. Функция создает и возвращает объект типа `FilterRenderPen`, который представляет собой настройки отображения для фильтра.
*   `Create`: Название функции, подразумевает создание нового объекта `FilterRenderPen`.
*   `(bool enabledVisible, bool enabled)`:  Параметры функции:
    *   `enabledVisible`:  Булевый параметр, вероятно, определяющий, должна ли быть видима настройка видимости (например, в интерфейсе пользователя).
    *   `enabled`: Булевый параметр, вероятно, определяющий, включена ли настройка в целом.
*   `protected virtual`:  `protected` означает, что функция доступна внутри класса и в его производных классах. `virtual` указывает, что функция может быть переопределена в производных классах (хотя в данном случае она уже переопределяет базовую реализацию).
*   **Назначение:** Функция `Create` отвечает за создание нового экземпляра `FilterRenderPen`. Параметры `enabledVisible` и `enabled` позволяют инициализировать состояние видимости и включения создаваемого объекта.

#### `override void WriteValue(JsonWriter writer, PenSettings value)`

```csharp
override void WriteValue (JsonWriter writer, PenSettings value)
```

**Аннотация:**
*   `override void`: `override` указывает на переопределение, `void` означает, что функция не возвращает значения.
*   `WriteValue`: Название функции, подразумевает запись значения в JSON.
*   `(JsonWriter writer, PenSettings value)`: Параметры функции:
    *   `JsonWriter writer`: Объект типа `JsonWriter`, который отвечает за запись данных в формате JSON.
    *   `PenSettings value`: Объект типа `PenSettings`, содержащий настройки пера/отображения, которые нужно записать в JSON.
*   **Назначение:** Функция `WriteValue` берет объект `PenSettings` и записывает его свойства в JSON формат, используя предоставленный `JsonWriter`. Это процесс сериализации настроек в JSON.

#### `override FilterRenderPen ReadFromObject(JsonReader reader, FilterRenderPen filter)`

```csharp
override FilterRenderPen ReadFromObject (JsonReader reader, FilterRenderPen filter)
```

**Аннотация:**
*   `override FilterRenderPen`: `override` указывает на переопределение, `FilterRenderPen` - тип возвращаемого значения. Функция читает данные из JSON и возвращает объект `FilterRenderPen`.
*   `ReadFromObject`: Название функции, подразумевает чтение данных из JSON объекта.
*   `(JsonReader reader, FilterRenderPen filter)`: Параметры функции:
    *   `JsonReader reader`: Объект типа `JsonReader`, который отвечает за чтение данных из JSON формата.
    *   `FilterRenderPen filter`:  Существующий объект `FilterRenderPen`. Функция, вероятно, будет обновлять свойства этого объекта данными, прочитанными из JSON.
*   **Назначение:** Функция `ReadFromObject` читает данные из JSON формата, используя `JsonReader`, и обновляет свойства существующего объекта `FilterRenderPen` этими данными. Это процесс десериализации настроек из JSON в объект.

#### `override PenSettings ReadValue(JsonReader reader)`

```csharp
override PenSettings ReadValue (JsonReader reader)
```

**Аннотация:**
*   `override PenSettings`: `override` указывает на переопределение, `PenSettings` - тип возвращаемого значения. Функция читает данные из JSON и возвращает объект `PenSettings`.
*   `ReadValue`: Название функции, подразумевает чтение значения из JSON.
*   `(JsonReader reader)`: Параметр функции:
    *   `JsonReader reader`: Объект типа `JsonReader`, который отвечает за чтение данных из JSON формата.
*   **Назначение:** Функция `ReadValue` читает данные из JSON формата, используя `JsonReader`, и создает и возвращает новый объект `PenSettings`, инициализированный данными из JSON. Это также процесс десериализации, но, возможно, создает новый объект, а не обновляет существующий.

### Properties (Свойства)

#### `string ColorPropertyName = nameof(PenSettings.Color).ToLower() [get]`

```csharp
string ColorPropertyName = nameof(PenSettings.Color).ToLower() [get]
```

**Аннотация:**
*   `string ColorPropertyName`: Объявление свойства с именем `ColorPropertyName` и типом `string`. Это свойство будет хранить имя свойства `Color` из класса `PenSettings` в нижнем регистре.
*   `= nameof(PenSettings.Color)`:  Оператор `nameof` возвращает строковое представление имени указанного элемента кода. В данном случае, он возвращает строку `"Color"`.
*   `.ToLower()`:  Метод `ToLower()` преобразует строку в нижний регистр. В результате, `ColorPropertyName` будет содержать строку `"color"`.
*   `[get]`:  Указывает, что свойство имеет только `get` аксессор, то есть его можно только читать, но не устанавливать напрямую извне класса.
*   **Назначение:**  `ColorPropertyName` хранит строковое имя свойства цвета (`"color"`) из класса `PenSettings`. Это имя используется для сериализации и десериализации свойства цвета в JSON.

#### `string LineStylePropertyName = nameof(PenSettings.LineDashStyle).ToLower() [get]`

```csharp
string LineStylePropertyName = nameof(PenSettings.LineDashStyle).ToLower() [get]
```

**Аннотация:**
*   `string LineStylePropertyName`: Объявление свойства с именем `LineStylePropertyName` и типом `string`.
*   `= nameof(PenSettings.LineDashStyle)`: Оператор `nameof` возвращает строку `"LineDashStyle"`.
*   `.ToLower()`: Метод `ToLower()` преобразует строку в `"linedashstyle"`.
*   `[get]`: Только `get` аксессор, свойство доступно только для чтения.
*   **Назначение:** `LineStylePropertyName` хранит строковое имя свойства стиля линии (`"linedashstyle"`) из класса `PenSettings`. Используется для JSON сериализации/десериализации стиля линии.

#### `string WidthPropertyName = nameof(PenSettings.Width).ToLower() [get]`

```csharp
string WidthPropertyName = nameof(PenSettings.Width).ToLower() [get]
```

**Аннотация:**
*   `string WidthPropertyName`: Объявление свойства с именем `WidthPropertyName` и типом `string`.
*   `= nameof(PenSettings.Width)`: Оператор `nameof` возвращает строку `"Width"`.
*   `.ToLower()`: Метод `ToLower()` преобразует строку в `"width"`.
*   `[get]`: Только `get` аксессор, свойство доступно только для чтения.
*   **Назначение:** `WidthPropertyName` хранит строковое имя свойства толщины линии (`"width"`) из класса `PenSettings`. Используется для JSON сериализации/десериализации толщины линии.

### Дополнительная информация

*   **Файл:** `FilterRenderPenJsonConverter.cs` - Исходный код класса `FilterRenderPenJsonConverter` находится в файле `FilterRenderPenJsonConverter.cs`.

### Выводы для разработки индикаторов ATAS

1.  **Сериализация настроек:** Класс `FilterRenderPenJsonConverter` позволяет сохранять визуальные настройки индикаторов (цвет, стиль линии, толщину) в JSON формате. Это полезно для сохранения настроек между сессиями ATAS или для обмена настройками.
2.  **Десериализация настроек:**  Также класс позволяет восстанавливать настройки из JSON, загружая ранее сохраненные визуальные параметры.
3.  **Использование свойств:** Свойства `ColorPropertyName`, `LineStylePropertyName`, `WidthPropertyName` определяют, как именно имена свойств цвета, стиля линии и толщины будут представлены в JSON. Важно использовать эти свойства при работе с JSON сериализацией/десериализацией настроек `PenSettings`.
4.  **Наследование и переопределение:**  Класс `FilterRenderPenJsonConverter` построен на основе механизма наследования и переопределения методов базового класса `FilterJsonConverterBase`. При создании собственных JSON конвертеров для настроек индикаторов можно использовать аналогичный подход.

**Для дальнейшего изучения:**

*   Изучить класс `FilterJsonConverterBase` и его функциональность.
*   Рассмотреть класс `PenSettings` и его свойства, чтобы понять, какие именно визуальные настройки он представляет.
*   Понять, как использовать `JsonWriter` и `JsonReader` для работы с JSON в ATAS.
*   Разобраться с процессом сериализации и десериализации объектов в C# в контексте ATAS индикаторов.






## Методичка по классу `ATAS.Indicators.Filters.Converters.FilterStringJsonConverter`

**Название класса:** `ATAS.Indicators.Filters.Converters.FilterStringJsonConverter`

**Описание:**

Этот класс предназначен для преобразования (конвертации) строковых фильтров (`FilterString`) в формат JSON и обратно, вероятно, для использования в индикаторах платформы ATAS.

**Диаграмма наследования:**

```
FilterJsonConverterBase<FilterString, string>
    ↑
ATAS.Indicators.Filters.Converters.FilterStringJsonConverter
```

*   `ATAS.Indicators.Filters.Converters.FilterStringJsonConverter` является наследником класса `FilterJsonConverterBase`, который, вероятно, предоставляет базовую функциональность для конвертации JSON.
*   `<FilterString, string>` указывает на то, что `FilterJsonConverterBase` параметризован типами `FilterString` (тип фильтра) и `string` (вероятно, строковое представление JSON).

**Диаграмма взаимодействия:**

[Диаграмма взаимодействия аналогична диаграмме наследования, просто визуализирует отношения между классами.]

**Защищенные члены-функции (Protected Member Functions):**

*   **`override FilterString Create(bool enabledVisible, bool enabled)`**

    *   **`override`**: Ключевое слово `override` означает, что этот метод переопределяет метод `Create` из базового класса `FilterJsonConverterBase`. Это позволяет классу `FilterStringJsonConverter` предоставлять свою собственную реализацию создания `FilterString`.
    *   **`FilterString`**:  Тип возвращаемого значения метода. Метод `Create` возвращает объект типа `FilterString`.
    *   **`Create`**: Название метода. Предположительно, этот метод создает и возвращает экземпляр `FilterString`.
    *   **(bool enabledVisible, bool enabled)`**:  Список параметров метода:
        *   `bool enabledVisible`:  Параметр типа `bool` (логическое значение). Название `enabledVisible` предполагает, что этот параметр управляет видимостью или активностью фильтра.
        *   `bool enabled`:  Еще один параметр типа `bool`. Название `enabled` также указывает на управление активностью или включением фильтра. Возможно, разница между `enabledVisible` и `enabled` заключается в разных аспектах "включения" фильтра (например, видимость на графике vs. активное применение фильтрации данных).
    *   **`protected virtual`**:
        *   `protected`:  Модификатор доступа `protected` означает, что метод `Create` может быть вызван изнутри класса `FilterStringJsonConverter`, из классов-наследников и из классов, находящихся в той же сборке (assembly).
        *   `virtual`: Модификатор `virtual` указывает, что метод `Create` может быть переопределен в классах-наследниках `FilterStringJsonConverter`.

**Реализует интерфейс:**

*   `Implements ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterString, string >.`

    *   Это подтверждает, что класс `FilterStringJsonConverter` реализует интерфейс или является наследником `FilterJsonConverterBase<FilterString, string >` и обеспечивает конкретную реализацию метода `Create`, определенного в базовом классе.

**Файл документации:**

*   `FilterStringJsonConverter.cs`

    *   Указывает на имя файла с исходным кодом, где определен класс `FilterStringJsonConverter`.

**Унаследованные члены:**

*   **Protected Member Functions inherited from `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterString, string >`**:  Указывает на наличие защищенных методов, унаследованных от базового класса. Детали этих методов не приведены в данном фрагменте документации.
*   **Public Member Functions inherited from `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterString, string >`**:  Указывает на наличие публичных методов, унаследованных от базового класса. Детали этих методов не приведены.
*   **Properties inherited from `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterString, string >`**: Указывает на наличие свойств, унаследованных от базового класса. Детали этих свойств не приведены.

**Заключение:**

Класс `ATAS.Indicators.Filters.Converters.FilterStringJsonConverter` отвечает за создание и управление строковыми фильтрами в контексте индикаторов ATAS, используя механизм JSON-конвертации, унаследованный от `FilterJsonConverterBase`.  Метод `Create` является ключевым для создания экземпляров `FilterString` и принимает параметры, контролирующие их активность и видимость. Для полного понимания функциональности класса необходимо изучить документацию и исходный код `FilterJsonConverterBase` и, возможно, других связанных классов.






## Методичка для создания индикаторов ATAS

### Класс `ATAS.Indicators.Filters.Converters.FilterTimeSpanJsonConvert`

**Описание:**  Класс `ATAS.Indicators.Filters.Converters.FilterTimeSpanJsonConvert` является частью пространства имен ATAS Indicators и предназначен для работы с фильтрами времени (TimeSpan) в формате JSON.

**Иерархия наследования:**

```
FilterJsonConverterBase<FilterTimeSpan, TimeSpan>
    ↑
    ATAS.Indicators.Filters.Converters.FilterTimeSpanJsonConvert
```

*   `FilterJsonConverterBase<FilterTimeSpan, TimeSpan>` - Базовый класс конвертеров JSON для фильтров времени.
*   `ATAS.Indicators.Filters.Converters.FilterTimeSpanJsonConvert` - Класс, реализующий конвертацию FilterTimeSpan в JSON.

**Диаграмма взаимодействия:**

```
FilterJsonConverterBase<FilterTimeSpan, TimeSpan>
    ↑
    ATAS.Indicators.Filters.Converters.FilterTimeSpanJsonConvert
```

**Защищенные члены класса (Protected Member Functions):**

*   `override FilterTimeSpan Create(bool enabledVisible, bool enabled)`
    *   **Описание:**  Переопределенный метод `Create` для создания экземпляра `FilterTimeSpan`.
    *   **Параметры:**
        *   `bool enabledVisible` -  Флаг, определяющий видимость (enabled and visible).
        *   `bool enabled` - Флаг, определяющий активность (enabled).
    *   **Реализация:**  Реализует интерфейс из `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterTimeSpan, TimeSpan>`.

**Унаследованные защищенные члены класса (Protected Member Functions inherited from):**

*   `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterTimeSpan, TimeSpan>`
    *   *Перечень унаследованных защищенных членов класса из базового класса `FilterJsonConverterBase<FilterTimeSpan, TimeSpan>`.* (Детализация членов базового класса не представлена в данном документе)

**Унаследованные публичные члены класса (Public Member Functions inherited from):**

*   `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterTimeSpan, TimeSpan>`
    *   *Перечень унаследованных публичных членов класса из базового класса `FilterJsonConverterBase<FilterTimeSpan, TimeSpan>`.* (Детализация членов базового класса не представлена в данном документе)

**Унаследованные свойства (Properties inherited from):**

*   `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterTimeSpan, TimeSpan>`
    *   *Перечень унаследованных свойств из базового класса `FilterJsonConverterBase<FilterTimeSpan, TimeSpan>`.* (Детализация свойств базового класса не представлена в данном документе)

**Детальная документация для члена класса `Create()`:**

```csharp
protected virtual override FilterTimeSpan Create (
    bool enabledVisible,
    bool enabled
)
```

*   **Пространство имен:** `ATAS.Indicators.Filters.Converters`
*   **Класс:** `ATAS.Indicators.Filters.Converters.FilterTimeSpanJsonConvert`
*   **Метод:** `Create`
*   **Модификаторы доступа:** `protected`, `virtual`, `override`
*   **Возвращаемый тип:** `FilterTimeSpan`
*   **Параметры:**
    *   `enabledVisible` (`bool`)
    *   `enabled` (`bool`)
*   **Реализация интерфейса:** `ATAS.Indicators.Filters.Converters.FilterJsonConverterBase<FilterTimeSpan, TimeSpan>`

**Файл с исходным кодом:**

*   `FilterTimeSpanJsonConvert.cs`





## Методичка по классу ATAS.Indicators.Filters.TrackedPropertyBase

**Обзор класса:**

`ATAS.Indicators.Filters.TrackedPropertyBase` - это базовый класс для отслеживания изменений свойств и уведомления подписчиков при изменении значения свойства.

**Диаграмма наследования:**

* `INotifyPropertyChanged`
* `ATAS.Indicators.Filters.TrackedPropertyBase`
* `ATAS.Indicators.ChartObject`
* `ATAS.Indicators.BaseIndicator`
* `ATAS.Indicators.ExtendedIndicator`
* `ATAS.Indicators.Indicator`
* `ATAS.Strategies.Chart.ChartStrategy`
* `ATAS.Strategies.Chart.SmaChartStrategy`

**Диаграмма взаимодействия:**

* `INotifyPropertyChanged`
* `ATAS.Indicators.Filters.TrackedPropertyBase`

**Защищенные функции-члены:**

1.  **`SetProperty<TProperty>()`**

    *   **Описание:** Устанавливает значение свойства и уведомляет подписчиков, если значение изменилось.

    *   **Синтаксис:**
        ```csharp
        void SetProperty<TProperty> (ref TProperty store, TProperty value, Action onChanged=null, Func<TProperty, bool> onChanging=null, [CallerMemberName] string propertyName="")
        ```

    *   **Параметры шаблона:**
        *   `TProperty`: Тип свойства.

    *   **Параметры:**
        *   `store`:  Ссылка на поле хранения свойства (backing field).
        *   `value`: Новое значение для установки.
        *   `onChanged`:  Необязательное действие, выполняемое после изменения значения свойства.
        *   `onChanging`: Необязательная функция, выполняемая перед изменением значения свойства, чтобы определить, можно ли изменить значение.
        *   `propertyName`: Имя свойства (автоматически заполняется с помощью `[CallerMemberName]`).

2.  **`SetTrackedProperty<TProperty>()`**

    *   **Описание:** Устанавливает значение свойства, которое реализует интерфейс `INotifyPropertyChanged`, и уведомляет подписчиков, если значение изменилось.

    *   **Синтаксис:**
        ```csharp
        void SetTrackedProperty<TProperty> (ref TProperty store, TProperty value, Action<string> onChanged=null, [CallerMemberName] string propertyName="")
        ```

    *   **Параметры шаблона:**
        *   `TProperty`: Тип свойства, реализующего интерфейс `INotifyPropertyChanged`.

    *   **Параметры:**
        *   `store`:  Ссылка на поле хранения свойства (backing field).
        *   `value`: Новое значение для установки.
        *   `onChanged`:  Необязательное действие, выполняемое после изменения значения свойства.
        *   `propertyName`: Имя свойства (автоматически заполняется с помощью `[CallerMemberName]`).

    *   **Ограничения типа:**
        *   `TProperty : class`
        *   `TProperty : INotifyPropertyChanged`

3.  **`OnChangeProperty()`**

    *   **Описание:** Уведомляет подписчиков, когда значение свойства изменяется.

    *   **Синтаксис:**
        ```csharp
        virtual void OnChangeProperty ([CallerMemberName] string propertyName="")
        ```

    *   **Параметры:**
        *   `propertyName`: Имя свойства, значение которого изменилось (автоматически заполняется с помощью `[CallerMemberName]`).

**События:**

1.  **`PropertyChanged`**

    *   **Тип обработчика события:** `PropertyChangedEventHandler`

    *   **Описание:** Событие возникает, когда свойство изменяется.

**Подробное описание:**

`ATAS.Indicators.Filters.TrackedPropertyBase` является базовым классом для отслеживания изменений свойств и уведомления подписчиков при изменении значения свойства.  Он предоставляет механизмы для установки значений свойств (`SetProperty`, `SetTrackedProperty`) и автоматического уведомления об изменениях через событие `PropertyChanged` и виртуальный метод `OnChangeProperty`. Класс разработан для упрощения управления свойствами индикаторов и обеспечения реактивности на их изменения.

**Файл расположения:**

*   `TrackedPropertyBase.cs`






## Методичка по классу `ATAS.Indicators.BaseDataSeries<T>` для создания индикаторов ATAS

### 1. Введение

Класс `ATAS.Indicators.BaseDataSeries<T>` является базовым универсальным классом серий данных, предоставляющим общую функциональность для индикаторов ATAS. Он служит основой для создания различных типов серий данных, используемых в платформе ATAS. Класс является шаблонным, где `T` определяет тип данных, хранящихся в серии.

### 2. Иерархия наследования и взаимодействия

**Иерархия наследования:**

`ATAS.Indicators.BaseDataSeries<T>` наследуется от следующих интерфейсов и классов:

- `INotifyPropertyChanged`
- `INotifyPanelPropertyChanged`
- `IDataSeries`
- `IDataSeries<T>`

**Диаграмма взаимодействия:**

`ATAS.Indicators.BaseDataSeries<T>` взаимодействует со следующими интерфейсами:

- `INotifyPropertyChanged`
- `INotifyPanelPropertyChanged`
- `IDataSeries`
- `IDataSeries<T>`

### 3. Публичные члены класса

#### Публичные функции-члены

- `virtual void Clear ()`
    - Очищает все элементы из серии данных.

- `override string ToString ()`
    - Возвращает строковое представление текущего объекта `BaseDataSeries<T>`.

- `void Clear ()`
    - Очищает все элементы из серии данных.

### 4. Защищенные члены класса

#### Защищенные функции-члены

- `void RaiseChanged (int bar)`
    -  Метод для уведомления об изменении данных в серии на определенном баре.

- `virtual void RaisePropertyChanged (string propertyName)`
    - Метод для уведомления об изменении свойства.

- `virtual void RaisePanelPropertyChanged (string propertyName)`
    - Метод для уведомления об изменении свойства панели.

#### Конструкторы

- `BaseDataSeries (string id, DataSeriesType type)`
    - Конструктор для создания экземпляра `BaseDataSeries<T>` с указанием идентификатора (`id`) и типа данных (`type`).

- `BaseDataSeries (string id, string name, DataSeriesType type)`
    - Конструктор для создания экземпляра `BaseDataSeries<T>` с указанием идентификатора (`id`), имени (`name`) и типа данных (`type`).

- `BaseDataSeries (DataSeriesType type)`
    - Конструктор для создания экземпляра `BaseDataSeries<T>` с указанием только типа данных (`type`).

### 5. Свойства класса

#### Свойства

- `bool DrawAbovePrice  [get, set]`
    - Определяет, должна ли серия данных отображаться над ценовыми свечами на графике.

- `bool IgnoredByAlerts  [get, set]`
    - Определяет, должна ли серия данных игнорироваться системой оповещений.

- `string Id  [get, set]`
    - Уникальный и постоянный идентификатор серии данных для сериализации данных.

- `string RenderId  [get]`
    - Уникальный идентификатор серии для всех панелей и индикаторов.

- `virtual bool IsVisible  [get]`
    - Определяет, должна ли отображаться серия данных.

- `DataSeriesType Type  [get]`
    - Возвращает тип данных серии из перечисления `DataSeriesType`.

- `string Name  [get, set]`
    - Определяет имя серии данных.

- `string DescriptionKey  [get, set]`
    - Определяет ключ описания серии данных.

- `abstract int Count  [get]`
    - Возвращает количество элементов в серии данных.

- `abstract T this [int index]  [get, set]`
    - Позволяет получить или установить элемент серии данных по указанному индексу.

- `bool IsHidden  [get, set]`
    - Определяет, должны ли свойства серии данных быть скрыты в окне настроек.

- `bool ShowTooltip  [get, set]`
    - Определяет, должна ли отображаться подсказка для серии данных. По умолчанию `true`.

- `bool UseMinimizedModeIfEnabled  [get, set]`
    - Определяет, должен ли использоваться минимизированный режим, если он включен.

- `bool ResetAlertsOnNewBar  [get, set]`
    - Определяет, должны ли сбрасываться оповещения при появлении нового бара.

- `bool ShowNameOnMouseOver  [get, set]`
    - Определяет, должно ли отображаться имя серии данных при наведении курсора мыши. По умолчанию `true`.

### 6. События класса

#### События

- `Action<int> Changed`
    - Событие, возникающее при изменении данных в серии.

- `PropertyChangedEventHandler PropertyChanged`
    - Событие, возникающее при изменении свойства класса.

- `PropertyChangedEventHandler PanelPropertyChanged`
    - Событие, возникающее при изменении свойства панели.

### 7. Подробное описание

`ATAS.Indicators.BaseDataSeries<T>` — это базовый класс для создания различных серий данных в ATAS. Он предоставляет общую функциональность, такую как управление элементами данных, свойства отображения и уведомления об изменениях.

#### Шаблонные параметры

- `T`
    - Тип данных серии. Определяет тип данных, которые будут храниться в экземпляре `BaseDataSeries<T>`.







## Методичка по классу `ATAS.Indicators.BaseIndicator` для создания индикаторов ATAS

### 1. Иерархия наследования

```
INotifyPropertyChanged
    TrackedPropertyBase
        ChartObject
            IDisposable
            INotifyPanelPropertyChanged
                ATAS.Indicators.BaseIndicator
                    ATAS.Indicators.ExtendedIndicator
                        ATAS.Indicators.Indicator
                            ATAS.Strategies.Chart.ChartStrategy
                                ATAS.Strategies.Chart.SmaChartStrategy
```

**Аннотация:** `ATAS.Indicators.BaseIndicator` является базовым классом для создания пользовательских индикаторов. Он наследуется от ряда классов, предоставляющих ему функциональность для работы с графиками, свойствами и событиями.  Эта иерархия показывает, что `BaseIndicator` является частью более широкой системы индикаторов и стратегий в ATAS.

### 2. Диаграмма взаимодействия

```
INotifyPropertyChanged
    TrackedPropertyBase
        ChartObject
            IDisposable
            INotifyPanelPropertyChanged
                ATAS.Indicators.BaseIndicator
```

**Аннотация:** Диаграмма взаимодействия показывает прямые зависимости класса `ATAS.Indicators.BaseIndicator` от интерфейсов и базовых классов, необходимых для его функционирования.

### 3. Открытые методы (Public Member Functions)

*   `virtual void Dispose()`
    **Аннотация:** Метод `Dispose` используется для освобождения ресурсов, занимаемых индикатором, когда он больше не нужен. `virtual` означает, что метод может быть переопределен в классах-наследниках.

*   `override string ToString()`
    Возвращает строковое представление текущего экземпляра индикатора.
    **Аннотация:** Метод `ToString` преобразует объект индикатора в строку, которая может быть использована для отображения или логирования. `override` указывает, что этот метод переопределяет метод `ToString` из базового класса.

### 4. Защищенные методы (Protected Member Functions)

*   `BaseIndicator(bool useCandles = false)`
    Конструктор класса `BaseIndicator`. `useCandles` определяет, использовать ли свечные данные.
    **Аннотация:** Конструктор — это специальный метод, который вызывается при создании нового объекта класса `BaseIndicator`. Параметр `useCandles` позволяет указать, будет ли индикатор использовать свечные данные (цены открытия, закрытия, максимума, минимума). `protected` означает, что конструктор доступен для классов-наследников и внутри текущей сборки.

*   `virtual void RecalculateValues()`
    Пересчитывает значения индикатора на каждом баре.
    **Аннотация:** Метод `RecalculateValues` отвечает за пересчет значений индикатора при изменении данных или параметров. Вызывается для каждого бара графика.

*   `virtual void OnInitialize()`
    Метод выполняется перед первым расчетом.
    **Аннотация:** Метод `OnInitialize` вызывается один раз при первом запуске индикатора, до начала каких-либо расчетов. Используется для инициализации переменных или настроек.

*   `virtual void OnRecalculate()`
    Метод выполняется перед новым расчетом.
    **Аннотация:** Метод `OnRecalculate` вызывается перед каждым новым расчетом индикатора, например, при поступлении нового тика или изменении таймфрейма.

*   `virtual void OnFinishRecalculate()`
    Метод выполняется после завершения расчета.
    **Аннотация:** Метод `OnFinishRecalculate` вызывается после завершения всех расчетов индикатора.

*   `virtual void Calculate(int bar, decimal value)`
    Выполняет расчет индикатора для указанного бара и значения.
    **Аннотация:** Метод `Calculate` является основной точкой входа для расчета значения индикатора для конкретного бара (`bar`) и входного значения (`value`). `virtual` позволяет классам-наследникам переопределить логику расчета.

*   `abstract void OnCalculate(int bar, decimal value)`
    **Аннотация:** `abstract` метод `OnCalculate` должен быть реализован в классах-наследниках `BaseIndicator`. Это основной метод для вычисления значения индикатора для каждого бара. Он вызывается для каждого бара на истории, а затем на каждом тике.

*   `void Add(Indicator indicator)`
    Добавляет индикатор в список используемых индикаторов текущим индикатором.
    **Аннотация:** Метод `Add` позволяет добавлять другие индикаторы в список зависимостей текущего индикатора. Это может быть полезно, если ваш индикатор использует значения других индикаторов для своих расчетов.

*   `void Clear()`
    Очищает все серии данных.
    **Аннотация:** Метод `Clear` удаляет все накопленные данные индикатора, например, при изменении исходных данных.

*   `virtual void OnSourceChanged()`
    Метод вызывается при изменении свойства `SourceDataSeries`.
    **Аннотация:** Метод `OnSourceChanged` вызывается, когда изменяется источник данных для индикатора (`SourceDataSeries`). Это позволяет индикатору пересчитать свои значения на основе новых данных.

*   `void RaisePropertyChanged(string propertyName)`
    Вызывает событие `PropertyChanged` для указанного имени свойства.
    **Аннотация:** Метод `RaisePropertyChanged` используется для уведомления системы о том, что свойство индикатора с именем `propertyName` было изменено. Это часть механизма уведомления об изменениях свойств.

*   `void RaisePropertyChanged(object sender, PropertyChangedEventArgs e)`
    Вызывает событие `PropertyChanged` с указанными аргументами события.
    **Аннотация:** Перегрузка метода `RaisePropertyChanged`, позволяющая передавать более подробную информацию об изменении свойства через аргументы события `PropertyChangedEventArgs`.

*   `void RaisePanelPropertyChanged(string name)`
    Вызывает событие `PanelPropertyChanged` с указанным именем свойства.
    **Аннотация:** Аналогично `RaisePropertyChanged`, но вызывает событие `PanelPropertyChanged`, которое может быть связано с изменениями в панели индикатора.

*   `void RaiseBarValueChanged(int bar)`
    Вызывает событие `BarValueChanged` с указанным номером бара.
    **Аннотация:** Метод `RaiseBarValueChanged` уведомляет о том, что значение индикатора для определенного бара (`bar`) было изменено.

*   `virtual void OnDispose()`
    Вызывается, когда индикатор удаляется.
    **Аннотация:** Метод `OnDispose` вызывается при удалении индикатора с графика. Используется для освобождения ресурсов, которые не были освобождены методом `Dispose()`.

*   `override void OnVisibleChanged()`
    Вызывается при изменении свойства `Visible`.
    **Аннотация:** Метод `OnVisibleChanged` вызывается, когда изменяется видимость индикатора (свойство `Visible`).

### 5. Статические защищенные методы (Static Protected Member Functions)

*   `static PerfCounter MeasurePerformance(string name)`
    Измеряет производительность определенной операции с заданным именем.
    **Аннотация:** Статический метод `MeasurePerformance` используется для измерения времени выполнения определенных операций внутри индикатора.  `static` означает, что метод принадлежит классу `BaseIndicator` в целом, а не конкретному объекту. `PerfCounter` вероятно, класс для измерения производительности.

### 6. Защищенные атрибуты (Protected Attributes)

*   `readonly List<Indicator> UsedIndicators = new()`
    Список индикаторов, используемых текущим индикатором.
    **Аннотация:** `UsedIndicators` - это список, содержащий индикаторы, от которых зависит текущий индикатор. `readonly` означает, что список может быть инициализирован только в конструкторе класса и доступен только для чтения после этого.

### 7. Свойства (Properties)

*   `static PerformanceDiagnoser? PerformanceDiagnoser { get; }`
    Отслеживание производительности индикатора.
    **Аннотация:**  Статическое свойство `PerformanceDiagnoser` предоставляет доступ к объекту, который отслеживает производительность индикаторов. `static` означает, что это свойство общее для всех экземпляров `BaseIndicator`. `?` означает, что значение может быть `null`.

*   `static bool UseProfiling { get; set; }`
    Устанавливает в `true` для измерения производительности всех индикаторов.
    **Аннотация:** Статическое свойство `UseProfiling` включает или отключает профилирование производительности для всех индикаторов.

*   `string Name { get; set; }`
    Имя индикатора.
    **Аннотация:** Свойство `Name` позволяет получить или установить имя индикатора, которое отображается в интерфейсе ATAS.

*   `bool IsDisposed { get; set; }`
    Возвращает или устанавливает значение, указывающее, был ли объект индикатора удален.
    **Аннотация:** Свойство `IsDisposed` показывает, был ли вызван метод `Dispose` для индикатора, сигнализируя о том, что он был освобожден.

*   `List<IDataSeries> DataSeries { get; }`
    Список серий данных, используемых индикатором.
    **Аннотация:** Свойство `DataSeries` возвращает список серий данных, которые индикатор использует для своих расчетов и отображения. `IDataSeries` вероятно, интерфейс для работы с данными на графике.

*   `bool SupportsExtendedSeries { get; }`
    Возвращает значение, указывающее, могут ли серии данных быть выведены за пределы баров графика.
    **Аннотация:** Свойство `SupportsExtendedSeries` указывает, может ли индикатор рисовать линии или другие элементы за пределами стандартных баров графика.

*   `List<LineSeries> LineSeries { get; }`
    Список серий линий, используемых индикатором.
    **Аннотация:** Свойство `LineSeries` предоставляет доступ к списку объектов `LineSeries`, которые индикатор использует для отображения линий на графике.

*   `string Panel { get; set; }`
    Имя панели, где размещен индикатор.
    **Аннотация:** Свойство `Panel` позволяет получить или установить имя панели графика, на которой отображается индикатор.

*   `bool IsVerticalIndicator { get; set; }`
    Возвращает или устанавливает значение, указывающее, предназначен ли индикатор для отображения в виде вертикального индикатора.
    **Аннотация:** Свойство `IsVerticalIndicator` указывает, предназначен ли индикатор для отображения в отдельной вертикальной панели под ценовым графиком.

*   `bool UseCandles { get; }`
    Возвращает значение, указывающее, использует ли индикатор свечные данные.
    **Аннотация:** Свойство `UseCandles` показывает, использует ли индикатор свечные данные (Open, High, Low, Close).

*   `int CurrentBar { get; }`
    Номер бара. Все бары и значения соответствующих серий данных имеют серийный номер. Самый ранний бар графика имеет номер 0; следующий бар имеет номер 1 и так далее.
    **Аннотация:** Свойство `CurrentBar` возвращает текущий номер бара, обрабатываемого индикатором. Нумерация баров начинается с 0 для самого старого бара на графике.

*   `IDataSeries<decimal>? SourceDataSeries { get; set; }`
    Возвращает или устанавливает серию данных, используемую в качестве источника для расчетов индикатора.
    **Аннотация:** Свойство `SourceDataSeries` является ключевым свойством, определяющим источник данных для индикатора (например, цена, объем). Установка этого свойства приводит к пересчету индикатора. `IDataSeries<decimal>?` указывает на серию данных, содержащую десятичные значения, и может быть `null` если источник данных не задан.

*   `decimal this[int index] { get; protected set; }`
    Возвращает или устанавливает значение первой серии данных индикатора по указанному индексу.
    **Аннотация:** Индексатор `this[int index]` позволяет получить доступ к значениям первой серии данных индикатора по индексу бара (`index`). `protected set` означает, что устанавливать значение можно только внутри класса или его наследников.

### 8. События (Events)

*   `PropertyChangedEventHandler PropertyChanged`
    **Аннотация:** Событие `PropertyChanged` возникает при изменении любого свойства индикатора. `PropertyChangedEventHandler` - тип делегата для обработчиков этого события.

*   `PropertyChangedEventHandler PanelPropertyChanged`
    **Аннотация:** Событие `PanelPropertyChanged` возникает при изменении свойств панели индикатора.

*   `Action<int> BarValueChanged`
    Событие, которое возникает при изменении значения бара в индикаторе.
    **Аннотация:** Событие `BarValueChanged` возникает, когда значение индикатора для определенного бара изменяется. `Action<int>` указывает, что обработчик события принимает целочисленный аргумент (номер бара).

### 9. Подробное описание (Detailed Description)

Базовый класс для пользовательских индикаторов на графике.

### 10. Конструктор и деструктор (Constructor & Destructor Documentation)

*   `BaseIndicator()`
    `ATAS.Indicators.BaseIndicator.BaseIndicator(bool useCandles = false)` `protected`
    **Аннотация:** Конструктор класса `BaseIndicator`, уже описанный в разделе "Защищенные методы".

### 11. Документация методов-членов (Member Function Documentation)

*   `Add()`
    `void ATAS.Indicators.BaseIndicator.Add(Indicator indicator)` `protected`
    Добавляет индикатор в список используемых индикаторов этим индикатором.
    **Параметры:**
    *   `indicator` - Индикатор, который нужно добавить в список используемых индикаторов.

*   `Calculate()`
    `virtual void ATAS.Indicators.BaseIndicator.Calculate(int bar, decimal value)` `protected virtual`
    Выполняет расчет индикатора для указанного бара и значения.
    **Параметры:**
    *   `bar` - Номер бара, для которого выполняется расчет.
    *   `value` - Входное значение из серии данных для указанного бара.
    **Реализован в:** `ATAS.Indicators.ExtendedIndicator`.

*   `Clear()`
    `void ATAS.Indicators.BaseIndicator.Clear()` `protected`
    Очищает все серии данных.

*   `Dispose()`
    `virtual void ATAS.Indicators.BaseIndicator.Dispose()` `virtual`
    **Реализован в:** `ATAS.Indicators.ExtendedIndicator`.

*   `MeasurePerformance()`
    `static PerfCounter ATAS.Indicators.BaseIndicator.MeasurePerformance(string name)` `static protected`
    Измеряет производительность определенной операции с заданным именем.
    **Параметры:**
    *   `name` - Имя операции для измерения производительности.
    **Возвращает:**
    Счетчик производительности, представляющий измеренную производительность. Если недоступен диагностик производительности, будет возвращен счетчик производительности по умолчанию.

*   `OnCalculate()`
    `abstract void ATAS.Indicators.BaseIndicator.OnCalculate(int bar, decimal value)` `protected pure virtual`
    Основной метод расчета индикатора вызывается для каждого бара на истории, а затем на каждом тике.
    **Параметры:**
    *   `bar` - Номер бара.
    *   `value` - Входное значение серии данных.
    **Реализован в:** `ATAS.Strategies.Chart.SmaChartStrategy`.

*   `OnDispose()`
    `virtual void ATAS.Indicators.BaseIndicator.OnDispose()` `protected virtual`
    Вызывается, когда индикатор удаляется.

*   `OnFinishRecalculate()`
    `virtual void ATAS.Indicators.BaseIndicator.OnFinishRecalculate()` `protected virtual`
    Метод выполняется после завершения расчета.

*   `OnInitialize()`
    `virtual void ATAS.Indicators.BaseIndicator.OnInitialize()` `protected virtual`
    Метод выполняется перед первым расчетом.

*   `OnRecalculate()`
    `virtual void ATAS.Indicators.BaseIndicator.OnRecalculate()` `protected virtual`
    Метод выполняется перед новым расчетом.

*   `OnSourceChanged()`
    `virtual void ATAS.Indicators.BaseIndicator.OnSourceChanged()` `protected virtual`
    Этот метод вызывается при изменении свойства `SourceDataSeries`.

*   `OnVisibleChanged()`
    `override void ATAS.Indicators.BaseIndicator.OnVisibleChanged()` `protected virtual`
    Вызывается при изменении свойства `Visible`.
    **Реализован из:** `ATAS.Indicators.ChartObject`.

*   `RaiseBarValueChanged()`
    `void ATAS.Indicators.BaseIndicator.RaiseBarValueChanged(int bar)` `protected`
    Вызывает событие `BarValueChanged` с указанным номером бара.
    **Параметры:**
    *   `bar` - Значение бара для передачи обработчикам событий.

*   `RaisePanelPropertyChanged()`
    `void ATAS.Indicators.BaseIndicator.RaisePanelPropertyChanged(string name)` `protected`
    Вызывает событие `PanelPropertyChanged` с указанным именем свойства.
    **Параметры:**
    *   `name` - Имя измененного свойства.

*   `RaisePropertyChanged()` `[1/2]`
    `void ATAS.Indicators.BaseIndicator.RaisePropertyChanged(object sender, PropertyChangedEventArgs e)` `protected`
    Вызывает событие `PropertyChanged` с указанными аргументами события.
    **Параметры:**
    *   `sender` - Отправитель события.
    *   `e` - Аргументы события.

*   `RaisePropertyChanged()` `[2/2]`
    `void ATAS.Indicators.BaseIndicator.RaisePropertyChanged(string propertyName)` `protected`
    Вызывает событие `PropertyChanged` для указанного имени свойства.
    **Параметры:**
    *   `propertyName` - Имя измененного свойства.

*   `RecalculateValues()`
    `virtual void ATAS.Indicators.BaseIndicator.RecalculateValues()` `protected virtual`
    Пересчитывает значения индикатора на каждом баре.
    **Реализован в:** `ATAS.Indicators.ExtendedIndicator`.

*   `ToString()`
    `override string ATAS.Indicators.BaseIndicator.ToString()`
    Преобразует текущий экземпляр индикатора в его строковое представление.
    **Возвращает:**
    Строковое представление индикатора.

### 12. Документация данных-членов (Member Data Documentation)

*   `UsedIndicators`
    `readonly List<Indicator> ATAS.Indicators.BaseIndicator.UsedIndicators = new()` `protected`
    Список индикаторов, которые используются этим индикатором.

### 13. Документация свойств (Property Documentation)

*   `CurrentBar`
    `int ATAS.Indicators.BaseIndicator.CurrentBar` `get`
    Номер бара. Все бары и значения соответствующих серий данных имеют серийный номер. Самый ранний бар графика имеет номер 0; следующий бар имеет номер 1 и так далее.

*   `DataSeries`
    `List<IDataSeries> ATAS.Indicators.BaseIndicator.DataSeries` `get`
    Список серий данных, используемых индикатором.

*   `IsDisposed`
    `bool ATAS.Indicators.BaseIndicator.IsDisposed` `get set`
    Возвращает или устанавливает значение, указывающее, был ли объект индикатора удален.

*   `IsVerticalIndicator`
    `bool ATAS.Indicators.BaseIndicator.IsVerticalIndicator` `get set`
    Возвращает или устанавливает значение, указывающее, предназначен ли индикатор для отображения в виде вертикального индикатора.

*   `LineSeries`
    `List<LineSeries> ATAS.Indicators.BaseIndicator.LineSeries` `get`
    Список серий линий, используемых индикатором.

*   `Name`
    `string ATAS.Indicators.BaseIndicator.Name` `get set`
    Имя индикатора.

*   `Panel`
    `string ATAS.Indicators.BaseIndicator.Panel` `get set`
    Имя панели, где размещен индикатор.

*   `PerformanceDiagnoser`
    `? PerformanceDiagnoser ATAS.Indicators.BaseIndicator.PerformanceDiagnoser` `static get protected`
    Индикатор отслеживания производительности.

*   `SourceDataSeries`
    `IDataSeries<decimal>? ATAS.Indicators.BaseIndicator.SourceDataSeries` `get set`
    Возвращает или устанавливает серию данных, используемую в качестве источника для расчетов индикатора. Серия исходных данных предоставляет входные данные для расчетов индикатора. Когда это свойство установлено, индикатор автоматически пересчитает свои значения на основе новой серии исходных данных.

*   `SupportsExtendedSeries`
    `bool ATAS.Indicators.BaseIndicator.SupportsExtendedSeries` `get`
    Возвращает значение, указывающее, могут ли серии данных быть выведены за пределы баров графика.

*   `this[int index]`
    `decimal ATAS.Indicators.BaseIndicator.this[int index]` `get protected set`
    Возвращает или устанавливает значение первой серии данных индикатора по указанному индексу.
    **Параметры:**
    *   `index` - Индекс значения для получения или установки.
    **Возвращает:**
    Значение индикатора по указанному индексу.

*   `UseCandles`
    `bool ATAS.Indicators.BaseIndicator.UseCandles` `get`
    Возвращает значение, указывающее, использует ли индикатор свечные данные.

*   `UseProfiling`
    `bool ATAS.Indicators.BaseIndicator.UseProfiling` `static get set protected`
    Установите значение `true` для измерения производительности всех индикаторов.

### 14. Документация событий (Event Documentation)

*   `BarValueChanged`
    `Action<int> ATAS.Indicators.BaseIndicator.BarValueChanged`
    Событие, которое возникает при изменении значения бара в индикаторе.

*   `PanelPropertyChanged`
    `PropertyChangedEventHandler ATAS.Indicators.BaseIndicator.PanelPropertyChanged`

*   `PropertyChanged`
    `PropertyChangedEventHandler ATAS.Indicators.BaseIndicator.PropertyChanged`

### 15. Файл документации класса

Документация для этого класса была сгенерирована из файла:

*   `BaseIndicator.cs`







```методичка
## ATAS.Indicators.Candle Class Reference

**Описание:**
Представляет собой свечу в трейдинге, которая включает цены открытия, максимум, минимум и закрытия.

**Свойства:**

### Open
`decimal` `ATAS.Indicators.Candle.Open`  `[get, set]`
> Описание: Получает или устанавливает цену открытия свечи.
> Тип данных: `decimal`
> Атрибуты доступа: `get`, `set`
> Аннотация: Свойство `Open` типа `decimal` класса `ATAS.Indicators.Candle` используется для получения или установки цены открытия текущей свечи. Атрибуты `get` и `set` указывают на то, что свойство доступно для чтения и записи.

### High
`decimal` `ATAS.Indicators.Candle.High`  `[get, set]`
> Описание: Получает или устанавливает самую высокую цену, достигнутую в течение таймфрейма свечи.
> Тип данных: `decimal`
> Атрибуты доступа: `get`, `set`
> Аннотация: Свойство `High` типа `decimal` класса `ATAS.Indicators.Candle` используется для получения или установки максимальной цены свечи за выбранный период времени.  Атрибуты `get` и `set` указывают на возможность чтения и записи значения свойства.

### Low
`decimal` `ATAS.Indicators.Candle.Low`  `[get, set]`
> Описание: Получает или устанавливает самую низкую цену, достигнутую в течение таймфрейма свечи.
> Тип данных: `decimal`
> Атрибуты доступа: `get`, `set`
> Аннотация: Свойство `Low` типа `decimal` класса `ATAS.Indicators.Candle` предназначено для получения или установки минимальной цены свечи за период ее формирования. Доступно как для чтения (`get`), так и для записи (`set`).

### Close
`decimal` `ATAS.Indicators.Candle.Close`  `[get, set]`
> Описание: Получает или устанавливает цену закрытия свечи.
> Тип данных: `decimal`
> Атрибуты доступа: `get`, `set`
> Аннотация: Свойство `Close` типа `decimal` в классе `ATAS.Indicators.Candle` используется для доступа к цене закрытия свечи. Атрибуты `get` и `set` говорят о том, что можно как получить текущее значение цены закрытия, так и установить новое значение, если это необходимо.

**Подробное описание:**
Класс `ATAS.Indicators.Candle` представляет собой свечу в торговле и включает в себя цены открытия (Open), максимум (High), минимум (Low) и закрытия (Close).

**Файл документации для класса:**
`Candle.cs`
> Аннотация:  Документация для класса `ATAS.Indicators.Candle` была сгенерирована из файла `Candle.cs`. Это означает, что исходный код и, вероятно, более подробная информация о классе находятся в файле `Candle.cs`.
```






## Методичка для создания индикаторов ATAS на основе класса `CandleDataSeries`

### Введение

**Название класса:** `ATAS.Indicators.CandleDataSeries`

**Описание класса:**  Представляет собой серию данных свечей. Каждый элемент в серии является свечой.

**Файл:** `CandleDataSeries.cs`

### Иерархия наследования и взаимодействия

**Диаграмма наследования:**

```
BaseDataSeries< Candle >
    ↑
ATAS.Indicators.CandleDataSeries
```

*Аннотация: `CandleDataSeries` наследуется от `BaseDataSeries< Candle >`, что означает, что он обладает всеми свойствами и методами `BaseDataSeries` и расширяет их для работы с серией свечей.*

**Диаграмма взаимодействия:**

```
BaseDataSeries< Candle >
    ↑
ATAS.Indicators.CandleDataSeries
```

*Аннотация: Диаграмма взаимодействия показывает аналогичную структуру наследования, акцентируя внимание на взаимосвязи между классами.*

### Публичные члены класса

#### Публичные функции-члены (Конструкторы и методы)

1.  **`CandleDataSeries(string id, string name)` [1/2]**

    ```csharp
    ATAS.Indicators.CandleDataSeries.CandleDataSeries (string id, string name)
    ```

    **Описание:** Инициализирует новый экземпляр класса `CandleDataSeries` с указанным уникальным и постоянным идентификатором серии данных для сериализации данных и уникальным именем.

    **Параметры:**

    *   `id`:  Уникальный и постоянный идентификатор серии данных для сериализации.
    *   `name`: Уникальное имя серии данных.

    *Аннотация: Первый конструктор позволяет создать `CandleDataSeries` с заданным ID и именем. Имя используется для отображения в интерфейсе ATAS, а ID для внутренней идентификации и сохранения данных.*

2.  **`CandleDataSeries(string id)` [2/2]**

    ```csharp
    ATAS.Indicators.CandleDataSeries.CandleDataSeries (string id)
    ```

    **Описание:** Инициализирует новый экземпляр класса `CandleDataSeries` с указанным уникальным и постоянным идентификатором серии данных для сериализации.

    **Параметры:**

    *   `id`: Уникальный и постоянный идентификатор серии данных для сериализации.

    *Аннотация: Второй конструктор создает `CandleDataSeries` только с ID. Имя серии данных, вероятно, будет установлено по умолчанию или позже.*

3.  **`Clear()`**

    ```csharp
    override void Clear ()
    ```

    **Описание:**  Метод для очистки данных в серии свечей. Переопределяет метод из `ATAS.Indicators.BaseDataSeries< Candle >`.

    *Аннотация: Метод `Clear()` используется для удаления всех свечей из `CandleDataSeries`.  `override void` указывает, что метод переопределяет метод базового класса, а `virtual` (из базового класса, хотя здесь не указано явно) может означать, что метод может быть переопределен в производных классах.*

#### Свойства (Properties)

1.  **`BorderColor`**

    ```csharp
    CrossColor BorderColor [get, set]
    ```

    **Описание:**  Возвращает или устанавливает цвет границы элемента серии данных.

    *Аннотация: Свойство `BorderColor` типа `CrossColor` позволяет управлять цветом границы свечей в серии данных. `[get, set]` означает, что можно как получить текущий цвет, так и установить новый.*

2.  **`Count`**

    ```csharp
    override int Count [get]
    ```

    **Описание:** Возвращает количество элементов (свечей) в серии данных. Переопределяет метод из `ATAS.Indicators.BaseDataSeries< Candle >`.

    *Аннотация: Свойство `Count` типа `int` возвращает общее количество свечей, содержащихся в `CandleDataSeries`. `[get]` означает, что свойство доступно только для чтения.*

3.  **`Digits`**

    ```csharp
    int Digits [get, set]
    ```

    **Описание:**  Возвращает или устанавливает количество знаков после десятичной точки.

    *Аннотация: Свойство `Digits` типа `int` определяет точность отображения цен в серии данных, задавая количество знаков после запятой. `[get, set]` означает, что можно как получить, так и установить это значение.*

4.  **`DownCandleColor`**

    ```csharp
    CrossColor DownCandleColor [get, set]
    ```

    **Описание:** Возвращает или устанавливает цвет элемента серии данных на медвежьей (нисходящей) свече.

    *Аннотация: Свойство `DownCandleColor` типа `CrossColor` управляет цветом медвежьих свечей в серии. `[get, set]` позволяет получить и установить цвет.*

5.  **`IsVisible`**

    ```csharp
    override bool IsVisible [get]
    ```

    **Описание:** Возвращает значение, указывающее, видна ли серия данных на графике. Переопределяет метод из `ATAS.Indicators.BaseDataSeries< Candle >`.

    *Аннотация: Свойство `IsVisible` типа `bool` определяет, отображается ли серия данных на графике. `[get]` означает, что можно только узнать, видна ли серия, но для изменения видимости, вероятно, используется другой механизм (возможно, через базовый класс или другие свойства).  `override bool` указывает на переопределение метода базового класса.*

6.  **`Mode`**

    ```csharp
    CandleVisualMode Mode [get, set]
    ```

    **Описание:** Возвращает или устанавливает режим визуализации серии данных.

    *Аннотация: Свойство `Mode` типа `CandleVisualMode` определяет способ отображения свечей на графике (например, бары, свечи, линия и т.д.). `[get, set]` позволяет получить и установить режим визуализации.*

7.  **`ScaleIt`**

    ```csharp
    bool ScaleIt [get, set]
    ```

    **Описание:** Возвращает или устанавливает значение, указывающее, масштабировать ли серию данных на графике.

    *Аннотация: Свойство `ScaleIt` типа `bool` управляет масштабированием серии данных относительно ценовой шкалы графика. `[get, set]` позволяет включить или отключить масштабирование.*

8.  **`ShowCurrentValue`**

    ```csharp
    bool ShowCurrentValue [get, set]
    ```

    **Описание:** Возвращает или устанавливает значение, указывающее, отображать ли текущие значения на ценовой панели.

    *Аннотация: Свойство `ShowCurrentValue` типа `bool` определяет, будет ли отображаться текущая цена серии данных на панели цен. `[get, set]` позволяет включить или отключить отображение текущего значения.*

9.  **`StringFormat`**

    ```csharp
    string StringFormat [get, set]
    ```

    **Описание:** Возвращает или устанавливает формат строки цены, используемый для отображения значений.

    *Аннотация: Свойство `StringFormat` типа `string` позволяет задать формат отображения цен (например, количество знаков после запятой, разделители тысяч и т.д.). `[get, set]` позволяет получить и установить формат строки.*

10. **`this[int index]`**

    ```csharp
    override Candle this[int index] [get, set]
    ```

    **Описание:** Индексатор для доступа к элементу Candle по индексу. Переопределяет метод из `ATAS.Indicators.BaseDataSeries< Candle >`.

    *Аннотация: Индексатор `this[int index]` позволяет получать доступ к отдельным свечам в серии данных по их индексу (порядковому номеру). `override Candle` указывает, что он возвращает объект типа `Candle` и переопределяет индексатор базового класса. `[get, set]` означает, что можно как получить свечу по индексу, так и установить ее (хотя установка может быть нетипичным использованием для серии данных свечей).*

11. **`UpCandleColor`**

    ```csharp
    CrossColor UpCandleColor [get, set]
    ```

    **Описание:** Возвращает или устанавливает цвет элемента серии данных на бычьей (восходящей) свече.

    *Аннотация: Свойство `UpCandleColor` типа `CrossColor` управляет цветом бычьих свечей в серии данных. `[get, set]` позволяет получить и установить цвет.*

12. **`ValuesColor`**

    ```csharp
    System.Drawing.Color ValuesColor [get, set]
    ```

    **Описание:** Возвращает или устанавливает цвет текста значений.

    *Аннотация: Свойство `ValuesColor` типа `System.Drawing.Color` определяет цвет текста, используемого для отображения значений серии данных на графике или панели цен. `[get, set]` позволяет получить и установить цвет текста.*

13. **`Visible`**

    ```csharp
    bool Visible [get, set]
    ```

    **Описание:** Возвращает или устанавливает значение, указывающее, видна ли серия данных на графике.

    *Аннотация: Свойство `Visible` типа `bool` управляет видимостью серии данных на графике. В отличие от `IsVisible`, `Visible` позволяет как получить текущее состояние видимости, так и изменить его. `[get, set]` означает, что можно и получить, и установить видимость.*

### Дополнительные унаследованные члены

*   **Защищенные функции-члены, унаследованные от `ATAS.Indicators.BaseDataSeries< Candle >`**
    *Аннотация:  `CandleDataSeries` наследует защищенные методы от `BaseDataSeries< Candle >`. Эти методы предназначены для использования внутри класса и его наследников.*

*   **События, унаследованные от `ATAS.Indicators.BaseDataSeries< Candle >`**
    *Аннотация: `CandleDataSeries` также наследует события от `BaseDataSeries< Candle >`. События позволяют реагировать на определенные действия или изменения в серии данных.*

---
*Конец методички*






## Методичка по классу `ATAS.Indicators.CandlePartSeries`

### Описание класса

Класс `ATAS.Indicators.CandlePartSeries` представляет собой серию данных десятичных значений, полученных из определенных частей `IndicatorCandle`, созданного с помощью `ICandleCreator`.

### Диаграмма наследования

```
BaseDataSeries< decimal >
    ↑
ATAS.Indicators.CandlePartSeries
```

**Аннотация:** `ATAS.Indicators.CandlePartSeries` наследуется от класса `BaseDataSeries< decimal >`. Это означает, что `CandlePartSeries` обладает всеми свойствами и методами `BaseDataSeries< decimal >` и расширяет их, специализируясь на работе с частями свечей индикатора.

### Диаграмма взаимодействия

```
BaseDataSeries< decimal >
    ↑
ATAS.Indicators.CandlePartSeries
```

**Аннотация:** Диаграмма взаимодействия показывает аналогичную структуру наследования, подчеркивая связь `CandlePartSeries` с `BaseDataSeries< decimal >`.

### Открытые члены класса (Public Member Functions)

#### Конструктор

`CandlePartSeries(ICandleCreator candleCreator, DataSeriesType type)`

**Описание:** Инициализирует новый экземпляр класса `CandlePartSeries`.

**Параметры:**

*   `candleCreator`: `ICandleCreator` -  Интерфейс, используемый для создания `IndicatorCandles`.
    **Аннотация:** `ICandleCreator` отвечает за создание свечей индикатора, которые будут использоваться для получения частей данных.
*   `type`: `DataSeriesType` - Тип серии данных, основанный на конкретной части `IndicatorCandle`.
    **Аннотация:** `DataSeriesType` определяет, какая именно часть свечи (например, Open, High, Low, Close) будет использоваться для формирования серии данных.

#### Метод `GetCandle`

`IndicatorCandle GetCandle(int bar)`

**Описание:** Возвращает `IndicatorCandle` по указанному индексу бара.

**Параметры:**

*   `bar`: `int` - Индекс бара, для которого необходимо получить `IndicatorCandle`.
    **Аннотация:** `bar` представляет собой порядковый номер бара в серии данных, начиная с 0.

**Возвращаемое значение:**

*   `IndicatorCandle` - Свеча индикатора по указанному индексу бара.
    **Аннотация:** Метод возвращает объект `IndicatorCandle`, представляющий свечу индикатора для запрошенного бара.

### Свойства (Properties)

#### Свойство `Count`

`override int Count [get]`

**Описание:** Возвращает общее количество элементов в серии данных.

**Аннотация:** `override` указывает, что это свойство переопределяет свойство из базового класса `BaseDataSeries< decimal >`. `int` означает, что свойство возвращает целое число, представляющее количество баров. `[get]` указывает, что свойство доступно только для чтения (get accessor).

#### Индексатор `this[int index]`

`override decimal this[int index] [get, set]`

**Описание:**  Позволяет получать или устанавливать значение элемента серии данных по указанному индексу.

**Параметры:**

*   `index`: `int` - Индекс элемента, к которому осуществляется доступ.
    **Аннотация:** `index` - это порядковый номер элемента в серии данных.

**Возвращаемое значение (get):**

*   `decimal` - Значение элемента серии данных по указанному индексу.

**Установка значения (set):**

*   `decimal` - Новое значение для элемента серии данных по указанному индексу.

**Аннотация:** `override` указывает на переопределение индексатора базового класса. `decimal` означает, что индексатор работает со значениями типа `decimal` (десятичные числа). `[get, set]` указывает, что индексатор доступен как для чтения (get accessor), так и для записи (set accessor).

### Дополнительные унаследованные члены (Additional Inherited Members)

*   **► Protected Member Functions inherited from `ATAS.Indicators.BaseDataSeries< decimal >`**
    **Аннотация:**  Существуют защищенные методы, унаследованные от `BaseDataSeries< decimal >`, которые могут использоваться внутри класса `CandlePartSeries` и его наследников.
*   **► Events inherited from `ATAS.Indicators.BaseDataSeries< decimal >`**
    **Аннотация:** Существуют события, унаследованные от `BaseDataSeries< decimal >`, которые могут быть использованы для обработки различных ситуаций, связанных с серией данных.

### Подробное описание (Detailed Description)

Класс `ATAS.Indicators.CandlePartSeries` предназначен для представления набора данных, состоящего из десятичных значений, которые извлекаются из определенных частей свечей индикатора (`IndicatorCandle`), созданных с помощью объекта, реализующего интерфейс `ICandleCreator`.

### Документация по файлу

Документация для этого класса была сгенерирована из файла:

*   `CandlePartSeries.cs`
    **Аннотация:** Исходный код класса `CandlePartSeries` находится в файле `CandlePartSeries.cs`.

**Конец методички.**





## Методичка по классу `ATAS.Indicators.ChartObject` для создания индикаторов ATAS

### Обзор класса `ATAS.Indicators.ChartObject`

**`ATAS.Indicators.ChartObject`** - это базовый класс для объектов, отображаемых на графике.

**Диаграмма наследования:**

```
INotifyPropertyChanged
    TrackedPropertyBase
        ATAS.Indicators.ChartObject
            ATAS.Indicators.BaseIndicator
                ATAS.Indicators.ExtendedIndicator
                    ATAS.Indicators.Indicator
            ATAS.Strategies.Chart.ChartStrategy
                ATAS.Strategies.Chart.SmaChartStrategy
```

*   **`INotifyPropertyChanged`**: Интерфейс для уведомления об изменении свойств объекта.
*   **`TrackedPropertyBase`**: Базовый класс для свойств с отслеживанием изменений.
*   **`ATAS.Indicators.ChartObject`**: Текущий рассматриваемый базовый класс для графических объектов.
*   **`ATAS.Indicators.BaseIndicator`**: Базовый класс для индикаторов.
*   **`ATAS.Indicators.ExtendedIndicator`**: Расширенный класс индикаторов.
*   **`ATAS.Indicators.Indicator`**: Основной класс индикаторов.
*   **`ATAS.Strategies.Chart.ChartStrategy`**: Базовый класс для стратегий на графике.
*   **`ATAS.Strategies.Chart.SmaChartStrategy`**: Пример стратегии, использующей скользящую среднюю.

**Диаграмма взаимодействия:**

```
INotifyPropertyChanged
    TrackedPropertyBase
        ATAS.Indicators.ChartObject
```

### Публичные методы (Public Member Functions)

| Метод                                  | Описание                                                                                   |
| :-------------------------------------- | :----------------------------------------------------------------------------------------- |
| `virtual bool ProcessMouseClick(RenderControlMouseEventArgs e)` | Обрабатывает событие клика мыши на графическом объекте.                                         |
| `virtual bool ProcessMouseWheel(int delta)`                   | Обрабатывает событие вращения колеса мыши на графическом объекте.                              |
| `virtual bool ProcessMouseDown(RenderControlMouseEventArgs e)`  | Обрабатывает событие нажатия кнопки мыши на графическом объекте.                               |
| `virtual bool ProcessMouseUp(RenderControlMouseEventArgs e)`    | Обрабатывает событие отпускания кнопки мыши на графическом объекте.                             |
| `virtual bool ProcessMouseMove(RenderControlMouseEventArgs e)`  | Обрабатывает событие перемещения мыши над графическим объектом.                               |
| `virtual bool ProcessMouseDoubleClick(RenderControlMouseEventArgs e)` | Обрабатывает событие двойного клика мыши на графическом объекте.                             |
| `virtual StdCursor GetCursor(RenderControlMouseEventArgs e)`      | Возвращает курсор для отображения, когда мышь находится над графическим объектом.                |
| `virtual bool ProcessKeyDown(CrossKeyEventArgs e)`             | Обрабатывает событие нажатия клавиши на графическом объекте.                                  |
| `virtual bool ProcessKeyUp(CrossKeyEventArgs e)`               | Обрабатывает событие отпускания клавиши на графическом объекте.                                |

### Защищенные методы (Protected Member Functions)

| Метод                        | Описание                                           |
| :--------------------------- | :------------------------------------------------- |
| `virtual void OnVisibleChanged()` | Вызывается при изменении свойства `Visible`.         |
| `virtual void LockedOnChanged()`   | Вызывается при изменении свойства `Locked`.          |

*   **Наследованные защищенные методы:**  Указывает на наличие защищенных методов, унаследованных от `ATAS.Indicators.Filters.TrackedPropertyBase`. (Детали не приведены в текущей документации).

### Свойства (Properties)

| Свойство              | Тип   | Доступ | Описание                                                                 |
| :-------------------- | :---- | :----- | :----------------------------------------------------------------------- |
| `bool Visible`        | `bool` | `get`, `set` | Получает или устанавливает значение, определяющее, видим ли графический объект.   |
| `bool Locked`         | `bool` | `get`, `set` | Получает или устанавливает значение, определяющее, заблокирован ли графический объект. |
| `bool AllowedInteraction` | `bool` | `get`    | Получает значение, указывающее, разрешено ли взаимодействие с графическим объектом. |

### Дополнительные унаследованные члены (Additional Inherited Members)

*   **Унаследованные события:** Указывает на наличие событий, унаследованных от `ATAS.Indicators.Filters.TrackedPropertyBase`. (Детали не приведены в текущей документации).

### Подробное описание (Detailed Description)

**`ATAS.Indicators.ChartObject`** - Базовый класс для объектов на графике.

### Документация методов (Member Function Documentation)

#### `GetCursor()`

```c++
virtual StdCursor GetCursor (RenderControlMouseEventArgs e) virtual
```

*   **Описание:** Возвращает курсор для отображения, когда мышь находится над графическим объектом.
*   **Параметры:**
    *   `e` - Аргументы события мыши (`RenderControlMouseEventArgs`).
*   **Возвращает:** Стандартный курсор для отображения (`StdCursor`).

#### `LockedOnChanged()`

```c++
virtual void LockedOnChanged() protected virtual
```

*   **Описание:** Вызывается при изменении свойства `Locked`.

#### `OnVisibleChanged()`

```c++
virtual void OnVisibleChanged() protected virtual
```

*   **Описание:** Вызывается при изменении свойства `Visible`.
*   **Реализован в:** `ATAS.Indicators.BaseIndicator`. (Указывает, что метод переопределен в классе `ATAS.Indicators.BaseIndicator`).

#### `ProcessKeyDown()`

```c++
virtual bool ProcessKeyDown (CrossKeyEventArgs e) virtual
```

*   **Описание:** Обрабатывает событие нажатия клавиши на графическом объекте.
*   **Параметры:**
    *   `e` - Аргументы события клавиши (`CrossKeyEventArgs`).
*   **Возвращает:** `true`, если событие было обработано; иначе `false`.

#### `ProcessKeyUp()`

```c++
virtual bool ProcessKeyUp (CrossKeyEventArgs e) virtual
```

*   **Описание:** Обрабатывает событие отпускания клавиши на графическом объекте.
*   **Параметры:**
    *   `e` - Аргументы события клавиши (`CrossKeyEventArgs`).
*   **Возвращает:** `true`, если событие было обработано; иначе `false`.

#### `ProcessMouseClick()`

```c++
virtual bool ProcessMouseClick (RenderControlMouseEventArgs e) virtual
```

*   **Описание:** Обрабатывает событие клика мыши на графическом объекте.
*   **Параметры:**
    *   `e` - Аргументы события мыши (`RenderControlMouseEventArgs`).
*   **Возвращает:** `true`, если событие было обработано; иначе `false`.

#### `ProcessMouseDoubleClick()`

```c++
virtual bool ProcessMouseDoubleClick (RenderControlMouseEventArgs e) virtual
```

*   **Описание:** Обрабатывает событие двойного клика мыши на графическом объекте.
*   **Параметры:**
    *   `e` - Аргументы события мыши (`RenderControlMouseEventArgs`).
*   **Возвращает:** `true`, если событие было обработано; иначе `false`.

#### `ProcessMouseDown()`

```c++
virtual bool ProcessMouseDown (RenderControlMouseEventArgs e) virtual
```

*   **Описание:** Обрабатывает событие нажатия кнопки мыши на графическом объекте.
*   **Параметры:**
    *   `e` - Аргументы события мыши (`RenderControlMouseEventArgs`).
*   **Возвращает:** `true`, если событие было обработано; иначе `false`.

#### `ProcessMouseMove()`

```c++
virtual bool ProcessMouseMove (RenderControlMouseEventArgs e) virtual
```

*   **Описание:** Обрабатывает событие перемещения мыши над графическим объектом.
*   **Параметры:**
    *   `e` - Аргументы события мыши (`RenderControlMouseEventArgs`).
*   **Возвращает:** `true`, если событие было обработано; иначе `false`.

#### `ProcessMouseUp()`

```c++
virtual bool ProcessMouseUp (RenderControlMouseEventArgs e) virtual
```

*   **Описание:** Обрабатывает событие отпускания кнопки мыши на графическом объекте.
*   **Параметры:**
    *   `e` - Аргументы события мыши (`RenderControlMouseEventArgs`).
*   **Возвращает:** `true`, если событие было обработано; иначе `false`.

#### `ProcessMouseWheel()`

```c++
virtual bool ProcessMouseWheel (int delta) virtual
```

*   **Описание:** Обрабатывает событие вращения колеса мыши на графическом объекте.
*   **Параметры:**
    *   `delta` - Значение дельты колеса мыши (`int`).
*   **Возвращает:** `true`, если событие было обработано; иначе `false`.

### Документация свойств (Property Documentation)

#### `AllowedInteraction`

```c++
bool AllowedInteraction get
```

*   **Описание:** Получает значение, указывающее, разрешено ли взаимодействие с графическим объектом.
*   **Доступ:** Только чтение (`get`).

#### `Locked`

```c++
bool Locked get set
```

*   **Описание:** Получает или устанавливает значение, определяющее, заблокирован ли графический объект.
*   **Доступ:** Чтение и запись (`get`, `set`).

#### `Visible`

```c++
bool Visible get set
```

*   **Описание:** Получает или устанавливает значение, определяющее, видим ли графический объект.
*   **Доступ:** Чтение и запись (`get`, `set`).

### Файл, в котором определен класс

*   `ChartObject.cs`

---
**Аннотации на русском языке:**

*   **Class Reference** - Ссылка на класс
*   **Base class** - Базовый класс
*   **Inheritance diagram** - Диаграмма наследования
*   **Collaboration diagram** - Диаграмма взаимодействия
*   **Public Member Functions** - Публичные методы класса
*   **Protected Member Functions** - Защищенные методы класса
*   **Properties** - Свойства класса
*   **List of all members** - Список всех членов класса
*   **virtual** - Виртуальный метод (может быть переопределен в производных классах)
*   **bool** - Булевский тип данных (логическое значение: true или false)
*   **int** - Целочисленный тип данных
*   **void** - Метод, не возвращающий значение
*   **StdCursor** - Тип данных для представления курсора мыши
*   **RenderControlMouseEventArgs** - Аргументы события мыши, связанные с элементом управления рендеринга
*   **CrossKeyEventArgs** - Аргументы события клавиши
*   **get** - Аксессор "геттер" (для чтения значения свойства)
*   **set** - Аксессор "сеттер" (для записи значения свойства)
*   **Detailed Description** - Подробное описание
*   **Member Function Documentation** - Документация методов класса
*   **Parameters** - Параметры метода
*   **Returns** - Возвращаемое значение метода
*   **Property Documentation** - Документация свойств класса
*   **The documentation for this class was generated from the following file** - Документация для этого класса была сгенерирована из следующего файла.








## Методичка по классу `ATAS.Indicators.Container` для создания индикаторов ATAS

**Описание класса:**

```csharp
// Описание класса ATAS.Indicators.Container
// Представляет контейнер с определенной областью на графике.
// Подробнее смотрите в документации ATAS.
public class Container : IContainer
{
    // Публичные члены класса (функции и свойства) описаны ниже.
}
```

**Диаграмма наследования:**

```
// Диаграмма наследования класса ATAS.Indicators.Container:
//
// IContainer
//   |
//   +-- Container
//
// [legend] - обозначение типа связи на диаграмме (наследование)
```

**Диаграмма взаимодействия:**

```
// Диаграмма взаимодействия класса ATAS.Indicators.Container:
//
// IContainer
//   |
//   +-- Container
//
// [legend] - обозначение типа связи на диаграмме (взаимодействие)
```

**Публичные функции-члены:**

```csharp
// Публичная функция-член класса: SetRegion
// void SetRegion (Rectangle region)
//
// Устанавливает область контейнера.
public void SetRegion(Rectangle region)
{
    // region - параметр функции типа Rectangle, представляющий область для установки контейнеру.
}
```

**Свойства:**

```csharp
// Свойство класса: Region
// Rectangle Region { get; }
//
// Возвращает область контейнера.
public Rectangle Region
{
    get
    {
        // Возвращает текущую область контейнера как объект Rectangle.
        // Доступно только для чтения (get).
        return область_контейнера; // Замените 'область_контейнера' на реальную реализацию.
    }
}
```

**Свойства, унаследованные от `ATAS.Indicators.IContainer`:**

```
// Свойства, унаследованные от интерфейса ATAS.Indicators.IContainer:
// (Обратитесь к документации ATAS.Indicators.IContainer для получения подробной информации об унаследованных свойствах.)
// В данном документе не приведены детали унаследованных свойств.
// Необходимо изучить документацию по интерфейсу IContainer.
```

**Подробное описание:**

```
// Подробное описание класса:
// Представляет контейнер с определенной областью на графике.
// Используется для группировки и управления элементами индикатора в пределах заданной прямоугольной области.
```

**Документация функции-члена:**

**`SetRegion()`**

```csharp
// Документация функции SetRegion():
// void ATAS.Indicators.Container.SetRegion (Rectangle region)
//
// Устанавливает область контейнера.
//
// Параметры:
// region - область типа Rectangle, которую необходимо установить для контейнера.
//          Определяет границы контейнера на графике.
```

**Документация свойства:**

**`Region`**

```csharp
// Документация свойства Region:
// Rectangle ATAS.Indicators.Container.Region { get; }
//
// Возвращает область контейнера.
//
// Возвращаемое значение:
// Объект Rectangle, представляющий текущую область контейнера.
// Доступно только для чтения.
//
// Реализует:
// ATAS.Indicators.IContainer.Region - интерфейс, который реализует данный класс.
```

**Файл документации для класса:**

```
// Файл, из которого была сгенерирована документация для этого класса:
// • IIndicatorContainer.cs
//  - Имя файла указывает на то, что класс Container вероятно реализует интерфейс IIndicatorContainer,
//    определенный в файле IIndicatorContainer.cs.
```







## Методичка по классу `ATAS.Indicators.CumulativeTrade`

### Описание класса
Класс `ATAS.Indicators.CumulativeTrade` представляет собой кумулятивную сделку, которая включает в себя несколько принтов или исполнений.

### Публичные члены класса

#### Публичные функции-члены

*   `ToString()`
    *   **Описание:** Возвращает строковое представление объекта `CumulativeTrade`.
    *   **Возвращает:** Строка, содержащая информацию о сделке.

#### Свойства

*   `FirstPrice`
    *   **Тип:** `decimal`
    *   **Описание:** Получает или устанавливает первую цену сделки.
    *   `[get, set]` - доступно для чтения и записи.

*   `Lastprice`
    *   **Тип:** `decimal`
    *   **Описание:** Получает или устанавливает последнюю цену сделки.
    *   `[get, set]` - доступно для чтения и записи.

*   `Volume`
    *   **Тип:** `decimal`
    *   **Описание:** Получает или устанавливает кумулятивный объем сделки.
    *   `[get, set]` - доступно для чтения и записи.

*   `Time`
    *   **Тип:** `DateTime`
    *   **Описание:** Получает или устанавливает время сделки.
    *   `[get, set]` - доступно для чтения и записи.

*   `Direction`
    *   **Тип:** `TradeDirection`
    *   **Описание:** Получает или устанавливает направление сделки (Покупка или Продажа).
    *   `[get, set]` - доступно для чтения и записи.

*   `Ticks`
    *   **Тип:** `List<MarketDataArg>`
    *   **Описание:** Получает или устанавливает список отдельных тиков (`MarketDataArg`), включенных в кумулятивную сделку.
    *   `[get, set]` - доступно для чтения и записи.

*   `PreviousAsk`
    *   **Тип:** `MarketDataArg`
    *   **Описание:** Получает или устанавливает лучший Ask до сделки.
    *   `[get, set]` - доступно для чтения и записи.

*   `PreviousBid`
    *   **Тип:** `MarketDataArg`
    *   **Описание:** Получает или устанавливает лучший Bid до сделки.
    *   `[get, set]` - доступно для чтения и записи.

*   `NewAsk`
    *   **Тип:** `MarketDataArg`
    *   **Описание:** Получает или устанавливает лучший Ask после сделки.
    *   `[get, set]` - доступно для чтения и записи.

*   `NewBid`
    *   **Тип:** `MarketDataArg`
    *   **Описание:** Получает или устанавливает лучший Bid после сделки.
    *   `[get, set]` - доступно для чтения и записи.

### Подробное описание

Класс `ATAS.Indicators.CumulativeTrade` предназначен для представления агрегированных торговых данных, где одна запись может включать в себя несколько отдельных сделок, произошедших практически одновременно. Это позволяет анализировать торговлю на более высоком уровне, рассматривая совокупные объемы и цены.

### Расположение

Документация для этого класса была сгенерирована из файла:

*   `CumulativeTrade.cs`







## Методичка по классу `ATAS.Indicators.CumulativeTradesRequest` для создания индикаторов ATAS

### Класс `ATAS.Indicators.CumulativeTradesRequest`

**Описание:**

Представляет запрос на получение кумулятивных торговых данных в указанном диапазоне времени или за определенную дату.  Не должен превышать 7 дней.

### Публичные функции-члены (Конструкторы)

#### `CumulativeTradesRequest(DateTime beginTime, DateTime endTime, int minVolume, int maxVolume)`

**Описание:**

Инициализирует новый экземпляр класса `CumulativeTradesRequest` с указанным диапазоном времени и фильтрами объема.

**Параметры:**

*   `beginTime`: `DateTime` -  Время начала запрашиваемых данных.
*   `endTime`: `DateTime` - Время окончания запрашиваемых данных.
*   `minVolume`: `int` - Минимальный фильтр кумулятивного объема для запрашиваемых данных.
*   `maxVolume`: `int` - Максимальный фильтр кумулятивного объема для запрашиваемых данных.

#### `CumulativeTradesRequest(DateTime date)`

**Описание:**

Инициализирует новый экземпляр класса `CumulativeTradesRequest` для получения данных за определенную дату.

**Параметры:**

*   `date`: `DateTime` - Дата, для которой необходимо получить данные.

### Свойства

#### `BeginTime`

*   Тип: `DateTime`
*   Доступ: `get`

**Описание:**

Возвращает время начала запрашиваемых данных.

#### `EndTime`

*   Тип: `DateTime`
*   Доступ: `get`

**Описание:**

Возвращает время окончания запрашиваемых данных.

#### `GetDataForParticularDate`

*   Тип: `bool`
*   Доступ: `get`, `set`

**Описание:**

Возвращает или устанавливает флаг, указывающий, нужно ли получать данные только за определенную дату.

#### `MaxVolume`

*   Тип: `decimal`
*   Доступ: `get`

**Описание:**

Возвращает максимальный фильтр кумулятивного объема для запрашиваемых данных.

#### `MinVolume`

*   Тип: `decimal`
*   Доступ: `get`

**Описание:**

Возвращает минимальный фильтр кумулятивного объема для запрашиваемых данных.

#### `RequestId`

*   Тип: `int`
*   Доступ: `get`

**Описание:**

Возвращает уникальный идентификатор запроса.







## Методичка по классу `ATAS.Indicators.CustomValue` для создания индикаторов ATAS

**Описание класса:** `ATAS.Indicators.CustomValue`

Представляет пользовательское значение с сопутствующими свойствами.

**Свойства класса:**

1.  **`CrossColor` / `ValueColor`**
    *   **Тип данных:** `ValueColor`
    *   **Тип доступа:** `[get, set]`
    *   **Описание:**  Получает или устанавливает цвет пользовательского значения. Используйте альфа-канал для прозрачности.

2.  **`Price`**
    *   **Тип данных:** `decimal`
    *   **Тип доступа:** `[get, set]`
    *   **Описание:** Получает или устанавливает ценовое значение пользовательского значения.

3.  **`StringValue`**
    *   **Тип данных:** `string`
    *   **Тип доступа:** `[get, set]`
    *   **Описание:** Получает или устанавливает строковое представление пользовательского значения.

**Подробная документация свойств:**

*   **`Price`**:
    *   **Тип данных:** `decimal ATAS.Indicators.CustomValue.Price`
    *   **Описание:** Получает или устанавливает ценовое значение пользовательского значения.

*   **`StringValue`**:
    *   **Тип данных:** `string ATAS.Indicators.CustomValue.StringValue`
    *   **Описание:** Получает или устанавливает строковое представление пользовательского значения.

*   **`ValueColor` / `CrossColor`**:
    *   **Тип данных:** `CrossColor ATAS.Indicators.CustomValue.ValueColor`
    *   **Описание:** Получает или устанавливает цвет пользовательского значения. Используйте альфа-канал для прозрачности.

**Файл документации:**

*   Документация для этого класса была сгенерирована из файла `CustomValue.cs`.








## Методичка по классу `ATAS.Indicators.CustomValueDataSeries` для создания индикаторов ATAS

### Описание класса

**`ATAS.Indicators.CustomValueDataSeries`** - представляет пользовательский ряд данных, который хранит объекты `CustomValue`.

*   **Наследование:**  `BaseDataSeries<CustomValue>` → `ATAS.Indicators.CustomValueDataSeries`

*   **Описание:** Класс предназначен для создания и управления пользовательскими рядами данных в ATAS, где каждое значение представлено объектом `CustomValue`. Это позволяет хранить не только числовые значения, но и дополнительную информацию, связанную с каждым значением ряда.

### Конструкторы

1.  **`CustomValueDataSeries(string id, string name)`**

    *   **Описание:** Инициализирует новый экземпляр класса `CustomValueDataSeries` с указанным уникальным и постоянным идентификатором ряда данных (`id`) для сериализации данных и уникальным именем (`name`).
    *   **Параметры:**
        *   `id`:  Уникальный и постоянный идентификатор ряда данных для сериализации.
        *   `name`: Уникальное имя ряда данных.
    *   **Аннотация:** Этот конструктор используется для создания ряда данных с именем, которое будет отображаться в интерфейсе ATAS, и идентификатором, который ATAS использует для сохранения и загрузки настроек индикатора.

2.  **`CustomValueDataSeries(string id)`**

    *   **Описание:** Инициализирует новый экземпляр класса `CustomValueDataSeries` с указанным идентификатором (`id`).
    *   **Параметры:**
        *   `id`: Уникальный идентификатор для ряда данных.
    *   **Аннотация:** Этот конструктор создает ряд данных, используя только идентификатор. Имя ряда данных в этом случае может быть установлено позже или использоваться для внутренних целей без отображения в интерфейсе пользователя.

### Свойства

1.  **`ScaleIt`**  `bool`  `[get, set]`

    *   **Описание:**  Получает или устанавливает значение, указывающее, должен ли ряд данных масштабироваться.
    *   **Аннотация:** Это свойство контролирует, будет ли ряд данных автоматически масштабироваться по вертикальной оси графика. Если `ScaleIt` установлено в `true`, ряд данных будет масштабироваться в соответствии с диапазоном цен на графике. Если `false` -  масштабирование будет зависеть от настроек шкалы графика.

2.  **`Count`**  `int`  `[get]`

    *   **Описание:**  Возвращает количество элементов в ряде данных.
    *   **Аннотация:**  Это свойство позволяет узнать, сколько значений `CustomValue` в данный момент содержится в ряде данных.

3.  **`this[int index]`**  `CustomValue`  `[get, set]`

    *   **Описание:**  Получает или устанавливает объект `CustomValue` по указанному индексу бара.
    *   **Параметры:**
        *   `index`: Индекс бара, отсчитываемый от нуля.
    *   **Возвращает:** Объект `CustomValue` по указанному индексу бара.
    *   **Аннотация:**  Это индексатор, позволяющий получать доступ к конкретному значению `CustomValue` в ряде данных по его индексу. Индекс соответствует номеру бара, начиная с нуля для самого первого бара в ряде данных.

### Методы

1.  **`Clear()`**  `override void`

    *   **Описание:**  Очищает все значения в ряде данных.
    *   **Аннотация:**  Этот метод удаляет все объекты `CustomValue` из ряда данных, делая его пустым.

### Унаследованные члены

*   **Публичные методы, унаследованные от `ATAS.Indicators.BaseDataSeries<CustomValue>`:**  (Необходимо обратиться к документации `ATAS.Indicators.BaseDataSeries<CustomValue>` для подробностей).
*   **Защищенные методы, унаследованные от `ATAS.Indicators.BaseDataSeries<CustomValue>`:** (Необходимо обратиться к документации `ATAS.Indicators.BaseDataSeries<CustomValue>` для подробностей).
*   **События, унаследованные от `ATAS.Indicators.BaseDataSeries<CustomValue>`:** (Необходимо обратиться к документации `ATAS.Indicators.BaseDataSeries<CustomValue>` для подробностей).

### Подробное описание

Класс `ATAS.Indicators.CustomValueDataSeries` предназначен для представления рядов данных, содержащих объекты `CustomValue`. Это позволяет создавать индикаторы, которые могут отображать не только числовые значения, но и дополнительную информацию, связанную с каждым значением. Например, можно использовать `CustomValueDataSeries` для отображения сигналов на графике, где каждый сигнал (объект `CustomValue`) может содержать информацию о типе сигнала, его силе и т.д.

### Файл

Документация для этого класса была сгенерирована из файла: `CustomValueDataSeries.cs`








**Методичка: Класс ATAS.Indicators.ExtendedIndicator**

**1. Обзор класса**

*   **Название класса:** `ATAS.Indicators.ExtendedIndicator`
*   **Описание:**  Расширенный базовый класс для пользовательских индикаторов, который предоставляет дополнительную функциональность для рисования, алертов, обработки рыночных данных и т.д.
*   **Наследование:** Класс `ATAS.Indicators.ExtendedIndicator` наследуется от класса `ATAS.Indicators.Indicator`.

**2. Диаграмма наследования**

Диаграмма показывает иерархию наследования класса `ATAS.Indicators.ExtendedIndicator`.

*   `INotifyPropertyChanged`
*   `TrackedPropertyBase`
*   `ChartObject`
*   `IDisposable`
*   `INotifyPanelPropertyChanged`
*   `BaseIndicator`
*   `ICustomTypeDescriptor`
*   `IMarketTimeProvider`
*   `ATAS.Indicators.ExtendedIndicator`
*   `ATAS.Indicators.Indicator`
*   `ATAS.Strategies.Chart.ChartStrategy`
*   `ATAS.Strategies.Chart.SmaChartStrategy`

**3. Диаграмма взаимодействия**

Диаграмма показывает взаимодействие класса `ATAS.Indicators.ExtendedIndicator` с другими классами и интерфейсами.  (Диаграмма визуально повторяет диаграмму наследования, но акцентирует связи между объектами).

*   `INotifyPropertyChanged`
*   `TrackedPropertyBase`
*   `ChartObject`
*   `IDisposable`
*   `INotifyPanelPropertyChanged`
*   `BaseIndicator`
*   `ICustomTypeDescriptor`
*   `IMarketTimeProvider`
*   `ATAS.Indicators.ExtendedIndicator`

**4. Публичные методы (Public Member Functions)**

*   **`ApplyDefaultColors()`**
    *   **Сигнатура:** `void ApplyDefaultColors ()`
    *   **Описание:** Применяет стандартные цвета к элементам рисования индикатора.

*   **`Dispose()`**
    *   **Сигнатура:** `override void Dispose ()`
    *   **Описание:** Освобождает ресурсы, используемые индикатором. (Переопределенный метод).

*   **`Draw(RenderContext context, DrawingLayouts layout)`**
    *   **Сигнатура:** `void Draw (RenderContext context, DrawingLayouts layout)`
    *   **Описание:** Метод для рисования индикатора на графике.
    *   **Параметры:**
        *   `context`: `RenderContext` - Контекст рендеринга, используемый для рисования на графике.
        *   `layout`: `DrawingLayouts` -  Информация о размещении для рисования.

*   **`SubscribeToTimer(TimeSpan period, Action callback)`**
    *   **Сигнатура:** `void SubscribeToTimer (TimeSpan period, Action callback)`
    *   **Описание:** Регистрирует функцию обратного вызова (callback), которая будет периодически вызываться через заданный интервал времени.
    *   **Параметры:**
        *   `period`: `TimeSpan` - Период времени рыночных данных, используемый для повторного вызова callback.
        *   `callback`: `Action` - Метод, который вызывается каждый раз, когда проходит указанный период времени рыночных данных.

*   **`UnsubscribeFromTimer(TimeSpan period, Action callback)`**
    *   **Сигнатура:** `void UnsubscribeFromTimer (TimeSpan period, Action callback)`
    *   **Описание:** Отменяет регистрацию функции обратного вызова, зарегистрированной для периодических вызовов.
    *   **Параметры:**
        *   `period`: `TimeSpan` - Подписанный период вызовов.
        *   `callback`: `Action` - Метод, который нужно отменить.

**5. Защищенные методы (Protected Member Functions)**

*   **`AddAlert(string soundFile, string instrument, string message, CrossColor background, CrossColor foreground)`**
    *   **Сигнатура:** `void AddAlert (string soundFile, string instrument, string message, CrossColor background, CrossColor foreground)`
    *   **Описание:** Добавляет алерт для конкретного инструмента с пользовательскими свойствами.
    *   **Параметры:**
        *   `soundFile`: `string` - Путь к файлу или имя звукового файла для алерта.
        *   `instrument`: `string` - Имя инструмента или символа, связанного с алертом.
        *   `message`: `string` - Сообщение или описание алерта.
        *   `background`: `CrossColor` - Цвет фона для визуализации алерта.
        *   `foreground`: `CrossColor` - Цвет переднего плана для визуализации алерта.

*   **`AddAlert(string soundFile, string message)`**
    *   **Сигнатура:** `void AddAlert (string soundFile, string message)`
    *   **Описание:** Добавляет алерт с указанным звуковым файлом и сообщением к графику.
    *   **Параметры:**
        *   `soundFile`: `string` - Путь к звуковому файлу, который будет воспроизведен при срабатывании алерта.
        *   `message`: `string` - Сообщение, отображаемое в алерте.

*   **`AddText(string tag, string text, bool isAbovePrice, int bar, decimal price, int yoffset, int xOffset, Color textColor, Color outlineColor, Color fillColor, float fontSize, DrawingText.TextAlign align, bool autoSize = false)`**
    *   **Сигнатура:** `DrawingText AddText (string tag, string text, bool isAbovePrice, int bar, decimal price, int yoffset, int xOffset, Color textColor, Color outlineColor, Color fillColor, float fontSize, DrawingText.TextAlign align, bool autoSize = false)`
    *   **Описание:** Добавляет текстовый элемент рисования на график в указанной позиции.
    *   **Параметры:**
        *   `tag`: `string` - Уникальный тег или идентификатор для текстового элемента.
        *   `text`: `string` - Фактический текст для отображения.
        *   `isAbovePrice`: `bool` - Флаг, указывающий, должен ли текст отображаться выше цены или нет.
        *   `bar`: `int` - Индекс бара, где должен отображаться текст.
        *   `price`: `decimal` - Цена, на которой должен отображаться текст.
        *   `yoffset`: `int` - Вертикальное смещение текста от его позиции по умолчанию.
        *   `xOffset`: `int` - Горизонтальное смещение текста от его позиции по умолчанию.
        *   `textColor`: `Color` - Цвет текста.
        *   `outlineColor`: `Color` - Цвет, используемый для обводки текста.
        *   `fillColor`: `Color` - Цвет фона текста.
        *   `fontSize`: `float` - Размер шрифта текста.
        *   `align`: `DrawingText.TextAlign` - Выравнивание текста в пределах ограничивающей рамки.
        *   `autoSize`: `bool` - Флаг, указывающий, должен ли размер текста быть автоматическим или нет.
    *   **Возвращает:** `DrawingText` - Созданный объект `DrawingText`.

*   **`ApplyDefaultColors()`**
    *   **Сигнатура:** `virtual void ApplyDefaultColors ()`
    *   **Описание:** Применяет стандартные цвета к элементам рисования. Этот метод вызывает виртуальный метод `OnApplyDefaultColors`, который может быть переопределен в производных классах для настройки стандартных цветовых параметров для элементов рисования.

*   **`Calculate(int bar, decimal value)`**
    *   **Сигнатура:** `override void Calculate (int bar, decimal value)`
    *   **Описание:** Выполняет расчет значения индикатора для указанного бара и значения. (Переопределенный метод).
    *   **Параметры:**
        *   `bar`: `int` - Номер бара, для которого выполняется расчет.
        *   `value`: `decimal` - Входное значение из серии данных на указанном баре.

*   **`Dispose()`**
    *   **Сигнатура:** `override void Dispose ()`
    *   **Описание:** Освобождает ресурсы, используемые индикатором. (Переопределенный метод).

*   **`Draw(RenderContext context, DrawingLayouts layout)`**
    *   **Сигнатура:** `void Draw (RenderContext context, DrawingLayouts layout)`
    *   **Описание:** Метод для рисования индикатора на графике.

*   **`ExtendedIndicator(bool useCandles = false)`**
    *   **Сигнатура:** `ExtendedIndicator (bool useCandles = false)`
    *   **Описание:** Конструктор класса `ExtendedIndicator`.
    *   **Параметры:**
        *   `useCandles`: `bool` -  Указывает, использовать ли свечи для отображения. По умолчанию `false`.

*   **`ExtendedIndicator(DataSeriesType seriesType)`**
    *   **Сигнатура:** `ExtendedIndicator (DataSeriesType seriesType)`
    *   **Описание:** Конструктор класса `ExtendedIndicator`.
    *   **Параметры:**
        *   `seriesType`: `DataSeriesType` - Тип серии данных, используемый индикатором.

*   **`GetCandle(int bar)`**
    *   **Сигнатура:** `IndicatorCandle GetCandle (int bar)`
    *   **Описание:** Возвращает объект `IndicatorCandle` для указанного индекса бара.
    *   **Параметры:**
        *   `bar`: `int` - Индекс бара, для которого нужно получить `IndicatorCandle`.
    *   **Возвращает:** `IndicatorCandle` - Объект `IndicatorCandle` для указанного индекса бара.

*   **`GetFixedProfile(FixedProfileRequest request)`**
    *   **Сигнатура:** `void GetFixedProfile (FixedProfileRequest request)`
    *   **Описание:** Запрашивает данные фиксированного профиля от онлайн-провайдера данных.
    *   **Параметры:**
        *   `request`: `FixedProfileRequest` - Объект запроса, содержащий информацию о данных фиксированного профиля, которые нужно получить.

*   **`GetMarketByOrdersCache(TimeSpan period)`**
    *   **Сигнатура:** `IMarketByOrdersCache GetMarketByOrdersCache (TimeSpan period)`
    *   **Описание:** Возвращает кэш данных Market by Order.
    *   **Параметры:**
        *   `period`: `TimeSpan` - Указывает период времени, в течение которого хранятся обновления Market-by-Order в `IMarketByOrdersCache`.
    *   **Возвращает:** `IMarketByOrdersCache` - Объект `IMarketByOrdersCache`, представляющий текущее состояние и позволяющий получать обновления в реальном времени.

*   **`GetMarketByOrdersWithTradesCache(TimeSpan period)`**
    *   **Сигнатура:** `IMarketByOrdersWithTradesCache GetMarketByOrdersWithTradesCache (TimeSpan period)`
    *   **Описание:** Возвращает кэш данных Market by Order и сделок.
    *   **Параметры:**
        *   `period`: `TimeSpan` - Указывает период времени, в течение которого сделки хранятся в `IMarketByOrdersWithTradesCache`.
    *   **Возвращает:** `IMarketByOrdersWithTradesCache` - Объект `IMarketByOrdersWithTradesCache`, представляющий кэш.

*   **`GetTradesCache(TimeSpan period)`**
    *   **Сигнатура:** `ITradesCache GetTradesCache (TimeSpan period)`
    *   **Описание:** Возвращает кэш сделок.
    *   **Параметры:**
        *   `period`: `TimeSpan` - Указывает период времени, в течение которого сделки хранятся в `ITradesCache`.
    *   **Возвращает:** `ITradesCache` - Объект `ITradesCache`, представляющий кэш сделок.

*   **`MarketDepthChanged(MarketDataArg depth)`**
    *   **Сигнатура:** `virtual void MarketDepthChanged (MarketDataArg depth)`
    *   **Описание:** Обрабатывает изменения в единичной глубине рынка, представленные объектом `MarketDataArg`.
    *   **Параметры:**
        *   `depth`: `MarketDataArg` - `MarketDataArg`, представляющий данные глубины рынка.

*   **`MarketDepthsChanged(IEnumerable<MarketDataArg> depths)`**
    *   **Сигнатура:** `virtual void MarketDepthsChanged (IEnumerable<MarketDataArg> depths)`
    *   **Описание:** Обрабатывает изменения в множественной глубине рынка, представленные `IEnumerable<MarketDataArg>`.
    *   **Параметры:**
        *   `depths`: `IEnumerable<MarketDataArg>` - Коллекция `MarketDataArg`, представляющая данные глубины рынка.

*   **`OnApplyDefaultColors()`**
    *   **Сигнатура:** `virtual void OnApplyDefaultColors ()`
    *   **Описание:** Вызывается для применения стандартных цветов к элементам рисования. Этот метод может быть переопределен в производных классах для настройки или установки стандартных значений цвета для элементов рисования. Когда этот метод вызывается, это возможность для производного класса применить свою собственную цветовую схему к элементам рисования, используемым в конкретной реализации.

*   **`OnBestBidAskChanged(MarketDataArg depth)`**
    *   **Сигнатура:** `virtual void OnBestBidAskChanged (MarketDataArg depth)`
    *   **Описание:** Обрабатывает изменения в лучшей цене покупки или продажи в глубине рынка, представленные объектом `MarketDataArg`.
    *   **Параметры:**
        *   `depth`: `MarketDataArg` - `MarketDataArg`, представляющий данные глубины рынка.

*   **`OnContainerChanged(IIndicatorContainer container)`**
    *   **Сигнатура:** `virtual void OnContainerChanged (IIndicatorContainer container)`
    *   **Описание:** Этот метод вызывается, когда изменяется значение свойства `Container`.
    *   **Параметры:**
        *   `container`: `IIndicatorContainer` - Новое значение `IIndicatorContainer`, к которому привязан индикатор.

*   **`OnCumulativeTrade(CumulativeTrade trade)`**
    *   **Сигнатура:** `virtual void OnCumulativeTrade (CumulativeTrade trade)`
    *   **Описание:** Обрабатывает событие кумулятивной сделки, представленное объектом `CumulativeTrade`.
    *   **Параметры:**
        *   `trade`: `CumulativeTrade` - `CumulativeTrade`, представляющий данные кумулятивной сделки.

*   **`OnCumulativeTradesResponse(CumulativeTradesRequest request, IEnumerable<CumulativeTrade> cumulativeTrades)`**
    *   **Сигнатура:** `virtual void OnCumulativeTradesResponse (CumulativeTradesRequest request, IEnumerable<CumulativeTrade> cumulativeTrades)`
    *   **Описание:** Вызывается, когда получен ответ на запрос кумулятивных сделок.
    *   **Параметры:**
        *   `request`: `CumulativeTradesRequest` - Объект `CumulativeTradesRequest`, представляющий исходный запрос.
        *   `cumulativeTrades`: `IEnumerable<CumulativeTrade>` - Коллекция `CumulativeTrade`, представляющая данные кумулятивных сделок.

*   **`OnDataProviderChanged(IIndicatorDataProvider oldDataProvider, IIndicatorDataProvider newDataProvider)`**
    *   **Сигнатура:** `virtual void OnDataProviderChanged (IIndicatorDataProvider oldDataProvider, IIndicatorDataProvider newDataProvider)`
    *   **Описание:** Этот метод вызывается, когда изменяется значение свойства `DataProvider`.
    *   **Параметры:**
        *   `oldDataProvider`: `IIndicatorDataProvider` - Старое значение `IIndicatorDataProvider`.
        *   `newDataProvider`: `IIndicatorDataProvider` - Новое значение `IIndicatorDataProvider`.

*   **`OnFixedProfilesResponse(IndicatorCandle fixedProfile, FixedProfilePeriods period)`**
    *   **Сигнатура:** `virtual void OnFixedProfilesResponse (IndicatorCandle fixedProfile, FixedProfilePeriods period)`
    *   **Описание:** Этот метод вызывается, когда онлайн-провайдер данных отвечает данными фиксированного профиля. Переопределите этот метод в производных классах для обработки полученных данных фиксированного профиля.
    *   **Параметры:**
        *   `fixedProfile`: `IndicatorCandle` - Данные фиксированного профиля.
        *   `period`: `FixedProfilePeriods` - Период фиксированного профиля.

*   **`OnFixedProfilesResponse(IndicatorCandle fixedProfileScaled, IndicatorCandle fixedProfileOriginScale, FixedProfilePeriods period)`**
    *   **Сигнатура:** `virtual void OnFixedProfilesResponse (IndicatorCandle fixedProfileScaled, IndicatorCandle fixedProfileOriginScale, FixedProfilePeriods period)`
    *   **Описание:** Этот метод вызывается, когда онлайн-провайдер данных отвечает данными фиксированного профиля. Переопределите этот метод в производных классах для обработки полученных данных фиксированного профиля.
    *   **Параметры:**
        *   `fixedProfileScaled`: `IndicatorCandle` - Данные фиксированного профиля, масштабированные до определенного периода.
        *   `fixedProfileOriginScale`: `IndicatorCandle` - Данные фиксированного профиля без масштабирования.
        *   `period`: `FixedProfilePeriods` - Период данных фиксированного профиля (например, дневной, недельный, месячный и т.д.).

*   **`OnMarketByOrdersChanged(IEnumerable<MarketByOrder> values)`**
    *   **Сигнатура:** `virtual void OnMarketByOrdersChanged (IEnumerable<MarketByOrder> values)`
    *   **Описание:** Обрабатывает коллекцию данных Market by Order, представленную объектами `MarketByOrder`.
    *   **Параметры:**
        *   `values`: `IEnumerable<MarketByOrder>` - `IEnumerable<MarketByOrder>`, представляющий данные Market by Order.

*   **`OnNewMyTrade(MyTrade myTrade)`**
    *   **Сигнатура:** `virtual void OnNewMyTrade (MyTrade myTrade)`
    *   **Описание:** Вызывается, когда получена новая сделка, исполненная собственной стратегией индикатора.
    *   **Параметры:**
        *   `myTrade`: `MyTrade` - Новая сделка, исполненная собственной стратегией индикатора.

*   **`OnNewOrder(Order order)`**
    *   **Сигнатура:** `virtual void OnNewOrder (Order order)`
    *   **Описание:** Этот метод вызывается, когда получен новый ордер.
    *   **Параметры:**
        *   `order`: `Order` - Новый полученный ордер.

*   **`OnNewTrade(MarketDataArg trade)`**
    *   **Сигнатура:** `virtual void OnNewTrade (MarketDataArg trade)`
    *   **Описание:** Обрабатывает единичную сделку, представленную объектом `MarketDataArg`.
    *   **Параметры:**
        *   `trade`: `MarketDataArg` - `MarketDataArg`, представляющий данные сделки.

*   **`OnNewTrades(IEnumerable<MarketDataArg> trades)`**
    *   **Сигнатура:** `virtual void OnNewTrades (IEnumerable<MarketDataArg> trades)`
    *   **Описание:** Обрабатывает коллекцию новых данных о сделках, представленную объектами `MarketDataArg`.
    *   **Параметры:**
        *   `trades`: `IEnumerable<MarketDataArg>` - `IEnumerable<MarketDataArg>`, представляющий новые данные о сделках.

*   **`OnOrderCancelFailed(Order order, string message)`**
    *   **Сигнатура:** `virtual void OnOrderCancelFailed (Order order, string message)`
    *   **Описание:** Вызывается при попытке отмены существующего ордера, если она не удалась.
    *   **Параметры:**
        *   `order`: `Order` - Ордер, который не удалось отменить.
        *   `message`: `string` - Сообщение об ошибке, указывающее причину неудачи.

*   **`OnOrderChanged(Order order)`**
    *   **Сигнатура:** `virtual void OnOrderChanged (Order order)`
    *   **Описание:** Вызывается, когда существующий ордер изменен (изменен).
    *   **Параметры:**
        *   `order`: `Order` - Измененный ордер.

*   **`OnOrderModifyFailed(Order order, Order newOrder, string error)`**
    *   **Сигнатура:** `virtual void OnOrderModifyFailed (Order order, Order newOrder, string error)`
    *   **Описание:** Вызывается при попытке изменить существующий ордер, если она не удалась.
    *   **Параметры:**
        *   `order`: `Order` - Ордер, который не удалось изменить.
        *   `newOrder`: `Order` - Новый ордер с модификациями, которые не удалось применить.
        *   `error`: `string` - Сообщение об ошибке, указывающее причину неудачи.

*   **`OnOrderRegisterFailed(Order order, string message)`**
    *   **Сигнатура:** `virtual void OnOrderRegisterFailed (Order order, string message)`
    *   **Описание:** Вызывается при попытке зарегистрировать (разместить) новый ордер, если она не удалась.
    *   **Параметры:**
        *   `order`: `Order` - Ордер, который не удалось зарегистрировать.
        *   `message`: `string` - Сообщение об ошибке, указывающее причину неудачи.

*   **`OnPortfolioChanged(Portfolio portfolio)`**
    *   **Сигнатура:** `virtual void OnPortfolioChanged (Portfolio portfolio)`
    *   **Описание:** Этот метод вызывается, когда изменяется значение свойства `Portfolio`.
    *   **Параметры:**
        *   `portfolio`: `Portfolio` - Новое значение `Portfolio`.

*   **`OnPositionChanged(Position position)`**
    *   **Сигнатура:** `virtual void OnPositionChanged (Position position)`
    *   **Описание:** Этот метод вызывается, когда изменяется значение свойства `Position`.
    *   **Параметры:**
        *   `position`: `Position` - Новое значение `Position`.

*   **`OnRender(RenderContext context, DrawingLayouts layout)`**
    *   **Сигнатура:** `virtual void OnRender (RenderContext context, DrawingLayouts layout)`
    *   **Описание:** Переопределите этот метод для реализации собственной логики рендеринга данных.
    *   **Параметры:**
        *   `context`: `RenderContext` - Контекст рендеринга, используемый для рисования на графике.
        *   `layout`: `DrawingLayouts` - Информация о размещении для рисования.

*   **`OnUpdateCumulativeTrade(CumulativeTrade trade)`**
    *   **Сигнатура:** `virtual void OnUpdateCumulativeTrade (CumulativeTrade trade)`
    *   **Описание:** Обрабатывает событие обновления для кумулятивной сделки, представленное объектом `CumulativeTrade`.
    *   **Параметры:**
        *   `trade`: `CumulativeTrade` - `CumulativeTrade`, представляющий обновленные данные кумулятивной сделки.

*   **`RecalculateValues()`**
    *   **Сигнатура:** `override void RecalculateValues ()`
    *   **Описание:** Пересчитывает значения индикатора на каждом баре. (Переопределенный метод).

*   **`RedrawChart(RedrawArg? redrawArg = null)`**
    *   **Сигнатура:** `void RedrawChart (RedrawArg? redrawArg = null)`
    *   **Описание:** Вызывает перерисовку графика.
    *   **Параметры:**
        *   `redrawArg`: `RedrawArg?` - Аргументы для перерисовки. Может быть `null`.

*   **`RequestFixedProfileAsync(FixedProfileRequest request)`**
    *   **Сигнатура:** `async Task<FixedProfileResponse?> RequestFixedProfileAsync (FixedProfileRequest request)`
    *   **Описание:** Асинхронно запрашивает данные фиксированного профиля, параметризованные с помощью `FixedProfileRequest`.
    *   **Параметры:**
        *   `request`: `FixedProfileRequest` - Запрос, содержащий период фиксированного профиля.
    *   **Возвращает:** `Task<FixedProfileResponse?>` - Асинхронная задача, возвращающая `FixedProfileResponse?`.

*   **`RequestForCumulativeTrades(CumulativeTradesRequest request)`**
    *   **Сигнатура:** `void RequestForCumulativeTrades (CumulativeTradesRequest request)`
    *   **Описание:** Отправляет запрос на данные кумулятивных сделок онлайн-провайдеру данных.
    *   **Параметры:**
        *   `request`: `CumulativeTradesRequest` - Объект `CumulativeTradesRequest`, представляющий запрос, который нужно отправить.

*   **`SubscribeMarketByOrderData()`**
    *   **Сигнатура:** `Task SubscribeMarketByOrderData ()`
    *   **Описание:** Подписывается на данные Market by Order.
    *   **Возвращает:** `Task` - Асинхронная задача.

*   **`SubscribeToDrawingEvents(DrawingLayouts flags)`**
    *   **Сигнатура:** `void SubscribeToDrawingEvents (DrawingLayouts flags)`
    *   **Описание:** Метод для указания списка слоев, в которых будет выполняться рендеринг.
    *   **Параметры:**
        *   `flags`: `DrawingLayouts` - Флаги слоев рисования, указывающие, на какие события подписаться.

*   **`SubscribeToTimer(TimeSpan period, Action callback)`**
    *   **Сигнатура:** `void SubscribeToTimer (TimeSpan period, Action callback)`
    *   **Описание:** Регистрирует функцию обратного вызова и периодически вызывает ее с указанным периодом.
    *   **Параметры:**
        *   `period`: `TimeSpan` - Период времени рыночных данных, используемый для повторного вызова callback.
        *   `callback`: `Action` - Метод, который вызывается каждый раз, когда проходит указанный период времени рыночных данных.

*   **`UnsubscribeFromTimer(TimeSpan period, Action callback)`**
    *   **Сигнатура:** `void UnsubscribeFromTimer (TimeSpan period, Action callback)`
    *   **Описание:** Отменяет регистрацию функции обратного вызова от периодических вызовов.
    *   **Параметры:**
        *   `period`: `TimeSpan` - Подписанный период вызовов.
        *   `callback`: `Action` - Метод, который нужно отменить.

**6. Свойства (Properties)**

*   **`Container`**
    *   **Сигнатура:** `IIndicatorContainer? Container { get; set; }`
    *   **Описание:** Возвращает или устанавливает контейнер, в котором размещен индикатор.

*   **`DataProvider`**
    *   **Сигнатура:** `IIndicatorDataProvider? DataProvider { get; set; }`
    *   **Описание:** Возвращает или устанавливает поставщика данных для индикатора.

*   **`DenyToChangePanel`**
    *   **Сигнатура:** `bool DenyToChangePanel { get; set; }`
    *   **Описание:** Возвращает или устанавливает значение, указывающее, запрещено ли изменение панели индикатора. Для вертикальных индикаторов это всегда запрещено.

*   **`DrawAbovePrice`**
    *   **Сигнатура:** `bool DrawAbovePrice { get; set; }`
    *   **Описание:** Возвращает или устанавливает значение, указывающее, следует ли выполнять пользовательскую отрисовку поверх отрисованных баров.

*   **`EnableCustomDrawing`**
    *   **Сигнатура:** `bool EnableCustomDrawing { get; protected set; }`
    *   **Описание:** Возвращает или устанавливает значение, указывающее, включена ли пользовательская отрисовка для индикатора.

*   **`FullScreenMode`**
    *   **Сигнатура:** `bool FullScreenMode { get; set; }`
    *   **Описание:** Возвращает или устанавливает полноэкранный режим для индикатора. Может применяться только к вертикальным индикаторам (`IsVerticalIndicator = true`).

*   **`HorizontalLinesTillTouch`**
    *   **Сигнатура:** `List<LineTillTouch> HorizontalLinesTillTouch { get; }`
    *   **Описание:** Возвращает коллекцию горизонтальных линий, используемых для отслеживания точек касания на графике.

*   **`Labels`**
    *   **Сигнатура:** `SyncDictionary<string, DrawingText> Labels { get; set; }`
    *   **Описание:** Возвращает или устанавливает коллекцию именованных текстовых элементов рисования.

*   **`MarketByOrders`**
    *   **Сигнатура:** `IEnumerable<MarketByOrder> MarketByOrders { get; protected }`
    *   **Описание:** Возвращает снимок текущих данных Market by Order.

*   **`MarketTime`**
    *   **Сигнатура:** `DateTime MarketTime { get; }`
    *   **Описание:** Текущее рыночное время.

*   **`Rectangles`**
    *   **Сигнатура:** `List<DrawingRectangle> Rectangles { get; }`
    *   **Описание:** Возвращает коллекцию прямоугольников на графике.

*   **`ShowDescription`**
    *   **Сигнатура:** `bool ShowDescription { get; set; }`
    *   **Описание:** Возвращает или устанавливает значение, указывающее, следует ли отображать описание индикатора.

*   **`TrendLines`**
    *   **Сигнатура:** `List<TrendLine> TrendLines { get; }`
    *   **Описание:** Возвращает коллекцию линий тренда на графике.

*   **`UtcTime`**
    *   **Сигнатура:** `DateTime UtcTime { get; }`
    *   **Описание:** Текущее время UTC.

**7. Дополнительные унаследованные члены**

*   **Статические защищенные методы, унаследованные от `ATAS.Indicators.BaseIndicator`**
*   **Защищенные атрибуты, унаследованные от `ATAS.Indicators.BaseIndicator`**
*   **События, унаследованные от `ATAS.Indicators.BaseIndicator`**
*   **События, унаследованные от `ATAS.Indicators.Filters.TrackedPropertyBase`**
*   **События, унаследованные от `ATAS.Indicators.INotifyPanelPropertyChanged`**

**8. Подробное описание**

Расширенный базовый класс для пользовательских индикаторов, который предоставляет дополнительную функциональность для рисования, алертов, обработки рыночных данных и т.д.

**9. Файл документации**

*   `Baselndicator.cs`







## Методичка по классу `ATAS.Indicators.Filter<TValue>` для создания индикаторов ATAS

### 1. Общая информация о классе

**Название класса:** `ATAS.Indicators.Filter<TValue>`

**Описание:**  Универсальный класс фильтров, реализующий интерфейс `IFilterValue`.

*   Представляет фильтр со значением десятичного типа. Наследуется от `Filter<TValue, TFilter>`, где `TValue` установлен как `decimal`, а `TFilter` установлен как `Filter`. Сериализация JSON фильтра обрабатывается пользовательским конвертером `FilterJsonConverter`.
*   Представляет фильтр со значением определенного типа `TValue`. Наследуется от `Filter<TValue, TFilter>`, где `TFilter` установлен как `Filter<TValue>`.

**Реализуемые интерфейсы:** `IFilterValue`

**Диаграмма наследования:**

```
INotifyPropertyChanged
    ICloneable
        NotifyPropertyChangedBase
            IFilter
                Filter<TValue, Filter<TValue>>
                    FilterBase
                        IFilterValue
                            ATAS.Indicators.Filter<TValue>
```

**Диаграмма взаимодействия:**

*(Аналогична диаграмме наследования)*

### 2. Параметры шаблона (Template Parameters)

*   **`TValue`**:  Тип значения фильтра. Тип значения, хранимого фильтром.
*   **`TFilter`**: Тип производного фильтра.

### 3. Конструкторы (Constructor & Destructor Documentation)

#### 3.1. `Filter()` [1/6]

```csharp
ATAS.Indicators.Filter<TValue>.Filter(
    bool enabledVisible,
    bool asScale = false
)
```

**Описание:** Инициализирует новый экземпляр класса `Filter<TValue, TFilter>` с указанными параметрами.

**Параметры:**

*   `enabledVisible`:  `bool` - Указывает, видимо ли свойство "Enabled" для пользователей.
*   `asScale`: `bool` - Указывает, работает ли фильтр в скалярном режиме.

#### 3.2. `Filter()` [2/6]

```csharp
ATAS.Indicators.Filter<TValue>.Filter()
```

**Описание:** Инициализирует новый экземпляр класса `Filter<TValue, TFilter>` с параметрами по умолчанию.

#### 3.3. `Filter()` [3/6]

```csharp
ATAS.Indicators.Filter<TValue>.Filter(
    bool enabledVisible,
    bool asScalar = false
)
```

**Описание:** Инициализирует новый экземпляр класса `Filter<TValue>` с указанной видимостью свойства `Enabled` и скалярным значением.

**Параметры:**

*   `enabledVisible`: `bool` -  `true`, если свойство `Enabled` должно быть видимым; иначе `false`.
*   `asScalar`: `bool` - `true`, если фильтр содержит скалярное значение; иначе `false`.

#### 3.4. `Filter()` [4/6]

```csharp
ATAS.Indicators.Filter<TValue>.Filter()
```

**Описание:** Инициализирует новый экземпляр класса `Filter<TValue>` с настройками видимости по умолчанию.

#### 3.5. `Filter()` [5/6]

```csharp
ATAS.Indicators.Filter<TValue>.Filter(
    bool enableVisible,
    bool asScalar = false
)
```

**Описание:** Инициализирует новый экземпляр класса `Filter` с указанной видимостью свойства `Enabled` и скалярным значением.

**Параметры:**

*   `enableVisible`: `bool` - `true`, если свойство `Enabled` должно быть видимым; иначе `false`.
*   `asScalar`: `bool` - `true`, если фильтр содержит скалярное значение; иначе `false`.

#### 3.6. `Filter()` [6/6]

```csharp
ATAS.Indicators.Filter<TValue>.Filter()
```

**Описание:** Инициализирует новый экземпляр класса `Filter` с настройками видимости по умолчанию.

### 4. Публичные методы (Public Member Functions)

*   **`override bool Equals(object? obj)`**
    *   Описание: *(Не указано в документе, но, вероятно, стандартная реализация `Equals` для сравнения объектов).*
*   **`override int GetHashCode()`**
    *   Описание: *(Не указано в документе, но, вероятно, стандартная реализация `GetHashCode` для получения хеш-кода объекта).*
*   **`bool SetValueSilently(TValue value)`**
    *   Описание: Устанавливает значение свойства `Value`. Возвращает `false`, если новое значение равно новому значению.
    *   Параметры:
        *   `value`: `TValue` -  Значение.
    *   Возвращает: `bool`
*   **`TFilter ValueOnChanging(Func<ValueChangingEventArgs<TValue>, TValue> onChanging)`**
    *   Описание: Устанавливает функцию, вызываемую, когда значение фильтра изменяется.
    *   Параметры:
        *   `onChanging`: `Func<ValueChangingEventArgs<TValue>, TValue>` - Функция, вызываемая при изменении значения фильтра.
    *   Возвращает: `TFilter`
*   **`TFilter ValueOnChanged(Action<TValue> onChanged)`**
    *   Описание: Устанавливает действие, которое будет вызвано, когда значение фильтра изменится.
    *   Параметры:
        *   `onChanged`: `Action<TValue>` - Действие, вызываемое при изменении значения фильтра.
    *   Возвращает: `TFilter`
*   **`override string ToString()`**
    *   Описание: Преобразует фильтр в строковое представление.
    *   Возвращает: `string` - Строка, представляющая фильтр.
*   **`override object Clone()`**
    *   Описание: *(Не указано в документе, но, вероятно, создает копию объекта фильтра).*
    *   Возвращает: `object`

### 5. Статические публичные методы (Static Public Member Functions)

*   **`static bool operator ==(Filter<TValue, TFilter>? left, Filter<TValue, TFilter>? right)`**
    *   Описание: *(Оператор равенства для сравнения двух фильтров).*
    *   Параметры:
        *   `left`: `Filter<TValue, TFilter>?` - Левый операнд.
        *   `right`: `Filter<TValue, TFilter>?` - Правый операнд.
    *   Возвращает: `bool`
*   **`static bool operator !=(Filter<TValue, TFilter>? left, Filter<TValue, TFilter>? right)`**
    *   Описание: *(Оператор неравенства для сравнения двух фильтров).*
    *   Параметры:
        *   `left`: `Filter<TValue, TFilter>?` - Левый операнд.
        *   `right`: `Filter<TValue, TFilter>?` - Правый операнд.
    *   Возвращает: `bool`
*   **`static explicit operator TValue(Filter<TValue, TFilter> other)`**
    *   Описание: Преобразует `Filter<TValue, TFilter>` к его значению типа `TValue`.
    *   Параметры:
        *   `other`: `Filter<TValue, TFilter>` - Фильтр для преобразования.
    *   Возвращает: `TValue`

### 6. Защищенные методы (Protected Member Functions)

*   **`bool Equals(Filter<TValue, TFilter> other)`**
    *   Описание: *(Вероятно, защищенная реализация `Equals` для сравнения фильтров).*
    *   Параметры:
        *   `other`: `Filter<TValue, TFilter>` - Другой фильтр для сравнения.
    *   Возвращает: `bool`
*   **`virtual ? TValue GetRealValue(TValue? value)`**
    *   Описание: *(Вероятно, получение "реального" значения фильтра, возможно, с какой-то дополнительной логикой).*
    *   Параметры:
        *   `value`: `TValue?` - Значение.
    *   Возвращает: `? TValue` *(Тип может быть nullable TValue)*
*   **`void RaiseValueOnChanged()`**
    *   Описание: Вызывает событие `NotifyPropertyChangedBase.PropertyChanged` для свойства `Value` и вызывает действие изменения значения.
*   **`virtual TValue ValueOnChanging(TValue? oldValue, TValue? newValue)`** [2/2]
    *   Описание: Вызывается, когда значение фильтра изменяется.
    *   Параметры:
        *   `oldValue`: `TValue?` - Старое значение фильтра.
        *   `newValue`: `TValue?` - Новое значение фильтра.
    *   Возвращает: `TValue` - Новое значение, которое будет установлено для фильтра.
*   **`virtual TFilter CreateNew()`**
    *   Описание: Создает новый экземпляр производного типа фильтра.
    *   Возвращает: `TFilter` - Новый экземпляр производного типа фильтра.

### 7. Свойства (Properties)

*   **`TValue Value { get; set; }`**
    *   Описание: Получает или устанавливает значение фильтра.
    *   Реализует интерфейс: `ATAS.Indicators.IFilterValue`.

### 8. Дополнительные унаследованные члены (Additional Inherited Members)

*   **Защищенные атрибуты, унаследованные от `ATAS.Indicators.FilterBase`**
*   **События, унаследованные от `ATAS.Indicators.NotifyPropertyChangedBase`**

### 9. Расположение файла с кодом класса

*   `Filter.cs`






## Методичка по классу `ATAS.Indicators.FilterBase` для создания индикаторов ATAS

### Заголовок класса

**Название класса:** `ATAS.Indicators.FilterBase`

**Описание:** Базовый класс для фильтров, реализующих интерфейс `IFilter`.

**Диаграмма наследования:**

```
INotifyPropertyChanged
    ICloneable
        NotifyPropertyChangedBase
            IFilter
                ATAS.Indicators.FilterBase
                    ATAS.Indicators.Filter<TValue>
```

*Аннотация: Диаграмма наследования показывает, от каких классов наследуется `FilterBase`. Это помогает понять его структуру и унаследованные свойства.*

**Диаграмма взаимодействия:**

```
INotifyPropertyChanged
    ICloneable
        NotifyPropertyChangedBase
            IFilter
                ATAS.Indicators.FilterBase
```

*Аннотация: Диаграмма взаимодействия показывает, с какими интерфейсами и классами взаимодействует `FilterBase`. Это дает представление о его роли в системе.*

### Публичные функции-члены

*   `abstract object Clone()`

    *Аннотация: `abstract` означает, что функция должна быть переопределена в производных классах.*
    *Аннотация: `object` — это общий тип данных в C#, от которого наследуются все остальные типы.*
    *Аннотация: `Clone()` — функция для создания копии объекта.*

### Защищенные функции-члены

*   `FilterBase(bool enabledVisible, bool asScalar)`

    *Аннотация: `protected` означает, что функция доступна внутри класса и в его производных классах.*
    *Аннотация: `bool enabledVisible` — параметр типа `bool` (логическое значение), определяющий видимость свойства "Enabled" для пользователя.*
    *Аннотация: `bool asScalar` — параметр типа `bool`, определяющий, работает ли фильтр в скалярном режиме.*
    *Аннотация: Инициализирует новый экземпляр класса `FilterBase` с указанными параметрами.

*   `FilterBase()`

    *Аннотация: Это еще один конструктор с тем же именем, но без параметров (перегрузка конструктора).*
    *Аннотация: Инициализирует новый экземпляр класса `FilterBase` с параметрами по умолчанию.

### Защищенные атрибуты

*   `readonly bool _asScalar`

    *Аннотация: `readonly` означает, что значение атрибута может быть установлено только в конструкторе класса.*
    *Аннотация: `bool` — тип данных (логическое значение).*
    *Аннотация: `_asScalar` — имя атрибута, обычно начинающееся с `_` для обозначения приватного или защищенного члена.*

### Свойства

*   `bool Enabled [get, set]`

    *Аннотация: `bool` — тип данных свойства (логическое значение).*
    *Аннотация: `Enabled` — имя свойства.*
    *Аннотация: `[get, set]` — означает, что свойство имеет методы доступа для чтения (`get`) и записи (`set`) значения.*
    *Аннотация: Возвращает или устанавливает значение, указывающее, включен ли фильтр.

*   `bool EnabledVisible [get]`

    *Аннотация: `bool` — тип данных свойства (логическое значение).*
    *Аннотация: `EnabledVisible` — имя свойства.*
    *Аннотация: `[get]` — означает, что свойство имеет только метод доступа для чтения (`get`), значение нельзя изменить напрямую извне.*
    *Аннотация: Возвращает значение, указывающее, видна ли пользователям видимость свойства "Enabled".

### Унаследованные защищенные функции-члены

*   Унаследованы от `ATAS.Indicators.NotifyPropertyChangedBase`

    *Аннотация: Указывает, что класс `FilterBase` наследует защищенные функции-члены от класса `NotifyPropertyChangedBase`. Детали этих функций нужно смотреть в документации `NotifyPropertyChangedBase`.*

### Унаследованные свойства

*   Унаследованы от `ATAS.Indicators.IFilter`

    *Аннотация: Указывает, что класс `FilterBase` наследует свойства от интерфейса `IFilter`. Детали этих свойств нужно смотреть в документации `IFilter`.*

### Дополнительные унаследованные члены

*   Унаследованы от `ATAS.Indicators.NotifyPropertyChangedBase`

    *Аннотация: Указывает на наличие других унаследованных членов, возможно, методов или событий, от класса `NotifyPropertyChangedBase`.*

### События, унаследованные от `ATAS.Indicators.NotifyPropertyChangedBase`

*   Унаследованы от `ATAS.Indicators.NotifyPropertyChangedBase`

    *Аннотация: Указывает, что класс `FilterBase` наследует события от класса `NotifyPropertyChangedBase`. События используются для оповещения об изменениях в объекте.*

### Подробное описание

Базовый класс для фильтров, реализующих интерфейс `IFilter`.

### Документация конструктора и деструктора

*   `FilterBase() [1/2]`

    *   `ATAS.Indicators.FilterBase.FilterBase (bool enabledVisible, bool asScalar)` `protected`

        *Аннотация: `protected` — модификатор доступа, конструктор защищенный.*
        *Аннотация: Полное имя конструктора, включая пространство имен и класс.*
        *Аннотация: Параметры конструктора: `enabledVisible` и `asScalar` типа `bool`.*
        *Аннотация: Инициализирует новый экземпляр класса `FilterBase` с указанными параметрами.

        **Параметры:**

        *   `enabledVisible`: Определяет, видимо ли свойство "Enabled" для пользователей.
        *   `asScalar`: Определяет, работает ли фильтр в скалярном режиме.

*   `FilterBase() [2/2]`

    *   `ATAS.Indicators.FilterBase.FilterBase ()` `protected`

        *Аннотация: `protected` — модификатор доступа, конструктор защищенный.*
        *Аннотация: Полное имя конструктора, включая пространство имен и класс.*
        *Аннотация: Конструктор без параметров.*
        *Аннотация: Инициализирует новый экземпляр класса `FilterBase` с параметрами по умолчанию.

### Документация функций-членов

*   `Clone()`

    *   `abstract object ATAS.Indicators.FilterBase.Clone()` `pure virtual`

        *Аннотация: `abstract` — функция абстрактная, должна быть реализована в производных классах.*
        *Аннотация: `pure virtual` —  указывает на виртуальную функцию (для полиморфизма) и `abstract`, то есть без реализации в текущем классе.*
        *Аннотация: Полное имя функции, включая пространство имен, класс и имя функции.*
        *Аннотация: Реализовано в `ATAS.Indicators.Filter<TValue>`.

### Документация данных-членов

*   `_asScalar`

    *   `readonly bool ATAS.Indicators.FilterBase._asScalar` `protected`

        *Аннотация: `readonly` —  значение может быть установлено только в конструкторе.*
        *Аннотация: `bool` — тип данных (логическое значение).*
        *Аннотация: `protected` — модификатор доступа, атрибут защищенный.*
        *Аннотация: Полное имя атрибута, включая пространство имен, класс и имя атрибута.*

### Документация свойств

*   `Enabled`

    *   `bool ATAS.Indicators.FilterBase.Enabled` `get set`

        *Аннотация: `bool` — тип данных свойства (логическое значение).*
        *Аннотация: `get set` — свойство доступно для чтения и записи.*
        *Аннотация: Полное имя свойства, включая пространство имен, класс и имя свойства.*
        *Аннотация: Возвращает или устанавливает значение, указывающее, включен ли фильтр.
        *Аннотация: Реализует `ATAS.Indicators.IFilter`.

*   `EnabledVisible`

    *   `bool ATAS.Indicators.FilterBase.EnabledVisible` `get`

        *Аннотация: `bool` — тип данных свойства (логическое значение).*
        *Аннотация: `get` — свойство доступно только для чтения.*
        *Аннотация: Полное имя свойства, включая пространство имен, класс и имя свойства.*
        *Аннотация: Возвращает значение, указывающее, видна ли пользователям видимость свойства "Enabled".
        *Аннотация: Реализует `ATAS.Indicators.IFilter`.

### Файл документации для этого класса был сгенерирован из следующего файла:

*   `FilterBase.cs`

    *Аннотация: Указывает имя файла с исходным кодом, где определен класс `FilterBase`.*







## Методическое описание класса `ATAS.Indicators.FilterBool`

### Общая информация

Класс `ATAS.Indicators.FilterBool` представляет собой фильтр с логическим (boolean) типом значения. Он наследуется от класса `Filter<TValue, TFilter>`, где `TValue` установлен как `bool`, а `TFilter` установлен как `FilterBool`.

### Иерархия наследования

```
Filter< bool, FilterBool >
  |
  └── ATAS.Indicators.FilterBool
```

### Диаграмма взаимодействия

```
Filter< bool, FilterBool >
  |
  └── ATAS.Indicators.FilterBool
```

### Открытые члены класса (Public Member Functions)

#### Конструкторы

1.  `FilterBool(bool enableVisible, bool asScalar = false)`

    *   **Описание:** Инициализирует новый экземпляр класса `FilterBool` с указанной видимостью свойства `FilterBase.Enabled` и скалярным значением.
    *   **Параметры:**
        *   `enableVisible` (bool):
            *   `true`: если свойство `FilterBase.Enabled` должно быть видимым.
            *   `false`: в противном случае.
        *   `asScalar` (bool, опционально, по умолчанию `false`):
            *   `true`: если фильтр содержит скалярное значение.
            *   `false`: в противном случае.

2.  `FilterBool()`

    *   **Описание:** Инициализирует новый экземпляр класса `FilterBool` с настройками видимости по умолчанию.

### Дополнительные унаследованные члены (Additional Inherited Members)

*   **Статические открытые члены (Static Public Member Functions)**, унаследованные от `ATAS.Indicators.Filter< bool, FilterBool >`.
*   **Защищенные члены (Protected Member Functions)**, унаследованные от `ATAS.Indicators.Filter< bool, FilterBool >`.
*   **Свойства (Properties)**, унаследованные от `ATAS.Indicators.Filter< bool, FilterBool >`.

### Подробное описание

Класс `FilterBool` предназначен для создания фильтров, оперирующих с логическими значениями. Он является специализированной версией более общего класса `Filter<TValue, TFilter>`, адаптированной для работы с типом `bool`.

### Документация конструкторов и деструкторов

#### `FilterBool() [1/2]`

```csharp
ATAS.Indicators.FilterBool.FilterBool (bool enableVisible,
                                     bool asScalar = false
                                    )
```

*   **Описание:** Инициализирует новый экземпляр класса `FilterBool` с указанной видимостью свойства `FilterBase.Enabled` и скалярным значением.
*   **Параметры:**
    *   `enableVisible`: `true`, если свойство `FilterBase.Enabled` должно быть видимым; иначе `false`.
    *   `asScalar`: `true`, если фильтр содержит скалярное значение; иначе `false`.

#### `FilterBool() [2/2]`

```csharp
ATAS.Indicators.FilterBool.FilterBool ()
```

*   **Описание:** Инициализирует новый экземпляр класса `FilterBool` с настройками видимости по умолчанию.

### Файл, в котором сгенерирована документация для этого класса:

*   `FilterBool.cs`






---

## Методичка для ИИ: Класс ATAS.Indicators.FilterColor

**Цель:** Данная методичка описывает класс `ATAS.Indicators.FilterColor` из API платформы ATAS. Информация предназначена для использования ИИ при генерации кода индикаторов для ATAS.

**1. Описание Класса**

*   **Полное имя:** `ATAS.Indicators.FilterColor`
*   **Назначение:** Представляет собой фильтр, значением которого является цвет типа `CrossColor`. Этот тип цвета обычно используется для отображения пересечений или условий (например, бычий/медвежий цвет). Класс `FilterColor` позволяет пользователю индикатора выбирать или настраивать этот цвет через настройки индикатора в ATAS.
*   **Наследование:**
    *   Наследуется от базового класса `Filter<TValue, TFilter>`.
    *   В данном случае, `TValue` установлен как `CrossColor`, а `TFilter` как `FilterColor`.
    *   Это означает, что `FilterColor` является специализированной версией `Filter` для работы со значениями типа `CrossColor` и предоставляет стандартные свойства и методы фильтра (например, `Value` для получения текущего цвета, `Enabled` для включения/выключения фильтра), адаптированные для цвета.

**2. Конструкторы Класса**

Класс `FilterColor` имеет два публичных конструктора для создания его экземпляров:

*   **Конструктор 1:** `FilterColor(bool enableVisible, bool asScalar = false)`
    *   **Назначение:** Инициализирует новый экземпляр класса `FilterColor` с явно заданными параметрами видимости и скалярности.
    *   **Параметры:**
        *   `enableVisible` (тип `bool`): Определяет, будет ли элемент управления для выбора цвета (связанный с этим фильтром) виден пользователю в окне настроек индикатора.
            *   `true`: Элемент управления будет виден.
            *   `false`: Элемент управления будет скрыт.
        *   `asScalar` (тип `bool`, значение по умолчанию `false`): Указывает, должен ли фильтр хранить скалярное значение.
            *   `true`: Фильтр рассматривается как хранящий одно, скалярное значение.
            *   `false` (по умолчанию): Фильтр может представлять более сложную структуру или диапазон (хотя для `FilterColor` это менее типично, параметр унаследован от базового класса).
    *   **Пример использования (в коде индикатора):**
        ```csharp
        // Создать видимый фильтр цвета
        FilterColor myVisibleColorFilter = new FilterColor(true); 
        
        // Создать скрытый фильтр цвета
        FilterColor myHiddenColorFilter = new FilterColor(false); 
        ```

*   **Конструктор 2:** `FilterColor()`
    *   **Назначение:** Инициализирует новый экземпляр класса `FilterColor` с настройками видимости по умолчанию. Документация не указывает явно, каковы эти настройки по умолчанию, но обычно это означает, что элемент управления будет виден (`enableVisible = true`).
    *   **Параметры:** Нет.
    *   **Пример использования (в коде индикатора):**
        ```csharp
        // Создать фильтр цвета с настройками по умолчанию (вероятно, видимый)
        FilterColor defaultColorFilter = new FilterColor(); 
        ```

**3. Наследуемые Члены**

*   Класс `FilterColor` наследует различные члены (публичные методы, статические публичные методы, защищенные методы, свойства) от своего базового класса `ATAS.Indicators.Filter<CrossColor, FilterColor>`.
*   Ключевые унаследованные свойства, которые часто используются:
    *   `Value` (тип `CrossColor`): Содержит текущее значение цвета, выбранное пользователем или установленное по умолчанию.
    *   `Enabled` (тип `bool`): Позволяет программно включать или выключать использование этого фильтра в логике индикатора.
    *   Другие свойства и методы, общие для всех фильтров в ATAS (управление видимостью, состоянием и т.д.).

**4. Использование в Индикаторах ATAS**

*   Экземпляры `FilterColor` объявляются как поля внутри класса индикатора.
*   Они автоматически отображаются в окне настроек индикатора (если `enableVisible` установлено в `true` при создании), позволяя пользователю выбрать цвет.
*   В методе `OnCalculate` индикатора значение фильтра (`Value`) используется для определения цвета отрисовки графических элементов (линий, точек, областей и т.д.).

**5. Исходный Файл**

*   Определение класса находится в файле: `FilterColor.cs`

---










## Методичка по классу `ATAS.Indicators.FilterEnum<TEnum>` для создания индикаторов ATAS

### Описание класса `ATAS.Indicators.FilterEnum<TEnum>`

**Template Reference** -  *Шаблон класса*

Класс `ATAS.Indicators.FilterEnum<TEnum>` является шаблоном класса, что означает, что он может быть использован с различными типами перечислений (Enum). `<TEnum>` обозначает, что при использовании класса необходимо указать конкретный тип перечисления.

**Диаграмма наследования для `ATAS.Indicators.FilterEnum<TEnum>`:**

```
INotifyPropertyChanged  - Интерфейс для уведомления об изменении свойств объектов.
    |
    +-- IFilter  - Интерфейс фильтрации данных.
        |
        +-- IFilterEnum - Интерфейс для фильтрации на основе перечислений.
            |
            +-- Filter<TEnum, FilterEnum<TEnum>> - Базовый класс фильтра, параметризованный типом перечисления и самим собой.
                |
                +-- ATAS.Indicators.FilterEnum<TEnum> -  **Целевой класс FilterEnum, предназначенный для фильтрации по перечислениям в индикаторах ATAS.**
```

*Аннотация: Диаграмма наследования показывает, что `FilterEnum<TEnum>` является частью иерархии классов, предназначенных для фильтрации данных в ATAS. Он наследует функциональность от интерфейсов `INotifyPropertyChanged`, `IFilter`, `IFilterEnum` и базового класса `Filter<TEnum, FilterEnum<TEnum>>`.*

**Диаграмма взаимодействия для `ATAS.Indicators.FilterEnum<TEnum>`:**

```
INotifyPropertyChanged  - Интерфейс для уведомления об изменении свойств объектов.
    |
    +-- IFilter  - Интерфейс фильтрации данных.
        |
        +-- IFilterEnum - Интерфейс для фильтрации на основе перечислений.
            |
            +-- Filter<TEnum, FilterEnum<TEnum>> - Базовый класс фильтра, параметризованный типом перечисления и самим собой.
                |
                +-- ATAS.Indicators.FilterEnum<TEnum> - **Целевой класс FilterEnum.**
```

*Аннотация: Диаграмма взаимодействия, по сути, повторяет диаграмму наследования, подчеркивая взаимосвязь между классами и интерфейсами, участвующими в реализации `FilterEnum<TEnum>`.*

### Public Member Functions (Открытые методы класса)

*Список всех открытых методов, доступных для использования с экземплярами класса `ATAS.Indicators.FilterEnum<TEnum>`.*

1.  **`FilterEnum(bool enabledVisible, bool asScalar=false)`**

    *   **`FilterEnum`** - *Конструктор класса*.
    *   **`bool enabledVisible`** - *Параметр, определяющий, является ли фильтр видимым и включенным по умолчанию.*
    *   **`bool asScalar=false`** - *Необязательный параметр (по умолчанию `false`).  Вероятно, определяет, как фильтр обрабатывает скалярные значения (единичные значения, а не массивы).*

2.  **`FilterEnum()`**

    *   **`FilterEnum`** - *Конструктор класса без параметров*. *Вероятно, создает экземпляр `FilterEnum` с настройками по умолчанию.*

*Аннотация: Класс `FilterEnum<TEnum>` предоставляет два конструктора. Первый позволяет задать начальную видимость и режим обработки скалярных значений. Второй конструктор создает объект с настройками по умолчанию.*

### Public Member Functions inherited from `ATAS.Indicators.Filter<TEnum, FilterEnum<TEnum>>`

*Открытые методы, унаследованные от базового класса `ATAS.Indicators.Filter<TEnum, FilterEnum<TEnum>>`.  Для подробной информации необходимо обратиться к документации класса `ATAS.Indicators.Filter<TEnum, FilterEnum<TEnum>>`.*

### Properties (Свойства)

*Список свойств класса, доступных для чтения и записи (если не указано иное).*

1.  **`EnumType`**

    *   **`Type EnumType = typeof(TEnum) [get]`**
        *   **`Type EnumType`** - *Свойство, возвращающее тип перечисления, с которым работает данный экземпляр `FilterEnum<TEnum>`.*
        *   **`typeof(TEnum)`** - *Оператор `typeof` возвращает объект `Type`, представляющий тип `TEnum`, который был указан при создании экземпляра `FilterEnum<TEnum>`.*
        *   **`[get]`** - *Указывает, что свойство доступно только для чтения (get accessor).*

*Аннотация: Свойство `EnumType` позволяет получить тип перечисления, который используется в текущем фильтре. Это может быть полезно для динамической работы с фильтром, зная тип данных, которые он фильтрует.*

### Properties inherited from

*   `ATAS.Indicators.Filter<TEnum, FilterEnum<TEnum>>`
*   `ATAS.Indicators.IFilterEnum`
*   `ATAS.Indicators.IFilter`

*Свойства, унаследованные от указанных классов и интерфейсов. Для полного списка и описания свойств необходимо обратиться к документации соответствующих классов и интерфейсов.*

### Additional Inherited Members (Дополнительные унаследованные члены)

*   **Static Public Member Functions inherited from `ATAS.Indicators.Filter<TEnum, FilterEnum<TEnum>>`**
*   **Protected Member Functions inherited from `ATAS.Indicators.Filter<TEnum, FilterEnum<TEnum>>`**

*Статические открытые и защищенные методы, унаследованные от базового класса `ATAS.Indicators.Filter<TEnum, FilterEnum<TEnum>>`.  Детализация методов требует обращения к документации класса `ATAS.Indicators.Filter<TEnum, FilterEnum<TEnum>>`.*

### Constructor & Destructor Documentation (Документация конструкторов и деструкторов)

#### `FilterEnum() [1/2]`

```csharp
ATAS.Indicators.FilterEnum<TEnum>.FilterEnum (
    bool enabledVisible,
    bool asScalar = false
)
```

*   **`ATAS.Indicators.FilterEnum<TEnum>.FilterEnum`** - *Полное имя конструктора класса `ATAS.Indicators.FilterEnum<TEnum>`.*
*   **`bool enabledVisible`** - *Первый параметр конструктора, определяющий видимость и включение фильтра.*
*   **`bool asScalar = false`** - *Второй, необязательный параметр, определяющий режим обработки скалярных значений (по умолчанию `false`).*

#### `FilterEnum() [2/2]`

```csharp
ATAS.Indicators.FilterEnum<TEnum>.FilterEnum ()
```

*   **`ATAS.Indicators.FilterEnum<TEnum>.FilterEnum`** - *Полное имя второго конструктора класса `ATAS.Indicators.FilterEnum<TEnum>`.*
*   *Конструктор без параметров.*

*Аннотация: Раздел документации подробно описывает сигнатуры и параметры обоих конструкторов класса `FilterEnum<TEnum>`, предоставляя информацию о том, как создавать экземпляры класса с различными настройками.*

### Property Documentation (Документация свойств)

#### `EnumType`

```csharp
Type ATAS.Indicators.FilterEnum<TEnum>.EnumType = typeof(TEnum)
```

*   **`Type ATAS.Indicators.FilterEnum<TEnum>.EnumType`** - *Полное имя свойства `EnumType` класса `ATAS.Indicators.FilterEnum<TEnum>`.*
*   **`Type`** - *Тип данных свойства - `Type` (представляет информацию о типе).*
*   **`typeof(TEnum)`** - *Значение свойства - тип перечисления `TEnum`.*
*   **`Implements ATAS.Indicators.IFilterEnum.`** - *Указывает, что свойство `EnumType` реализует свойство с тем же именем из интерфейса `ATAS.Indicators.IFilterEnum`.*

*Аннотация: Раздел документации детально описывает свойство `EnumType`, подтверждая его назначение - предоставлять информацию о типе перечисления, используемого фильтром, и указывает на реализацию интерфейса `IFilterEnum`.*

### Файл документации

*   **`FilterEnum.cs`** - *Указывает, что документация для класса `ATAS.Indicators.FilterEnum<TEnum>` была сгенерирована из файла исходного кода `FilterEnum.cs`.*

*Аннотация:  Информация о файле исходного кода помогает понять, где искать реализацию класса в проекте.*







## Методичка по классу `ATAS.Indicators.FilterHeatmapTypes` для создания индикаторов ATAS

### Описание класса `ATAS.Indicators.FilterHeatmapTypes`

Представляет фильтр для типов тепловых карт с пользовательской JSON-сериализацией/десериализацией.

### Диаграмма наследования

```
Filter< HeatmapTypes,
FilterHeatmap Types >
    ↑
ATAS.Indicators.FilterHeatmapTypes
```

### Диаграмма взаимодействия

```
Filter< HeatmapTypes,
FilterHeatmap Types >
    ↑
ATAS.Indicators.FilterHeatmapTypes
```

### Открытые члены класса (Public Member Functions)

* **`FilterHeatmapTypes(bool enabledVisible, bool asScalar=false)`**
    * Инициализирует новый экземпляр класса `FilterHeatmapTypes`.
    * **Параметры:**
        * `enabledVisible`: Значение типа `bool`, указывающее, видимо ли свойство `Enabled` фильтра.
        * `asScalar`: Значение типа `bool`, указывающее, представляет ли фильтр скалярное значение.

* **`FilterHeatmapTypes()`**
    * Инициализирует новый экземпляр класса `FilterHeatmapTypes` с настройками по умолчанию.

### Унаследованные открытые члены класса (Public Member Functions inherited from `ATAS.Indicators.Filter< HeatmapTypes, FilterHeatmapTypes >`)

* *(Перечисление унаследованных открытых членов класса)* -  *Детали унаследованных членов класса смотрите в документации к классу `ATAS.Indicators.Filter`.*

### Дополнительные унаследованные члены (Additional Inherited Members)

* **Статические открытые члены класса (Static Public Member Functions inherited from `ATAS.Indicators.Filter< HeatmapTypes, FilterHeatmapTypes >`)**
    * *(Перечисление статических унаследованных членов класса)* - *Детали статических унаследованных членов класса смотрите в документации к классу `ATAS.Indicators.Filter`.*

* **Защищенные члены класса (Protected Member Functions inherited from `ATAS.Indicators.Filter< HeatmapTypes, FilterHeatmapTypes >`)**
    * *(Перечисление защищенных унаследованных членов класса)* - *Детали защищенных унаследованных членов класса смотрите в документации к классу `ATAS.Indicators.Filter`.*

* **Свойства (Properties inherited from `ATAS.Indicators.Filter< HeatmapTypes, FilterHeatmapTypes >`)**
    * *(Перечисление унаследованных свойств)* - *Детали унаследованных свойств смотрите в документации к классу `ATAS.Indicators.Filter`.*

### Подробное описание конструкторов

* **`FilterHeatmapTypes() [1/2]`**
    ```csharp
    ATAS.Indicators.FilterHeatmapTypes.FilterHeatmapTypes (bool enabledVisible,
                                                          bool asScalar = false
                                                         )
    ```
    * Инициализирует новый экземпляр класса `FilterHeatmapTypes`.
    * **Параметры:**
        * `enabledVisible`: Значение типа `bool`, указывающее, видимо ли свойство `Enabled` фильтра.
        * `asScalar`: Значение типа `bool`, указывающее, представляет ли фильтр скалярное значение.

* **`FilterHeatmapTypes() [2/2]`**
    ```csharp
    ATAS.Indicators.FilterHeatmapTypes.FilterHeatmapTypes ()
    ```
    * Инициализирует новый экземпляр класса `FilterHeatmapTypes` с настройками по умолчанию.

### Файл, в котором находится класс

* `FilterHeatmapTypes.cs`






``` методичка
## Методичка по классу `ATAS.Indicators.FilterInt` для создания индикаторов ATAS

### Описание класса `ATAS.Indicators.FilterInt`
// **Аннотация:** Заголовок раздела с названием класса.

`ATAS.Indicators.FilterInt` - Класс-ссылка.
// **Аннотация:** Указание на то, что это класс и его пространство имен.

Представляет фильтр для целочисленных значений с настраиваемой JSON-сериализацией/десериализацией.
// **Аннотация:** Описание основной функции класса - фильтрация целых чисел и работа с JSON.

[Подробнее...]
// **Аннотация:** Указание на наличие дополнительной информации (в оригинале "More..."). В методичке можно опустить или заменить на ссылку на полный документ, если это необходимо.

### Диаграмма наследования для `ATAS.Indicators.FilterInt`
// **Аннотация:** Заголовок подраздела, описывающего наследование класса.

`Filter<int, FilterInt >`  ->  `ATAS.Indicators.FilterInt`
// **Аннотация:**  Текстовое представление диаграммы наследования. `ATAS.Indicators.FilterInt` наследуется от `Filter<int, FilterInt >`.

[legend]
// **Аннотация:**  Указание на легенду к диаграмме (в методичке можно опустить, если легенда не требуется в текстовом формате).

### Диаграмма взаимодействия для `ATAS.Indicators.FilterInt`
// **Аннотация:** Заголовок подраздела, описывающего взаимодействие класса.

`Filter<int, FilterInt >`  ->  `ATAS.Indicators.FilterInt`
// **Аннотация:** Текстовое представление диаграммы взаимодействия. В данном случае она идентична диаграмме наследования.

[legend]
// **Аннотация:** Указание на легенду к диаграмме взаимодействия (в методичке можно опустить, если легенда не требуется в текстовом формате).

### Открытые публичные функции-члены
// **Аннотация:** Заголовок раздела, описывающего публичные функции класса.

#### `FilterInt (bool enableVisible, bool asScalar=false)`
// **Аннотация:** Описание первого конструктора класса `FilterInt`.

Инициализирует новый экземпляр класса `FilterInt`.
// **Аннотация:** Описание действия конструктора.

##### Параметры:
// **Аннотация:** Заголовок подраздела, описывающего параметры конструктора.

`enableVisible`  (bool) - Значение, указывающее, видимо ли свойство "Enabled" фильтра.
// **Аннотация:** Описание параметра `enableVisible` типа `bool`.

`asScalar` (bool, optional, default = `false`) - Значение, указывающее, представляет ли фильтр скалярное значение.
// **Аннотация:** Описание параметра `asScalar` типа `bool`, который является необязательным и по умолчанию равен `false`.

#### `FilterInt ()`
// **Аннотация:** Описание второго конструктора класса `FilterInt`.

Инициализирует новый экземпляр класса `FilterInt` с настройками по умолчанию.
// **Аннотация:** Описание действия конструктора по умолчанию.

### Унаследованные публичные функции-члены
// **Аннотация:** Заголовок раздела, упоминающего унаследованные публичные функции.

► Публичные функции-члены, унаследованные от `ATAS.Indicators.Filter< int, FilterInt >`
// **Аннотация:** Указание на наличие унаследованных публичных функций, но без их детального описания.

### Дополнительные унаследованные члены
// **Аннотация:** Заголовок раздела, упоминающего другие унаследованные члены класса.

► Статические публичные функции-члены, унаследованные от `ATAS.Indicators.Filter< int, FilterInt >`
// **Аннотация:** Указание на наличие унаследованных статических публичных функций.

► Защищенные функции-члены, унаследованные от `ATAS.Indicators.Filter< int, FilterInt >`
// **Аннотация:** Указание на наличие унаследованных защищенных функций.

► Свойства, унаследованные от `ATAS.Indicators.Filter< int, FilterInt >`
// **Аннотация:** Указание на наличие унаследованных свойств.

### Подробное описание
// **Аннотация:** Заголовок раздела с подробным описанием класса.

Представляет фильтр для целочисленных значений с настраиваемой JSON-сериализацией/десериализацией.
// **Аннотация:** Повторение основного описания класса.

### Документация для этого класса была сгенерирована из файла:
// **Аннотация:** Заголовок раздела, указывающего на исходный файл документации.

- `FilterInt.cs`
// **Аннотация:** Указание на имя файла, из которого была сгенерирована документация.

```






## Методичка по классу `ATAS.Indicators.FilterKey` для создания индикаторов ATAS

### 1. Описание класса

**Класс:** `ATAS.Indicators.FilterKey`

**Описание:** Представляет фильтр для ключевых значений с пользовательской JSON-сериализацией/десериализацией.

**Наследование:**  `Filter< CrossKey, FilterKey >`

**Назначение:** Класс `FilterKey` предназначен для фильтрации данных, основываясь на ключевых значениях. Он может быть использован в индикаторах ATAS для обработки и отбора определенных данных, используя механизм фильтрации по ключам.  JSON-сериализация/десериализация указывает на возможность сохранения и восстановления настроек фильтра в формате JSON.

### 2. Конструкторы класса `FilterKey`

#### 2.1. Конструктор `FilterKey(bool enableVisible, bool asScalar = true)`

**Сигнатура:** `FilterKey(bool enableVisible, bool asScalar = true)`

**Описание:** Инициализирует новый экземпляр класса `FilterKey`.

**Параметры:**

*   `enableVisible` (bool):
    *   **Описание:**  Указывает, будет ли свойство `Enabled` фильтра видимо в настройках индикатора.
    *   **Значение:**
        *   `true` - свойство `Enabled` будет отображаться и может быть изменено пользователем.
        *   `false` - свойство `Enabled` будет скрыто от пользователя.

*   `asScalar` (bool, опционально, значение по умолчанию: `true`):
    *   **Описание:** Указывает, представляет ли фильтр скалярное значение.
    *   **Значение:**
        *   `true` - фильтр обрабатывает скалярные значения (единичные значения).
        *   `false` - фильтр может обрабатывать нескалярные значения (например, массивы или объекты).

**Пример использования:**

```csharp
// Создание экземпляра FilterKey, свойство Enabled видимо, фильтр для скалярных значений
ATAS.Indicators.FilterKey filter1 = new ATAS.Indicators.FilterKey(true, true);

// Создание экземпляра FilterKey, свойство Enabled скрыто, фильтр для нескалярных значений
ATAS.Indicators.FilterKey filter2 = new ATAS.Indicators.FilterKey(false, false);
```

#### 2.2. Конструктор `FilterKey()`

**Сигнатура:** `FilterKey()`

**Описание:** Инициализирует новый экземпляр класса `FilterKey` с настройками по умолчанию.

**Параметры:** Отсутствуют.

**Настройки по умолчанию:**  Конструктор без параметров использует значения по умолчанию для свойств фильтра.  Исходя из описания конструктора `FilterKey(bool enableVisible, bool asScalar = true)`,  можно предположить, что значения по умолчанию следующие:

*   `enableVisible = false` (скорее всего, по умолчанию свойство `Enabled` скрыто, если не указано иное)
*   `asScalar = true` (по умолчанию фильтр для скалярных значений)

**Пример использования:**

```csharp
// Создание экземпляра FilterKey с настройками по умолчанию
ATAS.Indicators.FilterKey defaultFilter = new ATAS.Indicators.FilterKey();
```

### 3. Наследование и унаследованные члены

Класс `ATAS.Indicators.FilterKey` наследуется от класса `ATAS.Indicators.Filter< CrossKey, FilterKey >`. Это означает, что `FilterKey`  получает в наследство все публичные, защищенные и некоторые статические члены класса `Filter`.

Для полного понимания функциональности `FilterKey` необходимо также изучить документацию класса `ATAS.Indicators.Filter< CrossKey, FilterKey >`, чтобы узнать о доступных унаследованных членах, таких как:

*   **Публичные члены:** Методы и свойства, доступные для использования вне класса.
*   **Защищенные члены:** Методы и свойства, доступные для использования в самом классе `FilterKey` и в его производных классах (если бы они были).
*   **Статические члены:**  Общие для всех экземпляров класса `Filter`, могут предоставлять общую функциональность.
*   **Свойства:**  Характеристики фильтра, которые можно настраивать и получать.

**Для дальнейшей разработки индикаторов на основе `FilterKey` рекомендуется ознакомиться с документацией  `ATAS.Indicators.Filter< CrossKey, FilterKey >` для использования унаследованной функциональности.**

### 4. Практическое применение в индикаторах ATAS

Для использования класса `FilterKey` в индикаторе ATAS необходимо выполнить следующие шаги:

1.  **Создать экземпляр `FilterKey`:**  В коде индикатора создать переменную типа `ATAS.Indicators.FilterKey`, используя один из доступных конструкторов.
2.  **Настроить фильтр (если необходимо):**  Используя свойства унаследованные от `ATAS.Indicators.Filter< CrossKey, FilterKey >` (после изучения документации), настроить параметры фильтрации, если требуется нестандартное поведение.
3.  **Применить фильтр к данным:** В логике индикатора использовать созданный экземпляр `FilterKey` для фильтрации входных данных.  Конкретный способ применения фильтра будет зависеть от унаследованной функциональности класса `ATAS.Indicators.Filter< CrossKey, FilterKey >` и целей индикатора.

**Пример концептуального использования (требуется изучение `ATAS.Indicators.Filter`):**

```csharp
using ATAS.Indicators;
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;

namespace MyCustomIndicators
{
    [DisplayName("My Filter Indicator")]
    public class MyFilterIndicator : Indicator
    {
        // Создаем экземпляр FilterKey с настройками по умолчанию
        private readonly ATAS.Indicators.FilterKey _keyFilter = new ATAS.Indicators.FilterKey();

        public MyFilterIndicator() : base(true)
        {
            // Инициализация индикатора
            DataSeries[0] = new ValueDataSeries("Filtered Data");
        }

        protected override void OnCalculate(int bar, decimal value)
        {
            // Предположим, что у класса Filter есть метод, например, IsMatch(ключ, значение)
            // И предположим, что у нас есть некий ключ для текущего бара (например, номер бара)
            // decimal key = bar; // Пример ключа - номер бара

            //  !!!  Необходимо изучить  ATAS.Indicators.Filter< CrossKey, FilterKey >  для понимания
            //  !!!  как именно использовать FilterKey для фильтрации данных.
            //  !!!  Предположим, что есть метод для применения фильтра и получения отфильтрованного значения.

            //  !!!  Пример -  нужно заменить на реальный метод фильтрации из  ATAS.Indicators.Filter
            // if (_keyFilter.IsMatch(key, value))
            // {
            //     DataSeries[0][bar] = value; // Отображаем значение, если оно прошло фильтр
            // }
            // else
            // {
            //     DataSeries[0][bar] = 0;     // Иначе - ноль (или другое значение по умолчанию)
            // }

            //  !!!  В РЕАЛЬНОМ КОДЕ НУЖНО ИСПОЛЬЗОВАТЬ МЕТОДЫ КЛАССА Filter ДЛЯ ФИЛЬТРАЦИИ ДАННЫХ !!!
            DataSeries[0][bar] = value; // Временно - отображаем исходные данные, пока не изучим Filter
        }
    }
}
```

**Важно:**  Для полноценного использования класса `ATAS.Indicators.FilterKey`  необходимо детально изучить документацию и примеры использования класса `ATAS.Indicators.Filter< CrossKey, FilterKey >`, от которого он наследуется.  Это позволит понять, как настраивать фильтр, какие свойства и методы доступны для управления фильтрацией, и как правильно применять `FilterKey` в логике индикатора для достижения желаемого результата.








## Методичка по классу `ATAS.Indicators.FilterRangeBase<TValue, TFilter>` для создания индикаторов ATAS

### Обзор класса `ATAS.Indicators.FilterRangeBase<TValue, TFilter>`

**Название класса:** `ATAS.Indicators.FilterRangeBase<TValue, TFilter>`

**Описание:**  Абстрактный базовый класс для фильтров, которые представляют диапазон значений с пользовательской JSON-сериализацией/десериализацией.

*   **Аннотация:** Этот класс является основой для создания фильтров, которые работают с диапазонами значений. Он обеспечивает механизм для сохранения и загрузки настроек фильтра в формате JSON.

**Диаграмма наследования:**

```
Filter< FilterRangeValue<TValue>, TFilter >
  ^
  |
ATAS.Indicators.FilterRangeBase<TValue, TFilter>
```

*   **Аннотация:** `ATAS.Indicators.FilterRangeBase` наследуется от класса `Filter`, который, в свою очередь, параметризован `FilterRangeValue<TValue>` и `TFilter`. Это означает, что `FilterRangeBase` является специализированным типом `Filter` для работы с диапазонами значений.

**Диаграмма взаимодействия:**

```
Filter< FilterRangeValue<TValue>, TFilter >
  ^
  |
ATAS.Indicators.FilterRangeBase<TValue, TFilter>
```

*   **Аннотация:** Диаграмма взаимодействия показывает ту же структуру наследования, что и диаграмма наследования, подчеркивая связь между базовым классом `Filter` и производным классом `FilterRangeBase`.

### Защищенные члены класса (Protected Member Functions)

**1. Конструктор `FilterRangeBase(bool enabledVisible, bool asScale = false)` [1/2]**

*   **Сигнатура:** `ATAS.Indicators.FilterRangeBase<TValue, TFilter>.FilterRangeBase(bool enabledVisible, bool asScale = false)`
*   **Видимость:** `protected` (защищенный)
*   **Описание:** Инициализирует новый экземпляр класса `FilterRangeBase<TValue, TFilter>` с указанными настройками.
*   **Параметры:**
    *   `enabledVisible`: `bool` - Значение, указывающее, видимо ли свойство `Enabled` фильтра.
    *   `asScale`: `bool` - Значение, указывающее, представляет ли фильтр скалярное значение. По умолчанию `false`.

*   **Аннотация:** Этот конструктор позволяет создать экземпляр `FilterRangeBase` с возможностью управления видимостью свойства `Enabled` и указанием, является ли фильтр скалярным. Параметр `protected` означает, что конструктор предназначен для использования внутри класса и его наследников.

**2. Конструктор `FilterRangeBase()` [2/2]**

*   **Сигнатура:** `ATAS.Indicators.FilterRangeBase<TValue, TFilter>.FilterRangeBase()`
*   **Видимость:** `protected` (защищенный)
*   **Описание:** Инициализирует новый экземпляр класса `FilterRangeBase<TValue, TFilter>` с настройками по умолчанию.

*   **Аннотация:** Этот конструктор предоставляет способ создания объекта `FilterRangeBase` с настройками по умолчанию, что упрощает создание экземпляров класса, когда не требуется настраивать видимость `Enabled` или тип скалярности на этапе инициализации.

**3. Метод `ValueOnChanging(FilterRangeValue<TValue>? oldValue, FilterRangeValue<TValue>? newValue)`**

*   **Сигнатура:** `override FilterRangeValue<TValue> ValueOnChanging(FilterRangeValue<TValue>? oldValue, FilterRangeValue<TValue>? newValue)`
*   **Видимость:** `protected override` (защищенный, переопределенный)
*   **Описание:**  Метод, который вызывается при изменении значения фильтра. Принимает старое и новое значения типа `FilterRangeValue<TValue>`.

*   **Аннотация:** Этот метод предназначен для переопределения в производных классах. Он позволяет реализовать логику, которая должна выполняться при изменении значения фильтра. Параметры `oldValue` и `newValue` предоставляют доступ к предыдущему и текущему значениям фильтра, что полезно для реализации различных реакций на изменение значения. `override` указывает на то, что этот метод переопределяет метод из базового класса `Filter`.

### Дополнительные унаследованные члены

*   **► Public Member Functions inherited from `ATAS.Indicators.Filter< FilterRangeValue<TValue> , TFilter >`**
    *   **Аннотация:**  Указывает на наличие публичных методов, унаследованных от базового класса `Filter`. Для подробной информации необходимо обратиться к документации класса `ATAS.Indicators.Filter`.
*   **► Static Public Member Functions inherited from `ATAS.Indicators.Filter< FilterRangeValue<TValue> , TFilter >`**
    *   **Аннотация:** Указывает на наличие статических публичных методов, унаследованных от базового класса `Filter`. Статические методы принадлежат самому классу, а не его экземплярам.
*   **► Properties inherited from `ATAS.Indicators.Filter< FilterRangeValue<TValue> , TFilter >`**
    *   **Аннотация:** Указывает на наличие свойств (properties), унаследованных от базового класса `Filter`. Свойства предоставляют доступ к данным объекта через методы `get` и `set`.

### Подробное описание (Detailed Description)

**Описание:**  `FilterRangeBase` представляет собой абстрактный базовый класс для фильтров, которые представляют диапазон значений с пользовательской JSON-сериализацией/десериализацией.

*   **Аннотация:**  Повторение ключевой информации о классе, подчеркивающее его роль как основы для создания фильтров диапазонов и его способность к JSON-сериализации. Абстрактный класс означает, что нельзя создать экземпляр непосредственно `FilterRangeBase`, а нужно создавать производные классы.

**Параметры шаблона (Template Parameters):**

*   `TValue`: Тип значений в диапазоне.
    *   **Аннотация:**  Определяет тип данных, которые будут представлять значения в фильтруемом диапазоне (например, `decimal`, `int`, `double`).
*   `TFilter`: Производный класс фильтра, реализующий интерфейс `IFilterValue`.
    *   **Аннотация:**  Определяет тип фильтра, который будет использоваться для фильтрации значений. `TFilter` должен быть классом, который наследуется от `FilterBase` и реализует интерфейс `IFilterValue`.

**Ограничения типа (Type Constraints):**

*   `TFilter : FilterBase`
    *   **Аннотация:**  `TFilter` должен быть производным от класса `FilterBase`.
*   `TFilter : IFilterValue`
    *   **Аннотация:**  `TFilter` должен реализовывать интерфейс `IFilterValue`.

### Документация конструктора и деструктора (Constructor & Destructor Documentation)

*   **`FilterRangeBase() [1/2]` и `FilterRangeBase() [2/2]`**
    *   **Аннотация:**  Повторение информации о конструкторах, описанных ранее в разделе "Защищенные члены класса".

### Документация метода-члена класса (Member Function Documentation)

*   **`ValueOnChanging()`**
    *   **Аннотация:** Повторение информации о методе `ValueOnChanging()`, описанном ранее в разделе "Защищенные члены класса".

### Файл, в котором сгенерирована документация для этого класса:

*   `FilterRangeBase.cs`
    *   **Аннотация:** Указывает на имя файла исходного кода, в котором определен класс `FilterRangeBase`. Это полезно для поиска исходного кода класса в проекте ATAS.

**Заключение:**

Данная методичка предоставляет подробное описание класса `ATAS.Indicators.FilterRangeBase<TValue, TFilter>`, включая его структуру, конструкторы, методы, параметры шаблона и ограничения типов. Эта информация необходима для понимания принципов работы класса и его использования при разработке пользовательских индикаторов для платформы ATAS, которые используют фильтрацию диапазонов значений.







```csharp
// Аннотация: Объявление пространства имен ATAS.Indicators, где будет располагаться класс FilterRangeInt.
namespace ATAS.Indicators
{
    // Аннотация: Объявление класса FilterRangeInt, который представляет фильтр для диапазона целочисленных значений.
    // Класс наследуется от FilterRangeBase<int, FilterRangeInt>, что указывает на его базовую функциональность фильтра диапазона.
    public class FilterRangeInt : FilterRangeBase<int, FilterRangeInt>
    {
        // Аннотация: Конструктор класса FilterRangeInt.
        // Этот конструктор инициализирует новый экземпляр класса FilterRangeInt с заданными параметрами.
        // Параметры:
        // enabledVisible - определяет, будет ли свойство "Enabled" фильтра видимым в интерфейсе пользователя.
        // asScale - определяет, представляет ли фильтр скалярное значение (false по умолчанию).
        public FilterRangeInt(bool enabledVisible, bool asScale = false)
            : base(enabledVisible, asScale)
        {
            // Аннотация: Вызов базового конструктора FilterRangeBase<int, FilterRangeInt> с переданными параметрами.
            // Это обеспечивает инициализацию базовой функциональности фильтра.
        }

        // Аннотация: Конструктор класса FilterRangeInt без параметров.
        // Этот конструктор инициализирует новый экземпляр класса FilterRangeInt с настройками по умолчанию.
        public FilterRangeInt()
            : base(true, false) // Аннотация: Вызов базового конструктора с значениями по умолчанию: enabledVisible = true, asScale = false.
        {
            // Аннотация: Настройка значений по умолчанию для видимости и скалярности фильтра.
        }
    }
}
```







## Методичка по ATAS.Indicators.FilterRangeValue<TValue> для создания индикаторов ATAS

### Определение класса

**Класс:** `ATAS.Indicators.FilterRangeValue<TValue>`

**Описание:** Представляет диапазон значений типа `TValue` с поддержкой уведомлений об изменении свойств.

*Аннотация: Это основной класс, который позволяет задавать диапазон значений. Тип значений, которые могут храниться в диапазоне, определяется параметром `TValue`.*

### Диаграмма наследования

```
INotifyPropertyChanged
    |
    v
NotifyPropertyChangedBase
    |
    v
ATAS.Indicators.FilterRangeValue<TValue>
```

*Аннотация: Диаграмма показывает, что `FilterRangeValue<TValue>` наследуется от `NotifyPropertyChangedBase`, который в свою очередь наследуется от `INotifyPropertyChanged`. Это означает, что `FilterRangeValue<TValue>` обладает функциональностью уведомлений об изменении свойств, унаследованной от этих базовых классов.*

### Диаграмма взаимодействия

```
INotifyPropertyChanged
    ^
    |
NotifyPropertyChangedBase
    ^
    |
ATAS.Indicators.FilterRangeValue<TValue>
```

*Аннотация: Диаграмма взаимодействия по сути повторяет диаграмму наследования, но визуализирует отношения классов в контексте взаимодействия. Здесь также показано, что `FilterRangeValue<TValue>` зависит от `NotifyPropertyChangedBase` и `INotifyPropertyChanged` для реализации функциональности уведомлений.*

### Параметры шаблона

**`TValue`**: Тип значений, которые может содержать диапазон.

*Аннотация: `TValue` является параметром шаблона, который нужно указать при использовании `FilterRangeValue<TValue>`. Он определяет, какие типы данных (например, `double`, `int`, `DateTime`) будут использоваться для представления начала и конца диапазона.*

### Свойства

#### `Start`

**Тип:** `TValue?`

**Доступ:** `[get, set]`

**Описание:** Возвращает или устанавливает начальное значение диапазона.

*Аннотация: Свойство `Start` определяет начало диапазона. Тип `TValue?` означает, что значение может быть как типа `TValue`, так и `null` (если значение не задано). `[get, set]` указывает, что свойство доступно для чтения и записи.*

#### `End`

**Тип:** `TValue?`

**Доступ:** `[get, set]`

**Описание:** Возвращает или устанавливает конечное значение диапазона.

*Аннотация: Свойство `End` определяет конец диапазона. Аналогично `Start`, тип `TValue?` допускает значения типа `TValue` или `null`. `[get, set]` также означает доступность для чтения и записи.*

### Дополнительные унаследованные члены

► **Защищенные функции-члены, унаследованные от** `ATAS.Indicators.NotifyPropertyChangedBase`

*Аннотация: Класс `FilterRangeValue<TValue>` наследует защищенные функции и события от `NotifyPropertyChangedBase`. Эти унаследованные члены связаны с механизмом уведомлений об изменении свойств и могут быть использованы внутри класса `FilterRangeValue<TValue>` или его наследников.*

► **События, унаследованные от** `ATAS.Indicators.NotifyPropertyChangedBase`

*Аннотация: Аналогично защищенным функциям-членам, класс также наследует события от `NotifyPropertyChangedBase`, которые также связаны с уведомлениями об изменении свойств.*

### Подробное описание

Представляет диапазон значений типа `TValue` с поддержкой уведомлений об изменении свойств.

*Аннотация: Повторение общего описания класса, подчеркивающее его основную функцию - представление диапазона значений и поддержку уведомлений об изменениях, что важно для динамического обновления индикаторов.*

### Документация свойств

#### `End`

**Тип:** `TValue?`  `ATAS.Indicators.FilterRangeValue< TValue >.End`

**Доступ:** `get`, `set`

**Описание:** Возвращает или устанавливает конечное значение диапазона.

*Аннотация: Более детальное описание свойства `End`, указывающее полный путь к свойству и подтверждение доступа на чтение (`get`) и запись (`set`).*

#### `Start`

**Тип:** `TValue?`  `ATAS.Indicators.FilterRangeValue< TValue >.Start`

**Доступ:** `get`, `set`

**Описание:** Возвращает или устанавливает начальное значение диапазона.

*Аннотация: Более детальное описание свойства `Start`, аналогично `End`, указывающее полный путь к свойству и подтверждение доступа на чтение и запись.*

### Файл документации для класса

*   `FilterRangeValue.cs`

*Аннотация: Указывает имя файла, в котором определен класс `FilterRangeValue<TValue>`. Эта информация может быть полезна для поиска исходного кода класса в проекте ATAS SDK.*







## Методичка по классу `ATAS.Indicators.FilterRenderPen` для создания индикаторов ATAS

### Описание класса

**`ATAS.Indicators.FilterRenderPen`** - представляет собой фильтр для объектов `PenSettings` с поддержкой уведомлений об изменении свойств.

**Назначение:**

*   Фильтрация объектов `PenSettings`.
*   Поддержка уведомлений об изменении свойств для `PenSettings`.

**Иерархия наследования:**

```
Filter< PenSettings,
FilterRenderPen >
    ↑
    ATAS.Indicators.FilterRenderPen
    ↑
ICustomTypeDescriptor
```

*   **`Filter< PenSettings, FilterRenderPen >`**: Базовый класс фильтра, параметризованный типами `PenSettings` и `FilterRenderPen`.
*   **`ICustomTypeDescriptor`**: Интерфейс для предоставления пользовательской информации о типе объекта.

**Диаграмма взаимодействия:**

```
Filter< PenSettings,
FilterRenderPen >
    \
     ATAS.Indicators.FilterRenderPen
    /
ICustomTypeDescriptor
```

### Конструкторы

#### `FilterRenderPen(bool enableVisible, bool asScalar = false)` [1/2]

**Инициализация:** Создает новый экземпляр класса `FilterRenderPen` с указанными настройками видимости.

**Параметры:**

*   `enableVisible` (`bool`):
    *   Указывает, является ли фильтр видимым.
    *   Значение `true` делает фильтр видимым, `false` - невидимым.
*   `asScalar` (`bool`, по умолчанию `false`):
    *   Указывает, представляет ли фильтр скалярное значение.
    *   Значение `true` указывает на скалярное значение, `false` - на не скалярное.

#### `FilterRenderPen()` [2/2]

**Инициализация:** Создает новый экземпляр класса `FilterRenderPen` с настройками видимости по умолчанию.

**Параметры:** Отсутствуют. Используются значения по умолчанию для видимости и типа скалярности.

### Методы-члены класса

#### `GetPropertyOwner(PropertyDescriptor pd)`

**Возвращает:** `object`

**Описание:** Возвращает объект, владеющий свойством, описанным дескриптором свойства `pd`.

**Параметры:**

*   `pd` (`PropertyDescriptor`): Дескриптор свойства, для которого нужно получить владельца.

### Защищенные методы-члены класса

#### `ValueOnChanging(PenSettings oldValue, PenSettings newValue)`

**Возвращает:** `override PenSettings`

**Описание:**  Переопределенный метод, вызывается при изменении значения `PenSettings`.

**Параметры:**

*   `oldValue` (`PenSettings`): Старое значение `PenSettings`.
*   `newValue` (`PenSettings`): Новое значение `PenSettings`.

**Защищенный:**  `protected` - доступен для классов-наследников и текущего класса.

### Дополнительные унаследованные члены

**Унаследованы от `ATAS.Indicators.Filter< PenSettings, FilterRenderPen >`**:

*   **Статические публичные методы-члены** (`Static Public Member Functions`).
*   **Свойства** (`Properties`).

*Подробности об унаследованных членах смотрите в документации класса `ATAS.Indicators.Filter< PenSettings, FilterRenderPen >`.*

### Подробное описание

Класс `ATAS.Indicators.FilterRenderPen` предназначен для фильтрации объектов `PenSettings`. `PenSettings` вероятно отвечает за настройки отображения графических элементов индикаторов в ATAS. Фильтр позволяет контролировать, какие `PenSettings` будут применяться или отображаться, и реагировать на изменения этих настроек. Поддержка уведомлений об изменении свойств позволяет динамически реагировать на изменения в `PenSettings`.

### Файл реализации

*   `FilterRenderPen.cs` - Файл, содержащий исходный код класса `ATAS.Indicators.FilterRenderPen`.






## Методичка по классу `ATAS.Indicators.FilterString` для создания индикаторов ATAS

### Описание класса `ATAS.Indicators.FilterString`

Представляет фильтр для строковых значений с поддержкой уведомлений об изменении свойств.

**Диаграмма наследования:**

```
Filter< string, FilterString >
    ATAS.Indicators.FilterString
```

**Диаграмма взаимодействия:**

```
Filter< string, FilterString >
    ATAS.Indicators.FilterString
```

### Открытые члены класса (Public Member Functions)

* **`FilterString(bool enableVisible = true, bool asScalar = false)`**
    * Инициализирует новый экземпляр класса `FilterString` с указанными настройками видимости.

* **`FilterString()`**
    * Инициализирует новый экземпляр класса `FilterString` с настройками видимости по умолчанию.

* **Унаследованные открытые члены класса от `ATAS.Indicators.Filter< string, FilterString >`**
    * (См. документацию по `ATAS.Indicators.Filter`)

### Защищенные члены класса (Protected Member Functions)

* **`override string GetRealValue(string? value)`**
    *  Переопределенный метод для получения реального значения строкового фильтра.

* **Унаследованные защищенные члены класса от `ATAS.Indicators.Filter< string, FilterString >`**
    * (См. документацию по `ATAS.Indicators.Filter`)

### Дополнительные унаследованные члены

* **Статические открытые члены класса, унаследованные от `ATAS.Indicators.Filter< string, FilterString >`**
    * (См. документацию по `ATAS.Indicators.Filter`)

* **Свойства, унаследованные от `ATAS.Indicators.Filter< string, FilterString >`**
    * (См. документацию по `ATAS.Indicators.Filter`)

### Подробное описание

Представляет фильтр для строковых значений с поддержкой уведомлений об изменении свойств.

### Документация конструктора и деструктора

* **`FilterString() [1/2]`**
    ```csharp
    ATAS.Indicators.FilterString.FilterString (
        bool enableVisible,
        bool asScalar = false
    )
    ```
    * Инициализирует новый экземпляр класса `FilterString` с указанными настройками видимости.

    * **Параметры:**
        * `enableVisible`:  Значение, указывающее, виден ли фильтр.
        * `asScalar`: Значение, указывающее, представляет ли фильтр скалярное значение.

* **`FilterString() [2/2]`**
    ```csharp
    ATAS.Indicators.FilterString.FilterString()
    ```
    * Инициализирует новый экземпляр класса `FilterString` с настройками видимости по умолчанию.

### Документация метода-члена

* **`GetRealValue()`**
    ```csharp
    override string ATAS.Indicators.FilterString.GetRealValue (
        string? value
    )
    ```
    * Защищенный метод.

Документация для этого класса была сгенерирована из следующего файла:

* `FilterString.cs`






## Методичка по классу `ATAS.Indicators.FilterTimeSpan`

### Обзор класса `ATAS.Indicators.FilterTimeSpan`

Класс `ATAS.Indicators.FilterTimeSpan` представляет собой фильтр для значений `TimeSpan` с поддержкой пользовательской JSON-сериализации/десериализации.

**Диаграмма наследования:**

```
Filter< TimeSpan, FilterTimeSpan >
    ↑
ATAS.Indicators.FilterTimeSpan
```

*   `ATAS.Indicators.FilterTimeSpan` наследуется от класса `Filter< TimeSpan, FilterTimeSpan >`.

**Диаграмма взаимодействия:**

```
Filter< TimeSpan, FilterTimeSpan >
    ↑
ATAS.Indicators.FilterTimeSpan
```

*   Диаграмма взаимодействия показывает аналогичную структуру наследования, что и диаграмма наследования.

### Открытые члены класса (Public Member Functions)

*   **`FilterTimeSpan(bool enableVisible, bool asScalar = false)`**
    *   Инициализирует новый экземпляр класса `FilterTimeSpan`.
    *   **Параметры:**
        *   `enableVisible` (bool): Указывает, видимо ли свойство "Enabled" фильтра.
        *   `asScalar` (bool, по умолчанию `false`): Указывает, представляет ли фильтр скалярное значение.

*   **`FilterTimeSpan()`**
    *   Инициализирует новый экземпляр класса `FilterTimeSpan` с настройками по умолчанию.

### Дополнительные унаследованные члены (Additional Inherited Members)

*   **Статические открытые члены (Static Public Member Functions)**: Унаследованы от `ATAS.Indicators.Filter< TimeSpan, FilterTimeSpan >`.
*   **Защищенные члены (Protected Member Functions)**: Унаследованы от `ATAS.Indicators.Filter< TimeSpan, FilterTimeSpan >`.
*   **Свойства (Properties)**: Унаследованы от `ATAS.Indicators.Filter< TimeSpan, FilterTimeSpan >`.

### Подробное описание (Detailed Description)

Класс `ATAS.Indicators.FilterTimeSpan` предназначен для фильтрации значений типа `TimeSpan` и поддерживает пользовательскую сериализацию и десериализацию в формате JSON.

### Документация конструкторов и деструкторов (Constructor & Destructor Documentation)

*   **`FilterTimeSpan() [1/2]`**

    ```csharp
    ATAS.Indicators.FilterTimeSpan.FilterTimeSpan (bool enableVisible,
        bool asScalar = false
    )
    ```

    *   Инициализирует новый экземпляр класса `FilterTimeSpan`.
    *   **Параметры:**
        *   `enableVisible`: Значение, указывающее, видимо ли свойство "Enabled" фильтра.
        *   `asScalar`: Значение, указывающее, представляет ли фильтр скалярное значение.

*   **`FilterTimeSpan() [2/2]`**

    ```csharp
    ATAS.Indicators.FilterTimeSpan.FilterTimeSpan ()
    ```

    *   Инициализирует новый экземпляр класса `FilterTimeSpan` с настройками по умолчанию.

### Файл документации класса

*   Документация для этого класса была сгенерирована из файла: `FilterTimeSpan.cs`.






## Методичка по классу `ATAS.Indicators.FixedProfileRequest`

**Описание класса:**

Класс `ATAS.Indicators.FixedProfileRequest` предназначен для создания запроса на построение фиксированного профиля объема за определенный период времени.  Фиксированный профиль объема показывает распределение объема торгов по ценовым уровням за заданный промежуток времени.

**Публичные члены класса:**

**1. Конструкторы (методы для создания объектов класса):**

*   **`FixedProfileRequest(FixedProfilePeriods period)`**

    *   **Описание:**  Этот конструктор создает новый объект `FixedProfileRequest`.  Он используется для инициализации запроса на фиксированный профиль с указанием только периода расчета.
    *   **Параметры:**
        *   `period` :  Период времени, за который необходимо рассчитать фиксированный профиль объема.  Тип параметра: `FixedProfilePeriods`.  `FixedProfilePeriods` - это, вероятно, перечисление (enum) в ATAS, которое определяет возможные варианты периодов (например, день, неделя, месяц и т.д.).

*   **`FixedProfileRequest(FixedProfilePeriods period, long? tradingSession)`**

    *   **Описание:** Этот конструктор также создает новый объект `FixedProfileRequest`, но позволяет задать не только период, но и торговую сессию. Это полезно, если вы хотите построить профиль объема только для определенной части торгового дня (например, только для основной торговой сессии ETH/RTH).
    *   **Параметры:**
        *   `period` : Период времени для расчета фиксированного профиля объема. Тип параметра: `FixedProfilePeriods`.
        *   `tradingSession` : Идентификатор торговой сессии. Тип параметра: `long?`.  `long?` означает, что параметр является nullable long (целое число, допускающее значение null).  Если значение указано, то профиль будет построен только для указанной торговой сессии. Если значение `null`, то профиль будет построен для всего периода.  Примеры торговых сессий: ETH, RTH и т.д.

**2. Свойства (характеристики объекта, доступные для чтения):**

*   **`Period`**

    *   **Тип:** `FixedProfilePeriods`
    *   **Описание:**  Это свойство позволяет получить значение периода, который был задан для запроса фиксированного профиля.  Другими словами, оно возвращает период, который используется в текущем запросе `FixedProfileRequest`.
    *   **Доступ:** Только для чтения (`[get]`).  Это означает, что вы можете только получить значение этого свойства, но не можете его изменить напрямую после создания объекта `FixedProfileRequest`.

*   **`TradingSession`**

    *   **Тип:** `long?`
    *   **Описание:**  Это свойство позволяет получить идентификатор торговой сессии, который был задан для запроса фиксированного профиля.  Оно возвращает торговую сессию (например, ETH, RTH), если она была указана при создании объекта `FixedProfileRequest`. Если торговая сессия не была указана, свойство может вернуть `null`.
    *   **Доступ:** Только для чтения (`[get]`).  Вы можете только прочитать значение торговой сессии, установленной для запроса.

**Дополнительная информация:**

*   Класс `ATAS.Indicators.FixedProfileRequest` находится в файле `PriceVolumeInfo.cs`.

**Примечание для ИИ:**

При использовании этого класса для создания индикаторов в ATAS, необходимо:

1.  Создать экземпляр класса `FixedProfileRequest`, используя один из конструкторов, передав необходимые параметры `period` и, опционально, `tradingSession`.
2.  Использовать созданный объект `FixedProfileRequest` при запросе данных для индикатора фиксированного профиля объема. (Дальнейшие шаги зависят от конкретного API и методов платформы ATAS для работы с индикаторами и данными).
3.  Для получения информации о периоде и торговой сессии, которые были заданы в запросе, можно использовать свойства `Period` и `TradingSession` объекта `FixedProfileRequest`.





## Методичка по интерфейсу `ATAS.Indicators.ICandleCreator` для создания индикаторов ATAS

### Описание интерфейса

Интерфейс `ATAS.Indicators.ICandleCreator` представляет собой интерфейс для создания и управления индикаторными свечами в платформе ATAS.

### Свойства

Интерфейс `ICandleCreator` имеет следующие свойства:

1.  **`Name`**

    *   **Тип:** `string`
    *   **Полное имя:** `ATAS.Indicators.ICandleCreator.Name`
    *   **Описание:** Возвращает имя создателя свечей.

2.  **`Candles`**

    *   **Тип:** `List<IndicatorCandle>`
    *   **Полное имя:** `ATAS.Indicators.ICandleCreator.Candles`
    *   **Описание:** Возвращает список объектов `IndicatorCandle`, созданных создателем свечей.

### События

Интерфейс `ICandleCreator` имеет следующие события:

1.  **`OnNewCandle`**

    *   **Тип:** `Action<IndicatorCandle>`
    *   **Полное имя:** `ATAS.Indicators.ICandleCreator.OnNewCandle`
    *   **Описание:** Событие возникает при создании новой `IndicatorCandle`.

2.  **`OnRemoveCandle`**

    *   **Тип:** `Action<IndicatorCandle>`
    *   **Полное имя:** `ATAS.Indicators.ICandleCreator.OnRemoveCandle`
    *   **Описание:** Событие возникает при удалении `IndicatorCandle`.

3.  **`OnCandleChanged`**

    *   **Тип:** `Action<IndicatorCandle>`
    *   **Полное имя:** `ATAS.Indicators.ICandleCreator.OnCandleChanged`
    *   **Описание:** Событие возникает при изменении `IndicatorCandle`.

### Подробное описание

Интерфейс `ICandleCreator` предназначен для представления интерфейса для создания и управления индикаторными свечами.

### Документация по свойствам

#### `Candles`

*   Возвращает список `IndicatorCandle`, созданных создателем свечей.

#### `Name`

*   Возвращает имя создателя свечей.

### Документация по событиям

#### `OnCandleChanged`

*   Событие возникает, когда `IndicatorCandle` изменяется.

#### `OnNewCandle`

*   Событие возникает, когда создается новая `IndicatorCandle`.

#### `OnRemoveCandle`

*   Событие возникает, когда `IndicatorCandle` удаляется.

### Файл документации

Документация для этого интерфейса была сгенерирована из следующего файла:

*   `ICandleCreator.cs`






## Методичка по интерфейсу `ATAS.Indicators.IChart` для создания индикаторов ATAS

### Описание интерфейса

Интерфейс `ATAS.Indicators.IChart` предназначен для работы с различными данными и методами, связанными с графиком в платформе ATAS. Он предоставляет доступ к информации о графике, такой как типы цен, таймфреймы, цвета, координаты и другие параметры.

### Диаграммы наследования и взаимодействия

* **Диаграмма наследования:** `ATAS.Indicators.IChart` наследуется от интерфейса `IContainer`. Это означает, что `ATAS.Indicators.IChart` включает в себя все члены интерфейса `IContainer` и добавляет свою собственную функциональность.
* **Диаграмма взаимодействия:** `ATAS.Indicators.IChart` взаимодействует с интерфейсом `IContainer`. Это показывает, что для работы с `ATAS.Indicators.IChart` также может потребоваться использование методов и свойств `IContainer`.

### Публичные функции-члены (Public Member Functions)

Интерфейс `ATAS.Indicators.IChart` предоставляет следующие публичные функции для получения строкового представления различных значений:

1.  **`TryGetMinimizedVolumeString(decimal value)`**
    *   **Описание:** Пытается получить строковое представление минимизированного объема для указанного значения.
    *   **Параметры:**
        *   `value` (decimal): Значение объема, для которого необходимо получить строковое представление.
    *   **Возвращаемое значение:** `string` - Строковое представление минимизированного объема.

2.  **`TryGetMinimizedVolumeString(decimal value, decimal price)` [Перегрузка 1/2]**
    *   **Описание:** Пытается получить строковое представление минимизированного объема для указанного значения. Если инструмент торгуется в USD, цена используется для конвертации объема в USD.
    *   **Параметры:**
        *   `value` (decimal): Значение объема.
        *   `price` (decimal): Цена инструмента (используется для инструментов в USD).
    *   **Возвращаемое значение:** `string` - Строковое представление минимизированного объема.

3.  **`GetPriceString(decimal value)`**
    *   **Описание:** Возвращает строковое представление указанной цены.
    *   **Параметры:**
        *   `value` (decimal): Значение цены, для которого необходимо получить строковое представление.
    *   **Возвращаемое значение:** `string` - Строковое представление цены.

### Свойства (Properties)

Интерфейс `ATAS.Indicators.IChart` предоставляет следующие свойства для доступа к различным параметрам графика:

1.  **`ColorsStore`** (`IChartColorsStore`) [только чтение, `get`]
    *   **Описание:** Возвращает хранилище цветов графика, предоставляющее различные цвета, связанные с графиком.

2.  **`CoordinatesManager`** (`IChartCoordinatesManager`) [только чтение, `get`]
    *   **Описание:** Возвращает менеджер координат графика, отвечающий за управление координатами и масштабированием графика.

3.  **`MouseLocationInfo`** (`IMouseLocationInfo`) [только чтение, `get`]
    *   **Описание:** Возвращает информацию о положении мыши внутри графика.

4.  **`KeyboardInfo`** (`IKeyboardInfo`) [только чтение, `get`]
    *   **Описание:** Возвращает информацию о клавиатуре внутри графика.

5.  **`DrawingObjectsListInfo`** (`IDrawingObjectsListInfo`) [только чтение, `get`]
    *   **Описание:** Возвращает список графических объектов, присутствующих на графике.

6.  **`PriceChartContainer`** (`IChartContainer`) [только чтение, `get`]
    *   **Описание:** Возвращает контейнер, представляющий область графика без оси цен.

7.  **`ChartContainer`** (`IContainer`) [только чтение, `get`]
    *   **Описание:** Возвращает контейнер, представляющий полную область графика.

8.  **`PriceAxisFont`** (`RenderFont`) [только чтение, `get`]
    *   **Описание:** Возвращает шрифт оси цен графика.

9.  **`StringFormat`** (`string`) [только чтение, `get`]
    *   **Описание:** Возвращает строковый формат, используемый внутри графика.

10. **`ChartType`** (`string`) [только чтение, `get`]
    *   **Описание:** Возвращает тип графика.

11. **`TimeFrame`** (`string`) [только чтение, `get`]
    *   **Описание:** Возвращает таймфрейм графика. Если таймфрейм имеет несколько параметров, они разделяются косой чертой (/).

12. **`TradingSessionDescriptions`** (`IEnumerable<TradingSessionDescription>`) [только чтение, `get`]
    *   **Описание:** Возвращает доступные торговые сессии.

13. **`ChartVisualMode`** (`ChartVisualModes`) [чтение и запись, `get`, `set`]
    *   **Описание:** Возвращает или устанавливает визуальный режим графика.

14. **`FootprintContentMode`** (`FootprintContentModes`) [чтение и запись, `get`, `set`]
    *   **Описание:** Возвращает или устанавливает режим отображения футпринта.

15. **`FootprintVisualMode`** (`FootprintVisualModes`) [чтение и запись, `get`, `set`]
    *   **Описание:** Возвращает или устанавливает визуальный режим футпринта.

16. **`FootprintColorScheme`** (`FootprintColorSchemes`) [чтение и запись, `get`, `set`]
    *   **Описание:** Возвращает или устанавливает цветовую схему футпринта.

17. **`HidePriceAxis`** (`bool`) [чтение и запись, `get`, `set`]
    *   **Описание:** Показывает или скрывает ось цен графика.

18. **`StatusLineHorizontalOffset`** (`int`) [чтение и запись, `get`, `set`]
    *   **Описание:** Возвращает или устанавливает горизонтальное смещение строки статуса.

19. **`IndicatorsListVerticalOffset`** (`int`) [чтение и запись, `get`, `set`]
    *   **Описание:** Возвращает или устанавливает вертикальное смещение списка индикаторов.

20. **`IndicatorsListHorizontalOffset`** (`int`) [чтение и запись, `get`, `set`]
    *   **Описание:** Возвращает или устанавливает горизонтальное смещение списка индикаторов.

21. **`DpiY`** (`decimal`) [только чтение, `get`]
    *   **Описание:** Возвращает значение DPI по оси Y (точек на дюйм) для графика.

22. **`DpiX`** (`decimal`) [только чтение, `get`]
    *   **Описание:** Возвращает значение DPI по оси X (точек на дюйм) для графика.

### Расположение файла интерфейса

*   `IIndicatorContainer.cs`

**Примечание:**  Свойства, унаследованные от `ATAS.Indicators.IContainer`, не были подробно описаны в данном документе, но также являются частью интерфейса `ATAS.Indicators.IChart`. Для полного понимания необходимо изучить документацию по `ATAS.Indicators.IContainer`.







## Методичка по интерфейсу `ATAS.Indicators.IChartColorsStore` для создания индикаторов ATAS

### Интерфейс `ATAS.Indicators.IChartColorsStore`

**Описание:** Интерфейс для доступа к цветам и перьям, используемым при отрисовке графика.

### Публичные функции-члены

1.  **`DrawBackground`**
    *   **Тип:** Функция
    *   **Возвращаемый тип:** `void`
    *   **Сигнатура:** `void DrawBackground (RenderContext context, Rectangle region)`
    *   **Описание:** Рисует фон графика в указанной области с использованием предоставленного контекста рендеринга.
    *   **Параметры:**
        *   `context`: Контекст рендеринга (`RenderContext`).
        *   `region`: Область (`Rectangle`), в которой нужно нарисовать фон.
    *   **Аннотация:** Функция `DrawBackground` отрисовывает фоновый цвет графика в заданной прямоугольной области `region`, используя контекст рисования `context`.

### Свойства

1.  **`Grid`**
    *   **Тип:** Свойство
    *   **Тип значения:** `RenderPen`
    *   **Описание:** Возвращает перо, используемое для рисования линий сетки.
    *   **Аннотация:** Свойство `Grid` типа `RenderPen` предоставляет доступ к перу, которое используется для отрисовки линий координатной сетки на графике.

2.  **`HorizontalGridIsEnabled`**
    *   **Тип:** Свойство
    *   **Тип значения:** `bool`
    *   **Описание:** Возвращает значение, указывающее, включены ли горизонтальные линии сетки.
    *   **Аннотация:** Свойство `HorizontalGridIsEnabled` типа `bool` показывает, активированы ли горизонтальные линии сетки на графике (истина/ложь).

3.  **`BaseBackgroundColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает основной фоновый цвет графика.
    *   **Аннотация:** Свойство `BaseBackgroundColor` типа `Color` определяет базовый цвет фона всего графика.

4.  **`UpCandleColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для восходящих (бычьих) свечей.
    *   **Аннотация:** Свойство `UpCandleColor` типа `Color` задает цвет для отображения растущих ("бычьих") ценовых свечей.

5.  **`DownCandleColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для нисходящих (медвежьих) свечей.
    *   **Аннотация:** Свойство `DownCandleColor` типа `Color` определяет цвет для падающих ("медвежьих") ценовых свечей.

6.  **`AxisTextColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет текста оси.
    *   **Аннотация:** Свойство `AxisTextColor` типа `Color` используется для установки цвета текста, отображаемого на осях графика (цена, время).

7.  **`PaneSeparators`**
    *   **Тип:** Свойство
    *   **Тип значения:** `RenderPen`
    *   **Описание:** Возвращает перо, используемое для рисования разделителей панелей.
    *   **Аннотация:** Свойство `PaneSeparators` типа `RenderPen` предоставляет доступ к перу, используемому для отрисовки линий, разделяющих панели индикаторов на графике.

8.  **`NewSessionPen`**
    *   **Тип:** Свойство
    *   **Тип значения:** `RenderPen`
    *   **Описание:** Возвращает перо, используемое для рисования начала новой сессии.
    *   **Аннотация:** Свойство `NewSessionPen` типа `RenderPen` задает перо для линий, обозначающих начало новой торговой сессии на графике.

9.  **`UpBarPen`**
    *   **Тип:** Свойство
    *   **Тип значения:** `RenderPen`
    *   **Описание:** Возвращает перо, используемое для рисования восходящих баров.
    *   **Аннотация:** Свойство `UpBarPen` типа `RenderPen` определяет перо для отрисовки растущих ценовых баров.

10. **`DownBarPen`**
    *   **Тип:** Свойство
    *   **Тип значения:** `RenderPen`
    *   **Описание:** Возвращает перо, используемое для рисования нисходящих баров.
    *   **Аннотация:** Свойство `DownBarPen` типа `RenderPen` задает перо для отрисовки падающих ценовых баров.

11. **`DojiBarPen`**
    *   **Тип:** Свойство
    *   **Тип значения:** `RenderPen`
    *   **Описание:** Возвращает перо, используемое для рисования баров Дожи.
    *   **Аннотация:** Свойство `DojiBarPen` типа `RenderPen` используется для настройки пера, которым рисуются бары типа "Дожи".

12. **`BarBorderPen`**
    *   **Тип:** Свойство
    *   **Тип значения:** `RenderPen`
    *   **Описание:** Возвращает перо, используемое для рисования границ баров.
    *   **Аннотация:** Свойство `BarBorderPen` типа `RenderPen` позволяет получить доступ к перу, которым отрисовываются границы ценовых баров.

13. **`FootprintBidColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для отображения объема Bid в футпринт-графике.
    *   **Аннотация:** Свойство `FootprintBidColor` типа `Color` определяет цвет для отображения объемов Bid (покупки) в футпринт графиках.

14. **`FootprintAskColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для отображения объема Ask в футпринт-графике.
    *   **Аннотация:** Свойство `FootprintAskColor` типа `Color` задает цвет для отображения объемов Ask (продажи) в футпринт графиках.

15. **`FootprintMainColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает основной цвет, используемый в футпринт-графике.
    *   **Аннотация:** Свойство `FootprintMainColor` типа `Color` определяет основной цвет, используемый для общего отображения футпринт графика.

16.  **`FootprintTextColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для текста в футпринт-графике.
    *   **Аннотация:** Свойство `FootprintTextColor` типа `Color` используется для настройки цвета текста, отображаемого внутри футпринт графика (например, значения объемов).

17. **`FootprintMaximumVolumeTextColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для текста максимального объема в футпринт-графике.
    *   **Аннотация:** Свойство `FootprintMaximumVolumeTextColor` типа `Color` задает цвет для текста, обозначающего максимальный объем в футпринт-графике.

18. **`PositiveColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для положительных значений.
    *   **Аннотация:** Свойство `PositiveColor` типа `Color` определяет цвет для визуализации положительных значений (например, положительной дельты).

19. **`NegativeColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для отрицательных значений.
    *   **Аннотация:** Свойство `NegativeColor` типа `Color` используется для настройки цвета отрицательных значений (например, отрицательной дельты).

20. **`NeutralColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для нейтральных значений.
    *   **Аннотация:** Свойство `NeutralColor` типа `Color` задает цвет для отображения нейтральных значений (например, нулевой дельты).

21. **`MouseBackground`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет фона для мыши.
    *   **Аннотация:** Свойство `MouseBackground` типа `Color` определяет цвет фона для элементов, связанных с мышью (например, всплывающих подсказок).

22. **`MouseTextColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет текста для мыши.
    *   **Аннотация:** Свойство `MouseTextColor` типа `Color` используется для настройки цвета текста элементов, связанных с мышью.

23. **`BuyOrdersColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для ордеров на покупку.
    *   **Аннотация:** Свойство `BuyOrdersColor` типа `Color` задает цвет для отображения ордеров на покупку на графике.

24. **`SellOrdersColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для ордеров на продажу.
    *   **Аннотация:** Свойство `SellOrdersColor` типа `Color` используется для настройки цвета ордеров на продажу.

25. **`OrdersTextColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет текста для ордеров.
    *   **Аннотация:** Свойство `OrdersTextColor` типа `Color` определяет цвет текста, используемого для отображения информации об ордерах.

26. **`PositiveProfitColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для положительных значений прибыли.
    *   **Аннотация:** Свойство `PositiveProfitColor` типа `Color` задает цвет для отображения положительной прибыли.

27. **`NegativeProfitColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для отрицательных значений прибыли.
    *   **Аннотация:** Свойство `NegativeProfitColor` типа `Color` используется для настройки цвета отрицательной прибыли.

28. **`NeutralProfitColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для нейтральных значений прибыли.
    *   **Аннотация:** Свойство `NeutralProfitColor` типа `Color` определяет цвет для отображения нейтральной (нулевой) прибыли.

29. **`ProfitTextColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет текста для значений прибыли.
    *   **Аннотация:** Свойство `ProfitTextColor` типа `Color` задает цвет текста, используемого для отображения значений прибыли.

30. **`DrawingObjectColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для рисования объектов.
    *   **Аннотация:** Свойство `DrawingObjectColor` типа `Color` используется для настройки цвета графических объектов, рисуемых на графике (линии, фигуры и т.д.).

31. **`HeatmapType`**
    *   **Тип:** Свойство
    *   **Тип значения:** `HeatmapTypes`
    *   **Описание:** Возвращает тип тепловой карты, используемый для рендеринга.
    *   **Аннотация:** Свойство `HeatmapType` типа `HeatmapTypes` определяет тип используемой тепловой карты для визуализации данных.

32. **`DrawingObjectsSelectionColor`**
    *   **Тип:** Свойство
    *   **Тип значения:** `Color`
    *   **Описание:** Возвращает цвет, используемый для рисования выделения графических объектов.
    *   **Аннотация:** Свойство `DrawingObjectsSelectionColor` типа `Color` задает цвет для выделения выбранных графических объектов на графике.







## Методичка по интерфейсу ATAS.Indicators.IChartContainer для создания индикаторов ATAS

### Интерфейс ATAS.Indicators.IChartContainer Reference

Интерфейс для контейнера графика, который содержит информацию и методы, связанные с графиком. Подробнее...

**Диаграмма наследования для ATAS.Indicators.IChartContainer:**

```
IContainer
  |
  └── ATAS.Indicators.IChartContainer
```
[легенда]

**Диаграмма взаимодействия для ATAS.Indicators.IChartContainer:**

```
IContainer
  |
  └── ATAS.Indicators.IChartContainer
```
[легенда]

### Открытые (Public) функции-члены

* `int GetYByPrice (decimal price, bool isStartOfPriceLevel)`
    * Возвращает значение Y-координаты, соответствующее указанной цене в контейнере графика.
* `decimal GetPriceByY (int y)`
    * Возвращает значение цены, соответствующее указанной Y-координате в контейнере графика.
* `int GetXByBarNumber (int i)`
    * Возвращает значение X-координаты, соответствующее указанному номеру бара в контейнере графика.
* `int GetXByBar (int bar, bool isStartOfBar = true)`
    * Возвращает значение X-координаты, соответствующее указанному бару в контейнере графика.
* `void SetCustomBarsSpacing (decimal? value)`
    * Устанавливает пользовательский интервал между барами.
* `void SetCustomBarsWidth (decimal value, bool freezeValue)`
    * Устанавливает пользовательскую ширину баров.

### Свойства (Properties)

* `decimal High` \[get]
    * Возвращает наивысшее значение цены в контейнере графика.
* `decimal Low` \[get]
    * Возвращает наименьшее значение цены в контейнере графика.
* `decimal Step` \[get]
    * Возвращает шаг цены для ценовых уровней в контейнере графика.
* `int PriceLevelsCount` \[get]
    * Возвращает количество видимых ценовых уровней в контейнере графика.
* `decimal PriceRowHeight` \[get]
    * Возвращает высоту каждой строки цены в контейнере графика.
* `decimal BarsWidth` \[get]
    * Возвращает ширину баров в контейнере графика.
* `decimal BarSpacing` \[get]
    * Возвращает интервал между барами в контейнере графика.
* `int GetVisibleBarsCount` \[get]
    * Возвращает количество видимых баров в контейнере графика.
* `int FirstVisibleBarNumber` \[get]
    * Возвращает номер бара первого видимого бара в контейнере графика.
* `int LastVisibleBarNumber` \[get]
    * Возвращает номер бара последнего видимого бара в контейнере графика.
* `int TotalBars` \[get]
    * Возвращает общее количество баров в контейнере графика.

► Свойства, унаследованные от `ATAS.Indicators.IContainer`

### Подробное описание

Интерфейс для контейнера графика, который содержит информацию и методы, связанные с графиком.

### Документация функций-членов

#### `GetPriceByY()`

`decimal ATAS.Indicators.IChartContainer.GetPriceByY (int y)`

Возвращает значение цены, соответствующее указанной Y-координате в контейнере графика.

**Параметры:**

* `y` - Y-координата

**Возвращает:**

#### `GetXByBar()`

`int ATAS.Indicators.IChartContainer.GetXByBar (int bar, bool isStartOfBar = true)`

Возвращает значение X-координаты, соответствующее указанному бару в контейнере графика.

**Параметры:**

* `bar` - Бар, для которого нужно получить X-координату.
* `isStartOfBar` - Флаг, указывающий, соответствует ли X-координата началу бара.

**Возвращает:**

X-координата, соответствующая указанному бару.

#### `GetXByBarNumber()`

`int ATAS.Indicators.IChartContainer.GetXByBarNumber (int i)`

Возвращает значение X-координаты, соответствующее указанному номеру бара в контейнере графика.

**Параметры:**

* `i` - Номер бара, для которого нужно получить X-координату.

**Возвращает:**

X-координата, соответствующая указанному номеру бара.

#### `GetYByPrice()`

`int ATAS.Indicators.IChartContainer.GetYByPrice (decimal price, bool isStartOfPriceLevel)`

Возвращает значение Y-координаты, соответствующее указанной цене в контейнере графика.

**Параметры:**

* `price` - Значение цены, для которого нужно получить Y-координату.
* `isStartOfPriceLevel` - Флаг, указывающий, является ли ценовой уровень началом строки.

**Возвращает:**

Y-координата, соответствующая указанной цене.

#### `SetCustomBarsSpacing()`

`void ATAS.Indicators.IChartContainer.SetCustomBarsSpacing (decimal? value)`

Устанавливает пользовательский интервал между барами.

**Параметры:**

* `value` - Интервал между барами. `Null` включает автоматический режим.

#### `SetCustomBarsWidth()`

`void ATAS.Indicators.IChartContainer.SetCustomBarsWidth (decimal value, bool freezeValue)`

Устанавливает пользовательскую ширину баров.

**Параметры:**

* `value` - Ширина баров. `Null` включает автоматический режим.
* `freezeValue` - Если `true`, пользователь не сможет изменить ширину баров с графика.

### Документация свойств

#### `BarSpacing`

`decimal ATAS.Indicators.IChartContainer.BarSpacing` \[get]

Возвращает интервал между барами в контейнере графика.

#### `BarsWidth`

`decimal ATAS.Indicators.IChartContainer.BarsWidth` \[get]

Возвращает ширину баров в контейнере графика.

#### `FirstVisibleBarNumber`

`int ATAS.Indicators.IChartContainer.FirstVisibleBarNumber` \[get]

Возвращает номер бара первого видимого бара в контейнере графика.

#### `GetVisibleBarsCount`

`int ATAS.Indicators.IChartContainer.GetVisibleBarsCount` \[get]

Возвращает количество видимых баров в контейнере графика.

#### `High`

`decimal ATAS.Indicators.IChartContainer.High` \[get]

Возвращает наивысшее значение цены в контейнере графика.

#### `LastVisibleBarNumber`

`int ATAS.Indicators.IChartContainer.LastVisibleBarNumber` \[get]

Возвращает номер бара последнего видимого бара в контейнере графика.

#### `Low`

`decimal ATAS.Indicators.IChartContainer.Low` \[get]

Возвращает наименьшее значение цены в контейнере графика.

#### `PriceLevelsCount`

`int ATAS.Indicators.IChartContainer.PriceLevelsCount` \[get]

Возвращает количество видимых ценовых уровней в контейнере графика.

#### `PriceRowHeight`

`decimal ATAS.Indicators.IChartContainer.PriceRowHeight` \[get]

Возвращает высоту каждой строки цены в контейнере графика.

#### `Step`

`decimal ATAS.Indicators.IChartContainer.Step` \[get]

Возвращает шаг цены для ценовых уровней в контейнере графика.

Шаг может отличаться от инструмента `TickSize`, если используется `Scale`.

#### `TotalBars`

`int ATAS.Indicators.IChartContainer.TotalBars` \[get]

Возвращает общее количество баров в контейнере графика.

Документация для этого интерфейса была сгенерирована из следующего файла:

* `IIndicatorContainer.cs`









## Методичка по интерфейсу ATAS.Indicators.IChartCoordinatesManager

### Общее описание интерфейса:
Интерфейс для управления координатами и масштабированием графика.

---

## ChangeRowsHeight()

### Пространство имен:
ATAS.Indicators.IChartCoordinatesManager

### Описание:
Изменяет высоту строк графика на новую указанную высоту.

### Параметры:
- newHeight (decimal): Новая высота для установки высоты строк графика.

### Тип возвращаемого значения:
void

---

## EnableAutoScale()

### Пространство имен:
ATAS.Indicators.IChartCoordinatesManager

### Описание:
Включает автоматическое масштабирование графика для соответствия данным в пределах видимости.

### Параметры:
Отсутствуют

### Тип возвращаемого значения:
void

---

## MoveChartUpAndDown()

### Пространство имен:
ATAS.Indicators.IChartCoordinatesManager

### Описание:
Перемещает весь график вверх или вниз на указанное смещение.

### Параметры:
- offset (int): Вертикальное смещение для перемещения графика. Положительные значения перемещают вверх; отрицательные значения перемещают вниз.

### Тип возвращаемого значения:
void

---

## ScrollPrice()

### Пространство имен:
ATAS.Indicators.IChartCoordinatesManager

### Описание:
Прокручивает график вертикально на указанное количество тиков.

### Параметры:
- ticksCount (int): Количество тиков для прокрутки. Положительные значения прокручивают вверх; отрицательные значения прокручивают вниз.

### Тип возвращаемого значения:
void

---

## UpdateChartYScaleOnMouseMove()

### Пространство имен:
ATAS.Indicators.IChartCoordinatesManager

### Описание:
Изменяет масштаб оси Y графика при движении мыши.

### Параметры:
- previousMouseY (int):
- newMouseY (int):

### Тип возвращаемого значения:
void

---

## UpdateChartYScaleOnMouseWheel()

### Пространство имен:
ATAS.Indicators.IChartCoordinatesManager

### Описание:
Изменяет масштаб оси Y графика при вращении колеса мыши.

### Параметры:
- delta (int):

### Тип возвращаемого значения:
void

---







## Методичка по интерфейсу `ATAS.Indicators.IContainer` для создания индикаторов ATAS

**Описание интерфейса:**

Интерфейс `ATAS.Indicators.IContainer` предназначен для определения контейнера, который представляет собой прямоугольную область.  Этот интерфейс используется для работы с элементами, имеющими прямоугольную форму и положение на графике.

**Диаграмма наследования:**

```
ATAS.Indicators.IContainer
├── ATAS.Indicators.Container
├── ATAS.Indicators.IChart
├── ATAS.Indicators.IChartContainer
└── ATAS.Indicators.IIndicatorContainer
```

*   **`ATAS.Indicators.IContainer`** (Базовый интерфейс):  Самый основной интерфейс, определяющий общие свойства для всех типов контейнеров.  *Аннотация: Это как основа, общий шаблон для разных видов контейнеров.*
*   **`ATAS.Indicators.Container`**:  Класс, который реализует интерфейс `IContainer`.  Это конкретная реализация контейнера. *Аннотация:  Это уже готовый контейнер, который можно использовать.*
*   **`ATAS.Indicators.IChart`**: Интерфейс для контейнеров, связанных с графиком цены. *Аннотация:  Контейнер, который работает именно с графиком.*
*   **`ATAS.Indicators.IChartContainer`**: Интерфейс для контейнеров, которые могут содержать другие контейнеры внутри себя на графике. *Аннотация:  Контейнер, который может вмещать в себя другие контейнеры, как матрёшка.*
*   **`ATAS.Indicators.IIndicatorContainer`**: Интерфейс для контейнеров, специально предназначенных для индикаторов. *Аннотация:  Контейнер, созданный специально для размещения индикаторов.*

**Свойства интерфейса `ATAS.Indicators.IContainer`:**

*   **`Region`**:
    *   Тип: `Rectangle`
    *   Доступ: `[get]` (только для чтения)
    *   Описание:  Возвращает прямоугольную область, которая определена данным контейнером.  Эта область описывает положение и размер контейнера.

    ```csharp
    Rectangle region = container.Region;
    ```
    *   *Аннотация (Русский):  Свойство `Region` позволяет получить информацию о прямоугольной области контейнера. `Rectangle` описывает положение (координаты) и размеры прямоугольника.*

**Детальное описание:**

Интерфейс `ATAS.Indicators.IContainer` является ключевым для работы с любыми элементами в ATAS, которые имеют прямоугольную форму и положение.  Он гарантирует, что любой контейнер, реализующий этот интерфейс, предоставляет информацию о своей прямоугольной области через свойство `Region`.

**Реализация:**

Интерфейс `ATAS.Indicators.IContainer` реализован в классе `ATAS.Indicators.Container`.  Это означает, что класс `Container` предоставляет функциональность, описанную в интерфейсе `IContainer`.

**Файл реализации интерфейса:**

*   `IIndicatorContainer.cs`  (Файл, в котором, вероятно, определен интерфейс `IIndicatorContainer` и возможно, интерфейс `IContainer`). *Аннотация:  Скорее всего, код интерфейса `IContainer` находится в файле `IIndicatorContainer.cs`.*








## Методичка по интерфейсу `ATAS.Indicators.IDataSeries<T>` для создания индикаторов ATAS

### Общая информация об интерфейсе

**Интерфейс:** `ATAS.Indicators.IDataSeries<T>`
**Тип:** Интерфейс, Template Reference (шаблон)
**Описание:** Интерфейс для серий данных, предоставляющий основные свойства и методы.

**Диаграмма наследования:**

```
INotifyPropertyChanged -> ATAS.Indicators.IDataSeries<T>
INotifyPanelPropertyChanged -> ATAS.Indicators.IDataSeries<T>
IDataSeries -> ATAS.Indicators.IDataSeries<T>
ATAS.Indicators.BaseDataSeries<T> -> ATAS.Indicators.IDataSeries<T>
```

**Диаграмма взаимодействия:**

```
INotifyPropertyChanged -> ATAS.Indicators.IDataSeries<T>
INotifyPanelPropertyChanged -> ATAS.Indicators.IDataSeries<T>
IDataSeries -> ATAS.Indicators.IDataSeries<T>
```

### Публичные методы (Public Member Functions)

#### `Clear()`

**Сигнатура:** `void Clear()`
**Описание:** Очищает все элементы из серии данных.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

### Свойства (Properties)

#### `Id`

**Тип:** `string`
**Доступ:** `[get]` (только чтение)
**Описание:** Возвращает уникальный и постоянный идентификатор серии данных (Data Series ID) для сериализации данных.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `RenderId`

**Тип:** `string`
**Доступ:** `[get]` (только чтение)
**Описание:** Уникальный идентификатор серии для всех панелей и индикаторов.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `Type`

**Тип:** `DataSeriesType`
**Доступ:** `[get]` (только чтение)
**Описание:** Возвращает тип серии данных из перечисления (enumeration).
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `Name`

**Тип:** `string`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает имя серии данных.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `DescriptionKey`

**Тип:** `string`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает описание серии данных.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `Count`

**Тип:** `int`
**Доступ:** `[get]` (только чтение)
**Описание:** Возвращает количество элементов в серии данных.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `IsHidden`

**Тип:** `bool`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает значение, указывающее, должны ли свойства серии данных быть скрыты в окне настроек.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `IsVisible`

**Тип:** `bool`
**Доступ:** `[get]` (только чтение)
**Описание:** Возвращает значение, указывающее, должна ли серия быть нарисована.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `DrawAbovePrice`

**Тип:** `bool`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает значение, указывающее, должна ли серия данных быть нарисована над свечами графика.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `UseMinimizedModeIfEnabled`

**Тип:** `bool`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает значение, указывающее, следует ли использовать минимизированный режим, если он включен.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `IgnoredByAlerts`

**Тип:** `bool`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает значение, указывающее, должна ли серия данных игнорироваться оповещениями (alerts).
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `ResetAlertsOnNewBar`

**Тип:** `bool`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает значение, указывающее, должны ли оповещения (alerts) быть сброшены на новом баре.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `ShowTooltip`

**Тип:** `bool`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает значение, указывающее, должна ли отображаться подсказка (tooltip) для серии данных.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `ShowNameOnMouseOver`

**Тип:** `bool`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает значение, указывающее, должно ли имя серии данных отображаться при наведении курсора мыши.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `this[int index] (object)`

**Тип:** `object`
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает элемент по указанному индексу.
**Параметры:**
    * `index`: `int` - индекс элемента, должен быть неотрицательным целым числом.
**Возвращает:** Элемент типа `object` по указанному индексу.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

#### `this[int index] (new T)`

**Тип:** `new T` (тип параметра шаблона)
**Доступ:** `[get, set]` (чтение и запись)
**Описание:** Возвращает или устанавливает элемент по указанному индексу.
**Параметры:**
    * `index`: `int` - индекс элемента, должен быть неотрицательным целым числом.
**Возвращает:** Элемент типа `T` по указанному индексу.
**Реализация:** Реализован в `ATAS.Indicators.BaseDataSeries<T>`.

### События (Events)

#### `Changed`

**Тип:** `Action<int>`
**Описание:** Событие, возникающее, когда серия данных изменяется на определенном баре.

### Подробное описание (Detailed Description)

**Описание:**
*   Интерфейс для серий данных, предоставляющий основные свойства и методы.
*   Интерфейс для типизированных серий данных, предоставляющий основные свойства и методы.

### Файл документации

**Файл:** `BaseDataSeries.cs` - Документация для этого интерфейса была сгенерирована из этого файла.







## Методичка: Интерфейс `ATAS.Indicators.IDrawingObjectsListInfo`

### Описание интерфейса

Интерфейс `ATAS.Indicators.IDrawingObjectsListInfo` предназначен для предоставления информации о списке графических объектов, нарисованных на графике.  Этот интерфейс позволяет получить доступ к коллекциям горизонтальных линий и горизонтальных каналов, которые были добавлены на график пользователем или программно.

### Свойства интерфейса

Интерфейс `IDrawingObjectsListInfo` включает в себя следующие свойства:

1.  **`HorizontalLines`**

    *   **Тип:** `IEnumerable<HorizontalLine>`
    *   **Описание:** Возвращает коллекцию горизонтальных линий (`HorizontalLine`), нарисованных на графике.
    *   **Аннотация:** `IEnumerable<HorizontalLine>` означает, что свойство `HorizontalLines` предоставляет возможность перечисления (итерации) по списку объектов типа `HorizontalLine`. Каждый объект `HorizontalLine` представляет собой горизонтальную линию, добавленную на график в ATAS.

2.  **`HorizontalChannels`**

    *   **Тип:** `IEnumerable<HorizontalChannel>`
    *   **Описание:** Возвращает коллекцию горизонтальных каналов (`HorizontalChannel`), нарисованных на графике.
    *   **Аннотация:** Аналогично `HorizontalLines`, `IEnumerable<HorizontalChannel>` означает перечислимую коллекцию объектов типа `HorizontalChannel`.  `HorizontalChannel` представляет собой графический элемент "горизонтальный канал" в ATAS, используемый для анализа ценовых уровней.

### Подробная документация свойств

**`HorizontalChannels`**

*   **Тип:** `IEnumerable<HorizontalChannel>`
*   **Полное имя свойства:** `ATAS.Indicators.IDrawingObjectsListInfo.HorizontalChannels`
*   **Описание:**  Свойство `HorizontalChannels` предоставляет доступ к коллекции всех горизонтальных каналов, которые в данный момент отображаются на графике.  Вы можете использовать эту коллекцию для получения информации о каждом горизонтальном канале, например, его координат, стиля и других параметров.
*   **Аннотация:** Полное имя свойства указывает на его принадлежность к интерфейсу `IDrawingObjectsListInfo` в пространстве имен `ATAS.Indicators`. Это помогает точно определить, где именно в программной структуре ATAS находится это свойство.

**`HorizontalLines`**

*   **Тип:** `IEnumerable<HorizontalLine>`
*   **Полное имя свойства:** `ATAS.Indicators.IDrawingObjectsListInfo.HorizontalLines`
*   **Описание:** Свойство `HorizontalLines` предоставляет коллекцию всех горизонтальных линий, нарисованных на графике.  Это свойство позволяет программно получать доступ и анализировать горизонтальные линии, добавленные на график.
*   **Аннотация:**  Как и `HorizontalChannels`, полное имя свойства `ATAS.Indicators.IDrawingObjectsListInfo.HorizontalLines` уточняет его местоположение в иерархии классов и интерфейсов ATAS.

### Источник информации

Документация для интерфейса `ATAS.Indicators.IDrawingObjectsListInfo` была сгенерирована из файла `IIndicatorContainer.cs`.  Этот файл, вероятно, является частью исходного кода ATAS и содержит определения интерфейсов и классов, связанных с индикаторами и графическими объектами.

**Аннотация общего характера:**

`IEnumerable<T>` (где `T` - это тип объекта, например `HorizontalLine` или `HorizontalChannel`)  является стандартным интерфейсом в .NET (платформа, на которой построен ATAS). Он позволяет перебирать элементы коллекции один за другим, не зная, как именно коллекция хранит данные. Это очень удобно для обработки списков объектов, таких как графические элементы на графике.  `[get]` указывает на то, что свойство доступно только для чтения (get-accessor), то есть вы можете получить значение свойства, но не можете его изменить напрямую.






## Методичка по интерфейсу `ATAS.Indicators.IFilter` для создания индикаторов ATAS

**Название интерфейса:** `ATAS.Indicators.IFilter`

**Описание:**
Интерфейс `ATAS.Indicators.IFilter` представляет собой фильтр с общими свойствами и функциональностью. Он используется для создания индикаторов в платформе ATAS, которые могут фильтровать или обрабатывать данные определенным образом.

**Диаграмма наследования:**

```
ATAS.Indicators.FilterBase
    /\
   /  \
  /    \
ATAS.Indicators.IFilter
    /\          /\
   /  \        /  \
  /    \      /    \
INotifyPropertyChanged   ICloneable
    /\
   /  \
  /    \
ATAS.Indicators.IFilterValue
    /\
   /  \
  /    \
ATAS.Indicators.IFilterEnum
    /\
   /  \
  /    \
ATAS.Indicators.<TValue>
    /\
   /  \
  /    \
ATAS.Indicators.<TEnum>
```

**Пояснение к диаграмме наследования:**

*   `ATAS.Indicators.IFilter` наследуется от `INotifyPropertyChanged` и `ICloneable`.
*   `ATAS.Indicators.IFilter` является частью более общей иерархии классов и интерфейсов для фильтров в ATAS.
*   `ATAS.Indicators.FilterBase` является базовым классом для фильтров и реализует часть функциональности `IFilter`.
*   `INotifyPropertyChanged` используется для уведомления об изменениях свойств фильтра.
*   `ICloneable` позволяет создавать копии объектов фильтра.
*   `ATAS.Indicators.IFilterValue`, `ATAS.Indicators.IFilterEnum`, `ATAS.Indicators.<TValue>`, `ATAS.Indicators.<TEnum>` представляют собой специализированные интерфейсы фильтров, которые расширяют `IFilter` для работы с определенными типами данных (значениями и перечислениями).

**Диаграмма взаимодействия:**

```
INotifyPropertyChanged   ICloneable
    \                  /
     \                /
      \              /
       \            /
        \          /
         \        /
          ATAS.Indicators.IFilter
```

**Пояснение к диаграмме взаимодействия:**

*   Диаграмма показывает, что `ATAS.Indicators.IFilter` взаимодействует с интерфейсами `INotifyPropertyChanged` и `ICloneable`.
*   Это взаимодействие необходимо для обеспечения функциональности уведомлений об изменениях и возможности клонирования объектов фильтра.

**Свойства интерфейса `ATAS.Indicators.IFilter`:**

| Имя свойства    | Тип данных | Get | Set | Описание                                                                 | Реализация                                  |
| :--------------- | :--------- | :-: | :-: | :----------------------------------------------------------------------- | :------------------------------------------ |
| `Enabled`        | `bool`     |  ✓  |  ✓  | Получает или устанавливает значение, указывающее, включен ли фильтр.     | `ATAS.Indicators.FilterBase`                |
| `EnabledVisible` | `bool`     |  ✓  |     | Получает значение, указывающее, видимо ли свойство "Enabled" для пользователей. | `ATAS.Indicators.FilterBase`                |
| `AsScalar`       | `bool`     |  ✓  |     | Получает значение, указывающее, работает ли фильтр в скалярном режиме. |  `ATAS.Indicators.IFilter.AsScalar` (собственное) |

**Описание свойств:**

*   **`Enabled`**:
    *   Тип: `bool` (логическое значение - истина или ложь).
    *   Доступ: `get`, `set` (можно читать и записывать значение).
    *   Описание: Это свойство позволяет программно включать или отключать фильтр. Когда `Enabled` установлено в `true`, фильтр активен и применяется к данным. Когда `Enabled` установлено в `false`, фильтр неактивен и не влияет на данные.
    *   Реализация: Реализовано в базовом классе `ATAS.Indicators.FilterBase`, что означает, что все фильтры, основанные на `FilterBase` или реализующие `IFilter`, автоматически получают это свойство.

*   **`EnabledVisible`**:
    *   Тип: `bool` (логическое значение - истина или ложь).
    *   Доступ: `get` (только чтение значения).
    *   Описание: Это свойство определяет, будет ли свойство `Enabled` видимо для пользователя в настройках индикатора в платформе ATAS. Если `EnabledVisible` возвращает `true`, пользователь сможет видеть и изменять настройку "Включено" для фильтра в интерфейсе ATAS. Если `false`, то свойство "Включено" будет скрыто от пользователя.
    *   Реализация: Реализовано в базовом классе `ATAS.Indicators.FilterBase`.

*   **`AsScalar`**:
    *   Тип: `bool` (логическое значение - истина или ложь).
    *   Доступ: `get` (только чтение значения).
    *   Описание: Это свойство указывает, работает ли фильтр в "скалярном" режиме. Скалярный режим может подразумевать, что фильтр обрабатывает каждое значение данных независимо, а не, например, серии данных или массивы.  Точное значение "скалярного режима" может зависеть от конкретной реализации фильтра.
    *   Реализация:  Свойство `AsScalar` определено непосредственно в интерфейсе `ATAS.Indicators.IFilter`, но детали его реализации будут зависеть от конкретного класса, реализующего этот интерфейс.

**Подробное описание интерфейса:**

Интерфейс `ATAS.Indicators.IFilter` предназначен для представления общих свойств и функциональности, которые должны быть у всех фильтров в ATAS.  Он обеспечивает стандартный набор свойств для управления состоянием фильтра (включен/выключен) и его видимостью в пользовательском интерфейсе.  Разработчики индикаторов могут использовать этот интерфейс для создания собственных фильтров, которые будут интегрироваться в платформу ATAS и предоставлять пользователям стандартные механизмы управления фильтрацией данных.

**Файл, в котором определен интерфейс:**

*   `IFilter.cs`

**Заключение:**

Интерфейс `ATAS.Indicators.IFilter` является важной частью архитектуры индикаторов ATAS, предоставляя основу для создания фильтров с общими свойствами управления. Понимание этого интерфейса необходимо для разработки продвинутых индикаторов, использующих механизмы фильтрации данных в платформе ATAS.






## Методичка по интерфейсу ATAS.Indicators.IFilterEnum для создания индикаторов ATAS

**Введение:**

Интерфейс `ATAS.Indicators.IFilterEnum` используется в платформе ATAS для создания индикаторов, которые работают с перечислениями (enums). Этот интерфейс расширяет базовый интерфейс `IFilter` и добавляет специфическую функциональность для обработки типов перечислений.

**Диаграмма наследования:**

```
INotifyPropertyChanged  // Интерфейс для уведомления об изменении свойств
    /\
     |
ICloneable             // Интерфейс для создания клонов объектов
    /\
     |
IFilter                // Базовый интерфейс фильтра в ATAS
    /\
     |
ATAS.Indicators.IFilterEnum // Интерфейс для фильтров, работающих с перечислениями
    /\
     |
ATAS.Indicators.FilterEnum<TEnum> // Реализация фильтра перечислений (TEnum - тип перечисления)
```

*   **`INotifyPropertyChanged`**:  Базовый интерфейс .NET Framework, позволяющий объектам уведомлять об изменениях своих свойств. Это важно для обновления интерфейса пользователя, когда данные индикатора меняются.
*   **`ICloneable`**: Интерфейс .NET Framework для поддержки клонирования объектов. Полезен для создания копий индикаторов.
*   **`IFilter`**: Базовый интерфейс для всех фильтров в ATAS. Он определяет общую структуру и поведение фильтров. `IFilterEnum` наследует от него, расширяя его возможности.
*   **`ATAS.Indicators.IFilterEnum`**:  Интерфейс, специфичный для фильтров, работающих с типами перечислений (enum). Он добавляет свойство `EnumType` для указания типа перечисления, с которым работает фильтр.
*   **`ATAS.Indicators.FilterEnum<TEnum>`**:  Это конкретная реализация интерфейса `IFilterEnum`. `<TEnum>` является параметром типа, который определяет, какое именно перечисление будет использоваться в фильтре.

**Диаграмма взаимодействия:**

Диаграмма взаимодействия показывает те же отношения, что и диаграмма наследования, но с точки зрения взаимодействия объектов во время выполнения программы.  Структура остается идентичной диаграмме наследования, подчеркивая, как `IFilterEnum` и `FilterEnum<TEnum>` вписываются в общую иерархию фильтров ATAS.

**Свойства интерфейса `ATAS.Indicators.IFilterEnum`:**

| Тип свойства | Имя свойства | Описание                                                                                                                                   |
| :----------- | :----------- | :------------------------------------------------------------------------------------------------------------------------------------------- |
| `EnumType`   | `EnumType`   |  **Описание:**  Свойство `EnumType` определяет тип перечисления (enum), с которым будет работать данный фильтр. <br> **Тип:** `EnumType` (тип данных, представляющий тип перечисления) <br> **Доступ:** `[get]` (только для чтения, можно получить значение свойства) |

**Реализация:**

Интерфейс `ATAS.Indicators.IFilterEnum` реализован в классе `ATAS.Indicators.FilterEnum<TEnum>`.  Это означает, что при создании индикатора, который должен работать с перечислениями, вам, вероятно, потребуется использовать класс `FilterEnum<TEnum>` и указать конкретный тип перечисления `<TEnum>`.

**Расположение в файлах:**

Документация для этого интерфейса была сгенерирована из файла `IFilterEnum.cs`. Это указывает на то, что исходный код интерфейса `IFilterEnum` находится в файле с именем `IFilterEnum.cs`.

**Заключение:**

Интерфейс `ATAS.Indicators.IFilterEnum` является важным элементом для создания индикаторов ATAS, которые могут фильтровать или обрабатывать данные на основе значений перечислений. Понимание его структуры, свойств и реализации поможет в разработке продвинутых и гибких технических индикаторов.









## Методичка по ATAS.Indicators.IFilterValue Interface Reference

**Описание:**

Интерфейс `ATAS.Indicators.IFilterValue` представляет собой фильтр с гибким значением, которое может содержать данные любого типа. Он наследует свойства интерфейса `ATAS.Indicators.IFilter`.

**Диаграмма наследования:**

```
INotifyPropertyChanged
    /\
    ||
IFilter
    /\
    ||
ATAS.Indicators.IFilterValue
    /\
    ||
ATAS.Indicators.Filter<TValue>
```

**Диаграмма взаимодействия:**

```
INotifyPropertyChanged   ICloneable
         \           /
          \         /
           \       /
            \     /
             \   /
              \ /
             IFilter
               |
               |
ATAS.Indicators.IFilterValue
```

**Свойства:**

*   **Value** `[get, set]`

    *   Тип: `object`
    *   Описание: Возвращает или устанавливает значение фильтра. Значение может содержать данные любого типа.

**Подробное описание:**

Интерфейс `ATAS.Indicators.IFilterValue` представляет собой фильтр с гибким значением, которое может содержать данные любого типа. Он наследует свойства интерфейса `ATAS.Indicators.IFilter`.

**Документация свойства:**

*   **Value**

    *   Тип: `object`
    *   Пространство имен: `ATAS.Indicators.IFilterValue`
    *   Операции: `get`, `set`
    *   Описание: Возвращает или устанавливает значение фильтра. Значение может содержать данные любого типа.
    *   Реализовано в: `ATAS.Indicators.Filter<TValue>`.

**Файл:**

*   `IFilterValue.cs`







## Методичка по интерфейсу `ATAS.Indicators.IIndicatorContainer` для создания индикаторов ATAS

### Описание интерфейса

Интерфейс `ATAS.Indicators.IIndicatorContainer` предназначен для контейнера индикатора, который хранит информацию и методы, связанные с индикаторами.

### Диаграмма наследования

```
IContainer
    ^
    |
ATAS.Indicators.IIndicatorContainer
```

**Аннотация:**
*   `IContainer`: Базовый интерфейс, от которого наследуется `ATAS.Indicators.IIndicatorContainer`.
*   `ATAS.Indicators.IIndicatorContainer`: Интерфейс контейнера индикаторов, который расширяет функциональность `IContainer`.

### Диаграмма взаимодействия

```
IContainer   <--   ATAS.Indicators.IIndicatorContainer
```

**Аннотация:**
*   `IContainer` и `ATAS.Indicators.IIndicatorContainer` взаимодействуют друг с другом.  `ATAS.Indicators.IIndicatorContainer` использует функциональность `IContainer`.

### Публичные функции-члены

#### `GetYByValue`

```csharp
int GetYByValue (decimal value)
```

**Описание:** Возвращает Y-координату, соответствующую указанному десятичному значению в контейнере индикатора.

**Параметры:**

*   `value` (decimal): Десятичное значение, для которого необходимо получить Y-координату.

**Возвращаемое значение:**

*   `int`: Y-координата, соответствующая указанному десятичному значению.

**Аннотация:**
*   `int`: Тип возвращаемого значения - целое число, представляющее Y-координату.
*   `GetYByValue`: Имя функции, используется для получения Y-координаты по заданному значению.
*   `(decimal value)`: Функция принимает один параметр `value` типа `decimal` (десятичное число).
*   Функция позволяет определить положение на вертикальной оси индикатора для конкретного значения.

### Свойства

#### `Maximum`

```csharp
decimal Maximum [get]
```

**Описание:** Возвращает максимальное значение в контейнере индикатора.

**Аннотация:**
*   `decimal`: Тип свойства - десятичное число, представляющее максимальное значение.
*   `Maximum`: Имя свойства, обозначает максимальное значение.
*   `[get]`:  Свойство доступно только для чтения (get-accessor), то есть можно только получить значение, но не изменить его напрямую.
*   Это свойство позволяет узнать максимальное значение, которое отображается в контейнере индикатора.

#### `Minimum`

```csharp
decimal Minimum [get]
```

**Описание:** Возвращает минимальное значение в контейнере индикатора.

**Аннотация:**
*   `decimal`: Тип свойства - десятичное число, представляющее минимальное значение.
*   `Minimum`: Имя свойства, обозначает минимальное значение.
*   `[get]`: Свойство доступно только для чтения (get-accessor).
*   Это свойство позволяет узнать минимальное значение, которое отображается в контейнере индикатора.

#### `RelativeRegion`

```csharp
Rectangle RelativeRegion [get, set]
```

**Описание:** Возвращает или устанавливает относительную область контейнера индикатора.

**Аннотация:**
*   `Rectangle`: Тип свойства - структура `Rectangle`, представляющая прямоугольную область.
*   `RelativeRegion`: Имя свойства, обозначает относительную область.
*   `[get, set]`: Свойство доступно для чтения и записи (get и set accessors), то есть можно как получить текущую область, так и установить новую.
*   Относительная область определяет видимую часть контейнера индикатора.

### События

#### `OnChange`

```csharp
Action<Rectangle> OnChange
```

**Описание:** Событие, которое возникает при изменении области контейнера индикатора.

**Аннотация:**
*   `Action<Rectangle>`: Тип события - делегат `Action`, который принимает параметр типа `Rectangle`.
*   `OnChange`: Имя события, срабатывает при изменении области.
*   `<Rectangle>`: Событие передает информацию о новой области в виде объекта `Rectangle`.
*   Это событие позволяет отслеживать изменения видимой области индикатора.

### Подробное описание

Интерфейс для контейнера индикатора, который хранит информацию и методы, связанные с индикаторами.

**Аннотация:**
*   Повторение основного назначения интерфейса для закрепления понимания.

### Документация интерфейса была сгенерирована из файла:

*   `IIndicatorContainer.cs`

**Аннотация:**
*   Указание на исходный файл, где определен интерфейс `IIndicatorContainer`. Это может быть полезно для поиска дополнительной информации или контекста в исходном коде.









## Методичка по интерфейсу `ATAS.Indicators.IIndicatorDataProvider`

### Описание интерфейса

Интерфейс `ATAS.Indicators.IIndicatorDataProvider` представляет собой поставщика данных для индикатора, обеспечивая доступ к различным данным и сервисам, связанным с индикатором.

### Диаграмма наследования

```
ATAS.Indicators.IIndicator
DataProvider
  ↑
ATAS.Indicators.Indicator
DataProvider
```

### Публичные функции-члены

#### `DateTime GetCustomStartTime(DateTime time, TimeSpan timeFrame)`

*   **Описание:** Возвращает пользовательское время начала свечи, определенное указанным таймфреймом и текущим временем.
*   **Параметры:**
    *   `time` - `DateTime`: Текущее время.
    *   `timeFrame` - `TimeSpan`: Таймфрейм.
*   **Возвращаемое значение:** `DateTime` - Пользовательское время начала свечи.

#### `bool IsNewSession(DateTime prevTime, DateTime newTime)`

*   **Описание:** Проверяет, началась ли новая торговая сессия между указанным предыдущим временем и новым временем.
*   **Параметры:**
    *   `prevTime` - `DateTime`: Предыдущее время.
    *   `newTime` - `DateTime`: Новое время.
*   **Возвращаемое значение:** `bool` - `true`, если новая торговая сессия началась; иначе `false`.

#### `bool IsNewWeek(DateTime prevTime, DateTime newTime)`

*   **Описание:** Проверяет, началась ли новая торговая неделя между указанным предыдущим временем и новым временем.
*   **Параметры:**
    *   `prevTime` - `DateTime`: Предыдущее время.
    *   `newTime` - `DateTime`: Новое время.
*   **Возвращаемое значение:** `bool` - `true`, если новая торговая неделя началась; иначе `false`.

#### `bool IsNewMonth(DateTime prevTime, DateTime newTime)`

*   **Описание:** Проверяет, начался ли новый торговый месяц между указанным предыдущим временем и новым временем.
*   **Параметры:**
    *   `prevTime` - `DateTime`: Предыдущее время.
    *   `newTime` - `DateTime`: Новое время.
*   **Возвращаемое значение:** `bool` - `true`, если новый торговый месяц начался; иначе `false`.

#### `void AddAlert(string soundFile, string instrument, string message, CrossColor background, CrossColor foreground)`

*   **Описание:** Добавляет оповещение с указанными деталями к индикатору.
*   **Параметры:**
    *   `soundFile` - `string`: Путь к звуковому файлу для воспроизведения при оповещении.
    *   `instrument` - `string`: Инструмент, связанный с оповещением.
    *   `message` - `string`: Текст сообщения оповещения.
    *   `background` - `CrossColor`: Цвет фона оповещения.
    *   `foreground` - `CrossColor`: Цвет переднего плана оповещения.
*   **Возвращаемое значение:** `void` - Функция не возвращает значения.

#### `void DoActionInGuiThread(Action action)`

*   **Описание:** Выполняет указанное действие в потоке GUI (графического интерфейса пользователя).
*   **Параметры:**
    *   `action` - `Action`: Действие для выполнения в потоке GUI.
*   **Возвращаемое значение:** `void` - Функция не возвращает значения.

### Свойства

#### `string Name { get; }`

*   **Описание:** Возвращает имя поставщика данных индикатора.
*   **Тип:** `string`
*   **Доступ:** `get` - Только для чтения.

#### `IChart ChartInfo { get; }`

*   **Описание:** Возвращает информацию о графике, связанную с индикатором.
*   **Тип:** `IChart`
*   **Доступ:** `get` - Только для чтения.

#### `IPlatformSettings GlobalPlatformSettings { get; }`

*   **Описание:** Возвращает глобальные настройки платформы, используемые индикатором.
*   **Тип:** `IPlatformSettings`
*   **Доступ:** `get` - Только для чтения.

#### `IOnlineDataProvider OnlineDataProvider { get; }`

*   **Описание:** Возвращает поставщика онлайн-данных, используемого индикатором для получения данных в реальном времени.
*   **Тип:** `IOnlineDataProvider`
*   **Доступ:** `get` - Только для чтения.

#### `ObservableCollection<CandlePartSeries> CandlesDataSeries { get; }`

*   **Описание:** Возвращает коллекцию серий частей свечей, используемых индикатором.
*   **Тип:** `ObservableCollection<CandlePartSeries>`
*   **Доступ:** `get` - Только для чтения.

#### `ObservableCollection<string> Panels { get; }`

*   **Описание:** Возвращает коллекцию панелей, связанных с индикатором.
*   **Тип:** `ObservableCollection<string>`
*   **Доступ:** `get` - Только для чтения.

#### `MarketDepthInfoProvider MarketDepthInfoProvider { get; }`

*   **Описание:** Возвращает поставщика информации о глубине рынка, используемого индикатором для доступа к данным глубины рынка.
*   **Тип:** `MarketDepthInfoProvider`
*   **Доступ:** `get` - Только для чтения.

#### `IInstrumentInfo InstrumentInfo { get; }`

*   **Описание:** Возвращает информацию об инструменте, связанную с инструментом индикатора.
*   **Тип:** `IInstrumentInfo`
*   **Доступ:** `get` - Только для чтения.

#### `ITradingManager TradingManager { get; }`

*   **Описание:** Возвращает менеджер торговли, используемый индикатором для управления задачами, связанными с торговлей.
*   **Тип:** `ITradingManager`
*   **Доступ:** `get` - Только для чтения.

#### `ITradingStatisticsProvider TradingStatisticsProvider { get; }`

*   **Описание:** Возвращает поставщика торговой статистики, используемого индикатором для доступа к статистике, связанной с торговлей.
*   **Тип:** `ITradingStatisticsProvider`
*   **Доступ:** `get` - Только для чтения.

### Файл документации

Документация для этого интерфейса была сгенерирована из следующего файла:

*   `IndicatorDataProvider.cs`







### Методичка по интерфейсу `ATAS.Indicators.IInstrumentInfo` для создания индикаторов ATAS

#### Описание интерфейса

Интерфейс `ATAS.Indicators.IInstrumentInfo` представляет информацию об инструменте.

**Диаграмма наследования:**

```
ATAS.Indicators.IInstrumentInfo
    ↑
ATAS.Indicators.InstrumentInfo
```

**Описание:**

Интерфейс предназначен для доступа к свойствам инструмента, таким как название инструмента, биржи, размер тика и временная зона.

#### Свойства интерфейса

*   **`Instrument`** `[get]`

    Тип: `string`

    Описание: Возвращает название инструмента.

    Реализовано в: `ATAS.Indicators.InstrumentInfo`.

*   **`Exchange`** `[get]`

    Тип: `string`

    Описание: Возвращает название биржи, на которой торгуется инструмент.

    Реализовано в: `ATAS.Indicators.InstrumentInfo`.

*   **`TickSize`** `[get]`

    Тип: `decimal`

    Описание: Возвращает размер тика инструмента, который является минимальным шагом цены.

    Реализовано в: `ATAS.Indicators.InstrumentInfo`.

*   **`TimeZone`** `[get]`

    Тип: `int`

    Описание: Возвращает временную зону инструмента.

    Реализовано в: `ATAS.Indicators.InstrumentInfo`.

#### Подробное описание свойств

*   **`Exchange`**

    Тип: `string` `ATAS.Indicators.IInstrumentInfo.Exchange` `[get]`

    Описание: Возвращает название биржи, на которой торгуется инструмент.

    Реализовано в: `ATAS.Indicators.InstrumentInfo`.

*   **`Instrument`**

    Тип: `string` `ATAS.Indicators.IInstrumentInfo.Instrument` `[get]`

    Описание: Возвращает название инструмента.

    Реализовано в: `ATAS.Indicators.InstrumentInfo`.

*   **`TickSize`**

    Тип: `decimal` `ATAS.Indicators.IInstrumentInfo.TickSize` `[get]`

    Описание: Возвращает размер тика инструмента, который является минимальным шагом цены.

    Реализовано в: `ATAS.Indicators.InstrumentInfo`.

*   **`TimeZone`**

    Тип: `int` `ATAS.Indicators.IInstrumentInfo.TimeZone` `[get]`

    Описание: Возвращает временную зону инструмента.

    Реализовано в: `ATAS.Indicators.InstrumentInfo`.

#### Файл документации

Документация для этого интерфейса была сгенерирована из следующего файла:

*   `InstrumentInfo.cs`









## Методичка по интерфейсу ATAS.Indicators.IIntCandle

**Описание интерфейса:** `ATAS.Indicators.IIntCandle` представляет интерфейс для целочисленной свечи.

### Свойства интерфейса

1.  **Свойство:** `Open`
    *   **Тип данных:** `int`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Цена открытия свечи.
    *   **Доступ:** `[get, set]`

2.  **Свойство:** `High`
    *   **Тип данных:** `int`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Максимальная цена свечи.
    *   **Доступ:** `[get, set]`

3.  **Свойство:** `Low`
    *   **Тип данных:** `int`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Минимальная цена свечи.
    *   **Доступ:** `[get, set]`

4.  **Свойство:** `Close`
    *   **Тип данных:** `int`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Цена закрытия свечи.
    *   **Доступ:** `[get, set]`

5.  **Свойство:** `Ask`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Цена Ask свечи.
    *   **Доступ:** `[get, set]`

6.  **Свойство:** `Bid`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Цена Bid свечи.
    *   **Доступ:** `[get, set]`

7.  **Свойство:** `Betweens`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Объем между Bid и Ask.
    *   **Доступ:** `[get, set]`

8.  **Свойство:** `Delta`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Значение дельты свечи.
    *   **Доступ:** `[get]`

9.  **Свойство:** `Ticks`
    *   **Тип данных:** `int`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Количество тиков в свече.
    *   **Доступ:** `[get, set]`

10. **Свойство:** `BeginTime`
    *   **Тип данных:** `DateTime`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Время начала свечи.
    *   **Доступ:** `[get, set]`

11. **Свойство:** `LastTime`
    *   **Тип данных:** `DateTime`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Время окончания свечи.
    *   **Доступ:** `[get, set]`

12. **Свойство:** `Volume`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Объем свечи.
    *   **Доступ:** `[get]`

13. **Свойство:** `OI`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Открытый интерес свечи.
    *   **Доступ:** `[get, set]`

14. **Свойство:** `MaxOI`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Максимальное значение открытого интереса свечи.
    *   **Доступ:** `[get, set]`

15. **Свойство:** `MinOI`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Минимальное значение открытого интереса свечи.
    *   **Доступ:** `[get, set]`

16. **Свойство:** `MaxDelta`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Максимальное значение дельты свечи.
    *   **Доступ:** `[get, set]`

17. **Свойство:** `MinDelta`
    *   **Тип данных:** `decimal`
    *   **Пространство имен:** `ATAS.Indicators.IIntCandle`
    *   **Описание:** Минимальное значение дельты свечи.
    *   **Доступ:** `[get, set]`






## Методичка по интерфейсу `ATAS.Indicators.IKeyboardInfo` для создания индикаторов ATAS

### Интерфейс `ATAS.Indicators.IKeyboardInfo`

**Описание интерфейса:**

Интерфейс для предоставления информации о состоянии клавиатуры.

**Свойства:**

*   **`PressedKey`** `[get]`
    *   Тип: `CrossKeyEventArgs`
    *   Описание: Возвращает информацию о клавише, которая в данный момент нажата на клавиатуре.

**Детальное описание:**

Интерфейс `ATAS.Indicators.IKeyboardInfo` предназначен для получения данных о текущем состоянии клавиатуры в платформе ATAS. Он позволяет индикаторам реагировать на действия пользователя, связанные с нажатием клавиш.

**Документация по свойству:**

#### `PressedKey`

```csharp
CrossKeyEventArgs ATAS.Indicators.IKeyboardInfo.PressedKey { get; }
```

*   Тип свойства: `CrossKeyEventArgs`
*   Пространство имен: `ATAS.Indicators.IKeyboardInfo`
*   Доступность: `get` (только для чтения)

**Описание свойства:**

Свойство `PressedKey` предоставляет объект типа `CrossKeyEventArgs`, который содержит информацию о клавише, в данный момент нажатой пользователем. Это свойство позволяет индикатору определить, какая клавиша была нажата, и использовать эту информацию для выполнения определенных действий или отображения данных.

**Источник:**

Документация сгенерирована из файла: `IIndicatorContainer.cs`

**Аннотации на русском:**

*   **Интерфейс (Interface):**  Набор правил и методов, которые описывают, как разные части программы могут взаимодействовать друг с другом. В данном случае, `IKeyboardInfo` — это интерфейс, который говорит индикаторам, как получать информацию о клавиатуре.
*   **Свойство (Property):** Характеристика объекта, к которой можно получить доступ. `PressedKey` — это свойство интерфейса `IKeyboardInfo`, которое позволяет получить информацию о нажатой клавише.
*   **`[get]`:**  Указывает, что свойство доступно только для чтения (get accessor). Это значит, что индикатор может только получить значение `PressedKey`, но не может его изменить.
*   **`CrossKeyEventArgs`:** Тип данных, который содержит информацию о событии нажатия клавиши. Включает в себя данные о том, какая клавиша была нажата, и другие детали события.
*   **Пространство имен (Namespace):** Способ организации кода в иерархические группы. `ATAS.Indicators.IKeyboardInfo` указывает, где именно в коде ATAS находится этот интерфейс.
*   **Доступность (Accessibility):**  Определяет, как можно использовать свойство или метод. `get` означает, что свойство можно только читать.
*   **Документация сгенерирована из файла:** Указывает, из какого файла исходного кода была создана эта документация. Это полезно для разработчиков, чтобы найти исходный код и лучше понять работу интерфейса.








## Методичка по интерфейсу `ATAS.Indicators.IKnowFixedProfiles` для создания индикаторов ATAS

### Описание интерфейса

Интерфейс `ATAS.Indicators.IKnowFixedProfiles` представляет собой интерфейс для объектов, которые работают с фиксированными профилями и могут запрашивать их.  Он предназначен для индикаторов, которым необходимо получать данные фиксированных профилей рынка для анализа и отображения.

### Диаграмма наследования

```
ATAS.Indicators.IKnowFixedProfiles
    ↑
ATAS.Indicators.IOnlineDataProvider
```

**Пояснение:** Интерфейс `ATAS.Indicators.IKnowFixedProfiles` наследуется от интерфейса `ATAS.Indicators.IOnlineDataProvider`. Это означает, что любой класс, реализующий `IKnowFixedProfiles`, также должен реализовывать функциональность `IOnlineDataProvider`, необходимую для работы с онлайн-данными в ATAS.

### Публичные методы интерфейса

#### `GetFixedProfile(FixedProfileRequest request)`

**Описание:** Метод `GetFixedProfile` предназначен для синхронного запроса фиксированного профиля рынка. Запрос основывается на параметрах, указанных в объекте `FixedProfileRequest`.

**Синтаксис:**
```csharp
void GetFixedProfile (FixedProfileRequest request)
```

**Параметры:**
*   `request` : `FixedProfileRequest` - Объект запроса, содержащий параметры для формирования фиксированного профиля (например, период, ценовой диапазон и т.д.).

**Возвращаемое значение:**
*   `void` - Метод не возвращает значения напрямую. Результат запроса фиксированного профиля будет обработан через события интерфейса (например, `FixedProfileReceived` или `FixedProfileBothCandlesReceived`).

#### `RequestFixedProfileAsync(FixedProfileRequest request)`

**Описание:** Метод `RequestFixedProfileAsync` предназначен для асинхронного запроса фиксированного профиля рынка. Асинхронность позволяет избежать блокировки основного потока исполнения во время ожидания ответа на запрос, что важно для поддержания отзывчивости интерфейса пользователя. Запрос также основывается на параметрах, указанных в объекте `FixedProfileRequest`.

**Синтаксис:**
```csharp
Task<FixedProfileResponse> RequestFixedProfileAsync (FixedProfileRequest request)
```

**Параметры:**
*   `request` : `FixedProfileRequest` - Объект запроса, содержащий параметры для формирования фиксированного профиля.

**Возвращаемое значение:**
*   `Task<FixedProfileResponse>` - Объект `Task`, представляющий асинхронную операцию. После завершения операции, `Task` будет содержать объект `FixedProfileResponse`, который содержит запрошенный фиксированный профиль.

### События интерфейса

#### `FixedProfileBothCandlesReceived`

**Описание:** Событие `FixedProfileBothCandlesReceived` возникает, когда получены как основная, так и вторичная индикаторные свечи фиксированного профиля. Это событие позволяет индикатору обрабатывать данные профиля, как только все необходимые данные для отображения профиля становятся доступны.

**Синтаксис:**
```csharp
Action<IndicatorCandle, IndicatorCandle, FixedProfilePeriods> FixedProfileBothCandlesReceived
```

**Параметры события:**
*   `IndicatorCandle` (первый) - Основная индикаторная свеча фиксированного профиля.
*   `IndicatorCandle` (второй) - Вторичная индикаторная свеча фиксированного профиля.
*   `FixedProfilePeriods` - Периоды, для которых был рассчитан фиксированный профиль.

#### `FixedProfileReceived` **[Obsolete - Устаревшее]**

**Описание:** Событие `FixedProfileReceived` (устаревшее) возникало, когда был получен фиксированный профиль.  Вероятно, в более новых версиях ATAS рекомендуется использовать `FixedProfileBothCandlesReceived` для более полного набора данных профиля.

**Синтаксис:**
```csharp
Action<IndicatorCandle, FixedProfilePeriods> FixedProfileReceived
```

**Параметры события:**
*   `IndicatorCandle` - Индикаторная свеча фиксированного профиля.
*   `FixedProfilePeriods` - Периоды, для которых был рассчитан фиксированный профиль.

### Расположение в файлах

Документация для данного интерфейса была сгенерирована из файла:

*   `PriceVolumeInfo.cs`

**Примечание:** Для реализации индикатора, использующего фиксированные профили, необходимо реализовать интерфейс `ATAS.Indicators.IKnowFixedProfiles` и обрабатывать события `FixedProfileReceived` или `FixedProfileBothCandlesReceived` для получения и обработки данных профиля.  Также необходимо использовать методы `GetFixedProfile` или `RequestFixedProfileAsync` для запроса фиксированных профилей с нужными параметрами.








## Методичка: ATAS.Indicators.IMarketByOrdersCache Interface Reference

**Описание:**

Интерфейс `ATAS.Indicators.IMarketByOrdersCache` предназначен для менеджера, предоставляющего доступ к кешу рынка по ордерам.

**Диаграмма наследования:**

```
ITimeMarketDataCache<MarketByOrder> --> ATAS.Indicators.IMarketByOrdersCache
IMarketByOrdersDataProvider --> ATAS.Indicators.IMarketByOrdersCache
```

* **ITimeMarketDataCache<MarketByOrder>**:  Интерфейс `ITimeMarketDataCache`, параметризованный типом `MarketByOrder`, является родительским интерфейсом для `ATAS.Indicators.IMarketByOrdersCache`. Это означает, что `ATAS.Indicators.IMarketByOrdersCache` наследует свойства и методы от `ITimeMarketDataCache<MarketByOrder>`.

* **IMarketByOrdersDataProvider**: Интерфейс `IMarketByOrdersDataProvider` также является родительским интерфейсом для `ATAS.Indicators.IMarketByOrdersCache`.  `ATAS.Indicators.IMarketByOrdersCache` наследует свойства и методы от `IMarketByOrdersDataProvider`.

**Диаграмма взаимодействия:**

```
ITimeMarketDataCache<MarketByOrder> --> ATAS.Indicators.IMarketByOrdersCache
IMarketByOrdersDataProvider --> ATAS.Indicators.IMarketByOrdersCache
```

* **ITimeMarketDataCache<MarketByOrder>**: Интерфейс `ITimeMarketDataCache`, параметризованный типом `MarketByOrder`, взаимодействует с `ATAS.Indicators.IMarketByOrdersCache`.

* **IMarketByOrdersDataProvider**: Интерфейс `IMarketByOrdersDataProvider` также взаимодействует с `ATAS.Indicators.IMarketByOrdersCache`.

**Дополнительные унаследованные члены:**

* Свойства, унаследованные от `ATAS.Indicators.ITimeMarketDataCache<MarketByOrder>`.
* Свойства, унаследованные от `ATAS.Indicators.IMarketByOrdersDataProvider`.

**Детальное описание:**

Интерфейс для менеджера, который обеспечивает доступ к кешу рынка по ордерам.

**Файл документации:**

* `IOnlineDataProvider.cs`







## Методическое описание интерфейса `ATAS.Indicators.IMarketByOrdersDataProvider`

### Обзор интерфейса
- **Полное название:** `ATAS.Indicators.IMarketByOrdersDataProvider`
- **Описание:** Интерфейс для менеджера, который предоставляет доступ к данным рынка по ордерам.

### Диаграмма наследования
- **Наследуется от:**
    - `ATAS.Indicators.IMarketByOrdersCache`
    - `ATAS.Indicators.IOnlineDataProvider`
- **Связанные интерфейсы:**
    - `ATAS.Indicators.IMarketByOrdersWithTradesCache` (наследуется от `ATAS.Indicators.IMarketByOrdersCache`)

### Свойства
- **`MarketByOrders`**:
    - **Тип:** `IEnumerable<MarketByOrder>`
    - **Доступ:** `get` (только для чтения)
    - **Описание:** Возвращает моментальный снимок текущих рыночных данных по ордерам.

### Расположение в файлах
- **Файл:** `IOnlineDataProvider.cs` (уточнить корректность имени файла, возможно, ошибка OCR и файл должен называться `IMarketByOrdersDataProvider.cs` или находиться в другом файле)








``` методика
## ATAS.Indicators.IMarketByOrdersWithTradesCache Interface Reference

### Введение
Interface for manager that provides access to market by orders and trades cache.

### Диаграмма наследования для ATAS.Indicators.IMarketByOrdersWithTradesCache:
```
ITimeMarketDataCache
< IEntity >
    ↑
    |
ATAS.Indicators.IMarketByOrdersWithTradesCache
    ↑
    |
IMarketByOrdersDataProvider
```
* **Аннотация:** `ATAS.Indicators.IMarketByOrdersWithTradesCache` интерфейс наследуется от `ITimeMarketDataCache<IEntity>` и `IMarketByOrdersDataProvider`.

### Диаграмма взаимодействия для ATAS.Indicators.IMarketByOrdersWithTradesCache:
```
ITimeMarketDataCache
< IEntity >
    ↑       ↑
    |       |
    --------
    |
ATAS.Indicators.IMarketByOrdersWithTradesCache
    |
    --------
    |       |
    ↓       ↓
IMarketByOrdersDataProvider
```
* **Аннотация:** `ATAS.Indicators.IMarketByOrdersWithTradesCache` интерфейс взаимодействует с `ITimeMarketDataCache<IEntity>` и `IMarketByOrdersDataProvider`.

### Дополнительные унаследованные члены
* Свойства, унаследованные от `ATAS.Indicators.ITimeMarketDataCache<IEntity>`
* Свойства, унаследованные от `ATAS.Indicators.IMarketByOrdersDataProvider`
* **Аннотация:** Перечислены интерфейсы, от которых унаследованы свойства `ATAS.Indicators.IMarketByOrdersWithTradesCache`.

### Подробное описание
Interface for manager that provides access to market by orders and trades cache.
* **Аннотация:**  Интерфейс для менеджера, предоставляющий доступ к рынку по ордерам и кешу сделок.

### Файл документации:
* `IOnlineDataProvider.cs`
* **Аннотация:**  Документация для этого интерфейса находится в файле `IOnlineDataProvider.cs`.
```








## Методичка по интерфейсу `ATAS.Indicators.IMarketDepthInfoProvider` для создания индикаторов ATAS

**Описание интерфейса:**

Интерфейс `ATAS.Indicators.IMarketDepthInfoProvider` предназначен для предоставления информации о глубине рынка (Market Depth). Он позволяет получать данные, связанные с объемами заявок и предложений в стакане цен (DOM - Depth of Market).

**Основные компоненты интерфейса:**

1.  **Метод `GetMarketDepthSnapshot()`**

    *   **Назначение:**  Возвращает моментальный снимок (snapshot) данных о глубине рынка в момент запроса.
    *   **Тип возвращаемого значения:** `IEnumerable<MarketDataArg>`. Это означает, что метод возвращает коллекцию объектов типа `MarketDataArg`, каждый из которых представляет собой данные об уровне цен и объемах в стакане.
    *   **Описание:**  Метод позволяет получить актуальные данные о текущей глубине рынка.

2.  **Свойство `CumulativeDomAsks`**

    *   **Тип данных:** `decimal`.
    *   **Доступ:** `[get]` (только для чтения).
    *   **Назначение:**  Возвращает кумулятивную сумму объемов Ask (предложений) в стакане цен.
    *   **Описание:**  Это свойство предоставляет суммарный объем всех заявок на продажу, присутствующих в данный момент в стакане цен.

3.  **Свойство `CumulativeDomBids`**

    *   **Тип данных:** `decimal`.
    *   **Доступ:** `[get]` (только для чтения).
    *   **Назначение:**  Возвращает кумулятивную сумму объемов Bid (спроса) в стакане цен.
    *   **Описание:**  Это свойство предоставляет суммарный объем всех заявок на покупку, присутствующих в данный момент в стакане цен.

**Применение в индикаторах ATAS:**

Для использования интерфейса `ATAS.Indicators.IMarketDepthInfoProvider` в индикаторе ATAS, необходимо:

1.  **Получить доступ к интерфейсу:**  В коде индикатора необходимо получить экземпляр интерфейса `IMarketDepthInfoProvider`. Обычно это делается через контекст индикатора или другие механизмы, предоставляемые платформой ATAS.
2.  **Использовать методы и свойства:**  После получения доступа к интерфейсу, можно использовать метод `GetMarketDepthSnapshot()` для получения детальных данных о глубине рынка или свойства `CumulativeDomAsks` и `CumulativeDomBids` для получения агрегированных кумулятивных объемов.

**Пример использования (концептуальный):**

```csharp
// Предположим, что 'marketDepthProvider' - это экземпляр IMarketDepthInfoProvider

// Получение кумулятивного объема Ask
decimal cumulativeAskVolume = marketDepthProvider.CumulativeDomAsks;

// Получение кумулятивного объема Bid
decimal cumulativeBidVolume = marketDepthProvider.CumulativeDomBids;

// Получение снимка глубины рынка
IEnumerable<MarketDataArg> depthSnapshot = marketDepthProvider.GetMarketDepthSnapshot();

// Дальнейшая обработка полученных данных для индикатора
// ...
```

**Важные моменты:**

*   Интерфейс `IMarketDepthInfoProvider` предоставляет данные о глубине рынка в реальном времени.
*   Метод `GetMarketDepthSnapshot()` возвращает детальную информацию, которую можно использовать для анализа распределения объемов по уровням цен.
*   Свойства `CumulativeDomAsks` и `CumulativeDomBids` предоставляют агрегированные данные, удобные для отображения общей картины спроса и предложения.
*   Для эффективного использования данных глубины рынка, необходимо понимать структуру объекта `MarketDataArg` (хотя в данной документации она не описана, её можно найти в полной документации ATAS API).









## Методичка по интерфейсу `ATAS.Indicators.IMarketTimeProvider`

### Описание интерфейса

Интерфейс `ATAS.Indicators.IMarketTimeProvider` предназначен для предоставления доступа к рыночному времени в индикаторах ATAS. Он позволяет индикаторам получать текущее рыночное время и время UTC, а также подписываться на периодические события, связанные с течением времени.

### Диаграмма наследования

```
ATAS.Indicators.IMarketTimeProvider
    /\
   /  \
  /    \
ATAS.Indicators.ExtendedIndicator     ATAS.Indicators.IOnlineDataProvider
    /\
   /  \
  /    \
ATAS.Indicators.Indicator
    /\
   /  \
  /    \
ATAS.Strategies.Chart.ChartStrategy
    /\
   /  \
  /    \
ATAS.Strategies.Chart.SmaChartStrategy
```

**Пояснение к диаграмме:**

*   Диаграмма показывает иерархию наследования классов и интерфейсов в ATAS.
*   `ATAS.Indicators.IMarketTimeProvider` — это интерфейс, который предоставляется классам, находящимся ниже в иерархии, для доступа к функциональности управления временем.
*   `ATAS.Indicators.ExtendedIndicator` и `ATAS.Indicators.IOnlineDataProvider` наследуют (или реализуют) `IMarketTimeProvider`, что означает, что индикаторы, основанные на `ExtendedIndicator` или использующие `IOnlineDataProvider`, могут использовать функции `IMarketTimeProvider`.
*   `ATAS.Indicators.Indicator`, `ATAS.Strategies.Chart.ChartStrategy` и `ATAS.Strategies.Chart.SmaChartStrategy` также находятся в этой иерархии и, следовательно, могут использовать функции `IMarketTimeProvider` через своих предков.

### Публичные функции-члены

#### `SubscribeToTimer()`

```csharp
void SubscribeToTimer (TimeSpan period, Action callback)
```

**Описание:**

*   Регистрирует функцию обратного вызова (`callback`) для периодического вызова через заданный интервал времени (`period`).
*   Это позволяет индикатору выполнять определенные действия через регулярные промежутки времени, связанные с рыночным временем.

**Параметры:**

*   `TimeSpan period`:
    *   Тип данных: `TimeSpan` (представляет интервал времени).
    *   Описание: Период рыночного времени, через который будет вызываться функция обратного вызова.  Например, можно задать период в 1 секунду, 1 минуту и т.д.
*   `Action callback`:
    *   Тип данных: `Action` (делегат, представляющий метод, который не принимает параметров и не возвращает значение).
    *   Описание: Функция (метод), которая будет вызываться каждый раз, когда проходит заданный `period` рыночного времени. В этой функции можно разместить код, который должен выполняться периодически.

**Реализация:**

*   Реализовано в: `ATAS.Indicators.ExtendedIndicator`. Это значит, что индикаторы, унаследованные от `ExtendedIndicator`, уже имеют доступ к этой функции.

#### `UnsubscribeFromTimer()`

```csharp
void UnsubscribeFromTimer (TimeSpan period, Action callback)
```

**Описание:**

*   Отменяет регистрацию функции обратного вызова (`callback`) для периодических вызовов.
*   Используется для остановки периодического выполнения функции, зарегистрированной через `SubscribeToTimer`.

**Параметры:**

*   `TimeSpan period`:
    *   Тип данных: `TimeSpan`.
    *   Описание: Период времени, для которого была зарегистрирована подписка.  Должен соответствовать периоду, использованному при вызове `SubscribeToTimer`.
*   `Action callback`:
    *   Тип данных: `Action`.
    *   Описание: Функция обратного вызова, которую нужно отменить. Должна быть той же функцией, которая была передана в `SubscribeToTimer`.

**Реализация:**

*   Реализовано в: `ATAS.Indicators.ExtendedIndicator`.

### Свойства

#### `MarketTime`

```csharp
DateTime MarketTime { get; }
```

**Описание:**

*   Возвращает текущее рыночное время.
*   Рыночное время может отличаться от системного времени компьютера, так как оно синхронизируется с торговой платформой или сервером данных.

**Тип данных:**

*   `DateTime` (структура, представляющая момент времени).

**Доступ:**

*   `get`:  Свойство доступно только для чтения (можно только получить значение, но не изменить его).

**Реализация:**

*   Реализовано в: `ATAS.Indicators.ExtendedIndicator`.

#### `UtcTime`

```csharp
DateTime UtcTime { get; }
```

**Описание:**

*   Возвращает текущее время в формате UTC (Coordinated Universal Time, всемирное координированное время).
*   UTC — это стандартное время, которое не зависит от часовых поясов и используется для синхронизации времени в разных системах.

**Тип данных:**

*   `DateTime`.

**Доступ:**

*   `get`: Свойство доступно только для чтения.

**Реализация:**

*   Реализовано в: `ATAS.Indicators.ExtendedIndicator`.

### Файл документации

*   Документация для этого интерфейса была сгенерирована из файла: `IMarketTimeProvider.cs`.
    *   Это указывает на то, что исходный код интерфейса `IMarketTimeProvider` находится в файле `IMarketTimeProvider.cs`.

**Заключение:**

Интерфейс `ATAS.Indicators.IMarketTimeProvider` предоставляет индикаторам ATAS инструменты для работы с рыночным временем. С помощью функций `SubscribeToTimer` и `UnsubscribeFromTimer` можно настроить периодическое выполнение кода в индикаторе. Свойства `MarketTime` и `UtcTime` позволяют получить текущее рыночное время и время UTC, что может быть полезно для индикаторов, зависящих от времени.







### Интерфейс `ATAS.Indicators.IMouseLocationInfo`

**Описание:**

Интерфейс `ATAS.Indicators.IMouseLocationInfo` предназначен для предоставления информации о местоположении мыши в пределах графика.

**Свойства:**

*   **`PriceBelowMouse`**

    *   Тип: `decimal`
    *   Доступ: `[get]`
    *   Описание: Возвращает значение цены, находящейся под курсором мыши на графике.

*   **`BarBelowMouse`**

    *   Тип: `int`
    *   Доступ: `[get]`
    *   Описание: Возвращает номер бара, находящегося под курсором мыши на графике.

*   **`LastPosition`**

    *   Тип: `Point`
    *   Доступ: `[get]`
    *   Описание: Возвращает последнюю позицию курсора мыши в пределах графика.

*   **`IsMouseLeave`**

    *   Тип: `bool`
    *   Доступ: `[get, set]`
    *   Описание: Возвращает или устанавливает значение, указывающее, покинул ли мышь область графика.

*   **`IsMovingChartUsingMouse`**

    *   Тип: `bool`
    *   Доступ: `[get, set]`
    *   Описание: Возвращает или устанавливает значение, указывающее, перемещается ли график с помощью мыши.

**Подробное описание свойств:**

*   **`BarBelowMouse`**

    *   Тип: `int` `ATAS.Indicators.IMouseLocationInfo.BarBelowMouse`
    *   Доступ: `get`
    *   Описание: Возвращает номер бара, который находится непосредственно под курсором мыши на графике.

*   **`IsMouseLeave`**

    *   Тип: `bool` `ATAS.Indicators.IMouseLocationInfo.IsMouseLeave`
    *   Доступ: `get`, `set`
    *   Описание: Возвращает или устанавливает логическое значение, которое показывает, покинул ли курсор мыши область графика. `true`, если мышь покинула область графика, и `false` в противном случае.

*   **`IsMovingChartUsingMouse`**

    *   Тип: `bool` `ATAS.Indicators.IMouseLocationInfo.IsMovingChartUsingMouse`
    *   Доступ: `get`, `set`
    *   Описание: Возвращает или устанавливает логическое значение, которое показывает, перемещается ли график в данный момент пользователем с помощью мыши. `true`, если график перемещается мышью, и `false` в противном случае.

*   **`LastPosition`**

    *   Тип: `Point` `ATAS.Indicators.IMouseLocationInfo.LastPosition`
    *   Доступ: `get`
    *   Описание: Возвращает объект `Point`, представляющий последние известные координаты (X, Y) курсора мыши в пределах области графика.

*   **`PriceBelowMouse`**

    *   Тип: `decimal` `ATAS.Indicators.IMouseLocationInfo.PriceBelowMouse`
    *   Доступ: `get`
    *   Описание: Возвращает значение цены, соответствующее вертикальному положению курсора мыши на графике.

**Файл документации:**

*   `IIndicatorContainer.cs`








### Класс `ATAS.Indicators.Indicator`

**Описание:**

Базовый класс для пользовательских индикаторов.

**Диаграмма наследования для `ATAS.Indicators.Indicator`:**

```
INotifyPropertyChanged
    TrackedPropertyBase
        ChartObject
            IDisposable
            INotifyPanelPropertyChanged
                BaseIndicator
                    ICustomTypeDescriptor
                    IMarketTimeProvider
                        ExtendedIndicator
                            ATAS.Indicators.Indicator
                                ATAS.Strategies.Chart.ChartStrategy
                                    ATAS.Strategies.Chart.SmaChartStrategy
```

**Диаграмма взаимодействия для `ATAS.Indicators.Indicator`:**

*(Диаграмма взаимодействия идентична диаграмме наследования и опущена для краткости)*

**Открытые члены (Public Member Functions)**

*   `virtual void RefreshData()`
    *   Обновляет данные индикатора. Переопределите этот метод для обновления данных индикатора при необходимости.

**Открытые члены, унаследованные от `ATAS.Indicators.ExtendedIndicator`**

*(Список унаследованных членов опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.ExtendedIndicator`)*

**Открытые члены, унаследованные от `ATAS.Indicators.BaseIndicator`**

*(Список унаследованных членов опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.BaseIndicator`)*

**Открытые члены, унаследованные от `ATAS.Indicators.ChartObject`**

*   `void SubscribeToTimer(TimeSpan period, Action callback)`
    *   Регистрирует обратный вызов и периодически вызывает его с указанным периодом.
*   `void UnsubscribeFromTimer(TimeSpan period, Action callback)`
    *   Отменяет регистрацию обратного вызова из периодических вызовов.

**Защищенные члены (Protected Member Functions)**

*   `Indicator(bool useCandles = false)`
*   `Indicator(DataSeriesType seriesType)`
*   `bool IsNewSession(int bar)`
    *   Функция возвращает флаг новой сессии.
*   `bool IsNewWeek(int bar)`
    *   Функция возвращает флаг новой недели.
*   `bool IsNewMonth(int bar)`
    *   Функция возвращает флаг нового месяца.
*   `DrawingText AddText(string tag, string text, bool isAbovePrice, int bar, decimal price, Color textColor, Color outlineColor, Color fillColor, float fontSize, DrawingText.TextAlign align, bool autoSize = false)`
    *   Добавляет элемент рисования текста на график в указанной позиции.
*   `DrawingText AddText(string tag, string text, bool isAbovePrice, int bar, decimal price, Color textColor, Color fillColor, float fontSize, DrawingText.TextAlign align, bool autoSize = false)`
    *   Добавляет элемент рисования текста на график в указанной позиции. *(Дубликат предыдущего метода, возможно, с другими параметрами по умолчанию)*
*   `void DoActionInGuiThread(Action action)`
    *   Выполняет указанное действие в потоке GUI.
*   `IEnumerable<MarketDataArg> GetMarketDepthSnapshot()`
*   `DrawingText AddText(string tag, string text, bool isAbovePrice, int bar, int price, int yoffset, int xOffset, Color textcolor, Color outlinecolor, Color fillcolor, float fontSize, DrawingText.TextAlign align, bool autoSize = false)`
*   `override void OnApplyDefaultColors()`
    *   Вызывается для применения цветов по умолчанию к элементам рисования. Этот метод может быть переопределен в производных классах для настройки или установки значений цвета по умолчанию для элементов рисования. Когда вызывается этот метод, это возможность для производного класса применить свою собственную цветовую схему по умолчанию к элементам рисования, используемым в конкретной реализации.

**Защищенные члены, унаследованные от `ATAS.Indicators.ExtendedIndicator`**

*(Список унаследованных членов опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.ExtendedIndicator`)*

**Защищенные члены, унаследованные от `ATAS.Indicators.BaseIndicator`**

*(Список унаследованных членов опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.BaseIndicator`)*

**Защищенные члены, унаследованные от `ATAS.Indicators.ChartObject`**

*(Список унаследованных членов опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.ChartObject`)*

**Защищенные члены, унаследованные от `ATAS.Indicators.Filters.TrackedPropertyBase`**

*(Список унаследованных членов опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.Filters.TrackedPropertyBase`)*

**Свойства (Properties)**

*   `string? Category` `[get]`
*   `IInstrumentInfo? InstrumentInfo` `[get]`
    *   Возвращает информацию об инструменте, связанную с поставщиком данных.
*   `ITradingManager? TradingManager` `[get]`
    *   Возвращает менеджер торговли, связанный с поставщиком данных.
*   `IPlatformSettings? PlatformSettings` `[get]`
    *   Возвращает настройки платформы, связанные с поставщиком данных.
*   `IMarketDepthInfoProvider? MarketDepthInfo` `[get]`
    *   Возвращает провайдера информации о глубине рынка, связанного с поставщиком данных.
*   `IChart? ChartInfo` `[get]`
    *   Возвращает информацию о графике, связанную с поставщиком данных.
*   `ITradingStatisticsProvider? TradingStatisticsProvider` `[get]`
    *   Возвращает провайдера торговой статистики, связанного с поставщиком данных.
*   `Rectangle ChartArea` `[get]`
    *   Возвращает прямоугольник, представляющий область графика на экране.
*   `IMouseLocationInfo MouseLocationInfo` `[get]`
    *   Возвращает информацию о местоположении мыши на графике.
*   `int FirstVisibleBarNumber` `[get]`
    *   Возвращает номер бара первого видимого бара на графике.
*   `int LastVisibleBarNumber` `[get]`
    *   Возвращает номер бара последнего видимого бара на графике.
*   `int VisibleBarsCount` `[get]`
    *   Возвращает количество видимых баров на графике.
*   `decimal CumulativeDomAsks` `[get]`
*   `decimal CumulativeDomBids` `[get]`
*   `string? Instrument` `[get]`
    *   Название инструмента.
*   `decimal TickSize` `[get]`
    *   Размер тика.
*   `string DataPath` `[get]`
    *   Путь к рабочей папке программы.
*   `int ValueAreaPercent` `[get]`
*   `string? ChartType` `[get]`
    *   Тип графика.
*   `string? TimeFrame` `[get]`
    *   Таймфрейм.
*   `string StringFormat` `[get]`
    *   Формат цены.

**Свойства, унаследованные от `ATAS.Indicators.ExtendedIndicator`**

*(Список унаследованных свойств опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.ExtendedIndicator`)*

**Свойства, унаследованные от `ATAS.Indicators.BaseIndicator`**

*(Список унаследованных свойств опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.BaseIndicator`)*

**Свойства, унаследованные от `ATAS.Indicators.ChartObject`**

*(Список унаследованных свойств опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.ChartObject`)*

**Свойства, унаследованные от `ATAS.Indicators.IMarketTimeProvider`**

*(Список унаследованных свойств опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.IMarketTimeProvider`)*

**Дополнительные унаследованные члены**

*   **Статические защищенные члены, унаследованные от `ATAS.Indicators.BaseIndicator`**
    *(Список унаследованных членов опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.BaseIndicator`)*
*   **Защищенные атрибуты, унаследованные от `ATAS.Indicators.BaseIndicator`**
    *(Список унаследованных атрибутов опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.BaseIndicator`)*
*   **События, унаследованные от `ATAS.Indicators.BaseIndicator`**
    *(Список унаследованных событий опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.BaseIndicator`)*
*   **События, унаследованные от `ATAS.Indicators.Filters.TrackedPropertyBase`**
    *(Список унаследованных событий опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.Filters.TrackedPropertyBase`)*
*   **События, унаследованные от `ATAS.Indicators.INotifyPanelPropertyChanged`**
    *(Список унаследованных событий опущен для краткости, полная информация доступна в документации по `ATAS.Indicators.INotifyPanelPropertyChanged`)*

**Подробное описание**

Базовый класс для пользовательских индикаторов.

**Документация конструктора и деструктора**

*   `Indicator()` `[1/2]`
*   `Indicator()` `[2/2]`

**Документация функций-членов**

*   `DrawingText AddText()` `[1/3]`
    ```csharp
    DrawingText ATAS.Indicators.Indicator.AddText(
        string tag,
        string text,
        bool isAbovePrice,
        int bar,
        decimal price,
        Color textColor,
        Color fillColor,
        float fontSize,
        DrawingText::TextAlign align,
        bool autoSize = false
    )
    ```
    Добавляет элемент рисования текста на график в указанной позиции.

    **Параметры:**

    *   `tag` — Тег, связанный с текстовым элементом.
    *   `text` — Текстовое содержимое элемента.
    *   `isAbovePrice` — Указывает, следует ли размещать текст над ценой.
    *   `bar` — Номер бара, где будет размещен текст.
    *   `price` — Цена, на которой будет размещен текст.
    *   `textColor` — Цвет текста.
    *   `fillColor` — Цвет заливки текста.
    *   `fontSize` — Размер шрифта текста.
    *   `align` — Выравнивание текста.
    *   `autoSize` — Указывает, следует ли автоматически регулировать размер текста.

    **Возвращает:**

    *   Вновь добавленный элемент `DrawingText`.

*   `DrawingText AddText()` `[2/3]`
    ```csharp
    DrawingText ATAS.Indicators.Indicator.AddText(
        string tag,
        string text,
        bool isAbovePrice,
        int bar,
        decimal price,
        Color textColor,
        Color outlineColor,
        Color fillColor,
        float fontSize,
        DrawingText::TextAlign align,
        bool autoSize = false
    )
    ```
    Добавляет элемент рисования текста на график в указанной позиции.

    **Параметры:**

    *   `tag` — Тег, связанный с текстовым элементом.
    *   `text` — Текстовое содержимое элемента.
    *   `isAbovePrice` — Указывает, следует ли размещать текст над ценой.
    *   `bar` — Номер бара, где будет размещен текст.
    *   `price` — Цена, на которой будет размещен текст.
    *   `textColor` — Цвет текста.
    *   `outlineColor` — Цвет обводки текста.
    *   `fillColor` — Цвет заливки текста.
    *   `fontSize` — Размер шрифта текста.
    *   `align` — Выравнивание текста.
    *   `autoSize` — Указывает, следует ли автоматически регулировать размер текста.

    **Возвращает:**

    *   Вновь добавленный элемент `DrawingText`.

*   `DrawingText AddText()` `[3/3]`
    ```csharp
    DrawingText ATAS.Indicators.Indicator.AddText(
        string tag,
        string text,
        bool isAbovePrice,
        int bar,
        int price,
        int yoffset,
        int xOffset,
        Color textcolor,
        Color outlinecolor,
        Color fillcolor,
        float fontSize,
        DrawingText::TextAlign align,
        bool autoSize = false
    )
    ```
    Добавляет элемент рисования текста на график в указанной позиции.

    **Параметры:**

    *   `tag` — Тег, связанный с текстовым элементом.
    *   `text` — Текстовое содержимое элемента.
    *   `isAbovePrice` — Указывает, следует ли размещать текст над ценой.
    *   `bar` — Номер бара, где будет размещен текст.
    *   `price` — Цена, на которой будет размещен текст.
    *   `yoffset` — Смещение текста по вертикали в пикселях.
    *   `xOffset` — Смещение текста по горизонтали в пикселях.
    *   `textcolor` — Цвет текста.
    *   `outlinecolor` — Цвет обводки текста.
    *   `fillcolor` — Цвет заливки текста.
    *   `fontSize` — Размер шрифта текста.
    *   `align` — Выравнивание текста.
    *   `autoSize` — Указывает, следует ли автоматически регулировать размер текста.

*   `void DoActionInGuiThread()`
    ```csharp
    void ATAS.Indicators.Indicator.DoActionInGuiThread(Action action)
    ```
    Выполняет указанное действие в потоке GUI.

    **Параметры:**

    *   `action` — Действие для выполнения в потоке GUI.

*   `IEnumerable<MarketDataArg> GetMarketDepthSnapshot()`
    ```csharp
    IEnumerable<MarketDataArg> ATAS.Indicators.Indicator.GetMarketDepthSnapshot()
    ```

*   `bool IsNewMonth()`
    ```csharp
    bool ATAS.Indicators.Indicator.IsNewMonth(int bar)
    ```
    Функция возвращает флаг нового месяца.

    **Параметры:**

    *   `bar` — Номер бара

    **Возвращает:**

    *   `true`, если бар является началом нового месяца, `false` в противном случае.

*   `bool IsNewSession()`
    ```csharp
    bool ATAS.Indicators.Indicator.IsNewSession(int bar)
    ```
    Функция возвращает флаг новой сессии.

    **Параметры:**

    *   `bar` — Номер бара

    **Возвращает:**

    *   `true`, если бар является началом новой сессии, `false` в противном случае.

*   `bool IsNewWeek()`
    ```csharp
    bool ATAS.Indicators.Indicator.IsNewWeek(int bar)
    ```
    Функция возвращает флаг новой недели.

    **Параметры:**

    *   `bar` — Номер бара

    **Возвращает:**

    *   `true`, если бар является началом новой недели, `false` в противном случае.

*   `void OnApplyDefaultColors()`
    ```csharp
    override void ATAS.Indicators.Indicator.OnApplyDefaultColors()
    ```
    Вызывается для применения цветов по умолчанию к элементам рисования. Этот метод может быть переопределен в производных классах для настройки или установки значений цвета по умолчанию для элементов рисования. Когда вызывается этот метод, это возможность для производного класса применить свою собственную цветовую схему по умолчанию к элементам рисования, используемым в конкретной реализации.

    **Переопределено из:** `ATAS.Indicators.ExtendedIndicator.OnApplyDefaultColors()`

*   `virtual void RefreshData()`
    ```csharp
    virtual void ATAS.Indicators.Indicator.RefreshData()
    ```
    Обновляет данные индикатора. Переопределите этот метод для обновления данных индикатора при необходимости.

**Документация свойств**

*   `Category`
    ```csharp
    string? ATAS.Indicators.Indicator.Category
    ```
*   `ChartArea`
    ```csharp
    Rectangle ATAS.Indicators.Indicator.ChartArea
    ```
    Возвращает прямоугольник, представляющий область графика на экране.
*   `ChartInfo`
    ```csharp
    IChart? ATAS.Indicators.Indicator.ChartInfo
    ```
    Возвращает информацию о графике, связанную с поставщиком данных.
*   `ChartType`
    ```csharp
    string? ATAS.Indicators.Indicator.ChartType
    ```
    Тип графика.
*   `CumulativeDomAsks`
    ```csharp
    decimal ATAS.Indicators.Indicator.CumulativeDomAsks
    ```
*   `CumulativeDomBids`
    ```csharp
    decimal ATAS.Indicators.Indicator.CumulativeDomBids
    ```
*   `DataPath`
    ```csharp
    string ATAS.Indicators.Indicator.DataPath
    ```
    Путь к рабочей папке программы.
*   `FirstVisibleBarNumber`
    ```csharp
    int ATAS.Indicators.Indicator.FirstVisibleBarNumber
    ```
    Возвращает номер бара первого видимого бара на графике.
*   `Instrument`
    ```csharp
    string? ATAS.Indicators.Indicator.Instrument
    ```
    Название инструмента.
*   `InstrumentInfo`
    ```csharp
    IInstrumentInfo? ATAS.Indicators.Indicator.InstrumentInfo
    ```
    Возвращает информацию об инструменте, связанную с поставщиком данных.
*   `LastVisibleBarNumber`
    ```csharp
    int ATAS.Indicators.Indicator.LastVisibleBarNumber
    ```
    Возвращает номер бара последнего видимого бара на графике.
*   `MarketDepthInfo`
    ```csharp
    IMarketDepthInfoProvider? ATAS.Indicators.Indicator.MarketDepthInfo
    ```
    Возвращает провайдера информации о глубине рынка, связанного с поставщиком данных.
*   `MouseLocationInfo`
    ```csharp
    IMouseLocationInfo ATAS.Indicators.Indicator.MouseLocationInfo
    ```
    Возвращает информацию о местоположении мыши на графике.
*   `PlatformSettings`
    ```csharp
    IPlatformSettings? ATAS.Indicators.Indicator.PlatformSettings
    ```
    Возвращает настройки платформы, связанные с поставщиком данных.
*   `StringFormat`
    ```csharp
    string ATAS.Indicators.Indicator.StringFormat
    ```
    Формат цены.
*   `TickSize`
    ```csharp
    decimal ATAS.Indicators.Indicator.TickSize
    ```
    Размер тика.
*   `TimeFrame`
    ```csharp
    string? ATAS.Indicators.Indicator.TimeFrame
    ```
    Таймфрейм.
*   `TradingManager`
    ```csharp
    ITradingManager? ATAS.Indicators.Indicator.TradingManager
    ```
    Возвращает менеджер торговли, связанный с поставщиком данных.
*   `TradingStatisticsProvider`
    ```csharp
    ITradingStatisticsProvider? ATAS.Indicators.Indicator.TradingStatisticsProvider
    ```
    Возвращает провайдера торговой статистики, связанного с поставщиком данных.
*   `ValueAreaPercent`
    ```csharp
    int ATAS.Indicators.Indicator.ValueAreaPercent
    ```
*   `VisibleBarsCount`
    ```csharp
    int ATAS.Indicators.Indicator.VisibleBarsCount
    ```
    Возвращает количество видимых баров на графике.

**Файл документации:**

*   `Indicator.cs`






## Методичка по классу `ATAS.Indicators.IndicatorCandle` для создания индикаторов ATAS

### Ссылка на класс `ATAS.Indicators.IndicatorCandle`

Представляет индикаторную свечу.  [Подробнее...]

**Диаграмма наследования для `ATAS.Indicators.IndicatorCandle`:**

```
ISupportedPriceInfo
    |
    └── ATAS.Indicators.IndicatorCandle
```

**Диаграмма взаимодействия для `ATAS.Indicators.IndicatorCandle`:**

```
ISupportedPriceInfo
    |
    └── ATAS.Indicators.IndicatorCandle
```

### Публичные функции-члены

#### `IndicatorCandle`

```csharp
IndicatorCandle (
    ISupportedPriceInfo dataprovider,
    IIntCandle parentCandle,
    decimal tickSize
)
```

Конструктор для класса `IndicatorCandle`.

#### `GetAllPriceLevels`

```csharp
IEnumerable<PriceVolumeInfo> GetAllPriceLevels ()
```

Возвращает все доступные ценовые уровни со связанной информацией об объеме.

**Возвращает:**

Перечисляемую коллекцию объектов `PriceVolumeInfo`, представляющих все ценовые уровни.

#### `GetAllPriceLevels`

```csharp
IEnumerable<PriceVolumeInfo> GetAllPriceLevels (
    PriceVolumeInfo cacheItem
)
```

Возвращает все доступные ценовые уровни со связанной информацией об объеме и кэширует данные последнего элемента в указанном `cacheItem`.

**Параметры:**

* `cacheItem`: Объект `PriceVolumeInfo` для кэширования.

**Возвращает:**

Перечисляемую коллекцию объектов `PriceVolumeInfo`, представляющих все ценовые уровни.

#### `GetPriceVolumeInfo`

```csharp
PriceVolumeInfo GetPriceVolumeInfo (
    decimal price
)
```

Возвращает объект `PriceVolumeInfo`, связанный с указанной ценой.

**Параметры:**

* `price`: Цена, для которой необходимо получить объект `PriceVolumeInfo`.

**Возвращает:**

Объект `PriceVolumeInfo`, представляющий указанную цену.

#### `GetPriceVolumeInfo`

```csharp
PriceVolumeInfo GetPriceVolumeInfo (
    decimal price,
    PriceVolumeInfo cacheItem
)
```

Возвращает объект `PriceVolumeInfo`, связанный с указанной ценой.

**Параметры:**

* `price`: Цена, для которой необходимо получить объект `PriceVolumeInfo`.
* `cacheItem`: Объект `PriceVolumeInfo` для кэширования.

**Возвращает:**

Объект `PriceVolumeInfo`, представляющий указанную цену.

### Свойства

#### `Open`

```csharp
decimal Open { get; }
```

Цена открытия свечи.

#### `High`

```csharp
decimal High { get; }
```

Наивысшая цена в свече.

#### `Low`

```csharp
decimal Low { get; }
```

Самая низкая цена в свече.

#### `Close`

```csharp
decimal Close { get; }
```

Цена закрытия свечи.

#### `Volume`

```csharp
decimal Volume { get; }
```

Общее количество проторгованных лотов в свече.

#### `Bid`

```csharp
decimal Bid { get; }
```

Количество проторгованных лотов по лучшей цене Bid в свече.

#### `Ask`

```csharp
decimal Ask { get; }
```

Количество проторгованных лотов по лучшей цене Ask в свече.

#### `Betweens`

```csharp
decimal Betweens { get; }
```

Количество проторгованных лотов по цене между Bid и Ask в свече.

#### `Ticks`

```csharp
decimal Ticks { get; }
```

Количество изменений цены в свече.

#### `Delta`

```csharp
decimal Delta { get; }
```

Разница между количеством покупок и количеством продаж в свече.

#### `Time`

```csharp
DateTime Time { get; }
```

Время открытия свечи.

#### `LastTime`

```csharp
DateTime LastTime { get; }
```

Время, когда произошла последняя сделка в свече.

#### `MaxDelta`

```csharp
decimal MaxDelta { get; }
```

Максимальное значение дельты, которое было в течение периода свечи.

#### `MinDelta`

```csharp
decimal MinDelta { get; }
```

Минимальное значение дельты, которое было в течение периода свечи.

#### `MaxOI`

```csharp
decimal MaxOI { get; }
```

Максимальное значение открытых позиций, которое было в течение периода свечи.

#### `MinOI`

```csharp
decimal MinOI { get; }
```

Минимальное значение открытых позиций, которое было в течение периода свечи.

#### `OI`

```csharp
decimal OI { get; }
```

Количество открытых позиций в свече.

#### `MaxVolumePriceInfo`

```csharp
PriceVolumeInfo MaxVolumePriceInfo { get; }
```

Возвращает объект `PriceVolumeInfo` с максимальным объемом.

#### `MaxTickPriceInfo`

```csharp
PriceVolumeInfo MaxTickPriceInfo { get; }
```

Возвращает объект `PriceVolumeInfo` с максимальным количеством тиков.

#### `MaxAskPriceInfo`

```csharp
PriceVolumeInfo MaxAskPriceInfo { get; }
```

Возвращает объект `PriceVolumeInfo` с максимальной ценой Ask.

#### `MaxBidPriceInfo`

```csharp
PriceVolumeInfo MaxBidPriceInfo { get; }
```

Возвращает объект `PriceVolumeInfo` с максимальной ценой Bid.

#### `MaxTimePriceInfo`

```csharp
PriceVolumeInfo MaxTimePriceInfo { get; }
```

Возвращает объект `PriceVolumeInfo` с максимальным временем.

#### `MaxPositiveDeltaPriceInfo`

```csharp
PriceVolumeInfo MaxPositiveDeltaPriceInfo { get; }
```

Возвращает объект `PriceVolumeInfo` с максимальной положительной дельтой.

#### `MaxNegativeDeltaPriceInfo`

```csharp
PriceVolumeInfo MaxNegativeDeltaPriceInfo { get; }
```

Возвращает объект `PriceVolumeInfo` с максимальной отрицательной дельтой.

#### `ValueArea`

```csharp
ValueArea ValueArea { get; }
```

Возвращает объект `ValueArea`, который представляет область стоимости свечи.

**► Свойства, унаследованные от `ATAS.Indicators.ISupportedPriceInfo`**

### Подробное описание

Представляет индикаторную свечу.

### Документация конструктора и деструктора

#### `IndicatorCandle()`

```csharp
IndicatorCandle (
    ISupportedPriceInfo dataprovider,
    IIntCandle parentCandle,
    decimal tickSize
)
```

Конструктор для класса `IndicatorCandle`.

**Параметры:**

* `dataprovider`: Объект, предоставляющий информацию о цене.
* `parentCandle`: Объект, предоставляющий информацию о данных свечи.
* `tickSize`: Минимальный шаг цены.

### Документация функций-членов

#### `GetAllPriceLevels()` [1/2]

```csharp
IEnumerable<PriceVolumeInfo> GetAllPriceLevels()
```

Возвращает все доступные ценовые уровни со связанной информацией об объеме.

**Возвращает:**

Перечисляемую коллекцию объектов `PriceVolumeInfo`, представляющих все ценовые уровни.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `GetAllPriceLevels()` [2/2]

```csharp
IEnumerable<PriceVolumeInfo> GetAllPriceLevels(
    PriceVolumeInfo cacheItem
)
```

Возвращает все доступные ценовые уровни со связанной информацией об объеме и кэширует данные последнего элемента в указанном `cacheItem`.

**Параметры:**

* `cacheItem`: Объект `PriceVolumeInfo` для кэширования.

**Возвращает:**

Перечисляемую коллекцию объектов `PriceVolumeInfo`, представляющих все ценовые уровни.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `GetPriceVolumeInfo()` [1/2]

```csharp
PriceVolumeInfo GetPriceVolumeInfo(
    decimal price
)
```

Возвращает объект `PriceVolumeInfo`, связанный с указанной ценой.

**Параметры:**

* `price`: Цена, для которой необходимо получить объект `PriceVolumeInfo`.

**Возвращает:**

Объект `PriceVolumeInfo`, представляющий указанную цену.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `GetPriceVolumeInfo()` [2/2]

```csharp
PriceVolumeInfo GetPriceVolumeInfo(
    decimal price,
    PriceVolumeInfo cacheItem
)
```

Возвращает объект `PriceVolumeInfo`, связанный с указанной ценой.

**Параметры:**

* `price`: Цена, для которой необходимо получить объект `PriceVolumeInfo`.
* `cacheItem`: Объект `PriceVolumeInfo` для кэширования.

**Возвращает:**

Объект `PriceVolumeInfo`, представляющий указанную цену.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

### Документация свойств

#### `Ask`

```csharp
decimal Ask { get; }
```

Количество проторгованных лотов по лучшей цене предложения в свече.

#### `Betweens`

```csharp
decimal Betweens { get; }
```

Количество проторгованных лотов по цене между бидами и асками в свече.

#### `Bid`

```csharp
decimal Bid { get; }
```

Количество проторгованных лотов по лучшей цене спроса в свече.

#### `Close`

```csharp
decimal Close { get; }
```

Цена закрытия свечи.

#### `Delta`

```csharp
decimal Delta { get; }
```

Разница между количеством покупок и количеством продаж в свече.

#### `High`

```csharp
decimal High { get; }
```

Наивысшая цена в свече.

#### `LastTime`

```csharp
DateTime LastTime { get; }
```

Время, когда произошла последняя сделка в свече.

#### `Low`

```csharp
decimal Low { get; }
```

Самая низкая цена в свече.

#### `MaxAskPriceInfo`

```csharp
PriceVolumeInfo MaxAskPriceInfo { get; }
```

Возвращает объект PriceVolumeInfo с максимальной ценой предложения.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `MaxBidPriceInfo`

```csharp
PriceVolumeInfo MaxBidPriceInfo { get; }
```

Возвращает объект PriceVolumeInfo с максимальной ценой спроса.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `MaxDelta`

```csharp
decimal MaxDelta { get; }
```

Максимальное значение дельты, которое было в течение периода свечи.

#### `MaxNegativeDeltaPriceInfo`

```csharp
PriceVolumeInfo MaxNegativeDeltaPriceInfo { get; }
```

Возвращает объект PriceVolumeInfo с максимальной отрицательной дельтой.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `MaxOI`

```csharp
decimal MaxOI { get; }
```

Максимальное значение открытых позиций, которое было в течение периода свечи.

#### `MaxPositiveDeltaPriceInfo`

```csharp
PriceVolumeInfo MaxPositiveDeltaPriceInfo { get; }
```

Возвращает объект PriceVolumeInfo с максимальной положительной дельтой.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `MaxTickPriceInfo`

```csharp
PriceVolumeInfo MaxTickPriceInfo { get; }
```

Возвращает объект PriceVolumeInfo с максимальным количеством тиков.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `MaxTimePriceInfo`

```csharp
PriceVolumeInfo MaxTimePriceInfo { get; }
```

Возвращает объект PriceVolumeInfo с максимальным временем.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `MaxVolumePriceInfo`

```csharp
PriceVolumeInfo MaxVolumePriceInfo { get; }
```

Возвращает объект PriceVolumeInfo с максимальным объемом.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `MinDelta`

```csharp
decimal MinDelta { get; }
```

Минимальное значение дельты, которое было в течение периода свечи.

#### `MinOI`

```csharp
decimal MinOI { get; }
```

Минимальное значение открытых позиций, которое было в течение периода свечи.

#### `OI`

```csharp
decimal OI { get; }
```

Количество открытых позиций в свече.

#### `Open`

```csharp
decimal Open { get; }
```

Цена открытия свечи.

#### `Ticks`

```csharp
decimal Ticks { get; }
```

Количество изменений цены в свече.

#### `Time`

```csharp
DateTime Time { get; }
```

Время открытия свечи.

#### `ValueArea`

```csharp
ValueArea ValueArea { get; }
```

Возвращает объект ValueArea, который представляет область стоимости свечи.

**Реализует:** `ATAS.Indicators.ISupportedPriceInfo`.

#### `Volume`

```csharp
decimal Volume { get; }
```

Общий объем торгов в свече.

Документация для этого класса была сгенерирована из следующего файла:

* `IndicatorCandle.cs`







## Методичка по классу `ATAS.Indicators.IndicatorDataProvider` для создания индикаторов ATAS

### Описание класса `ATAS.Indicators.IndicatorDataProvider`

**Назначение:** Реализация интерфейса `IIndicatorDataProvider`, предоставляющего доступ к различным данным и сервисам, связанным с индикатором.

**Иерархия наследования:**

```
IIndicatorDataProvider
    └─ ATAS.Indicators.IndicatorDataProvider
```

**Диаграмма сотрудничества:**

```
IIndicatorDataProvider
    └─ ATAS.Indicators.IndicatorDataProvider
```

### Публичные функции-члены

#### Конструктор `IndicatorDataProvider`

```csharp
IndicatorDataProvider (
    ICandleCreator candleCreator,
    IOnlineDataProvider onlineDataProvider,
    IPlatformSettings platformSettings,
    IInstrumentInfo instrumentInfo,
    ITradingManager tradingManager,
    IChart chartInfo,
    ITradingStatisticsProvider tradingStatisticsProvider
)
```

**Описание:** Инициализирует новый экземпляр класса `IndicatorDataProvider`.

**Параметры:**

*   `candleCreator`:  Интерфейс `ICandleCreator`, создатель свечей, связанный с индикатором.
*   `onlineDataProvider`: Интерфейс `IOnlineDataProvider`, поставщик онлайн-данных, используемый индикатором.
*   `platformSettings`: Интерфейс `IPlatformSettings`, глобальные настройки платформы, используемые индикатором.
*   `instrumentInfo`: Интерфейс `IInstrumentInfo`, информация об инструменте, связанная с инструментом индикатора.
*   `tradingManager`: Интерфейс `ITradingManager`, менеджер торговли, используемый индикатором для управления торговыми задачами.
*   `chartInfo`: Интерфейс `IChart`, информация о графике, связанная с индикатором.
*   `tradingStatisticsProvider`: Интерфейс `ITradingStatisticsProvider`, поставщик торговой статистики, используемый индикатором для доступа к статистике, связанной с торговлей.

#### Функция `GetCustomStartTime`

```csharp
DateTime GetCustomStartTime (
    DateTime time,
    TimeSpan timeFrame
)
```

**Описание:** Возвращает пользовательское время начала свечи с указанным таймфреймом и текущим временем.

**Параметры:**

*   `time`:  Текущее время (`DateTime`).
*   `timeFrame`:  Таймфрейм (`TimeSpan`).

**Возвращает:**

*   `true`, если началась новая торговая сессия; иначе `false`.

#### Функция `IsNewSession`

```csharp
bool IsNewSession (
    DateTime prevTime,
    DateTime newTime
)
```

**Описание:** Проверяет, началась ли новая торговая сессия между указанным предыдущим временем и новым временем.

**Параметры:**

*   `prevTime`:  Предыдущее время (`DateTime`).
*   `newTime`:  Новое время (`DateTime`).

**Возвращает:**

*   `true`, если началась новая торговая сессия; иначе `false`.

#### Функция `IsNewWeek`

```csharp
bool IsNewWeek (
    DateTime prevTime,
    DateTime newTime
)
```

**Описание:** Проверяет, началась ли новая торговая неделя между указанным предыдущим временем и новым временем.

**Параметры:**

*   `prevTime`:  Предыдущее время (`DateTime`).
*   `newTime`:  Новое время (`DateTime`).

**Возвращает:**

*   `true`, если началась новая торговая неделя; иначе `false`.

#### Функция `IsNewMonth`

```csharp
bool IsNewMonth (
    DateTime prevTime,
    DateTime newTime
)
```

**Описание:** Проверяет, начался ли новый торговый месяц между указанным предыдущим временем и новым временем.

**Параметры:**

*   `prevTime`:  Предыдущее время (`DateTime`).
*   `newTime`:  Новое время (`DateTime`).

**Возвращает:**

*   `true`, если начался новый торговый месяц; иначе `false`.

#### Функция `AddAlert`

```csharp
void AddAlert (
    string soundFile,
    string instrument,
    string message,
    CrossColor background,
    CrossColor foreground
)
```

**Описание:** Добавляет оповещение с указанными деталями к индикатору.

**Параметры:**

*   `soundFile`:  Путь к звуковому файлу для воспроизведения при оповещении (`string`).
*   `instrument`:  Инструмент, связанный с оповещением (`string`).
*   `message`:  Сообщение оповещения (`string`).
*   `background`:  Цвет фона оповещения (`CrossColor`).
*   `foreground`:  Цвет текста оповещения (`CrossColor`).

#### Функция `DoActionInGuiThread`

```csharp
void DoActionInGuiThread (
    Action action
)
```

**Описание:** Выполняет указанное действие в потоке GUI (графического интерфейса пользователя).

**Параметры:**

*   `action`:  Действие для выполнения (`Action`).

#### Функция `ToString` (переопределенная)

```csharp
override string ToString()
```

**Описание:** Возвращает имя поставщика данных индикатора.

**Возвращает:**

*   Имя поставщика данных индикатора (`string`).

### Публичные атрибуты

#### Атрибут `OnNewGuiActionRequested`

```csharp
Action<Action> OnNewGuiActionRequested
```

**Описание:** Получает или устанавливает действие для запроса нового действия GUI.

### Статические публичные атрибуты

#### Атрибут `NewPanel`

```csharp
const string NewPanel = "NewPanel"
```

**Описание:** Представляет имя новой панели. Значение: `"NewPanel"`.

#### Атрибут `CandlesPanel`

```csharp
const string CandlesPanel = "Chart"
```

**Описание:** Представляет имя панели свечей на графике. Значение: `"Chart"`.

### Свойства

#### Свойство `Name`

```csharp
string Name { get; }
```

**Описание:** Получает или устанавливает имя поставщика данных индикатора.

#### Свойство `ChartInfo`

```csharp
IChart ChartInfo { get; }
```

**Описание:** Получает информацию о графике, связанную с индикатором.

#### Свойство `GlobalPlatformSettings`

```csharp
IPlatformSettings GlobalPlatformSettings { get; }
```

**Описание:** Получает или устанавливает глобальные настройки платформы, используемые индикатором.

#### Свойство `OnlineDataProvider`

```csharp
IOnlineDataProvider OnlineDataProvider { get; }
```

**Описание:** Получает или устанавливает поставщика онлайн-данных, используемого индикатором для получения данных в реальном времени.

#### Свойство `CandlesDataSeries`

```csharp
ObservableCollection<CandlePartSeries> CandlesDataSeries { get; }
```

**Описание:** Получает или устанавливает коллекцию серий частей свечей, используемых индикатором.

#### Свойство `Panels`

```csharp
ObservableCollection<string> Panels { get; }
```

**Описание:** Получает или устанавливает коллекцию панелей, связанных с индикатором.

#### Свойство `MarketDepthInfoProvider`

```csharp
MarketDepthInfoProvider MarketDepthInfoProvider { get; }
```

**Описание:** Получает или устанавливает поставщика информации о глубине рынка, используемого индикатором для доступа к данным глубины рынка.

#### Свойство `InstrumentInfo`

```csharp
IInstrumentInfo InstrumentInfo { get; set; }
```

**Описание:** Получает или устанавливает информацию об инструменте, связанную с инструментом индикатора.

#### Свойство `TradingManager`

```csharp
ITradingManager TradingManager { get; }
```

**Описание:** Получает менеджер торговли, используемый индикатором для управления торговыми задачами.

#### Свойство `TradingStatisticsProvider`

```csharp
ITradingStatisticsProvider TradingStatisticsProvider { get; }
```

**Описание:** Получает поставщика торговой статистики, используемого индикатором для доступа к статистике, связанной с торговлей.

### Подробное описание

**Описание:** Класс `IndicatorDataProvider` является реализацией интерфейса `IIndicatorDataProvider`. Он предоставляет доступ к различным сервисам и данным, необходимым для работы индикатора в платформе ATAS. Через этот класс индикатор может получать доступ к данным свечей, настройкам платформы, информации об инструменте, торговому менеджеру и другим важным компонентам платформы.

### Файл документации

*   `IndicatorDataProvider.cs`







## Методичка по классу `ATAS.Indicators.IndicatorSeries` для создания индикаторов ATAS

### Описание класса `ATAS.Indicators.IndicatorSeries`

```
ATAS.Indicators.IndicatorSeries Class Reference
```
**Аннотация:**  Это заголовок, указывающий на описание класса `IndicatorSeries` в пространстве имен `ATAS.Indicators`.  `Class Reference` означает, что это справочная документация по классу.

```
Represents a custom data series for an indicator, derived from BaseDataSeries<decimal>. More...
```
**Аннотация:** Класс `IndicatorSeries` представляет собой пользовательский ряд данных для индикатора. Он наследуется от `BaseDataSeries<decimal>`, что означает, что он работает с десятичными значениями и обладает функциональностью базового ряда данных.  `More...` указывает на наличие дополнительной информации, возможно, в полном описании класса.

### Диаграмма наследования

```
Inheritance diagram for ATAS.Indicators.IndicatorSeries:
BaseDataSeries< decimal >
    ↑
ATAS.Indicators.IndicatorSeries
[legend]
```
**Аннотация:**  Диаграмма наследования показывает, что класс `IndicatorSeries` (`ATAS.Indicators.IndicatorSeries`) является производным от класса `BaseDataSeries<decimal>`. Стрелка указывает направление наследования: `IndicatorSeries` "является" `BaseDataSeries<decimal>`. Это означает, что `IndicatorSeries` получает свойства и методы от `BaseDataSeries<decimal>` и может их расширять или изменять. `[legend]` указывает на наличие легенды к диаграмме, хотя в данном случае она не представлена.

### Диаграмма сотрудничества

```
Collaboration diagram for ATAS.Indicators.IndicatorSeries:
BaseDataSeries< decimal >
    ↑
ATAS.Indicators.IndicatorSeries
[legend]
```
**Аннотация:** Диаграмма сотрудничества в данном контексте дублирует диаграмму наследования. Она также показывает связь "является" между `BaseDataSeries<decimal>` и `IndicatorSeries`. В контексте сотрудничества это может подразумевать, что `IndicatorSeries` использует функциональность `BaseDataSeries<decimal>` для своей работы. `[legend]`  указывает на наличие легенды к диаграмме, хотя она не представлена.

### Публичные функции-члены

```
Public Member Functions
IndicatorSeries (Indicator indicator, int seriesId)
Constructor to create an IndicatorSeries object for the specified indicator and series ID.
```
**Аннотация:**  Раздел "Public Member Functions" описывает публичные методы класса `IndicatorSeries`.  В данном случае представлен конструктор класса: `IndicatorSeries (Indicator indicator, int seriesId)`. Конструктор используется для создания новых объектов класса `IndicatorSeries`. Он принимает два параметра:
*   `Indicator indicator`:  Объект типа `Indicator`, представляющий индикатор, к которому будет привязан этот ряд данных.
*   `int seriesId`: Целочисленный идентификатор ряда данных внутри индикатора.

Описание "Constructor to create an IndicatorSeries object..." поясняет назначение этого метода - создание экземпляра (объекта) класса `IndicatorSeries`.

```
► Public Member Functions inherited from ATAS.Indicators.BaseDataSeries< decimal >
```
**Аннотация:**  Эта строка указывает на то, что класс `IndicatorSeries` также наследует публичные функции-члены от своего базового класса `ATAS.Indicators.BaseDataSeries<decimal>`. Чтобы узнать, какие именно функции унаследованы, нужно обратиться к документации класса `BaseDataSeries<decimal>`. Символ `►`  обычно используется для разворачивания списка или перехода к деталям.

### Свойства

```
Properties
Indicator Indicator [get]
Gets the associated indicator.
```
**Аннотация:**  Раздел "Properties" описывает свойства класса `IndicatorSeries`.  Первое свойство - `Indicator Indicator [get]`.
*   `Indicator`:  Тип свойства - `Indicator`, вероятно, класс `ATAS.Indicators.Indicator`.
*   `Indicator`:  Имя свойства - `Indicator`.
*   `[get]`:  Указывает, что свойство доступно только для чтения (get accessor).  Можно получить значение свойства, но нельзя его изменить напрямую извне.

Описание "Gets the associated indicator." поясняет, что это свойство возвращает индикатор, с которым связан данный ряд данных `IndicatorSeries`.

```
int SeriesId [get]
Gets the ID of the series within the indicator.
```
**Аннотация:** Второе свойство - `int SeriesId [get]`.
*   `int`: Тип свойства - целое число.
*   `SeriesId`: Имя свойства - `SeriesId`.
*   `[get]`:  Свойство доступно только для чтения.

Описание "Gets the ID of the series within the indicator."  поясняет, что это свойство возвращает идентификатор ряда данных (`SeriesId`) внутри родительского индикатора.

```
override int Count [get]
Gets the count of data points in the series.
```
**Аннотация:** Третье свойство - `override int Count [get]`.
*   `override`: Ключевое слово `override` указывает, что это свойство переопределяет свойство с таким же именем из базового класса `BaseDataSeries<decimal>`.
*   `int`: Тип свойства - целое число.
*   `Count`: Имя свойства - `Count`.
*   `[get]`:  Свойство доступно только для чтения.

Описание "Gets the count of data points in the series." поясняет, что это свойство возвращает количество точек данных, содержащихся в ряде `IndicatorSeries`.

```
override decimal this[int index] [get, set]
Gets the data value at the specified index in the IndicatorSeries.
```
**Аннотация:** Четвертое свойство - `override decimal this[int index] [get, set]`.
*   `override`:  Переопределяет свойство из базового класса.
*   `decimal`: Тип свойства - десятичное число.
*   `this[int index]`:  Индексатор. Позволяет получать доступ к элементам ряда данных по индексу, как к массиву. `int index` - параметр индекса, целое число, указывающее на позицию элемента.
*   `[get, set]`:  Свойство доступно как для чтения (`get`), так и для записи (`set`).

Описание "Gets the data value at the specified index in the IndicatorSeries."  поясняет, что при использовании `get` индексатор возвращает значение данных по указанному индексу.

### Дополнительные унаследованные члены

```
Additional Inherited Members
► Protected Member Functions inherited from ATAS.Indicators.BaseDataSeries< decimal >
► Events inherited from ATAS.Indicators. BaseDataSeries< decimal >
```
**Аннотация:**  Раздел "Additional Inherited Members" указывает на другие типы членов, унаследованных от `BaseDataSeries<decimal>`.
*   `► Protected Member Functions inherited from ATAS.Indicators.BaseDataSeries< decimal >`: Указывает на наличие унаследованных защищенных функций-членов. Защищенные члены доступны внутри класса `IndicatorSeries`, в его производных классах (если бы они были) и в классах, находящихся в том же пакете (в зависимости от языка программирования, в C# - в пределах сборки).
*   `► Events inherited from ATAS.Indicators. BaseDataSeries< decimal >`: Указывает на наличие унаследованных событий. События - это механизмы, позволяющие классу уведомлять другие части программы о происходящих в нем действиях.

### Подробное описание

```
Detailed Description
Represents a custom data series for an indicator, derived from BaseDataSeries<decimal>.
```
**Аннотация:**  Раздел "Detailed Description" повторяет краткое описание класса, предоставляя более развернутое пояснение, если бы оно было в оригинальной документации (в данном случае, описание идентично краткому).

### Документация конструктора и деструктора

```
Constructor & Destructor Documentation
IndicatorSeries()
ATAS.Indicators.IndicatorSeries.IndicatorSeries ( Indicator indicator,
                                                int
                                                seriesId
                                                )
Constructor to create an IndicatorSeries object for the specified indicator and series ID.
```
**Аннотация:**  Раздел "Constructor & Destructor Documentation" посвящен документации конструктора класса `IndicatorSeries`.  Здесь более подробно расписана сигнатура конструктора:

```csharp
ATAS.Indicators.IndicatorSeries.IndicatorSeries(
    Indicator indicator, // Параметр indicator типа Indicator
    int seriesId       // Параметр seriesId типа int
)
```

Описание "Constructor to create an IndicatorSeries object for the specified indicator and series ID." повторяет назначение конструктора.

#### Параметры конструктора

```
Parameters
indicator The associated indicator.
seriesId The ID of the series within the indicator.
```
**Аннотация:**  Подраздел "Parameters"  описывает параметры конструктора:
*   `indicator`: "The associated indicator." -  Параметр `indicator` представляет собой индикатор, с которым будет связан создаваемый ряд данных.
*   `seriesId`: "The ID of the series within the indicator." - Параметр `seriesId` представляет собой идентификатор создаваемого ряда данных внутри индикатора.

#### Исключения конструктора

```
Exceptions
ArgumentNullException Thrown if the 'indicator' parameter is null. An IndicatorSeries must be
                        associated with a valid indicator.
IndexOutOfRangeException Thrown if the specified 'seriesld' is not a valid index in the parent
                        indicator's DataSeries collection.
```
**Аннотация:** Подраздел "Exceptions" описывает исключения, которые может сгенерировать конструктор:
*   `ArgumentNullException`: "Thrown if the 'indicator' parameter is null. An IndicatorSeries must be associated with a valid indicator." -  Исключение `ArgumentNullException` возникает, если параметр `indicator` равен `null` (пустая ссылка). Это происходит, если при создании `IndicatorSeries` не был передан корректный объект индикатора.
*   `IndexOutOfRangeException`: "Thrown if the specified 'seriesld' is not a valid index in the parent indicator's DataSeries collection." - Исключение `IndexOutOfRangeException` возникает, если `seriesId` не является допустимым индексом в коллекции рядов данных родительского индикатора.  Это может произойти, если `seriesId` выходит за пределы допустимого диапазона индексов, который поддерживает родительский индикатор.

### Документация свойств

#### Свойство `Count`

```
Property Documentation
Count
override int ATAS. Indicators. IndicatorSeries.Count
get
Gets the count of data points in the series.
```
**Аннотация:** Раздел "Property Documentation"  детально описывает свойства класса.  Первое свойство - `Count`.

```csharp
override int ATAS.Indicators.IndicatorSeries.Count { get; }
```
*   `override int ATAS.Indicators.IndicatorSeries.Count`: Полное имя свойства, включая пространство имен и класс.
*   `{ get; }`:  Указывает, что свойство доступно только для чтения (get accessor).

Описание "Gets the count of data points in the series." повторяет назначение свойства.

#### Свойство `Indicator`

```
Indicator
Indicator ATAS. Indicators. IndicatorSeries. Indicator
get
Gets the associated indicator.
```
**Аннотация:** Второе свойство - `Indicator`.

```csharp
Indicator ATAS.Indicators.IndicatorSeries.Indicator { get; }
```
*   `Indicator ATAS.Indicators.IndicatorSeries.Indicator`: Полное имя свойства и его тип.
*   `{ get; }`: Свойство доступно только для чтения.

Описание "Gets the associated indicator." повторяет назначение свойства.

#### Свойство `SeriesId`

```
SeriesId
int ATAS. Indicators. IndicatorSeries. SeriesId
Gets the ID of the series within the indicator.
```
**Аннотация:** Третье свойство - `SeriesId`.

```csharp
int ATAS.Indicators.IndicatorSeries.SeriesId { get; }
```
*   `int ATAS.Indicators.IndicatorSeries.SeriesId`: Полное имя свойства и его тип.
*   `{ get; }`: Свойство доступно только для чтения.

Описание "Gets the ID of the series within the indicator." повторяет назначение свойства.

#### Индексатор `this[int index]`

```
this[int index]
override decimal ATAS.Indicators.IndicatorSeries.this[int index]
get     set
Gets the data value at the specified index in the IndicatorSeries.
```
**Аннотация:** Четвертое свойство - индексатор `this[int index]`.

```csharp
override decimal ATAS.Indicators.IndicatorSeries.this[int index] { get; set; }
```
*   `override decimal ATAS.Indicators.IndicatorSeries.this[int index]`: Полное имя индексатора и тип возвращаемого значения (`decimal`).
*   `{ get; set; }`: Индексатор доступен как для чтения (`get`), так и для записи (`set`).

Описание "Gets the data value at the specified index in the IndicatorSeries."  поясняет назначение индексатора при чтении.

##### Параметры индексатора

```
Parameters
index The index of the data value to retrieve.
```
**Аннотация:** Подраздел "Parameters" описывает параметр индексатора:
*   `index`: "The index of the data value to retrieve." - Параметр `index` представляет собой индекс элемента данных, который нужно получить.

##### Возвращаемое значение индексатора

```
Returns
The data value at the specified index.
```
**Аннотация:** Подраздел "Returns" описывает возвращаемое значение индексатора:
*   "The data value at the specified index." - Индексатор возвращает значение данных по указанному индексу.

##### Дополнительная информация об индексаторе

```
IndicatorSeries does not support modifying data values. It is read-only.
```
**Аннотация:**  Это утверждение **неверно**, так как сигнатура индексатора `[get, set]` указывает на возможность записи (set).  Вероятно, в документации ошибка или это устаревшая информация.  Нужно проверить актуальную документацию или код, чтобы убедиться, поддерживает ли `IndicatorSeries` изменение значений данных через индексатор.  *Прим. для пользователя:  Внимательно проверяйте документацию и, если есть возможность, код, так как в документации могут быть неточности.*

##### Исключения индексатора

```
Exceptions
IndexOutOfRangeException Thrown if the specified index is outside the valid range of the data
                        series.
```
**Аннотация:** Подраздел "Exceptions" описывает исключения, которые может сгенерировать индексатор при чтении или записи:
*   `IndexOutOfRangeException`: "Thrown if the specified index is outside the valid range of the data series." - Исключение `IndexOutOfRangeException` возникает, если указанный индекс `index` находится вне допустимого диапазона индексов для данного ряда данных `IndicatorSeries`.

### Файл документации

```
The documentation for this class was generated from the following file:
• IndicatorSeries.cs
```
**Аннотация:**  Последняя строка указывает на то, что документация для класса `IndicatorSeries` была сгенерирована из файла исходного кода `IndicatorSeries.cs`.  Это полезно для разработчиков, так как указывает на местонахождение кода класса в проекте.







## Методичка по интерфейсу `ATAS.Indicators.INotifyPanelPropertyChanged` для создания индикаторов ATAS

**Название интерфейса:** `ATAS.Indicators.INotifyPanelPropertyChanged`

**Описание:** Интерфейс `ATAS.Indicators.INotifyPanelPropertyChanged` уведомляет клиентов об изменении значения свойства панели индикатора.

**Наследование:**

Интерфейс `ATAS.Indicators.INotifyPanelPropertyChanged` наследуется от следующих интерфейсов и классов:

* `ATAS.Indicators.IDataSeries<decimal>`
* `ATAS.Indicators.BaseIndicator`
* `ATAS.Indicators.IDataSeries<T>`
* `ATAS.Indicators.BaseDataSeries<T>`
* `ATAS.Indicators.ExtendedIndicator`

_Аннотация: Это означает, что индикаторы, реализующие `INotifyPanelPropertyChanged`, также должны реализовывать функциональность, предоставляемую этими базовыми типами. В частности, они должны быть `IDataSeries<decimal>`, `BaseIndicator`, `IDataSeries<T>`, `BaseDataSeries<T>` и `ExtendedIndicator`._

**События:**

### `PanelPropertyChanged`

* **Тип обработчика события:** `PropertyChangedEventHandler`
* **Описание события:**  Событие `PanelPropertyChanged` возникает, когда изменяется значение свойства панели индикатора.

_Аннотация: Это событие является ключевым в интерфейсе. Индикатор должен вызывать это событие, чтобы уведомить платформу ATAS об изменении свойства панели. Другие части платформы, которые "слушают" это событие, могут реагировать на изменение свойства._

**Подробное описание:**

Интерфейс `ATAS.Indicators.INotifyPanelPropertyChanged` предназначен для уведомления клиентов об изменении значения свойства панели индикатора.  Это позволяет другим частям платформы ATAS динамически реагировать на изменения свойств индикатора, отображаемых на панели управления.

_Аннотация:  Этот интерфейс обеспечивает механизм обратной связи от индикатора к платформе, когда свойство, отображаемое пользователю на панели индикатора, меняется. Например, если пользователь изменяет параметр индикатора через панель, индикатор может уведомить об этом изменении, и платформа может обновить отображение или выполнить другие действия._

**Документация по событию:**

### `PanelPropertyChanged`

* **Тип обработчика события:** `PropertyChangedEventHandler ATAS.Indicators.INotifyPanelPropertyChanged.PanelPropertyChanged`
* **Описание события:** Возникает, когда значение свойства панели индикатора изменяется.

_Аннотация:  Здесь еще раз подчеркивается, что событие `PanelPropertyChanged` связано с изменением значений свойств панели индикатора и использует делегат `PropertyChangedEventHandler` для обработки этого события._

**Файл документации интерфейса:**

* `INotifyPanelPropertyChanged.cs`

_Аннотация:  Этот файл содержит исходный код определения интерфейса `INotifyPanelPropertyChanged`._







## Методичка по классу `ATAS.Indicators.InstrumentInfo` для создания индикаторов ATAS

### Описание класса
Класс `ATAS.Indicators.InstrumentInfo` является реализацией интерфейса `IInstrumentInfo` и представляет информацию об инструменте.

### Диаграмма наследования
```
IInstrumentInfo
  └─ ATAS.Indicators.InstrumentInfo
```

### Диаграмма взаимодействия
```
IInstrumentInfo
  └─ ATAS.Indicators.InstrumentInfo
```

### Публичные члены класса

#### Конструктор

`InstrumentInfo(string instrument, string exchange, decimal tickSize, int timeZone)`

* **Описание:** Конструктор для создания объекта `InstrumentInfo` с указанными параметрами.

* **Параметры:**
    * `instrument`: `string` - Название инструмента.
    * `exchange`: `string` - Название биржи, на которой торгуется инструмент.
    * `tickSize`: `decimal` - Размер тика инструмента, минимальное изменение цены.
    * `timeZone`: `int` - Временная зона инструмента.

#### Свойства

* `Instrument`: `string` [только чтение]
    * **Описание:** Возвращает название инструмента.

* `Exchange`: `string` [только чтение]
    * **Описание:** Возвращает название биржи, на которой торгуется инструмент.

* `TickSize`: `decimal` [только чтение]
    * **Описание:** Возвращает размер тика инструмента, минимальное изменение цены.

* `TimeZone`: `int` [чтение/запись]
    * **Описание:** Возвращает или устанавливает временную зону инструмента.

### Детальное описание

Класс `ATAS.Indicators.InstrumentInfo` реализует интерфейс `IInstrumentInfo`, представляя информацию об инструменте.

### Документация конструктора

#### `InstrumentInfo()`

`ATAS.Indicators.InstrumentInfo.InstrumentInfo(string instrument, string exchange, decimal tickSize, int timeZone)`

* **Описание:** Конструктор для создания объекта `InstrumentInfo` с указанными параметрами.

* **Параметры:**
    * `instrument`: `string` - Название инструмента.
    * `exchange`: `string` - Название биржи, на которой торгуется инструмент.
    * `tickSize`: `decimal` - Размер тика инструмента (минимальное движение цены).
    * `timeZone`: `int` - Временная зона, связанная с инструментом.

### Документация свойств

#### `Exchange`

`string ATAS.Indicators.InstrumentInfo.Exchange` [только чтение]

* **Описание:** Возвращает название биржи, на которой торгуется инструмент.
* **Реализует:** `ATAS.Indicators.IInstrumentInfo`.

#### `Instrument`

`string ATAS.Indicators.InstrumentInfo.Instrument` [только чтение]

* **Описание:** Возвращает название инструмента.
* **Реализует:** `ATAS.Indicators.IInstrumentInfo`.

#### `TickSize`

`decimal ATAS.Indicators.InstrumentInfo.TickSize` [только чтение]

* **Описание:** Возвращает размер тика инструмента, минимальное изменение цены.
* **Реализует:** `ATAS.Indicators.IInstrumentInfo`.

#### `TimeZone`

`int ATAS.Indicators.InstrumentInfo.TimeZone` [чтение/запись]

* **Описание:** Возвращает временную зону инструмента.
* **Реализует:** `ATAS.Indicators.IInstrumentInfo`.

### Файл документации

Документация для этого класса была сгенерирована из файла: `InstrumentInfo.cs`.







## Методичка по интерфейсу `ATAS.Indicators.IOnlineDataProvider` для создания индикаторов ATAS

**Введение:**

Данная методичка предназначена для описания интерфейса `ATAS.Indicators.IOnlineDataProvider`. Этот интерфейс предоставляет доступ к данным рынка в реальном времени и используется для разработки индикаторов в платформе ATAS. Методичка содержит подробное описание всех членов интерфейса, включая функции, свойства и события, для облегчения создания продвинутых индикаторов.

**1. Интерфейс `ATAS.Indicators.IOnlineDataProvider`**

Интерфейс `ATAS.Indicators.IOnlineDataProvider` является интерфейсом для поставщика онлайн-данных, который обеспечивает доступ к рыночным данным в реальном времени.

**Диаграмма наследования:**

```
IKnowFixedProfiles → ATAS.Indicators.IOnlineDataProvider
IMarketByOrdersDataProvider → ATAS.Indicators.IOnlineDataProvider
IMarketTimeProvider → ATAS.Indicators.IOnlineDataProvider
```

*Аннотация: `IKnowFixedProfiles`, `IMarketByOrdersDataProvider` и `IMarketTimeProvider` - это другие интерфейсы, от которых наследуется `IOnlineDataProvider`. Это означает, что `IOnlineDataProvider` включает в себя функциональность этих интерфейсов.*

**Диаграмма взаимодействия:**

```
IKnowFixedProfiles → ATAS.Indicators.IOnlineDataProvider
IMarketByOrdersDataProvider → ATAS.Indicators.IOnlineDataProvider
IMarketTimeProvider → ATAS.Indicators.IOnlineDataProvider
```

*Аннотация: Эта диаграмма показывает взаимодействие `IOnlineDataProvider` с другими интерфейсами, подчеркивая его роль как центрального интерфейса для доступа к различным типам рыночных данных.*

**2. Публичные функции-члены (Public Member Functions)**

Интерфейс `ATAS.Indicators.IOnlineDataProvider` предоставляет следующие публичные функции-члены:

**2.1. `Subscribe()`**

```csharp
void Subscribe ()
```

Подписывает поставщика данных на начало получения обновлений данных в реальном времени.

*Аннотация: Функция `Subscribe()` активирует получение данных в реальном времени от поставщика данных. Индикатор должен вызвать эту функцию, чтобы начать получать рыночные данные.*

**2.2. `Unsubscribe()`**

```csharp
void Unsubscribe ()
```

Отписывает поставщика данных от прекращения получения обновлений данных в реальном времени.

*Аннотация: Функция `Unsubscribe()` останавливает получение данных в реальном времени. Вызов этой функции может быть полезен для экономии ресурсов, когда индикатор временно не нуждается в данных.*

**2.3. `RedrawChart(RedrawArg redrawArg)`**

```csharp
void RedrawChart (RedrawArg redrawArg)
```

Перерисовывает график на основе предоставленного аргумента перерисовки.

*Параметры:*

*   `redrawArg` (`RedrawArg`): Аргумент перерисовки, определяющий область для перерисовки.

*Аннотация: Функция `RedrawChart()` используется для принудительной перерисовки графика индикатора. Параметр `redrawArg` позволяет указать, какую область графика нужно перерисовать, что может быть полезно для оптимизации производительности.*

**2.4. `RequestCumulativeTrades(CumulativeTradesRequest request)`**

```csharp
void RequestCumulativeTrades (CumulativeTradesRequest request)
```

Запрашивает исторические кумулятивные торговые данные на основе предоставленного запроса.

*Параметры:*

*   `request` (`CumulativeTradesRequest`): Запрос кумулятивных сделок, определяющий диапазон данных.

*Аннотация: Функция `RequestCumulativeTrades()` позволяет запросить исторические данные о кумулятивных сделках. Это может быть полезно для индикаторов, которым нужны исторические данные для анализа.*

**2.5. `GetCustomStartTime(DateTime time, TimeSpan timeFrame)`**

```csharp
DateTime GetCustomStartTime (DateTime time, TimeSpan timeFrame)
```

Получает пользовательское время начала свечи с указанным таймфреймом и текущим временем.

*Параметры:*

*   `time` (`DateTime`): Текущее время.
*   `timeFrame` (`TimeSpan`): Таймфрейм свечи.

*Возвращает:*

*   `DateTime`: Пользовательское время начала свечи.

*Аннотация: Функция `GetCustomStartTime()` помогает определить время начала свечи для нестандартных таймфреймов. Это важно для правильного расчета индикаторов, зависящих от времени свечей.*

**2.6. `IsNewSession(DateTime prevTime, DateTime newTime)`**

```csharp
bool IsNewSession (DateTime prevTime, DateTime newTime)
```

Проверяет, указывает ли предоставленное новое время на начало новой торговой сессии по сравнению с предыдущим временем.

*Параметры:*

*   `prevTime` (`DateTime`): Предыдущее время.
*   `newTime` (`DateTime`): Новое время.

*Возвращает:*

*   `bool`: `true`, если `newTime` указывает на новую сессию, иначе `false`.

*Аннотация: Функция `IsNewSession()` позволяет определить начало новой торговой сессии. Индикаторы могут использовать это для сброса или инициализации данных в начале каждой сессии.*

**2.7. `IsNewWeek(DateTime prevTime, DateTime newTime)`**

```csharp
bool IsNewWeek (DateTime prevTime, DateTime newTime)
```

Проверяет, указывает ли предоставленное новое время на начало новой торговой недели по сравнению с предыдущим временем.

*Параметры:*

*   `prevTime` (`DateTime`): Предыдущее время.
*   `newTime` (`DateTime`): Новое время.

*Возвращает:*

*   `bool`: `true`, если `newTime` указывает на начало новой недели, иначе `false`.

*Аннотация: Функция `IsNewWeek()` аналогична `IsNewSession()`, но проверяет начало новой торговой недели. Полезно для индикаторов, анализирующих данные на недельной основе.*

**2.8. `IsNewMonth(DateTime prevTime, DateTime newTime)`**

```csharp
bool IsNewMonth (DateTime prevTime, DateTime newTime)
```

Проверяет, указывает ли предоставленное новое время на начало нового торгового месяца по сравнению с предыдущим временем.

*Параметры:*

*   `prevTime` (`DateTime`): Предыдущее время.
*   `newTime` (`DateTime`): Новое время.

*Возвращает:*

*   `bool`: `true`, если `newTime` указывает на начало нового месяца, иначе `false`.

*Аннотация: Функция `IsNewMonth()` проверяет начало нового торгового месяца. Полезна для индикаторов, анализирующих данные на месячной основе.*

**2.9. `AddAlert(string soundFile, string instrument, string message, CrossColor background, CrossColor foreground)` [1/2]**

```csharp
void AddAlert (string soundFile, string instrument, string message, CrossColor background, CrossColor foreground)
```

Добавляет оповещение с указанными параметрами.

*Параметры:*

*   `soundFile` (`string`): Путь к звуковому файлу для воспроизведения в качестве части оповещения.
*   `instrument` (`string`): Имя инструмента, связанного с оповещением.
*   `message` (`string`): Текст сообщения оповещения.
*   `background` (`CrossColor`): Цвет фона для оповещения.
*   `foreground` (`CrossColor`): Цвет текста для оповещения.

*Аннотация: Первая версия функции `AddAlert()` позволяет добавлять оповещения с основными параметрами, такими как звук, инструмент, сообщение и цвета.*

**2.10. `AddAlert(string soundFile, string instrument, string message, CrossColor background, CrossColor foreground, DateTime time)` [2/2]**

```csharp
void AddAlert (string soundFile, string instrument, string message, CrossColor background, CrossColor foreground, DateTime time)
```

Добавляет оповещение с указанными подробностями к индикатору.

*Параметры:*

*   `soundFile` (`string`): Путь к звуковому файлу для воспроизведения в качестве части оповещения.
*   `instrument` (`string`): Имя инструмента, связанного с оповещением.
*   `message` (`string`): Текст сообщения оповещения.
*   `background` (`CrossColor`): Цвет фона для оповещения.
*   `foreground` (`CrossColor`): Цвет текста для оповещения.
*   `time` (`DateTime`): Время, когда было вызвано оповещение.

*Аннотация: Вторая версия `AddAlert()` добавляет параметр `time`, позволяющий указать время возникновения оповещения. Это может быть полезно для отображения времени оповещения на графике или в журнале.*

**2.11. `GetMarketDepthSnapshot()`**

```csharp
IEnumerable<MarketDataArg> GetMarketDepthSnapshot ()
```

Получает снимок текущих данных глубины рынка.

*Возвращает:*

*   `IEnumerable<MarketDataArg>`: Перечисление `MarketDataArg`, представляющее текущие данные глубины рынка.

*Аннотация: Функция `GetMarketDepthSnapshot()` возвращает моментальный снимок текущей глубины рынка (DOM). Индикатор может использовать эти данные для анализа текущего состояния рынка.*

**2.12. `GetMarketDepthSnapshotsAsync(MarketDepthSnapshotRequest request, CancellationToken cancellation = default)`**

```csharp
Task<IEnumerable<MarketDepthSnapshot>> GetMarketDepthSnapshotsAsync (MarketDepthSnapshotRequest request, CancellationToken cancellation = default)
```

Асинхронно извлекает снимки глубины рынка с сервера.

*Параметры:*

*   `request` (`MarketDepthSnapshotRequest`): Объект, описывающий параметры запрошенных снимков.
*   `cancellation` (`CancellationToken`): Токен отмены.

*Возвращает:*

*   `Task<IEnumerable<MarketDepthSnapshot>>`: Асинхронная задача, возвращающая перечисление снимков глубины рынка.

*Аннотация: Функция `GetMarketDepthSnapshotsAsync()` асинхронно запрашивает исторические снимки глубины рынка. Асинхронность позволяет избежать блокировки основного потока индикатора при запросе данных.*

**2.13. `GetTradesCache(TimeSpan period)`**

```csharp
ITradesCache GetTradesCache (TimeSpan period)
```

Возвращает кэш сделок.

*Параметры:*

*   `period` (`TimeSpan`): Указывает период времени, для которого сделки хранятся в `ITradesCache`.

*Возвращает:*

*   `ITradesCache`: Объект `ITradesCache`, представляющий кэш сделок.

*Аннотация: Функция `GetTradesCache()` предоставляет доступ к кэшу сделок за определенный период времени. Кэш сделок содержит информацию о последних сделках и может использоваться для анализа торговой активности.*

**2.14. `GetMarketByOrdersCache(TimeSpan period)`**

```csharp
IMarketByOrdersCache GetMarketByOrdersCache (TimeSpan period)
```

Возвращает кэш данных Market-by-Order.

*Параметры:*

*   `period` (`TimeSpan`): Указывает период времени, для которого обновления Market-by-Order хранятся в `IMarketByOrdersCache`.

*Возвращает:*

*   `IMarketByOrdersCache`: Объект `IMarketByOrdersCache`, представляющий кэш данных Market-by-Order.

*Аннотация: Функция `GetMarketByOrdersCache()` предоставляет доступ к кэшу данных Market-by-Order (MBO) за определенный период. MBO данные содержат информацию о лимитных ордерах в книге ордеров.*

**2.15. `GetMarketByOrdersWithTradesCache(TimeSpan period)`**

```csharp
IMarketByOrdersWithTradesCache GetMarketByOrdersWithTradesCache (TimeSpan period)
```

Возвращает кэш данных Market-by-Order и сделок.

*Параметры:*

*   `period` (`TimeSpan`): Указывает период времени, для которого сделки хранятся в `IMarketByOrdersWithTradesCache`.

*Возвращает:*

*   `IMarketByOrdersWithTradesCache`: Объект `IMarketByOrdersWithTradesCache`, представляющий кэш.

*Аннотация: Функция `GetMarketByOrdersWithTradesCache()` предоставляет доступ к кэшу, который объединяет данные Market-by-Order и данные о сделках за определенный период. Это позволяет анализировать взаимосвязь между лимитными ордерами и исполненными сделками.*

**2.16. `SubscribeMarketByOrdersData()`**

```csharp
Task SubscribeMarketByOrdersData ()
```

Подписывается на данные Market-by-Order.

*Возвращает:*

*   `Task`: Асинхронная задача, представляющая операцию подписки.

*Аннотация: Функция `SubscribeMarketByOrdersData()` подписывает индикатор на получение обновлений данных Market-by-Order в реальном времени. Необходимо вызвать эту функцию, чтобы получать данные MBO.*

**2.17. `RedrawChart(RedrawArg redrawArg)` (повторение)**

```csharp
void RedrawChart (RedrawArg redrawArg)
```

*Уже описана в пункте 2.3.*

**2.18. `RequestCumulativeTrades(CumulativeTradesRequest request)` (повторение)**

```csharp
void RequestCumulativeTrades (CumulativeTradesRequest request)
```

*Уже описана в пункте 2.4.*

**2.19. `Subscribe()` (повторение)**

```csharp
void Subscribe ()
```

*Уже описана в пункте 2.1.*

**2.20. `SubscribeMarketByOrdersData()` (повторение)**

```csharp
Task SubscribeMarketByOrdersData ()
```

*Уже описана в пункте 2.16.*

**2.21. `Unsubscribe()` (повторение)**

```csharp
void Unsubscribe ()
```

*Уже описана в пункте 2.2.*

**3. Свойства (Properties)**

Интерфейс `ATAS.Indicators.IOnlineDataProvider` предоставляет следующие свойства:

**3.1. `CumulativeDomAsks`**

```csharp
decimal CumulativeDomAsks [get]
```

Возвращает кумулятивный объем асков (Depth of Market).

*Аннотация: Свойство `CumulativeDomAsks` предоставляет доступ к кумулятивному объему асков в глубине рынка. Аски - это ордера на продажу.*

**3.2. `CumulativeDomBids`**

```csharp
decimal CumulativeDomBids [get]
```

Возвращает кумулятивный объем бидов (Depth of Market).

*Аннотация: Свойство `CumulativeDomBids` предоставляет доступ к кумулятивному объему бидов в глубине рынка. Биды - это ордера на покупку.*

**4. События (Events)**

Интерфейс `ATAS.Indicators.IOnlineDataProvider` предоставляет следующие события:

**4.1. `NewTrades`**

```csharp
Action<IEnumerable<MarketDataArg>> NewTrades
```

Событие, которое возникает при получении новых сделок.

*Аннотация: Событие `NewTrades` возникает каждый раз, когда поступают новые сделки. Индикатор может подписаться на это событие, чтобы реагировать на новые сделки в реальном времени.*

**4.2. `BestBidAskChanged`**

```csharp
Action<MarketDataArg> BestBidAskChanged
```

Событие, которое возникает при изменении лучших цен бид и аск.

*Параметры события:*

*   `marketDataArg` (`MarketDataArg`): `MarketDataArg`, представляющий новые лучшие цены бид и аск.

*Аннотация: Событие `BestBidAskChanged` возникает при изменении лучшей цены покупки (бид) или продажи (аск). Индикатор может использовать это событие для отслеживания изменений в текущих рыночных ценах.*

**4.3. `MarketDepthsChanged`**

```csharp
Action<IEnumerable<MarketDataArg>> MarketDepthsChanged
```

Событие, которое возникает при изменении глубины рынка.

*Параметры события:*

*   `marketDataArgs` (`IEnumerable<MarketDataArg>`): Коллекция `MarketDataArg`, представляющая обновленные глубины рынка.

*Аннотация: Событие `MarketDepthsChanged` возникает при любых изменениях в глубине рынка (DOM). Индикатор может использовать это событие для отслеживания динамики книги ордеров.*

**4.4. `NewCumulativeTrade`**

```csharp
Action<CumulativeTrade> NewCumulativeTrade
```

Событие, которое возникает при получении новой кумулятивной сделки.

*Параметры события:*

*   `trade` (`CumulativeTrade`): Новый объект кумулятивной сделки.

*Аннотация: Событие `NewCumulativeTrade` возникает при поступлении данных о новой кумулятивной сделке. Кумулятивные сделки агрегируют информацию о сделках за определенный период.*

**4.5. `UpdateCumulativeTrade`**

```csharp
Action<CumulativeTrade> UpdateCumulativeTrade
```

Событие, которое возникает при обновлении кумулятивной сделки.

*Параметры события:*

*   `trade` (`CumulativeTrade`): Обновленный объект кумулятивной сделки.

*Аннотация: Событие `UpdateCumulativeTrade` возникает, когда существующая кумулятивная сделка обновляется. Это может произойти, если в течение периода агрегации поступают новые сделки.*

**4.6. `HistoricalCumulativeTrades`**

```csharp
Action<CumulativeTradesRequest, IEnumerable<CumulativeTrade>> HistoricalCumulativeTrades
```

Событие, которое возникает при получении исторических кумулятивных торговых данных.

*Параметры события:*

*   `request` (`CumulativeTradesRequest`): Объект запроса исторических кумулятивных сделок.
*   `trades` (`IEnumerable<CumulativeTrade>`): Коллекция исторических кумулятивных торговых данных.

*Аннотация: Событие `HistoricalCumulativeTrades` возникает в ответ на запрос исторических кумулятивных сделок (через функцию `RequestCumulativeTrades`). Оно предоставляет запрошенные исторические данные.*

**4.7. `MarketByOrdersChanged`**

```csharp
Action<IEnumerable<MarketByOrder>> MarketByOrdersChanged
```

Событие, которое возникает при изменении данных Market-by-Order в реальном времени.

*Параметры события:*

*   `values` (`IEnumerable<MarketByOrder>`): Перечисление `MarketByOrder`, представляющее измененные данные Market-by-Order.

*Аннотация: Событие `MarketByOrdersChanged` возникает при любых изменениях в данных Market-by-Order (MBO) в реальном времени. Индикатор может использовать это для отслеживания изменений в книге лимитных ордеров.*

**5. Подробное описание (Detailed Description)**

Интерфейс `ATAS.Indicators.IOnlineDataProvider` предназначен для поставщиков онлайн-данных, обеспечивающих доступ к рыночным данным в реальном времени. Он предоставляет набор функций, свойств и событий для получения и обработки различных типов рыночных данных, включая сделки, глубину рынка и данные Market-by-Order.

**6. Документация функций-членов (Member Function Documentation)**

*   Подробное описание каждой функции-члена было представлено в разделе 2.

**7. Документация свойств (Property Documentation)**

*   Подробное описание каждого свойства было представлено в разделе 3.

**8. Документация событий (Event Documentation)**

*   Подробное описание каждого события было представлено в разделе 4.

**9. Файл документации:**

Документация для данного интерфейса была сгенерирована из следующего файла:

*   `IOnlineDataProvider.cs`

**Заключение:**

Интерфейс `ATAS.Indicators.IOnlineDataProvider` является ключевым компонентом для разработки индикаторов ATAS, работающих с рыночными данными в реальном времени. Понимание функций, свойств и событий этого интерфейса необходимо для создания эффективных и продвинутых индикаторов. Данная методичка предоставляет подробное описание каждого члена интерфейса, что должно помочь в процессе разработки.






## Методичка по интерфейсу `ATAS.Indicators.IPlatformSettings` для создания индикаторов ATAS

### Интерфейс `ATAS.Indicators.IPlatformSettings`

**Описание:** Интерфейс для доступа к настройкам платформы.

**Свойства:**

| Тип    | Свойство                | Описание                                                                                                                              |
| :----- | :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| `int`  | `ValueAreaPercent`      | Возвращает значение, представляющее процент value area, который указан в глобальных настройках платформы.                                |
| `int`  | `ValueAreaStep`         | Возвращает размер шага value area на каждой итерации.                                                                                   |
| `int`  | `ValueAreaUpdateDelayMs`| Возвращает частоту обновления value area для снижения нагрузки на CPU при частом обновлении свечей. В миллисекундах. 0 означает отсутствие кэша и каждое обновление будет форсировать перерасчет ValueArea. |
| `string` | `DataPath`            | Возвращает путь к папке, где платформа хранит все настройки. Может использоваться индикатором для сохранения служебной информации.        |
| `Version`| `Version`             | Возвращает текущую версию платформы.                                                                                                   |

### Подробное описание свойств

#### `DataPath`

```csharp
string ATAS.Indicators.IPlatformSettings.DataPath { get; }
```

**Описание:** Возвращает путь к папке, где платформа хранит все настройки. Может использоваться индикатором для сохранения служебной информации.

#### `ValueAreaPercent`

```csharp
int ATAS.Indicators.IPlatformSettings.ValueAreaPercent { get; }
```

**Описание:** Возвращает значение, представляющее процент value area, который указан в глобальных настройках платформы.

#### `ValueAreaStep`

```csharp
int ATAS.Indicators.IPlatformSettings.ValueAreaStep { get; }
```

**Описание:** Возвращает размер шага value area на каждой итерации.

#### `ValueAreaUpdateDelayMs`

```csharp
int ATAS.Indicators.IPlatformSettings.ValueAreaUpdateDelayMs { get; }
```

**Описание:** Возвращает частоту обновления value area для снижения нагрузки на CPU при частом обновлении свечей. В миллисекундах. 0 означает отсутствие кэша и каждое обновление будет форсировать перерасчет ValueArea.

#### `Version`

```csharp
Version ATAS.Indicators.IPlatformSettings.Version { get; }
```

**Описание:** Возвращает текущую версию платформы.

**Файл документации:** `IPlatformSettings.cs`







## Методические указания по интерфейсу `ATAS.Indicators.IPropertiesEditor` для создания индикаторов ATAS

**Введение:**

Данный документ представляет собой методическое руководство по интерфейсу `ATAS.Indicators.IPropertiesEditor`, предназначенному для настройки свойств индикаторов в платформе ATAS. Интерфейс предоставляет набор функций для управления видимостью и состоянием развернутости категорий и свойств в редакторе свойств индикатора.

**Описание интерфейса `ATAS.Indicators.IPropertiesEditor`:**

Интерфейс `ATAS.Indicators.IPropertiesEditor` содержит следующие публичные функции-члены:

**1. `BeginInit()`**

* **Тип:** `void`
* **Описание:**  Начало блока инициализации свойств индикатора.  Вызов этой функции сигнализирует системе о начале процесса изменения свойств индикатора, что может оптимизировать обновление интерфейса редактора свойств.

**2. `EndInit()`**

* **Тип:** `Task`
* **Описание:** Завершение блока инициализации свойств индикатора. Вызов этой функции завершает процесс изменения свойств, начатый функцией `BeginInit()`, и может инициировать обновление интерфейса редактора свойств для отображения внесенных изменений. Функция возвращает `Task`, что может указывать на асинхронный характер операции.

**3. `GetIsExpandedCategory(string? categoryName)`**

* **Тип:** `bool?` (Nullable Boolean - допускает значения `true`, `false` или `null`)
* **Параметры:**
    * `categoryName` (string?):  Название категории свойств, для которой необходимо получить состояние развернутости. Допускается значение `null`.
* **Описание:** Возвращает текущее состояние развернутости указанной категории свойств в редакторе свойств индикатора.  Возвращаемое значение типа `bool?` может быть:
    * `true`:  Категория развернута.
    * `false`: Категория свернута.
    * `null`:  Состояние категории не определено или категория не найдена.

**4. `SetIsExpandedCategory(string? categoryName, bool isExpanded)`**

* **Тип:** `void`
* **Параметры:**
    * `categoryName` (string?): Название категории свойств, состояние развернутости которой необходимо установить. Допускается значение `null`.
    * `isExpanded` (bool):  Новое состояние развернутости категории.
        * `true`:  Развернуть категорию.
        * `false`: Свернуть категорию.
* **Описание:** Устанавливает состояние развернутости указанной категории свойств в редакторе свойств индикатора.

**5. `GetIsExpandedProperty(string? propertyName)`**

* **Тип:** `bool?` (Nullable Boolean)
* **Параметры:**
    * `propertyName` (string?): Название свойства, для которого необходимо получить состояние развернутости. Допускается значение `null`.
* **Описание:** Возвращает текущее состояние развернутости указанного свойства в редакторе свойств индикатора.  Возвращаемое значение типа `bool?` может быть:
    * `true`: Свойство развернуто (если свойство является составным и может быть развернуто).
    * `false`: Свойство свернуто или не может быть развернуто.
    * `null`: Состояние свойства не определено или свойство не найдено.

**6. `SetIsExpandedProperty(string? propertyName, bool isExpanded)`**

* **Тип:** `void`
* **Параметры:**
    * `propertyName` (string?): Название свойства, состояние развернутости которого необходимо установить. Допускается значение `null`.
    * `isExpanded` (bool): Новое состояние развернутости свойства.
        * `true`: Развернуть свойство (если свойство является составным и может быть развернуто).
        * `false`: Свернуть свойство.
* **Описание:** Устанавливает состояние развернутости указанного свойства в редакторе свойств индикатора.

**Файл реализации:**

Интерфейс `ATAS.Indicators.IPropertiesEditor` реализован в файле `IPropertiesEditor.cs`.

**Заключение:**

Интерфейс `ATAS.Indicators.IPropertiesEditor` предоставляет инструменты для программного управления отображением категорий и свойств в редакторе свойств индикатора ATAS.  Использование функций данного интерфейса позволяет разработчикам индикаторов настраивать пользовательский интерфейс свойств, делая его более удобным и интуитивно понятным для конечных пользователей.





## Методичка для создания индикаторов ATAS на основе интерфейса `ATAS.Indicators.IPropertiesEditorOwner`

### 1. Описание интерфейса `ATAS.Indicators.IPropertiesEditorOwner`

Интерфейс `ATAS.Indicators.IPropertiesEditorOwner` предназначен для объектов, которые владеют редактором свойств. В контексте ATAS Indicators, это означает, что объекты, реализующие данный интерфейс, могут предоставлять пользовательский интерфейс для настройки своих свойств через редактор свойств платформы ATAS.

### 2. Свойство `PropertiesEditor`

Интерфейс `ATAS.Indicators.IPropertiesEditorOwner` содержит единственное свойство:

```csharp
IPropertiesEditor? PropertiesEditor { get; set; }
```

#### 2.1. Тип свойства: `IPropertiesEditor?`

*   `IPropertiesEditor?` -  указывает на тип свойства. `IPropertiesEditor` является интерфейсом, представляющим редактор свойств. Знак вопроса `?` после `IPropertiesEditor` означает, что свойство является **nullable** (допускает значение `null`). Это значит, что свойство `PropertiesEditor` может либо содержать ссылку на объект, реализующий интерфейс `IPropertiesEditor`, либо быть равным `null`, если редактор свойств не назначен или отсутствует.

#### 2.2. Accessors: `[get, set]`

*   `[get, set]` -  определяют **accessors** свойства.
    *   `get` -  указывает на наличие **getter** (метода получения значения свойства). Это позволяет получить текущий экземпляр `IPropertiesEditor`, связанный с объектом.
    *   `set` -  указывает на наличие **setter** (метода установки значения свойства). Это позволяет установить или изменить экземпляр `IPropertiesEditor`, связанный с объектом.

### 3. Назначение свойства `PropertiesEditor`

Свойство `PropertiesEditor` позволяет ATAS платформе взаимодействовать с редактором свойств индикатора или другого объекта, реализующего интерфейс `IPropertiesEditorOwner`.  Через это свойство платформа может:

*   **Получить доступ к редактору свойств:**  Используя `get` accessor, платформа может получить текущий экземпляр `IPropertiesEditor` для отображения пользовательского интерфейса редактирования свойств.
*   **Установить редактор свойств:** Используя `set` accessor, платформа может назначить или изменить редактор свойств для объекта.

### 4. Расположение в файле

Документация для данного интерфейса была сгенерирована из файла:

*   `IPropertiesEditorOwner.cs`

Это указывает на то, что исходный код интерфейса `IPropertiesEditorOwner` находится в файле `IPropertiesEditorOwner.cs`.

### 5. Практическое применение для индикаторов ATAS

При создании индикаторов для платформы ATAS, реализация интерфейса `ATAS.Indicators.IPropertiesEditorOwner` позволяет индикатору интегрироваться с системой свойств ATAS. Это дает возможность пользователям настраивать параметры индикатора через стандартный интерфейс свойств платформы.

Для реализации этого интерфейса в индикаторе необходимо:

1.  Указать, что индикатор реализует интерфейс `IPropertiesEditorOwner`.
2.  Реализовать свойство `PropertiesEditor` с `get` и `set` accessors.
3.  В `get` accessor вернуть экземпляр редактора свойств, который управляет свойствами индикатора.
4.  В `set` accessor предусмотреть логику для установки или изменения редактора свойств, если это необходимо.

**Пример (псевдокод):**

```csharp
public class MyCustomIndicator : Indicator, IPropertiesEditorOwner
{
    private IPropertiesEditor? _propertiesEditor; // Поле для хранения редактора свойств

    public IPropertiesEditor? PropertiesEditor
    {
        get { return _propertiesEditor; } // Возвращаем текущий редактор свойств
        set { _propertiesEditor = value; } // Устанавливаем новый редактор свойств
    }

    // ... остальной код индикатора ...

    public override void OnInitialize()
    {
        // ... инициализация индикатора ...

        // Создание и назначение редактора свойств (пример)
        _propertiesEditor = new MyIndicatorPropertiesEditor(this); // MyIndicatorPropertiesEditor - класс, реализующий IPropertiesEditor для свойств индикатора
    }
}
```

**Примечание:**  Для создания полноценного редактора свойств потребуется дополнительная реализация интерфейса `IPropertiesEditor` и логики управления свойствами индикатора. Данная методичка фокусируется на понимании интерфейса `IPropertiesEditorOwner` и его свойства `PropertiesEditor`.







## Методичка по интерфейсу ATAS.Indicators.ISupportedPriceInfo

**Описание интерфейса:**

Интерфейс `ATAS.Indicators.ISupportedPriceInfo` представляет собой интерфейс для поддержки информации о цене.  Он предназначен для индикаторов, которым требуется доступ к данным о цене и объеме на уровне каждого ценового уровня внутри свечи.

**Диаграмма наследования:**

```
ATAS.Indicators.ISupportedPriceInfo
    ↑
ATAS.Indicators.IndicatorCandle
```

*   **Аннотация:** `ATAS.Indicators.ISupportedPriceInfo` является интерфейсом, который определяет контракт для доступа к ценовой информации. `ATAS.Indicators.IndicatorCandle` является классом индикатора, который реализует этот интерфейс, то есть предоставляет конкретную реализацию методов и свойств, описанных в интерфейсе.

**Публичные функции-члены (методы):**

1.  **`IEnumerable<PriceVolumeInfo> GetAllPriceLevels()`**

    *   **Описание:** Возвращает все доступные ценовые уровни со связанной информацией об объеме.
    *   **Возвращаемое значение:** `IEnumerable<PriceVolumeInfo>`.  Представляет собой перечисляемую коллекцию объектов `PriceVolumeInfo`, каждый из которых содержит информацию о ценовом уровне и связанном с ним объеме.
    *   **Реализация:** Реализован в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Этот метод позволяет получить полный список всех ценовых уровней внутри свечи. `IEnumerable<PriceVolumeInfo>` означает, что метод возвращает коллекцию, которую можно перебрать, и каждый элемент в этой коллекции будет объектом типа `PriceVolumeInfo`.

2.  **`IEnumerable<PriceVolumeInfo> GetAllPriceLevels(PriceVolumeInfo cacheItem)`**

    *   **Описание:** Возвращает все доступные ценовые уровни со связанной информацией об объеме и кэширует данные последнего элемента в указанном `cacheItem`.
    *   **Параметры:**
        *   `cacheItem`: `PriceVolumeInfo`. Объект `PriceVolumeInfo` для кэширования.
    *   **Возвращаемое значение:** `IEnumerable<PriceVolumeInfo>`.  Перечисляемая коллекция объектов `PriceVolumeInfo`, представляющая все ценовые уровни.
    *   **Реализация:** Реализован в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:**  Эта перегрузка метода `GetAllPriceLevels` делает то же самое, что и первая, но также позволяет кэшировать данные для оптимизации производительности, особенно при повторных вызовах. Параметр `cacheItem` используется для хранения и быстрого доступа к данным.

3.  **`PriceVolumeInfo GetPriceVolumeInfo(decimal price)`**

    *   **Описание:** Возвращает объект `PriceVolumeInfo`, связанный с указанной ценой.
    *   **Параметры:**
        *   `price`: `decimal`. Цена, для которой нужно получить объект `PriceVolumeInfo`.
    *   **Возвращаемое значение:** `PriceVolumeInfo`. Объект `PriceVolumeInfo`, представляющий указанную цену.
    *   **Реализация:** Реализован в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Этот метод позволяет получить информацию о конкретном ценовом уровне, зная цену этого уровня. Он возвращает объект `PriceVolumeInfo`, содержащий данные для этой цены.

4.  **`PriceVolumeInfo GetPriceVolumeInfo(decimal price, PriceVolumeInfo cacheItem)`**

    *   **Описание:** Возвращает объект `PriceVolumeInfo`, связанный с указанной ценой.
    *   **Параметры:**
        *   `price`: `decimal`. Цена, для которой нужно получить объект `PriceVolumeInfo`.
        *   `cacheItem`: `PriceVolumeInfo`. Объект `PriceVolumeInfo` для кэширования.
    *   **Возвращаемое значение:** `PriceVolumeInfo`. Объект `PriceVolumeInfo`, представляющий указанную цену.
    *   **Реализация:** Реализован в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Эта перегрузка метода `GetPriceVolumeInfo` также позволяет получить информацию о конкретном ценовом уровне по цене, но с возможностью кэширования для улучшения производительности.

**Свойства (Properties):**

1.  **`PriceVolumeInfo MaxAskPriceInfo { get; }`**

    *   **Описание:** Возвращает объект `PriceVolumeInfo` с максимальной ценой Ask.
    *   **Тип свойства:** `PriceVolumeInfo`
    *   **Реализация:** Реализовано в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Это свойство позволяет получить информацию о ценовом уровне с самой высокой ценой предложения (Ask) внутри свечи.

2.  **`PriceVolumeInfo MaxBidPriceInfo { get; }`**

    *   **Описание:** Возвращает объект `PriceVolumeInfo` с максимальной ценой Bid.
    *   **Тип свойства:** `PriceVolumeInfo`
    *   **Реализация:** Реализовано в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Это свойство позволяет получить информацию о ценовом уровне с самой высокой ценой спроса (Bid) внутри свечи.

3.  **`PriceVolumeInfo MaxTimePriceInfo { get; }`**

    *   **Описание:** Возвращает объект `PriceVolumeInfo` с максимальным временем.
    *   **Тип свойства:** `PriceVolumeInfo`
    *   **Реализация:** Реализовано в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Это свойство, вероятно, связано с ценовым уровнем, который был активен самое продолжительное время в течение свечи (требует уточнения, так как описание не совсем ясное).

4.  **`PriceVolumeInfo MaxPositiveDeltaPriceInfo { get; }`**

    *   **Описание:** Возвращает объект `PriceVolumeInfo` с максимальной положительной дельтой.
    *   **Тип свойства:** `PriceVolumeInfo`
    *   **Реализация:** Реализовано в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Это свойство позволяет получить информацию о ценовом уровне с наибольшей положительной дельтой объема (покупками минус продажи).

5.  **`PriceVolumeInfo MaxNegativeDeltaPriceInfo { get; }`**

    *   **Описание:** Возвращает объект `PriceVolumeInfo` с максимальной отрицательной дельтой.
    *   **Тип свойства:** `PriceVolumeInfo`
    *   **Реализация:** Реализовано в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Это свойство позволяет получить информацию о ценовом уровне с наибольшей отрицательной дельтой объема (продажами минус покупки).

6.  **`PriceVolumeInfo MaxTickPriceInfo { get; }`**

    *   **Описание:** Возвращает объект `PriceVolumeInfo` с максимальным количеством тиков.
    *   **Тип свойства:** `PriceVolumeInfo`
    *   **Реализация:** Реализовано в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Это свойство позволяет получить информацию о ценовом уровне с наибольшим количеством тиков (изменений цены) внутри свечи.

7.  **`PriceVolumeInfo MaxVolumePriceInfo { get; }`**

    *   **Описание:** Возвращает объект `PriceVolumeInfo` с максимальным объемом.
    *   **Тип свойства:** `PriceVolumeInfo`
    *   **Реализация:** Реализовано в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Это свойство позволяет получить информацию о ценовом уровне с наибольшим объемом торгов внутри свечи.

8.  **`ValueArea ValueArea { get; }`**

    *   **Описание:** Возвращает объект `ValueArea`, который представляет область стоимости свечи.
    *   **Тип свойства:** `ValueArea`
    *   **Реализация:** Реализовано в `ATAS.Indicators.IndicatorCandle`.

    *   **Аннотация:** Это свойство возвращает объект `ValueArea`, который, вероятно, содержит информацию об области цен, где было проторговано наибольшее количество объема, то есть область стоимости для данной свечи.

**Подробное описание интерфейса:**

Интерфейс `ATAS.Indicators.ISupportedPriceInfo` представляет собой интерфейс для поддержки информации о цене. Он обеспечивает доступ к различным характеристикам ценовых уровней внутри свечи, таким как объем, дельта, цены Ask и Bid, время и область стоимости. Индикаторы, реализующие этот интерфейс, могут использовать эти данные для анализа рыночной активности на уровне цен.

**Документация для этого интерфейса была сгенерирована из следующего файла:**

*   `PriceVolumeInfo.cs`

**Заключение:**

Этот интерфейс предоставляет мощный набор инструментов для доступа к детальной информации о ценовых уровнях внутри свечи в платформе ATAS. Индикаторы, использующие этот интерфейс, могут выполнять глубокий анализ внутрибарной активности и создавать продвинутые торговые сигналы на основе объема и дельты на каждом ценовом уровне.







### Методичка по интерфейсу `ATAS.Indicators.ITimeMarketDataCache<out T>`

#### Описание интерфейса
`ATAS.Indicators.ITimeMarketDataCache<out T>` - интерфейс-шаблон, представляющий собой кэш, который хранит последние элементы, основанные на заданном количестве времени.

#### Свойства

1.  **`CachedItems`** `[get]`
    *   Тип: `IEnumerable<T>`
    *   Описание: Возвращает последние элементы за период времени, указанный свойством `CachePeriod`.

2.  **`CachePeriod`** `[get]`
    *   Тип: `TimeSpan`
    *   Описание: Возвращает период времени, за который возвращаются элементы свойством `CachedItems`.

#### Подробное описание
Кэш, который хранит последние элементы, основанные на заданном количестве времени.

#### Детальная документация свойств

**`CachedItems`**
*   Тип: `IEnumerable<T>`
*   Пространство имен: `ATAS.Indicators`
*   Интерфейс: `ITimeMarketDataCache<out T>`
*   Описание: Возвращает последние элементы за период времени, указанный свойством `CachePeriod`.

**`CachePeriod`**
*   Тип: `TimeSpan`
*   Пространство имен: `ATAS.Indicators`
*   Интерфейс: `ITimeMarketDataCache<out T>`
*   Описание: Возвращает период времени, за который возвращаются элементы свойством `CachedItems`.

#### Файл, в котором сгенерирована документация:
*   `IOnlineDataProvider.cs`








### Методичка по интерфейсу `ATAS.Indicators.ITradesCache`

**Название интерфейса:** `ATAS.Indicators.ITradesCache Interface Reference`

*Аннотация:*  Название интерфейса, указывающее на то, что это справочная информация по интерфейсу `ITradesCache`, расположенному в пространстве имен `ATAS.Indicators`.

**Описание интерфейса:** "Interface for manager that provides access to trades cache. More..."

*Аннотация:* Описание интерфейса. Указывает, что данный интерфейс предназначен для менеджера, который обеспечивает доступ к кешу сделок.  Ссылка "More..." может указывать на наличие дополнительной информации, но в текущем контексте она не раскрыта.

**Диаграмма наследования:** "Inheritance diagram for ATAS.Indicators.ITradesCache:"

```
ITimeMarketDataCache
< MarketDataArg >
    ↑
ATAS.Indicators.ITradesCache
[legend]
```

*Аннотация:* Схема наследования классов. `ATAS.Indicators.ITradesCache` наследуется от `ITimeMarketDataCache<MarketDataArg>`.  Стрелка "↑" показывает направление наследования (от производного класса к базовому). `[legend]` указывает на наличие легенды к схеме, которая в данном изображении не детализирована.

**Диаграмма взаимодействия:** "Collaboration diagram for ATAS.Indicators.ITradesCache:"

```
ITimeMarketDataCache
< MarketDataArg >
    ↑
ATAS.Indicators. ITradesCache
[legend]
```

*Аннотация:* Схема взаимодействия (коллаборации) классов. В данном случае она идентична схеме наследования, что может указывать на тесную связь или способ использования `ITradesCache` в контексте `ITimeMarketDataCache`.  Стрелка "↑" и `[legend]` имеют аналогичное значение, что и в диаграмме наследования.

**Дополнительные унаследованные члены:** "Additional Inherited Members"

`► Properties inherited from ATAS.Indicators.ITimeMarketDataCache< MarketDataArg >`

*Аннотация:* Указание на наличие дополнительных членов (свойств), унаследованных от базового интерфейса `ATAS.Indicators.ITimeMarketDataCache<MarketDataArg>`.  Символ "►" может означать раскрывающийся список или ссылку на подробную информацию о свойствах.

**Детальное описание:** "Detailed Description"

"Interface for manager that provides access to trades cache."

*Аннотация:*  Более подробное описание интерфейса, повторяющее основную функцию: предоставление доступа к кешу сделок для менеджера.

**Файл документации:** "The documentation for this interface was generated from the following file:"

`• IOnlineDataProvider.cs`

*Аннотация:*  Информация о файле, из которого была сгенерирована документация для данного интерфейса.  `IOnlineDataProvider.cs` является исходным файлом, содержащим определение интерфейса `ITradesCache`. Точка "•" может быть маркером списка.

**Список всех членов:** "List of all members" (в правом верхнем углу)

*Аннотация:*  Ссылка "List of all members" предполагает наличие страницы или раздела, где можно найти полный список членов интерфейса (методов, свойств, событий и т.д.).  Расположение в правом верхнем углу типично для навигационных элементов в документации.







## Методичка по интерфейсу `ATAS.Indicators.ITradingManager` для создания индикаторов ATAS

*Аннотация: Этот интерфейс `ITradingManager` предоставляет функциональность для управления торговыми операциями из индикаторов ATAS. Он включает методы для открытия, закрытия, изменения и отмены ордеров, а также для управления стоп-лоссами и тейк-профитами.*

**ATAS.Indicators.ITradingManager Interface Reference**

Интерфейс, представляющий торговый менеджер для обработки торговых операций.

**Public Member Functions (Публичные функции-члены)**

*Аннотация:  В этом разделе перечислены все публичные функции, доступные в интерфейсе `ITradingManager`. Каждая функция описана с указанием ее параметров и назначения. Эти функции позволяют индикатору выполнять различные торговые действия.*

* `void OpenOrder(Order order, bool setDefaultQuantity, bool askConfirmation = true, bool checkOrderStates = true)`
    * Открывает новый ордер для торговли.

* `Task OpenOrderAsync(Order order, bool setDefaultQuantity, bool askConfirmation = true, bool checkOrderStates = true)`
    * Асинхронно открывает новый ордер для торговли.

* `void ModifyOrder(Order order, Order newOrder, bool askConfirmation = true, bool checkOrderStates = true)`
    * Изменяет существующий ордер.

* `Task ModifyOrderAsync(Order order, Order newOrder, bool askConfirmation = true, bool checkOrderStates = true)`
    * Асинхронно изменяет существующий ордер.

* `void CancelOrder(Order order, bool askConfirmation = true, bool checkOrderStates = true)`
    * Отменяет существующий ордер.

* `Task CancelOrderAsync(Order order, bool askConfirmation = true, bool checkOrderStates = true)`
    * Асинхронно отменяет существующий ордер.

* `bool ClosePosition(Position position, bool askConfirmation = true, bool checkOrderStates = true)`
    * Закрывает указанную позицию.

* `Task ClosePositionAsync(Position position, bool askConfirmation = true, bool checkOrderStates = true)`
    * Асинхронно закрывает указанную позицию.

* `ISecurityTradingOptions? GetSecurityTradingOptions()`
    * Возвращает `ISecurityTradingOptions` для текущего `ITradingManager.Security`.

* `Task SetStopLoss(PriceUnit value)`
    * Устанавливает новое значение стоп-лосса.

* `Task SetTakeProfit(PriceUnit value)`
    * Устанавливает новое значение тейк-профита.

* `Task SetBreakeven()`
    * Устанавливает стоп-лосс в безубыток.

* `bool IsStopLossOrder(Order order)`
    * Проверяет, является ли ордер стоп-лосс ордером.

* `bool IsTakeProfitOrder(Order order)`
    * Проверяет, является ли ордер тейк-профит ордером.

**Properties (Свойства)**

*Аннотация:  В этом разделе описаны свойства интерфейса `ITradingManager`. Свойства предоставляют доступ к различной информации, связанной с торговлей, такой как выбранная ценная бумага, портфель, позиция и ордера. Эти свойства доступны только для чтения (`[get]`).*

* `Security? Security [get]`
    * Выбранная ценная бумага.

* `Portfolio? Portfolio [get]`
    * Выбранный портфель.

* `TPlusLimits? TPlusLimit [get]`
    * T+ лимиты для выбранной ценной бумаги.

* `Position? Position [get]`
    * Текущая позиция для выбранной ценной бумаги и портфеля.

* `IEnumerable<MyTrade> MyTrades [get]`
    * Коллекция `MyTrade` для выбранной ценной бумаги и портфеля.

* `IEnumerable<Order> Orders [get]`
    * Коллекция ордеров для выбранной ценной бумаги и портфеля.

* `ITradingVolumeInfo? TradingVolumeInfo [get]`
    * Возвращает `ITradingVolumeInfo` для текущего `ITradingManager.Security`.

* `bool IsStopLossModeActivated [get]`
    * Указывает, активирован ли режим стоп-лосса.

* `bool IsTakeProfitModeActivated [get]`
    * Указывает, активирован ли режим тейк-профита.

**Events (События)**

*Аннотация:  В этом разделе перечислены события, которые могут быть сгенерированы интерфейсом `ITradingManager`. Индикатор может подписываться на эти события, чтобы реагировать на изменения в торговой среде, такие как выбор новой ценной бумаги, изменение позиции или добавление нового ордера.*

* `Action<Security> SecuritySelected`
    * Событие, которое возникает при выборе ценной бумаги.

* `Action<Portfolio> PortfolioSelected`
    * Событие, которое возникает при выборе портфеля.

* `Action<Portfolio> PortfolioChanged`
    * Событие, которое возникает при изменении выбранного портфеля.

* `Action<Position> PositionChanged`
    * Событие, которое возникает при изменении позиции.

* `Action<Order> NewOrder`
    * Событие, которое возникает при добавлении нового ордера.

* `Action<Order> OrderChanged`
    * Событие, которое возникает при изменении ордера.

* `Action<MyTrade> NewMyTrade`
    * Событие, которое возникает при добавлении новой сделки `MyTrade`.

* `Action<Order, string> OrderRegisterFailed`
    * Событие, которое возникает, когда не удается зарегистрировать ордер.

* `Action<Order, string> OrderCancelFailed`
    * Событие, которое возникает, когда не удается отменить ордер.

* `Action<Order, Order, string> OrderModifyFailed`
    * Событие, которое возникает, когда не удается изменить ордер.

**Detailed Description (Подробное описание)**

Интерфейс, представляющий торговый менеджер для обработки торговых операций.

**Member Function Documentation (Документация функций-членов)**

* **`CancelOrder()`**

```csharp
void ATAS.Indicators.ITradingManager.CancelOrder(Order order,
    bool askConfirmation = true,
    bool checkOrderStates = true
)
```

Отменяет существующий ордер.

**Parameters (Параметры)**

* `order`
    * Ордер для отмены.
* `askConfirmation`
    * `true` для запроса подтверждения перед отменой ордера.
* `checkOrderStates`
    * `true` для проверки состояний ордера перед отменой ордера.

* **`CancelOrderAsync()`**

```csharp
Task ATAS.Indicators.ITradingManager.CancelOrderAsync(Order order,
    bool askConfirmation = true,
    bool checkOrderStates = true
)
```

Отменяет существующий ордер.

**Parameters (Параметры)**

* `order`
    * Ордер для отмены.
* `askConfirmation`
    * `true` для запроса подтверждения перед отменой ордера.
* `checkOrderStates`
    * `true` для проверки состояний ордера перед отменой ордера.

* **`ClosePosition()`**

```csharp
bool ATAS.Indicators.ITradingManager.ClosePosition(Position position,
    bool askConfirmation = true,
    bool checkOrderStates = true
)
```

Закрывает указанную позицию.

**Parameters (Параметры)**

* `position`
    * Позиция для закрытия.
* `askConfirmation`
    * `true` для запроса подтверждения перед закрытием позиции.
* `checkOrderStates`
    * `true` для проверки состояний ордера перед закрытием позиции.

* **`ClosePositionAsync()`**

```csharp
Task ATAS.Indicators.ITradingManager.ClosePositionAsync(Position position,
    bool askConfirmation = true,
    bool checkOrderStates = true
)
```

Закрывает указанную позицию.

**Parameters (Параметры)**

* `position`
    * Позиция для закрытия.
* `askConfirmation`
    * `true` для запроса подтверждения перед закрытием позиции.
* `checkOrderStates`
    * `true` для проверки состояний ордера перед закрытием позиции.

* **`GetSecurityTradingOptions()`**

```csharp
ISecurityTradingOptions? ATAS.Indicators.ITradingManager.GetSecurityTradingOptions()
```

Возвращает `ISecurityTradingOptions` для текущего `ITradingManager.Security`.

* **`IsStopLossOrder()`**

```csharp
bool ATAS.Indicators.ITradingManager.IsStopLossOrder(Order order)
```

Проверяет, является ли ордер стоп-лосс ордером.

**Parameters (Параметры)**

* `order`
    * Ордер для проверки.

**Returns (Возвращает)**

* `true`, если ордер является стоп-лосс ордером, иначе `false`.

* **`IsTakeProfitOrder()`**

```csharp
bool ATAS.Indicators.ITradingManager.IsTakeProfitOrder(Order order)
```

Проверяет, является ли ордер тейк-профит ордером.

**Parameters (Параметры)**

* `order`
    * Ордер для проверки.

**Returns (Возвращает)**

* `true`, если ордер является тейк-профит ордером, иначе `false`.

* **`ModifyOrder()`**

```csharp
void ATAS.Indicators.ITradingManager.ModifyOrder(Order order,
    Order newOrder,
    bool askConfirmation = true,
    bool checkOrderStates = true
)
```

Изменяет существующий ордер.

**Parameters (Параметры)**

* `order`
    * Ордер для изменения.
* `newOrder`
    * Измененный ордер.
* `askConfirmation`
    * `true` для запроса подтверждения перед изменением ордера.
* `checkOrderStates`
    * `true` для проверки состояний ордера перед изменением ордера.

* **`ModifyOrderAsync()`**

```csharp
Task ATAS.Indicators.ITradingManager.ModifyOrderAsync(Order order,
    Order newOrder,
    bool askConfirmation = true,
    bool checkOrderStates = true
)
```

Изменяет существующий ордер.

**Parameters (Параметры)**

* `order`
    * Ордер для изменения.
* `newOrder`
    * Измененный ордер.
* `askConfirmation`
    * `true` для запроса подтверждения перед изменением ордера.
* `checkOrderStates`
    * `true` для проверки состояний ордера перед изменением ордера.

* **`OpenOrder()`**

```csharp
void ATAS.Indicators.ITradingManager.OpenOrder(Order order,
    bool setDefaultQuantity,
    bool askConfirmation = true,
    bool checkOrderStates = true
)
```

Открывает новый ордер для торговли.

**Parameters (Параметры)**

* `order`
    * Ордер для открытия.
* `setDefaultQuantity`
    * `true` для использования количества по умолчанию.
* `askConfirmation`
    * `true` для запроса подтверждения перед открытием ордера.
* `checkOrderStates`
    * `true` для проверки состояний ордера перед открытием ордера.

* **`OpenOrderAsync()`**

```csharp
Task ATAS.Indicators.ITradingManager.OpenOrderAsync(Order order,
    bool setDefaultQuantity,
    bool askConfirmation = true,
    bool checkOrderStates = true
)
```

Открывает новый ордер для торговли.

**Parameters (Параметры)**

* `order`
    * Ордер для открытия.
* `setDefaultQuantity`
    * `true` для использования количества по умолчанию.
* `askConfirmation`
    * `true` для запроса подтверждения перед открытием ордера.
* `checkOrderStates`
    * `true` для проверки состояний ордера перед открытием ордера.

* **`SetBreakeven()`**

```csharp
Task ATAS.Indicators.ITradingManager.SetBreakeven()
```

Устанавливает стоп-лосс в безубыток.

* **`SetStopLoss()`**

```csharp
Task ATAS.Indicators.ITradingManager.SetStopLoss(PriceUnit value)
```

Устанавливает новое значение стоп-лосса.

**Parameters (Параметры)**

* `value`
    * Значение стоп-лосса.

* **`SetTakeProfit()`**

```csharp
Task ATAS.Indicators.ITradingManager.SetTakeProfit(PriceUnit value)
```

Устанавливает новое значение тейк-профита.

**Parameters (Параметры)**

* `value`
    * Значение тейк-профита.

**Property Documentation (Документация свойств)**

* **`IsStopLossModeActivated`**

```csharp
bool ATAS.Indicators.ITradingManager.IsStopLossModeActivated [get]
```

Указывает, активирован ли режим стоп-лосса.

* **`IsTakeProfitModeActivated`**

```csharp
bool ATAS.Indicators.ITradingManager.IsTakeProfitModeActivated [get]
```

Указывает, активирован ли режим тейк-профита.

* **`MyTrades`**

```csharp
IEnumerable<MyTrade> ATAS.Indicators.ITradingManager.MyTrades [get]
```

Коллекция `MyTrade` для выбранной ценной бумаги и портфеля.

* **`Orders`**

```csharp
IEnumerable<Order> ATAS.Indicators.ITradingManager.Orders [get]
```

Коллекция ордеров для выбранной ценной бумаги и портфеля.

* **`Portfolio`**

```csharp
Portfolio? ATAS.Indicators.ITradingManager.Portfolio [get]
```

Выбранный портфель.

* **`Position`**

```csharp
Position? ATAS.Indicators.ITradingManager.Position [get]
```

Текущая позиция для выбранной ценной бумаги и портфеля.

* **`Security`**

```csharp
Security? ATAS.Indicators.ITradingManager.Security [get]
```

Выбранная ценная бумага.

* **`TPlusLimit`**

```csharp
TPlusLimits? ATAS.Indicators.ITradingManager.TPlusLimit [get]
```

T+ лимиты для выбранной ценной бумаги.

* **`TradingVolumeInfo`**

```csharp
ITradingVolumeInfo? ATAS.Indicators.ITradingManager.TradingVolumeInfo [get]
```

Возвращает `ITradingVolumeInfo` для текущего `ITradingManager.Security`.

**Event Documentation (Документация событий)**

* **`NewMyTrade`**

```csharp
Action<MyTrade> ATAS.Indicators.ITradingManager.NewMyTrade
```

Событие, которое возникает при добавлении новой сделки `MyTrade`.

* **`NewOrder`**

```csharp
Action<Order> ATAS.Indicators.ITradingManager.NewOrder
```

Событие, которое возникает при добавлении нового ордера.

* **`OrderCancelFailed`**

```csharp
Action<Order, string> ATAS.Indicators.ITradingManager.OrderCancelFailed
```

Событие, которое возникает, когда не удается отменить ордер.

* **`OrderChanged`**

```csharp
Action<Order> ATAS.Indicators.ITradingManager.OrderChanged
```

Событие, которое возникает при изменении ордера.

* **`OrderModifyFailed`**

```csharp
Action<Order, Order, string> ATAS.Indicators.ITradingManager.OrderModifyFailed
```

Событие, которое возникает, когда не удается изменить ордер.

* **`OrderRegisterFailed`**

```csharp
Action<Order, string> ATAS.Indicators.ITradingManager.OrderRegisterFailed
```

Событие, которое возникает, когда не удается зарегистрировать ордер.

* **`PortfolioChanged`**

```csharp
Action<Portfolio> ATAS.Indicators.ITradingManager.PortfolioChanged
```

Событие, которое возникает при изменении выбранного портфеля.

* **`PortfolioSelected`**

```csharp
Action<Portfolio> ATAS.Indicators.ITradingManager.PortfolioSelected
```

Событие, которое возникает при выборе портфеля.

* **`PositionChanged`**

```csharp
Action<Position> ATAS.Indicators.ITradingManager.PositionChanged
```

Событие, которое возникает при изменении позиции.

* **`SecuritySelected`**

```csharp
Action<Security> ATAS.Indicators.ITradingManager.SecuritySelected
```

Событие, которое возникает при выборе ценной бумаги.

**The documentation for this interface was generated from the following file:**

* `ITradingManager.cs`









## Методичка по интерфейсу `ATAS.Indicators.ITradingVolumeInfo` для создания индикаторов ATAS

**Описание интерфейса:**

Интерфейс `ATAS.Indicators.ITradingVolumeInfo` предназначен для предоставления информации о селекторе объема в платформе ATAS. Он позволяет индикаторам получать доступ к текущим настройкам объема, таким как валюта объема, фактическое значение объема и формат отображения объема.

**Публичные члены интерфейса:**

Интерфейс `ITradingVolumeInfo` включает в себя следующие категории членов:

*   **Публичные функции-члены (Public Member Functions)**
*   **Свойства (Properties)**
*   **События (Events)**

### **Публичные функции-члены (Public Member Functions)**

Интерфейс `ITradingVolumeInfo` содержит одну публичную функцию-член:

1.  `SetCurrentVolumeItem(IVolumeSelectorItem? settings)`

    *   **Описание:** Метод `SetCurrentVolumeItem` позволяет установить определенный шаблон объема в качестве выбранного.
    *   **Синтаксис:** `void SetCurrentVolumeItem(IVolumeSelectorItem? settings)`
    *   **Параметры:**
        *   `settings`: Параметр типа `IVolumeSelectorItem?`, представляющий шаблон объема, который необходимо установить как текущий. Значение может быть `null`, если нужно снять выбор шаблона.
    *   **Возвращаемое значение:**  `void` (метод не возвращает значения).

### **Свойства (Properties)**

Интерфейс `ITradingVolumeInfo` предоставляет доступ к следующим свойствам, которые позволяют получить информацию о настройках объема:

1.  `Currency` \[get]

    *   **Тип:** `string`
    *   **Описание:** Свойство `Currency` возвращает строку, представляющую выбранную валюту (режим) торгового объема.
    *   **Доступ:** Только для чтения (`[get]`).

2.  `ActualVolume` \[get]

    *   **Тип:** `decimal`
    *   **Описание:** Свойство `ActualVolume` возвращает десятичное число (`decimal`), представляющее фактическое выбранное значение объема.
    *   **Доступ:** Только для чтения (`[get]`).

3.  `VolumeFormat` \[get]

    *   **Тип:** `string`
    *   **Описание:** Свойство `VolumeFormat` возвращает строку, определяющую формат отображения объема.
    *   **Доступ:** Только для чтения (`[get]`).

4.  `IsCustomVolumeSelected` \[get]

    *   **Тип:** `bool`
    *   **Описание:** Свойство `IsCustomVolumeSelected` возвращает логическое значение (`bool`). `true`, если введен пользовательский объем или выбран шаблон объема, и `false` в противном случае.
    *   **Доступ:** Только для чтения (`[get]`).

5.  `VolumeItems` \[get]

    *   **Тип:** `IVolumeSelectorItem?[]` (массив объектов типа `IVolumeSelectorItem?`)
    *   **Описание:** Свойство `VolumeItems` возвращает массив, содержащий коллекцию шаблонов объема. Каждый элемент массива может быть объектом типа `IVolumeSelectorItem?` или `null`.
    *   **Доступ:** Только для чтения (`[get]`).

6.  `CurrentVolumeItem` \[get]

    *   **Тип:** `IVolumeSelectorItem?`
    *   **Описание:** Свойство `CurrentVolumeItem` возвращает объект типа `IVolumeSelectorItem?`, представляющий выбранный в данный момент шаблон объема. Может быть `null`, если шаблон не выбран.
    *   **Доступ:** Только для чтения (`[get]`).

### **События (Events)**

Интерфейс `ITradingVolumeInfo` определяет одно событие:

1.  `TradingVolumeInfoChanged`

    *   **Тип события:** `Action` (делегат типа `Action`, не возвращает значения и не принимает параметров)
    *   **Описание:** Событие `TradingVolumeInfoChanged` возникает, когда изменяются настройки объема. Индикаторы могут подписаться на это событие, чтобы получать уведомления об изменениях настроек объема в реальном времени.

**Дополнительная информация:**

*   Интерфейс `ITradingVolumeInfo` определен в файле `IIndicatorContainer.cs`.
*   Интерфейс предназначен для использования в индикаторах ATAS, которым требуется информация о текущих настройках объема, выбранных пользователем.

**Аннотации для начинающих программистов:**

*   **Интерфейс (Interface):**  Это как контракт или шаблон, который определяет, какие функции, свойства и события должен иметь класс, реализующий этот интерфейс. В данном случае, любой индикатор ATAS, который хочет получать информацию об объеме, может "реализовать" интерфейс `ITradingVolumeInfo`.
*   **`void`:**  Означает, что функция не возвращает никакого значения.
*   **`string`:** Тип данных для текста (строка символов).
*   **`decimal`:** Тип данных для точных десятичных чисел, используется для финансовых расчетов.
*   **`bool`:** Логический тип данных, может быть `true` (истина) или `false` (ложь).
*   **`IVolumeSelectorItem?`:**  Тип данных, представляющий шаблон объема. Знак `?` означает, что переменная может иметь значение `null` (ничего или отсутствие значения).
*   **`IVolumeSelectorItem?[]`:** Массив (список) элементов типа `IVolumeSelectorItem?`.
*   **`[get]`:**  Указывает, что свойство доступно только для чтения (можно только получить значение, но не изменить его).
*   **`Action`:** Тип для событий, которые не передают никаких данных при возникновении. Когда происходит событие `TradingVolumeInfoChanged`, все подписанные на него функции будут вызваны.

Эта методичка предоставляет полное описание интерфейса `ATAS.Indicators.ITradingVolumeInfo` и может быть использована для разработки индикаторов ATAS, использующих информацию о настройках объема.







## Методичка по интерфейсу `ATAS.Indicators.IVolumeSelectorItem` для создания индикаторов ATAS

### Описание интерфейса

Интерфейс `ATAS.Indicators.IVolumeSelectorItem` представляет шаблон объема.

### Свойства интерфейса

Интерфейс `ATAS.Indicators.IVolumeSelectorItem` имеет следующие свойства:

1.  **`Volume`**

    *   Тип свойства: `decimal?` (Nullable Decimal) -  `decimal?` означает, что свойство может содержать десятичное число или значение `null`.
    *   Доступ: `[get]` -  `[get]` указывает, что свойство доступно только для чтения (получения значения).
    *   Описание:  Представляет объем, связанный с элементом.

2.  **`ValueFormat`**

    *   Тип свойства: `string?` (Nullable String) - `string?` означает, что свойство может содержать строку или значение `null`.
    *   Доступ: `[get]` - `[get]` указывает, что свойство доступно только для чтения (получения значения).
    *   Описание:  Представляет формат значения объема в виде строки.

### Подробное описание

Интерфейс `ATAS.Indicators.IVolumeSelectorItem` предназначен для представления шаблона объема в индикаторах ATAS. Он определяет структуру данных для элементов, связанных с объемом, и предоставляет свойства для доступа к значению объема и его строковому представлению формата.

### Документация свойств

#### `ValueFormat`

*   Тип: `string?` `ATAS.Indicators.IVolumeSelectorItem.ValueFormat`
*   Доступ: `get`
*   Описание:  Строковое представление формата значения объема для элемента `IVolumeSelectorItem`.

#### `Volume`

*   Тип: `decimal?` `ATAS.Indicators.IVolumeSelectorItem.Volume`
*   Доступ: `get`
*   Описание:  Значение объема для элемента `IVolumeSelectorItem`.

### Файл документации

Документация для данного интерфейса была сгенерирована из файла:

*   `IIndicatorContainer.cs`









## Методичка по классу `ATAS.Indicators.LineSeries` для создания индикаторов ATAS

### Описание класса

Класс `ATAS.Indicators.LineSeries` представляет горизонтальную линию с одним значением.

**Наследование:**

`BaseDataSeries<decimal>` → `ATAS.Indicators.LineSeries`

**Описание:**

Предназначен для отображения горизонтальной линии на графике индикатора, значение которой остается постоянным на всем протяжении графика.  Используется для визуального выделения уровней или границ на ценовом графике.

### Конструкторы

1.  `LineSeries(string id, string name)`

    *   **Описание:** Инициализирует новый экземпляр класса `LineSeries` с указанным уникальным и постоянным идентификатором серии данных для сериализации данных и уникальным именем.

    *   **Параметры:**
        *   `id` (string): Уникальный и постоянный идентификатор серии данных для сериализации.
        *   `name` (string): Уникальное имя серии данных.

    *   **Аннотация:**  Этот конструктор используется, когда необходимо задать как ID, так и имя для линии. ID важен для сохранения и восстановления настроек индикатора, а имя будет отображаться в интерфейсе ATAS.

2.  `LineSeries(string id)`

    *   **Описание:** Инициализирует новый экземпляр класса `LineSeries` с указанным уникальным и постоянным идентификатором серии данных для сериализации данных.

    *   **Параметры:**
        *   `id` (string): Уникальный и постоянный идентификатор серии данных для сериализации.

    *   **Аннотация:**  Этот конструктор является упрощенной версией первого, где задается только ID. Имя для линии в этом случае может быть задано по умолчанию или установлено позже через свойство.

### Свойства

1.  `CrossColor`

    *   **Тип:** `Color`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Цвет линии.

    *   **Аннотация:** Позволяет задать цвет горизонтальной линии. Используйте стандартные цветовые константы или RGB значения для настройки.

2.  `LineDashStyle`

    *   **Тип:** `LineDashStyle`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Стиль линии.

    *   **Аннотация:** Определяет стиль отображения линии, например, сплошная, пунктирная, штрих-пунктирная и т.д. Используйте значения из перечисления `LineDashStyle`.

3.  `Width`

    *   **Тип:** `int`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Толщина линии.

    *   **Аннотация:** Задает толщину линии в пикселях. Большие значения делают линию толще.

4.  `UseScale`

    *   **Тип:** `bool`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Указывает, использовать ли шкалу.

    *   **Аннотация:**  Если `true`, линия будет масштабироваться вместе с ценовой шкалой графика. Если `false`, положение линии на графике будет фиксированным в пикселях относительно вертикальной оси.

5.  `Value`

    *   **Тип:** `decimal`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Значение линии.

    *   **Аннотация:** Устанавливает или возвращает значение, на котором будет отображаться горизонтальная линия. Это значение соответствует цене на вертикальной оси графика.

6.  `Text`

    *   **Тип:** `string`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Текст, связанный с линией.

    *   **Аннотация:** Позволяет добавить текстовую метку к линии. Этот текст может быть отображен рядом с линией для пояснения ее значения или назначения.

7.  `Count` (унаследовано от `BaseDataSeries<decimal>`)

    *   **Тип:** `override int`
    *   **Доступ:** `[get]`
    *   **Описание:** Возвращает количество элементов в серии (всегда `int.MaxValue`).

    *   **Аннотация:**  Это свойство не предназначено для непосредственного использования при работе с `LineSeries`, так как линия представляет собой одно значение, повторяющееся на всем протяжении графика.

8.  `this[int index]` (индексатор, унаследован от `BaseDataSeries<decimal>`)

    *   **Тип:** `override decimal`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Возвращает или устанавливает значение по указанному индексу (всегда возвращает текущее `Value` и выбрасывает `NotSupportedException` при попытке установки).

    *   **Аннотация:**  Аналогично `Count`, индексатор не имеет практического применения для `LineSeries` в контексте установки значений, так как `LineSeries` отображает одно постоянное значение. Используйте свойство `Value` для изменения значения линии.

### Дополнительная информация

*   Класс `LineSeries` предназначен для создания **горизонтальных** линий.
*   Для создания вертикальных линий или других графических элементов следует использовать другие классы из пространства имен `ATAS.Indicators`.
*   Для изменения внешнего вида линии (цвет, стиль, толщина) используйте соответствующие свойства.
*   Значение линии задается через свойство `Value`.







## Методичка по классу `ATAS.Indicators.MarketDataArg` для создания индикаторов ATAS

### Обзор класса `ATAS.Indicators.MarketDataArg`
`ATAS.Indicators.MarketDataArg` представляет точку данных на рынке.

*   **Описание:** Представляет точку данных на рынке.

### Диаграммы наследования и взаимодействия
Диаграммы показывают, как класс `MarketDataArg` связан с другими элементами в системе.

*   **Диаграмма наследования:**
    ```
    IEntity
        ↑
    ATAS.Indicators.MarketDataArg
    ```
*   **Диаграмма взаимодействия:**
    ```
    IEntity
        ↑
    ATAS.Indicators.MarketDataArg
    ```

### Публичные функции-члены
Конструктор для создания экземпляра класса `MarketDataArg`.

*   **`MarketDataArg()`**
    *   **Описание:** Инициализирует новый экземпляр класса `MarketDataArg`.

### Свойства
Список свойств класса `MarketDataArg` и их описание.

*   **`Price`**
    *   **Тип данных:** `decimal`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Цена рыночных данных.
*   **`OriginPrice`**
    *   **Тип данных:** `decimal`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Исходная, немасштабированная цена рыночных данных.
*   **`Volume`**
    *   **Тип данных:** `decimal`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Объем, связанный с рыночными данными.
*   **`Time`**
    *   **Тип данных:** `DateTime`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Время, когда произошли рыночные данные.
*   **`Direction`**
    *   **Тип данных:** `TradeDirection`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Направление торговли рыночных данных (Покупка, Продажа или Между).
*   **`DataType`**
    *   **Тип данных:** `MarketDataType`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Тип рыночных данных (Bid, Ask или Trade).
*   **`OpenInterest`**
    *   **Тип данных:** `decimal`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Открытый интерес, связанный с рыночными данными.
*   **`IsAsk`**
    *   **Тип данных:** `bool`
    *   **Доступ:** `[get]`
    *   **Описание:** Получает значение, указывающее, являются ли рыночные данные типом Ask.
*   **`IsBid`**
    *   **Тип данных:** `bool`
    *   **Доступ:** `[get]`
    *   **Описание:** Получает значение, указывающее, являются ли рыночные данные типом Bid.
*   **`AggressorExchangeOrderId`**
    *   **Тип данных:** `long?`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Получает или задает идентификатор агрессора биржевого ордера (см. свойство `MarketByOrder.ExchangeOrderId`), связанный с этой сделкой.
*   **`ExchangeOrderId`**
    *   **Тип данных:** `long?`
    *   **Доступ:** `[get, set]`
    *   **Описание:** Получает или задает идентификатор биржевого ордера (см. свойство `MarketByOrder.ExchangeOrderId`), связанный с этой сделкой.

### Подробное описание
Более развернутое описание класса `ATAS.Indicators.MarketDataArg`.

*   **Описание:** Представляет точку данных на рынке.

### Документация конструктора и деструктора
Детальная информация о конструкторе класса.

*   **`MarketDataArg()`**
    *   **Сигнатура:** `ATAS.Indicators.MarketDataArg.MarketDataArg()`
    *   **Описание:** Инициализирует новый экземпляр класса `MarketDataArg`.

### Документация свойств
Подробная информация о каждом свойстве класса `MarketDataArg`.

*   **`AggressorExchangeOrderId`**
    *   **Сигнатура:** `long? ATAS.Indicators.MarketDataArg.AggressorExchangeOrderId` `[get, set]`
    *   **Описание:** Получает или задает идентификатор агрессора биржевого ордера (см. свойство `MarketByOrder.ExchangeOrderId`), связанный с этой сделкой.
*   **`DataType`**
    *   **Сигнатура:** `MarketDataType ATAS.Indicators.MarketDataArg.DataType` `[get, set]`
    *   **Описание:** Тип рыночных данных (Bid, Ask или Trade).
*   **`Direction`**
    *   **Сигнатура:** `TradeDirection ATAS.Indicators.MarketDataArg.Direction` `[get, set]`
    *   **Описание:** Направление торговли рыночных данных (Buy, Sell, or Between).
*   **`ExchangeOrderId`**
    *   **Сигнатура:** `long? ATAS.Indicators.MarketDataArg.ExchangeOrderId` `[get, set]`
    *   **Описание:** Получает или задает идентификатор биржевого ордера (см. свойство `MarketByOrder.ExchangeOrderId`), связанный с этой сделкой.
*   **`IsAsk`**
    *   **Сигнатура:** `bool ATAS.Indicators.MarketDataArg.IsAsk` `[get]`
    *   **Описание:** Получает значение, указывающее, являются ли рыночные данные типом Ask.
*   **`IsBid`**
    *   **Сигнатура:** `bool ATAS.Indicators.MarketDataArg.IsBid` `[get]`
    *   **Описание:** Получает значение, указывающее, являются ли рыночные данные типом Bid.
*   **`OpenInterest`**
    *   **Сигнатура:** `decimal ATAS.Indicators.MarketDataArg.OpenInterest` `[get, set]`
    *   **Описание:** Открытый интерес, связанный с рыночными данными.
*   **`OriginPrice`**
    *   **Сигнатура:** `decimal ATAS.Indicators.MarketDataArg.OriginPrice` `[get, set]`
    *   **Описание:** Исходная, немасштабированная цена рыночных данных.
*   **`Price`**
    *   **Сигнатура:** `decimal ATAS.Indicators.MarketDataArg.Price` `[get, set]`
    *   **Описание:** Цена рыночных данных.
*   **`Time`**
    *   **Сигнатура:** `DateTime ATAS.Indicators.MarketDataArg.Time` `[get, set]`
    *   **Описание:** Время, когда произошли рыночные данные.
*   **`Volume`**
    *   **Сигнатура:** `decimal ATAS.Indicators.MarketDataArg.Volume` `[get, set]`
    *   **Описание:** Объем, связанный с рыночными данными.

### Файл документации
Файл, из которого была сгенерирована документация.

*   **Файл:** `MarketDataArg.cs`









## Методичка по классу `ATAS.Indicators.MarketDepthInfoProvider` для создания индикаторов ATAS

### Обзор класса `ATAS.Indicators.MarketDepthInfoProvider`

**Полное имя класса:** `ATAS.Indicators.MarketDepthInfoProvider`

**Описание:** Класс `ATAS.Indicators.MarketDepthInfoProvider` реализует интерфейс `IMarketDepthInfoProvider` для предоставления информации о глубине рынка (Market Depth).

*   **Аннотация:** Этот класс используется для доступа к данным о глубине рынка, таким как объемы на покупку и продажу на разных ценовых уровнях. Он реализует интерфейс `IMarketDepthInfoProvider`, что означает, что он предоставляет стандартный набор функций для работы с глубиной рынка в ATAS.

**Диаграмма наследования:**

```
IMarketDepthInfoProvider
    ↑
ATAS.Indicators.MarketDepthInfoProvider
```

*   **Аннотация:** `ATAS.Indicators.MarketDepthInfoProvider` наследуется от интерфейса `IMarketDepthInfoProvider`. Это означает, что класс `MarketDepthInfoProvider` должен реализовывать все методы и свойства, определенные в интерфейсе `IMarketDepthInfoProvider`.

**Диаграмма взаимодействия:**

```
IMarketDepthInfoProvider
    ↑
ATAS.Indicators.MarketDepthInfoProvider
```

*   **Аннотация:** Диаграмма взаимодействия показывает, что класс `ATAS.Indicators.MarketDepthInfoProvider` работает в соответствии с интерфейсом `IMarketDepthInfoProvider`.  Другие компоненты системы могут взаимодействовать с `MarketDepthInfoProvider` через интерфейс `IMarketDepthInfoProvider`.

### Конструктор класса

**Конструктор:** `MarketDepthInfoProvider(IOnlineDataProvider onlineDataProvider)`

*   **Аннотация:** Конструктор используется для создания нового экземпляра класса `MarketDepthInfoProvider`.

**Параметры:**

*   `onlineDataProvider`: `IOnlineDataProvider`

    *   **Описание:**  Реализация интерфейса `IOnlineDataProvider` для получения данных о глубине рынка.
    *   **Аннотация:**  `onlineDataProvider` является обязательным параметром. Он предоставляет источник данных реального времени, необходимых для получения информации о глубине рынка. Класс `MarketDepthInfoProvider` будет использовать этот объект для запроса данных.

**Исключения:**

*   `ArgumentNullException`: Выбрасывается, если параметр `onlineDataProvider` равен `null`.

    *   **Аннотация:** Необходимо убедиться, что при создании экземпляра `MarketDepthInfoProvider` передан корректный объект `IOnlineDataProvider`. Если `onlineDataProvider` не будет предоставлен, возникнет ошибка.

### Публичные методы

#### `GetMarketDepthSnapshot()`

**Сигнатура метода:** `IEnumerable<MarketDataArg> GetMarketDepthSnapshot()`

*   **Аннотация:** Этот метод используется для получения "снимка" текущей глубины рынка. "Снимок" означает данные о глубине рынка на момент вызова метода.

**Возвращаемое значение:** `IEnumerable<MarketDataArg>`

*   **Описание:** Возвращает перечисляемую коллекцию объектов `MarketDataArg`, представляющих глубину рынка.
*   **Аннотация:**  Метод возвращает коллекцию `MarketDataArg`. Каждый объект `MarketDataArg` в коллекции, вероятно, содержит информацию об уровне цены и соответствующем объеме на этом уровне в глубине рынка (как для покупок, так и для продаж). `IEnumerable` означает, что данные можно перебирать последовательно, что эффективно для обработки больших объемов данных.

**Реализация интерфейса:** `ATAS.Indicators.IMarketDepthInfoProvider`

*   **Аннотация:** Метод `GetMarketDepthSnapshot()` является частью интерфейса `IMarketDepthInfoProvider`, который реализует класс `MarketDepthInfoProvider`.

### Свойства класса

#### `CumulativeDomAsks`

**Сигнатура свойства:** `decimal CumulativeDomAsks { get; }`

*   **Аннотация:** Это свойство предоставляет доступ только для чтения (только `get`).

**Тип данных:** `decimal`

**Описание:** Возвращает кумулятивную сумму объемов заявок на продажу (ask) в DOM (Depth of Market - Глубина рынка).

*   **Аннотация:**  `CumulativeDomAsks` представляет собой общее количество контрактов, выставленных на продажу в стакане цен (DOM) на всех уровнях цен. Это кумулятивное значение, то есть сумма объемов на всех уровнях ask.

**Реализация интерфейса:** `ATAS.Indicators.IMarketDepthInfoProvider`

*   **Аннотация:** Свойство `CumulativeDomAsks` является частью интерфейса `IMarketDepthInfoProvider`.

#### `CumulativeDomBids`

**Сигнатура свойства:** `decimal CumulativeDomBids { get; }`

*   **Аннотация:** Это свойство предоставляет доступ только для чтения (только `get`).

**Тип данных:** `decimal`

**Описание:** Возвращает кумулятивную сумму объемов заявок на покупку (bid) в DOM (Depth of Market - Глубина рынка).

*   **Аннотация:** `CumulativeDomBids` представляет собой общее количество контрактов, выставленных на покупку в стакане цен (DOM) на всех уровнях цен. Это также кумулятивное значение, то есть сумма объемов на всех уровнях bid.

**Реализация интерфейса:** `ATAS.Indicators.IMarketDepthInfoProvider`

*   **Аннотация:** Свойство `CumulativeDomBids` является частью интерфейса `IMarketDepthInfoProvider`.

### Файл исходного кода

*   `MarketDepthInfoProvider.cs`

    *   **Аннотация:** Исходный код класса `ATAS.Indicators.MarketDepthInfoProvider` находится в файле `MarketDepthInfoProvider.cs`.

**Конец методички.**











## Методичка по классу `ATAS.Indicators.MarketDepthSnapshot` для создания индикаторов ATAS

### Описание класса

**Название класса:** `ATAS.Indicators.MarketDepthSnapshot`

**Описание:**  Представляет конечное состояние глубины рынка за указанный период времени.

*   *Аннотация:*  Этот класс предназначен для получения моментального снимка (snapshot) глубины рынка (Market Depth или Order Book) на конец определенного временного интервала. Глубина рынка показывает текущие ордера на покупку (биды) и продажу (аски) для торгового инструмента.

### Свойства класса

#### 1. `StartTime`

**Тип данных:** `DateTime`

**Описание:** Время.

**Тип доступа:** `get` (только чтение)

*   *Аннотация:*  Свойство `StartTime` возвращает время, когда был сделан снимок глубины рынка. Тип данных `DateTime` представляет дату и время.  `get` означает, что значение этого свойства можно только получить, но не изменить извне.

#### 2. `Bids`

**Тип данных:** `IReadOnlyCollection<MarketDataArg>`

**Описание:** Коллекция бидов (заявок на покупку).

**Тип доступа:** `get` (только чтение)

*   *Аннотация:*  Свойство `Bids` представляет собой коллекцию заявок на покупку, которые присутствовали в глубине рынка на момент снимка. `IReadOnlyCollection<MarketDataArg>` указывает, что это коллекция объектов типа `MarketDataArg`, и она доступна только для чтения. Это значит, что вы можете просматривать биды, но не можете добавлять или удалять их из этой коллекции напрямую.  `MarketDataArg` скорее всего содержит информацию о цене и объеме каждой заявки на покупку.

#### 3. `Asks`

**Тип данных:** `IReadOnlyCollection<MarketDataArg>`

**Описание:** Коллекция асков (заявок на продажу).

**Тип доступа:** `get` (только чтение)

*   *Аннотация:*  Свойство `Asks` аналогично свойству `Bids`, но представляет собой коллекцию заявок на продажу (асков) на момент снимка глубины рынка.  Так же, как и `Bids`, это `IReadOnlyCollection<MarketDataArg>`, что означает коллекцию объектов `MarketDataArg` только для чтения.  `MarketDataArg` здесь будет содержать информацию о цене и объеме каждой заявки на продажу.

### Расположение файла класса

**Файл:** `MarketDepthSnapshot.cs`

*   *Аннотация:*  Исходный код класса `MarketDepthSnapshot` находится в файле `MarketDepthSnapshot.cs`. Эта информация может быть полезна для поиска дополнительной информации или контекста в документации или исходном коде ATAS, если это необходимо.

**Заключение:**

Данная методичка описывает класс `ATAS.Indicators.MarketDepthSnapshot`, который позволяет получить доступ к моментальному снимку глубины рынка.  Для создания индикаторов на основе этого класса, можно использовать свойства `StartTime`, `Bids` и `Asks` для анализа и отображения данных о глубине рынка в платформе ATAS.  Для более детальной работы с заявками на покупку и продажу, потребуется изучить структуру объекта `MarketDataArg`, который содержится в коллекциях `Bids` и `Asks`.






## Методичка по классу `ATAS.Indicators.MarketDepthSnapshotRequest` для создания индикаторов ATAS

### Описание класса

**`ATAS.Indicators.MarketDepthSnapshotRequest`** - Класс-запрос для получения моментального снимка (snapshot) глубины рынка за указанный период времени.

**Назначение:** Представляет запрос на получение данных о глубине рынка (стакан цен) за определенный временной интервал.

### Свойства класса

Класс `ATAS.Indicators.MarketDepthSnapshotRequest` содержит следующие свойства:

| Свойство     | Тип данных    | Обязательность | Описание                                                                 |
|--------------|---------------|----------------|--------------------------------------------------------------------------|
| `RequestId`  | `int`         | Нет            | Уникальный идентификатор запроса. Генерируется автоматически.              |
| `From`       | `DateTime`    | **Обязательно** | Время начала запрашиваемых данных.                                        |
| `To`         | `DateTime`    | **Обязательно** | Время окончания запрашиваемых данных.                                       |
| `Period`     | `TimeSpan`    | **Обязательно** | Период времени, за который необходимо получить данные глубины рынка.      |

### Детальное описание свойств

#### `RequestId`

*   **Тип данных:** `int` (целое число)
*   **Инициализация:** `DateTime.Now.GetHashCode()`
*   **Доступ:** `[get]` (только для чтения)
*   **Описание:** Уникальный идентификатор запроса.  Используется для отслеживания и идентификации конкретного запроса. Значение генерируется автоматически на основе хеш-кода текущего времени (`DateTime.Now.GetHashCode()`).

#### `From`

*   **Тип данных:** `DateTime` (дата и время)
*   **Обязательность:** **required** (обязательное свойство)
*   **Доступ:** `[get]` (только для чтения)
*   **Описание:**  Время начала временного интервала, для которого запрашиваются данные глубины рынка. Указывает начальную точку отсчета данных.

#### `Period`

*   **Тип данных:** `TimeSpan` (временной интервал)
*   **Обязательность:** **required** (обязательное свойство)
*   **Доступ:** `[get]` (только для чтения)
*   **Описание:**  Продолжительность временного интервала, за который необходимо получить данные. Определяет длину периода, в рамках которого запрашивается снимок глубины рынка.

#### `To`

*   **Тип данных:** `DateTime` (дата и время)
*   **Обязательность:** **required** (обязательное свойство)
*   **Доступ:** `[get]` (только для чтения)
*   **Описание:** Время окончания временного интервала, для которого запрашиваются данные глубины рынка. Указывает конечную точку отсчета данных.

### Дополнительная информация

*   Класс `ATAS.Indicators.MarketDepthSnapshotRequest` предназначен для запроса **моментального снимка** глубины рынка, а не для потоковой передачи данных.
*   Для создания запроса необходимо обязательно указать значения свойств `From`, `To` и `Period`.
*   Класс определен в файле `MarketDepthSnapshotRequest.cs`.

**Конец методички.**










## Методичка по классу `ATAS.Indicators.NotifyPropertyChangedBase`

**Описание класса:**

`ATAS.Indicators.NotifyPropertyChangedBase` - это базовый класс для реализации интерфейса `INotifyPropertyChanged`.

**Диаграмма наследования:**

```
INotifyPropertyChanged
    └── ATAS.Indicators.NotifyPropertyChangedBase
        ├── ATAS.Indicators.FilterBase
        │   └── ATAS.Indicators.FilterRangeValue<TValue>
        │       └── ATAS.Indicators.Filter<TValue>
```

**Диаграмма взаимодействия:**

```
INotifyPropertyChanged
    └── ATAS.Indicators.NotifyPropertyChangedBase
```

**Защищенные члены-функции:**

### `RaisePropertyChanged()`

```csharp
void RaisePropertyChanged ([CallerMemberName] string? propertyName=null!)
```

*   **Описание:** Вызывает событие `PropertyChanged` для указанного имени свойства.
*   **Параметры:**
    *   `propertyName`: `string?` - Имя свойства, которое изменилось.

### `SetProperty<TProperty>()`

```csharp
void SetProperty<TProperty> (ref TProperty store, TProperty value, [CallerMemberName] string? propertyName=null, Action? onChanged=null)
```

*   **Описание:**  Устанавливает значение свойства и вызывает событие `PropertyChanged`, если значение изменилось.
*   **Параметры:**
    *   `TProperty`: Тип свойства.
    *   `store`: `ref TProperty` - Ссылка на поле, хранящее значение свойства.
    *   `value`: `TProperty` - Новое значение свойства.
    *   `propertyName`: `string?` - Имя свойства (автоматически заполняется с помощью `[CallerMemberName]`).
    *   `onChanged`: `Action?` -  Действие, которое нужно выполнить после изменения свойства (опционально).

**События:**

### `PropertyChanged`

```csharp
PropertyChangedEventHandler? PropertyChanged
```

*   **Описание:** Событие, которое возникает при изменении значения свойства.
*   **Тип:** `PropertyChangedEventHandler?` -  Делегат для обработки события `PropertyChanged`.

**Подробное описание:**

Базовый класс для реализации интерфейса `INotifyPropertyChanged`.

**Документация по членам-функциям:**

### `RaisePropertyChanged()`

```csharp
void ATAS.Indicators.NotifyPropertyChangedBase.RaisePropertyChanged ( [CallerMemberName] string? propertyName )
```

*   **protected**
*   **Описание:** Вызывает событие `PropertyChanged` для указанного имени свойства.
*   **Параметры:**
    *   `propertyName`:  `string?` - Имя свойства, которое изменилось.

### `SetProperty<TProperty>()`

```csharp
void ATAS.Indicators.NotifyPropertyChangedBase.SetProperty< TProperty > (ref TProperty store, TProperty value, [CallerMemberName] string? propertyName, Action? onChanged )
```

*   **protected**
*   **Описание:** Устанавливает значение свойства и вызывает событие `PropertyChanged`, если значение изменилось.
*   **Параметры:**
    *   `TProperty`: Тип свойства.
    *   `store`: `ref TProperty` - Ссылка на поле, хранящее значение свойства.
    *   `value`: `TProperty` - Новое значение свойства.
    *   `propertyName`: `string?` - Имя свойства (автоматически заполняется с помощью `[CallerMemberName]`).
    *   `onChanged`: `Action?` - Действие, которое нужно выполнить после изменения свойства (опционально).

**Документация по событиям:**

### `PropertyChanged`

```csharp
PropertyChangedEventHandler? ATAS.Indicators.NotifyPropertyChangedBase.PropertyChanged
```

*   **Описание:** Событие, которое возникает при изменении значения свойства.

**Файл, в котором сгенерирована документация:**

*   `NotifyPropertyChangedBase.cs`










## Методичка по классу `ATAS.Indicators.ObjectDataSeries`

**Пространство имен:** `ATAS.Indicators`

**Класс:** `ObjectDataSeries`

**Описание:**

Представляет собой ряд данных объектов, позволяющий хранить элементы данных любого типа.

**Диаграмма наследования:**

```
BaseDataSeries< object >
  |
  └── ObjectDataSeries
```

**Диаграмма взаимодействия:**

```
BaseDataSeries< object >
  |
  └── ObjectDataSeries
```

**Публичные члены класса:**

### Конструкторы

#### `ObjectDataSeries(string id, string name)`

*   **Описание:** Инициализирует новый экземпляр класса `ObjectDataSeries` с указанным уникальным и постоянным идентификатором серии данных для сериализации данных и уникальным именем.

*   **Параметры:**
    *   `id` : `string` - Уникальный и постоянный идентификатор серии данных для сериализации данных.
    *   `name` : `string` - Уникальное имя серии данных.

#### `ObjectDataSeries(string id)`

*   **Описание:** Инициализирует новый экземпляр класса `ObjectDataSeries` с указанным уникальным и постоянным идентификатором серии данных для сериализации данных.

*   **Параметры:**
    *   `id` : `string` - Уникальный и постоянный идентификатор серии данных для сериализации данных.

### Методы

#### `Clear()`

*   **Описание:**  Очищает серию данных.
*   **Синтаксис:** `override void Clear()`
*   **Виртуальный:** `virtual`

### Свойства

#### `Count`

*   **Описание:** Возвращает количество элементов в серии данных.
*   **Синтаксис:** `override int Count [get]`
*   **Только чтение:** `get`

#### `ScaleIt`

*   **Описание:**  Управляет масштабированием серии данных.
*   **Синтаксис:** `bool ScaleIt [get, set]`
*   **Чтение и запись:** `get`, `set`

#### `this[int index]`

*   **Описание:** Индексатор для доступа к элементу серии данных по индексу.
*   **Синтаксис:** `override object this[int index] [get, set]`
*   **Чтение и запись:** `get`, `set`

**Дополнительные унаследованные члены:**

*   **Защищенные члены-функции, унаследованные от** `ATAS.Indicators.BaseDataSeries< object >`
*   **События, унаследованные от** `ATAS.Indicators.BaseDataSeries< object >`
*   **Свойства, унаследованные от** `ATAS.Indicators.BaseDataSeries< object >`

**Подробное описание:**

Класс `ObjectDataSeries` предназначен для хранения последовательности объектов. Это позволяет хранить данные различных типов в одной серии данных.

**Файл документации:**

*   `ObjectDataSeries.cs`










## Методичка по классу `ATAS.Indicators.PaintbarsDataSeries` для создания индикаторов ATAS

### Описание класса `ATAS.Indicators.PaintbarsDataSeries`

**`ATAS.Indicators.PaintbarsDataSeries`** - это класс, представляющий собой серию данных для отображения баров (paintbars) на графике. Каждый элемент этой серии может иметь значение типа `Color?` (nullable Color), что позволяет задавать цвет для каждого бара.

**Наследование:**

```
BaseDataSeries<CrossColor?>
  ↑
ATAS.Indicators.PaintbarsDataSeries
```

*   `ATAS.Indicators.PaintbarsDataSeries` наследуется от `BaseDataSeries<CrossColor?>`.
*   Это означает, что `PaintbarsDataSeries` обладает всеми свойствами и методами `BaseDataSeries`, специализируясь на отображении цветных баров.

**Диаграмма сотрудничества:**

```
BaseDataSeries<CrossColor?>
  ↑
ATAS.Indicators.PaintbarsDataSeries
```

*   Диаграмма сотрудничества показывает взаимосвязь между классами, в данном случае, наследование.

**Подробное описание:**

*   Класс `PaintbarsDataSeries` предназначен для хранения и управления данными, необходимыми для отрисовки цветных баров на графике.
*   Каждый бар в серии может быть окрашен в определенный цвет, заданный значением `Color?`. `null` означает отсутствие цвета или стандартный цвет.

### Конструкторы класса `PaintbarsDataSeries`

**1. `PaintbarsDataSeries(string id, string name)` [1/2]**

```csharp
ATAS.Indicators.PaintbarsDataSeries.PaintbarsDataSeries(
    string id,
    string name
)
```

*   **Описание:** Инициализирует новый экземпляр класса `PaintbarsDataSeries` с указанным уникальным идентификатором (`id`) и именем (`name`).
*   **Параметры:**
    *   `id`: `string` - Уникальный и постоянный идентификатор серии данных для сериализации данных и уникальности.
    *   `name`: `string` - Имя серии данных.

**2. `PaintbarsDataSeries(string id)` [2/2]**

```csharp
ATAS.Indicators.PaintbarsDataSeries.PaintbarsDataSeries(
    string id
)
```

*   **Описание:** Инициализирует новый экземпляр класса `PaintbarsDataSeries` с указанным уникальным идентификатором (`id`). Имя серии данных будет задано по умолчанию.
*   **Параметры:**
    *   `id`: `string` - Уникальный и постоянный идентификатор серии данных для сериализации данных и уникальности.

### Методы класса `PaintbarsDataSeries`

**1. `Clear()`**

```csharp
override void Clear()
```

*   **Описание:**  Метод `Clear()` переопределяет метод базового класса.  Предположительно, он очищает серию данных `PaintbarsDataSeries`, удаляя все добавленные бары и их цвета.
*   **Виртуальный:**  `virtual` (указано в таблице Member Function Documentation, но не в сигнатуре кода) -  Вероятно, метод является виртуальным в базовом классе и переопределен в `PaintbarsDataSeries`.
*   **Реализован из:** `ATAS.Indicators.BaseDataSeries<CrossColor?>`. - Указывает, что этот метод переопределен из базового класса `BaseDataSeries<CrossColor?>`.

### Свойства класса `PaintbarsDataSeries`

**1. `Count`**

```csharp
override int Count { get; }
```

*   **Описание:** Свойство `Count` возвращает количество элементов (баров) в серии данных `PaintbarsDataSeries`.
*   **`get`:**  Только для чтения (`get`).  Вы можете получить значение свойства, но не можете его установить напрямую.

**2. `HideChart`**

```csharp
bool HideChart { get; set; }
```

*   **Описание:** Свойство `HideChart` определяет, должен ли основной график быть скрыт при отображении данной серии `PaintbarsDataSeries`.
*   **`get, set`:**  Для чтения и записи (`get, set`). Вы можете получить текущее значение и установить новое значение, чтобы скрыть или показать основной график.

**3. `IsVisible`**

```csharp
override bool IsVisible { get; set; }
```

*   **Описание:** Свойство `IsVisible` определяет, видима ли серия данных `PaintbarsDataSeries` на графике.
*   **`get, set`:**  Для чтения и записи (`get, set`). Вы можете получить текущую видимость и установить новое значение, чтобы показать или скрыть серию.
*   **Переопределено:** `override` -  Переопределяет свойство `IsVisible` из базового класса.

**4. `this[int index]`**

```csharp
override? CrossColor this[int index] { get; set; }
```

*   **Описание:** Индексатор `this[int index]` позволяет получать или устанавливать цвет бара по его индексу в серии данных.
*   **`int index`:**  Индекс бара в серии данных (начиная с 0).
*   **`CrossColor?`:**  Возвращает или устанавливает значение типа `CrossColor?`, представляющее цвет бара. `null` означает отсутствие цвета.
*   **`get, set`:**  Для чтения и записи (`get, set`). Вы можете получить цвет бара по индексу и установить новый цвет для бара по индексу.
*   **Переопределено:** `override` - Переопределяет индексатор из базового класса.

**5. `Visible`**

```csharp
bool Visible { get; set; }
```

*   **Описание:** Свойство `Visible` также управляет видимостью серии данных `PaintbarsDataSeries`.  Вероятно, является синонимом или альтернативным названием для `IsVisible`. (Нужно проверить документацию ATAS для уточнения различий, если они есть).
*   **`get, set`:**  Для чтения и записи (`get, set`).  Позволяет получить и установить видимость серии данных.

### Дополнительные унаследованные члены

*   **Унаследованные защищенные методы (`Protected Member Functions inherited from ATAS.Indicators.BaseDataSeries< CrossColor?>`)**:  `PaintbarsDataSeries` также наследует защищенные методы от базового класса `BaseDataSeries< CrossColor?>`.  Детали этих методов не указаны в данном документе.
*   **Унаследованные события (`Events inherited from ATAS.Indicators.BaseDataSeries< CrossColor?>`)**: `PaintbarsDataSeries` также наследует события от базового класса `BaseDataSeries< CrossColor?>`. Детали событий также не указаны.

### Файл документации

*   Информация о классе `ATAS.Indicators.PaintbarsDataSeries` была сгенерирована из файла `PaintbarsDataSeries.cs`.

**Конец методички.**








## Методичка по ATAS.Indicators.ParameterAttribute Class Reference

**ATAS.Indicators.ParameterAttribute Class Reference**

**Диаграмма наследования для ATAS.Indicators.ParameterAttribute:**

```
Utils::Common::Attributes
::ParameterAttribute
      ▲
      │
OFT::Attributes:: Parameter
Attribute
      ▲
      │
ATAS.Indicators. Parameter
Attribute
```
[legend]  <font color="gray">  // [legend] -  обозначение легенды диаграммы, в данном случае она не представлена </font>


**Диаграмма коллаборации для ATAS.Indicators.ParameterAttribute:**

```
Utils::Common::Attributes
::ParameterAttribute
      ▲
      │
OFT::Attributes::Parameter
Attribute
      ▲
      │
ATAS.Indicators. Parameter
Attribute
```
[legend]  <font color="gray">  // [legend] -  обозначение легенды диаграммы, в данном случае она не представлена </font>


**Документация для этого класса была сгенерирована из следующего файла:**

*   Indicators/Attributies/ParameterAttribute.cs   <font color="gray">  // Путь к файлу с исходным кодом класса </font>








```text
## ATAS.Indicators.PriceSelectionDataSeries Class Reference

**Описание:**
Представляет ряд данных значений выбора цены, где каждый элемент является синхронизированным списком `PriceSelectionValue`.

**Диаграмма наследования для ATAS.Indicators.PriceSelectionDataSeries:**

```
BaseDataSeries< SyncList
< PriceSelectionValue > >
    |
    └── ATAS.Indicators.PriceSelectionDataSeries
```

*Аннотация: Диаграмма наследования показывает, что `PriceSelectionDataSeries` является производным классом от `BaseDataSeries`. Это означает, что он наследует функциональность `BaseDataSeries` и расширяет ее.*

**Диаграмма взаимодействия для ATAS.Indicators.PriceSelectionDataSeries:**

```
BaseDataSeries< SyncList
< PriceSelectionValue > >
    ^
    |
    ATAS.Indicators.PriceSelectionDataSeries
```

*Аннотация: Диаграмма взаимодействия показывает, как `PriceSelectionDataSeries` взаимодействует с `BaseDataSeries`. В данном случае, стрелка указывает на то, что `PriceSelectionDataSeries` использует или зависит от `BaseDataSeries`.*

---

## Публичные члены-функции

### PriceSelectionDataSeries (string id, string name)

```csharp
PriceSelectionDataSeries (string id, string name)
```

**Описание:**
Инициализирует новый экземпляр класса `PriceSelectionDataSeries` с указанным уникальным и постоянным ID ряда данных для сериализации данных и уникальным именем.

**Параметры:**

*   `id`   `string` - Уникальный и постоянный ID ряда данных для сериализации данных.
*   `name` `string` - Уникальное имя ряда данных.

### PriceSelectionDataSeries (string id)

```csharp
PriceSelectionDataSeries (string id)
```

**Описание:**
Инициализирует новый экземпляр класса `PriceSelectionDataSeries` с указанным уникальным и постоянным ID ряда данных для сериализации данных.

**Параметры:**

*   `id`   `string` - Уникальный и постоянный ID ряда данных для сериализации данных.

### override void Clear ()

```csharp
override void Clear ()
```

**Описание:**
*(Нет описания в предоставленной документации)*

**Переопределено из:** `ATAS.Indicators.BaseDataSeries< SyncList< PriceSelectionValue > >`

*Аннотация: `override` означает, что этот метод `Clear` переопределяет метод с таким же именем, унаследованный от базового класса `BaseDataSeries`. Метод `Clear` вероятно предназначен для очистки данных ряда.*

### ► Публичные члены-функции, унаследованные от `ATAS.Indicators.BaseDataSeries< SyncList< PriceSelectionValue > >`

*Аннотация: Эта строка указывает на то, что класс `PriceSelectionDataSeries` также наследует публичные члены-функции от своего базового класса `BaseDataSeries`. Чтобы узнать о них, нужно смотреть документацию для `BaseDataSeries`.*

---

## Свойства

### override bool IsVisible  \[get]

```csharp
override bool IsVisible [get]
```

**Описание:**
*(Нет описания в предоставленной документации)*

*Аннотация: `[get]` означает, что это свойство доступно только для чтения. `override` указывает на переопределение свойства базового класса.*

### bool Visible \[get, set]

```csharp
bool Visible [get, set]
```

**Описание:**
Возвращает или устанавливает видимость ряда данных выбора цены.

*Аннотация: `[get, set]` означает, что это свойство доступно как для чтения, так и для записи, то есть можно узнать текущее значение видимости и изменить его.*

### override int Count \[get]

```csharp
override int Count [get]
```

**Описание:**
*(Нет описания в предоставленной документации)*

*Аннотация: `[get]` означает, что это свойство доступно только для чтения и возвращает целое число (`int`). `override` указывает на переопределение свойства базового класса.*

### override SyncList< PriceSelectionValue > this\[int index] \[get, set]

```csharp
override SyncList< PriceSelectionValue > this[int index] [get, set]
```

**Описание:**
*(Нет описания в предоставленной документации)*

*Аннотация: `this[int index]` - это индексатор, позволяющий получать доступ к элементам ряда данных по индексу (целому числу). `SyncList<PriceSelectionValue>` указывает, что возвращается синхронизированный список объектов типа `PriceSelectionValue`. `[get, set]` означает, что доступен как для чтения, так и для записи элемента по индексу. `override` указывает на переопределение индексатора базового класса.*

### ► Свойства, унаследованные от `ATAS.Indicators.BaseDataSeries< SyncList< PriceSelectionValue > >`

*Аннотация:  Эта строка указывает на то, что класс `PriceSelectionDataSeries` также наследует свойства от своего базового класса `BaseDataSeries`. Чтобы узнать о них, нужно смотреть документацию для `BaseDataSeries`.*

---

## Дополнительные унаследованные члены

### ► Защищенные члены-функции, унаследованные от `ATAS.Indicators.BaseDataSeries< SyncList< PriceSelectionValue > >`

*Аннотация:  Эта строка указывает на то, что класс `PriceSelectionDataSeries` также наследует защищенные члены-функции от своего базового класса `BaseDataSeries`.  Защищенные члены обычно доступны в самом классе и в его производных классах.*

### ► События, унаследованные от `ATAS.Indicators.BaseDataSeries< SyncList< PriceSelectionValue > >`

*Аннотация: Эта строка указывает на то, что класс `PriceSelectionDataSeries` также наследует события от своего базового класса `BaseDataSeries`. События используются для уведомления о происходящих действиях или изменениях в классе.*

---

## Подробное описание

Представляет ряд данных значений выбора цены, где каждый элемент является синхронизированным списком `PriceSelectionValue`.

*Аннотация: Это повторение основного описания класса, подчеркивающее его назначение.*

---

## Документация конструктора и деструктора

### ◆ PriceSelectionDataSeries() \[1/2]

```csharp
ATAS.Indicators.PriceSelectionDataSeries.PriceSelectionDataSeries (string id,
                                                                    string name
                                                                    )
```

**Описание:**
Инициализирует новый экземпляр класса `PriceSelectionDataSeries` с указанным уникальным и постоянным ID ряда данных для сериализации данных и уникальным именем.

**Параметры:**

*   `id`   `string` - Уникальный и постоянный ID ряда данных для сериализации данных.
*   `name` `string` - Уникальное имя ряда данных.

### ◆ PriceSelectionDataSeries() \[2/2]

```csharp
ATAS.Indicators.PriceSelectionDataSeries.PriceSelectionDataSeries (string id)
```

**Описание:**
Инициализирует новый экземпляр класса `PriceSelectionDataSeries` с указанным уникальным и постоянным ID ряда данных для сериализации данных.

**Параметры:**

*   `id`   `string` - Уникальный и постоянный ID ряда данных для сериализации данных.

*Аннотация: Раздел "Документация конструктора и деструктора" подробно описывает конструкторы класса. В данном случае, есть два конструктора: один принимает `id` и `name`, другой только `id`. Конструкторы используются для создания новых объектов класса.*

---

## Документация членов-функций

### ◆ Clear()

```csharp
override void ATAS.Indicators.PriceSelectionDataSeries.Clear ( )
```

**Описание:**
*(Нет описания в предоставленной документации)*

**Переопределено из:** `ATAS.Indicators.BaseDataSeries< SyncList< PriceSelectionValue > >`.

*Аннотация: Раздел "Документация членов-функций" описывает методы класса. Здесь описан метод `Clear()`, который переопределен от базового класса.*

---

## Документация свойств

### ◆ Count

```csharp
override int ATAS.Indicators.PriceSelectionDataSeries.Count
```
`get`

**Описание:**
*(Нет описания в предоставленной документации)*

*Аннотация: Раздел "Документация свойств" подробно описывает свойства класса. Здесь описано свойство `Count`, которое возвращает целое число (`int`) и доступно только для чтения (`get`). Вероятно, оно возвращает количество элементов в ряде данных.*

### ◆ IsVisible

```csharp
override bool ATAS.Indicators.PriceSelectionDataSeries.IsVisible
```
`get`

**Описание:**
*(Нет описания в предоставленной документации)*

*Аннотация: Свойство `IsVisible` возвращает логическое значение (`bool`) и доступно только для чтения (`get`). Вероятно, оно указывает, является ли ряд данных видимым.*

### ◆ this[int index]

```csharp
override SyncList<PriceSelectionValue> ATAS.Indicators.PriceSelectionDataSeries.this[int index]
```
`get`   `set`

**Описание:**
*(Нет описания в предоставленной документации)*

*Аннотация: Индексатор `this[int index]` позволяет получать доступ к элементам ряда данных по индексу. Он возвращает `SyncList<PriceSelectionValue>` и доступен как для чтения (`get`), так и для записи (`set`).*

### ◆ Visible

```csharp
bool ATAS.Indicators.PriceSelectionDataSeries.Visible
```
`get`   `set`

**Описание:**
Возвращает или устанавливает видимость ряда данных выбора цены.

*Аннотация: Свойство `Visible` возвращает логическое значение (`bool`) и доступно как для чтения (`get`), так и для записи (`set`). Оно управляет видимостью ряда данных.*

---

**Документация для этого класса была сгенерирована из следующего файла:**

*   PriceSelectionDataSeries.cs

*Аннотация: Указывает, в каком файле исходного кода находится определение этого класса.*
```







## Методичка по классу `ATAS.Indicators.PriceSelectionValue` для создания индикаторов ATAS

### 1. Обзор класса `ATAS.Indicators.PriceSelectionValue`

**Описание:** Класс `ATAS.Indicators.PriceSelectionValue` предназначен для определения выделения ценового уровня в кластерах и барах. Используется в `PriceSelectionDataSeries`.

**Назначение:** Позволяет создавать и настраивать визуальное выделение определенных ценовых уровней на графике.

**Использование:** Применяется совместно с `PriceSelectionDataSeries` для отображения выделенных ценовых уровней на графиках ATAS.

### 2. Конструктор `PriceSelectionValue(decimal price)`

**Описание:** Конструктор `PriceSelectionValue(decimal price)` создает экземпляр класса `PriceSelectionValue` с заданной ценой.

**Параметры:**

- `price` (decimal): Цена, которая будет установлена как минимальная и максимальная цена выделения.

**Функциональность:** Инициализирует объект `PriceSelectionValue`, устанавливая начальную цену для выделения. Эта цена используется как для нижней, так и для верхней границы выделения на момент создания.

### 3. Публичные методы (функции)

#### 3.1. `GetBorderPen()`

**Описание:** Метод `GetBorderPen()` возвращает `RenderPen`, используемый для отрисовки границы графических объектов выделения.

**Возвращаемое значение:** `RenderPen` - объект пера для отрисовки границы.

**Функциональность:** Позволяет получить текущее перо, используемое для рисования контура выделения цены. Это перо можно настроить для изменения стиля линии границы.

#### 3.2. `GetObjectsColor()`

**Описание:** Метод `GetObjectsColor()` возвращает `System.Drawing.Color`, представляющий текущий цвет графических объектов выделения.

**Возвращаемое значение:** `System.Drawing.Color` - цвет графических объектов.

**Функциональность:** Предоставляет доступ к текущему цвету заливки выделения цены. Цвет можно изменить для визуальной кастомизации индикатора.

#### 3.3. `GetPriceSelectionColor()`

**Описание:** Метод `GetPriceSelectionColor()` возвращает `System.Drawing.Color`, представляющий текущий цвет выделения цены.

**Возвращаемое значение:** `System.Drawing.Color` - цвет выделения цены.

**Функциональность:** Аналогично `GetObjectsColor()`, но может быть специфичен для цвета именно выделения цены, если есть различие между цветом объекта и цветом выделения.

### 4. Свойства

#### 4.1. `Context`

**Описание:** Свойство `Context` типа `object` позволяет получить или установить коэффициент уменьшения высоты выделения цены.

**Тип:** `object`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущий контекстный объект, связанный с выделением цены.
- **set:** Устанавливает контекстный объект.

**Коэффициент уменьшения высоты:** Если значение контекста равно 100, выделяется вся высота бара. Если 50, выделяется половина высоты. Используется для динамического изменения высоты выделения в зависимости от контекста.

#### 4.2. `DrawValue`

**Описание:** Свойство `DrawValue` типа `bool` определяет, нужно ли отображать значение цены внутри объекта выделения.

**Тип:** `bool`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает `true`, если значение цены отображается внутри выделения, и `false` в противном случае.
- **set:** Устанавливает, нужно ли отображать значение цены.

**Отображение значения:** Управляет видимостью текстового значения цены внутри выделенной области.

#### 4.3. `HeightFactor` (Устаревшее)

**Описание:** Свойство `HeightFactor` типа `decimal` - устаревшее свойство, ранее отвечавшее за фактор высоты.

**Тип:** `decimal`

**Доступ:** `get`, `set`

**Статус:** Устаревшее (obsolete).

**Функциональность:** В текущей версии не рекомендуется к использованию. Возможно, ранее использовалось для управления высотой выделения.

#### 4.4. `MaximumPrice`

**Описание:** Свойство `MaximumPrice` типа `decimal` позволяет получить или установить максимальную цену выделения.

**Тип:** `decimal`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущую максимальную цену выделения.
- **set:** Устанавливает максимальную цену выделения.

**Верхняя граница выделения:** Определяет верхнюю границу ценового диапазона, который будет выделен.

#### 4.5. `MinimumPrice`

**Описание:** Свойство `MinimumPrice` типа `decimal` позволяет получить или установить минимальную цену выделения.

**Тип:** `decimal`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущую минимальную цену выделения.
- **set:** Устанавливает минимальную цену выделения.

**Нижняя граница выделения:** Определяет нижнюю границу ценового диапазона, который будет выделен.

#### 4.6. `ObjectColor`

**Описание:** Свойство `ObjectColor` типа `CrossColor` позволяет получить или установить цвет графических объектов выделения.

**Тип:** `CrossColor`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущий цвет графических объектов.
- **set:** Устанавливает цвет графических объектов.

**Цвет заливки:** Управляет цветом заливки выделенной области.

#### 4.7. `ObjectsTransparency`

**Описание:** Свойство `ObjectsTransparency` типа `int` позволяет получить или установить прозрачность заливки объектов выделения.

**Тип:** `int`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущий уровень прозрачности объектов.
- **set:** Устанавливает уровень прозрачности объектов.

**Прозрачность:** Значение прозрачности для заливки выделения. Значения варьируются от 0 (полностью прозрачный) до 255 (полностью непрозрачный).

#### 4.8. `PriceSelectionColor`

**Описание:** Свойство `PriceSelectionColor` типа `CrossColor` позволяет получить или установить цвет выделения цены. Поддерживает альфа-канал для прозрачности.

**Тип:** `CrossColor`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущий цвет выделения цены.
- **set:** Устанавливает цвет выделения цены.

**Цвет выделения с прозрачностью:** Позволяет задать цвет выделения и его прозрачность, используя альфа-канал.

#### 4.9. `RenderValue`

**Описание:** Свойство `RenderValue` типа `decimal?` позволяет получить или установить значение цены для отображения внутри объекта.

**Тип:** `decimal?` (Nullable Decimal)

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущее значение цены, отображаемое внутри объекта. Может быть `null`, если значение не задано.
- **set:** Устанавливает значение цены для отображения.

**Отображаемое значение цены:** Позволяет задать конкретное значение цены, которое будет показано внутри выделения, если `DrawValue` установлен в `true`.

#### 4.10. `SelectionSide`

**Описание:** Свойство `SelectionSide` типа `SelectionType` позволяет получить или установить тип выделения.

**Тип:** `SelectionType`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущий тип выделения.
- **set:** Устанавливает тип выделения.

**Тип выделения:** Определяет, как именно будет визуально выделена цена (например, прямоугольник, линия и т.д.). Тип `SelectionType` - это перечисление, определяющее возможные варианты.

#### 4.11. `Size`

**Описание:** Свойство `Size` типа `int` позволяет получить или установить размер графических объектов выделения.

**Тип:** `int`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущий размер графических объектов.
- **set:** Устанавливает размер графических объектов.

**Размер объекта:** Управляет размером визуального элемента выделения (например, толщина линии, высота прямоугольника).

#### 4.12. `Tooltip`

**Описание:** Свойство `Tooltip` типа `string` позволяет получить или установить текст всплывающей подсказки, связанной с выделением.

**Тип:** `string`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущий текст всплывающей подсказки.
- **set:** Устанавливает текст всплывающей подсказки.

**Всплывающая подсказка:** Задает текст, который будет отображаться при наведении курсора мыши на выделение цены.

#### 4.13. `VisualObject`

**Описание:** Свойство `VisualObject` типа `ObjectType` позволяет получить или установить тип визуального объекта выделения.

**Тип:** `ObjectType`

**Доступ:** `get`, `set`

**Функциональность:**
- **get:** Возвращает текущий тип визуального объекта.
- **set:** Устанавливает тип визуального объекта.

**Тип визуального объекта:** Определяет, какой графический примитив будет использоваться для отображения выделения (например, линия, прямоугольник, эллипс). Тип `ObjectType` - это перечисление, определяющее возможные варианты.

### 5. Подробное описание

Класс `ATAS.Indicators.PriceSelectionValue` является ключевым элементом для визуализации ценовых уровней в индикаторах ATAS. Он предоставляет гибкие настройки для внешнего вида и поведения выделений, позволяя разработчикам создавать информативные и настраиваемые индикаторы.

### 6. Документация конструктора и деструктора

**Конструктор:** `PriceSelectionValue()` -  уже описан в разделе 2.

**Деструктор:** Явно не указан в документации, деструкция объектов в C# происходит автоматически сборщиком мусора.

### 7. Исходный файл

Документация для класса `ATAS.Indicators.PriceSelectionValue` была сгенерирована из файла: `PriceSelectionValue.cs`.








## Методичка по классу `ATAS.Indicators.PriceVolumeInfo` для создания индикаторов ATAS

### Описание класса

Класс `ATAS.Indicators.PriceVolumeInfo` представляет информацию об объемах на определенной цене.

### Свойства класса

Класс `ATAS.Indicators.PriceVolumeInfo` имеет следующие свойства:

1.  **Volume**

    *   **Тип данных:** `decimal`
    *   **Полное имя свойства:** `ATAS.Indicators.PriceVolumeInfo.Volume`
    *   **Описание:** Получает или устанавливает данные об общем объеме (`Gets or sets the volume data`).
    *   **Доступ:** `[get, set]` - доступно для чтения и записи.

2.  **Bid**

    *   **Тип данных:** `decimal`
    *   **Полное имя свойства:** `ATAS.Indicators.PriceVolumeInfo.Bid`
    *   **Описание:** Получает или устанавливает данные об объеме Bid (`Gets or sets the bid data`).
    *   **Доступ:** `[get, set]` - доступно для чтения и записи.

3.  **Ask**

    *   **Тип данных:** `decimal`
    *   **Полное имя свойства:** `ATAS.Indicators.PriceVolumeInfo.Ask`
    *   **Описание:** Получает или устанавливает данные об объеме Ask (`Gets or sets the ask data`).
    *   **Доступ:** `[get, set]` - доступно для чтения и записи.

4.  **Ticks**

    *   **Тип данных:** `int`
    *   **Полное имя свойства:** `ATAS.Indicators.PriceVolumeInfo.Ticks`
    *   **Описание:** Получает или устанавливает данные о количестве тиков (`Gets or sets the tick count data`).
    *   **Доступ:** `[get, set]` - доступно для чтения и записи.

5.  **Between**

    *   **Тип данных:** `decimal`
    *   **Полное имя свойства:** `ATAS.Indicators.PriceVolumeInfo.Between`
    *   **Описание:** Получает или устанавливает данные об объеме между Bid и Ask (`Gets or sets the volume data between bids and asks`).
    *   **Доступ:** `[get, set]` - доступно для чтения и записи.

6.  **Time**

    *   **Тип данных:** `int`
    *   **Полное имя свойства:** `ATAS.Indicators.PriceVolumeInfo.Time`
    *   **Описание:** Получает или устанавливает данные о времени на текущем ценовом уровне (`Gets or sets the time data at the current level`).
    *   **Доступ:** `[get, set]` - доступно для чтения и записи.

7.  **Price**

    *   **Тип данных:** `decimal`
    *   **Полное имя свойства:** `ATAS.Indicators.PriceVolumeInfo.Price`
    *   **Описание:** Получает или устанавливает данные о цене (`Gets or sets the price data`).
    *   **Доступ:** `[get, set]` - доступно для чтения и записи.

### Детальное описание

Класс `ATAS.Indicators.PriceVolumeInfo` предназначен для работы с информацией об объемах, привязанных к определенной цене.  Это позволяет получать доступ к различным аспектам объема, таким как общий объем, объемы Bid и Ask, количество тиков, объем между Bid и Ask, время и цена, на которой эти объемы были зафиксированы.

### Документация свойств

Вся документация для этого класса была сгенерирована из файла `PriceVolumeInfo.cs`.

**Аннотации для пользователя:**

*   **Класс `ATAS.Indicators.PriceVolumeInfo`**: Это как шаблон или чертеж, который описывает, как работать с информацией об объемах на разных ценах в ATAS.
*   **Свойства**: Это как характеристики или детали внутри этого шаблона. Каждое свойство хранит определенный вид информации об объемах.
*   **`decimal` и `int`**: Это типы данных. `decimal` используется для чисел с десятичной точкой (например, объемы), а `int` для целых чисел (например, количество тиков).
*   **`[get, set]`**: Это означает, что вы можете как получить (узнать значение) свойства, так и установить (изменить значение) свойства. В контексте индикаторов, обычно вы будете *получать* значения свойств, чтобы использовать их в своих расчетах.
*   **Описание**:  Для каждого свойства дано краткое описание, что именно оно представляет. Например, `Volume` - это общий объем, `Bid` - объем на стороне Bid и так далее.
*   **Полное имя свойства**: Это как полный адрес свойства в системе ATAS. Когда вы будете писать код индикатора, вам нужно будет использовать эти полные имена, чтобы правильно обращаться к свойствам класса `PriceVolumeInfo`.

Эта методичка предоставляет всю необходимую информацию о классе `ATAS.Indicators.PriceVolumeInfo` и его свойствах для создания индикаторов ATAS, работающих с объемными данными на ценовых уровнях.








## Методичка по классу `ATAS.Indicators.RangeDataSeries` для создания индикаторов ATAS

### Обзор класса `ATAS.Indicators.RangeDataSeries`

Класс `ATAS.Indicators.RangeDataSeries` представляет собой серию данных диапазонов значений, где каждый элемент является `RangeValue`.

**Диаграмма наследования:**

```
BaseDataSeries< RangeValue >
    |
    ATAS.Indicators.RangeDataSeries
```

**Диаграмма взаимодействия:**

```
BaseDataSeries< RangeValue >
    |
    ATAS.Indicators.RangeDataSeries
```

### Публичные члены класса

#### Конструкторы

*   **`RangeDataSeries(string id, string name)`**
    *   Инициализирует новый экземпляр класса `RangeDataSeries` с указанным уникальным и постоянным идентификатором серии данных для сериализации данных и уникальным именем.
    *   **Параметры:**
        *   `id`: `string` - Уникальный и постоянный идентификатор серии данных для сериализации данных.
        *   `name`: `string` - Уникальное имя серии данных.

*   **`RangeDataSeries(string id)`**
    *   Инициализирует новый экземпляр класса `RangeDataSeries` с указанным уникальным и постоянным идентификатором серии данных для сериализации данных.
    *   **Параметры:**
        *   `id`: `string` - Уникальный и постоянный идентификатор серии данных для сериализации данных.

#### Методы

*   **`override void Clear()`**
    *   Очищает данные серии.
    *   Переопределен из: `ATAS.Indicators.BaseDataSeries< RangeValue >`.

#### Свойства

*   **`override int Count { get; }`**
    *   Возвращает количество элементов в серии данных.
    *   Переопределено из: `ATAS.Indicators.BaseDataSeries< RangeValue >`.

*   **`override bool IsVisible { get; set; }`**
    *   Получает или устанавливает видимость серии данных.
    *   Переопределено из: `ATAS.Indicators.BaseDataSeries< RangeValue >`.

*   **`CrossColor { get; set; }`**
    *   `System.Drawing.Color ATAS.Indicators.RangeDataSeries.CrossColor`
    *   Получает или устанавливает цвет перекрестия для серии данных диапазона.

*   **`RangeColor { get; set; }`**
    *   `System.Drawing.Color ATAS.Indicators.RangeDataSeries.RangeColor`
    *   Получает или устанавливает цвет диапазона для серии данных.

*   **`RenderColor { get; set; }`**
    *   `System.Drawing.Color ATAS.Indicators.RangeDataSeries.RenderColor`
    *   Получает или устанавливает цвет отрисовки серии данных.

*   **`ScaleIt { get; set; }`**
    *   `bool ATAS.Indicators.RangeDataSeries.ScaleIt`
    *   Получает или устанавливает значение, определяющее, масштабировать ли серию данных на графике.

*   **`this[int index] { get; set; }`**
    *   `override RangeValue ATAS.Indicators.RangeDataSeries.this[int index]`
    *   Индексатор для доступа к элементам серии данных по индексу.
    *   Переопределено из: `ATAS.Indicators.BaseDataSeries< RangeValue >`.

*   **`Visible { get; set; }`**
    *   `bool ATAS.Indicators.RangeDataSeries.Visible`
    *   Получает или устанавливает видимость серии данных.

### Дополнительные унаследованные члены

*   **Защищенные члены-функции, унаследованные от `ATAS.Indicators.BaseDataSeries< RangeValue >`**
*   **События, унаследованные от `ATAS.Indicators.BaseDataSeries< RangeValue >`**

**(Примечание: Для полного списка унаследованных членов обратитесь к документации класса `ATAS.Indicators.BaseDataSeries< RangeValue >`.)**

### Подробное описание

Класс `ATAS.Indicators.RangeDataSeries` предназначен для представления набора данных, где каждое значение является диапазоном (`RangeValue`). Это полезно для визуализации данных, которые имеют верхнюю и нижнюю границы, а не только одно значение.

### Документация по конструкторам и деструкторам

*   **`RangeDataSeries()` [1/2] и [2/2]**
    *   Описывают различные способы создания экземпляра класса `RangeDataSeries`, с идентификатором и именем или только с идентификатором.

**Файл, из которого сгенерирована документация для этого класса:**

*   `RangeDataSeries.cs`










## Методичка по классу `ATAS.Indicators.RangeValue`

### Заголовок

**`ATAS.Indicators.RangeValue Class Reference`**

*Описание:* `RangeDataSeries` элемент.  [аннотация: Указывает, что класс RangeValue является элементом RangeDataSeries, но подробности о RangeDataSeries в данном документе не приводятся.]

### Свойства

#### `Upper`

*   **Тип данных:** `decimal` [аннотация: Тип данных `decimal` используется для хранения десятичных чисел с высокой точностью, что важно для финансовых расчетов и цен.]
*   **Атрибуты доступа:** `[get, set]` [аннотация: `get` означает, что значение свойства можно получить (прочитать), `set` означает, что значение свойства можно установить (записать).]
*   **Описание:** `Upper border of diapason.` [аннотация: Описывает свойство `Upper` как верхнюю границу диапазона.]

#### `Lower`

*   **Тип данных:** `decimal` [аннотация: Как и `Upper`, свойство `Lower` также использует тип `decimal` для точности представления числовых значений.]
*   **Атрибуты доступа:** `[get, set]` [аннотация: Аналогично `Upper`, `Lower` также имеет атрибуты `get` и `set`, что позволяет читать и записывать значение свойства.]
*   **Описание:** `Lower border of diapason.` [аннотация: Описывает свойство `Lower` как нижнюю границу диапазона.]

### Детальное описание

`RangeDataSeries` элемент. [аннотация: Повторение описания из заголовка, подчеркивающее связь с `RangeDataSeries`.]

### Документация свойств

#### `Lower`

*   **Тип:** `decimal ATAS.Indicators.RangeValue.Lower` [аннотация: Полное указание типа свойства, включая пространство имен и класс.]
*   **Атрибуты доступа:** `get` `set` [аннотация: Повторное указание атрибутов доступа для свойства `Lower`.]
*   **Описание:** `Lower border of diapason.` [аннотация: Описание свойства `Lower` повторяется для подробности.]

#### `Upper`

*   **Тип:** `decimal ATAS.Indicators.RangeValue.Upper` [аннотация: Полное указание типа свойства `Upper`.]
*   **Атрибуты доступа:** `get` `set` [аннотация: Повторение атрибутов доступа для свойства `Upper`.]
*   **Описание:** `Upper border of diapason.` [аннотация: Описание свойства `Upper` повторяется для подробности.]

### Файл документации

*   `RangeValue.cs` [аннотация: Указывает имя файла, в котором определен класс `RangeValue`.]









## Методичка по классу `ATAS.Indicators.RedrawArg` для создания индикаторов ATAS

### Описание класса `ATAS.Indicators.RedrawArg`

Класс `ATAS.Indicators.RedrawArg` представляет собой аргументы для запроса перерисовки графика.  Он используется для управления областью и условиями перерисовки графиков в платформе ATAS.

### Конструктор класса `RedrawArg`

**`RedrawArg(Rectangle redrawRegion)`**

*   **Назначение:** Инициализирует новый экземпляр класса `RedrawArg` с указанной областью перерисовки.
*   **Параметры:**
    *   `redrawRegion` ([`Rectangle`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.rectangle)):  Область графика, которую необходимо перерисовать. Тип данных `Rectangle` представляет прямоугольную область, определяемую координатами левого верхнего угла, шириной и высотой.

### Свойства класса `RedrawArg`

#### `RedrawRegion`

*   **Тип данных:** [`Rectangle`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.rectangle)
*   **Доступ:** `[get, set]` (доступно для чтения и записи)
*   **Назначение:**  Получает или устанавливает область для перерисовки на графике.  Это свойство позволяет определить, какая часть графика должна быть обновлена при перерисовке.

#### `ForceRedraw`

*   **Тип данных:** `bool` (логическое значение: `true` или `false`)
*   **Доступ:** `[get, set]` (доступно для чтения и записи)
*   **Назначение:**  Получает или устанавливает значение, указывающее, следует ли перерисовывать график с настройками кадров в секунду (FPS), заданными пользователем.
    *   `true`:  График будет перерисован принудительно, используя пользовательские настройки FPS.
    *   `false`: График будет перерисован стандартным способом.
    *   **Важно:**  Использовать `ForceRedraw = true` следует только в случае крайней необходимости, так как это может привести к проблемам с производительностью, особенно при высокой частоте кадров. Рекомендуется использовать значение `true` только если это действительно необходимо для корректной работы индикатора или визуализации данных.

---
*Файл документации: `RedrawArg.cs`*







## Методичка для создания индикаторов ATAS на основе класса `ATAS.Indicators.ValueArea`

### 1. Определение класса

**Название класса:** `ValueArea`

**Пространство имен:** `ATAS.Indicators`

**Описание:** Представляет информацию о верхней и нижней границах зоны стоимости (Value Area High/Low).

### 2. Свойства класса

#### 2.1. Свойство `ValueAreaHigh`

**Тип данных:** `decimal`

**Полное имя типа данных:** `decimal`

**Пространство имен типа данных:** `System` (не указано явно, но подразумевается для `decimal` в C#)

**Доступ:** `[get, set]`

*   `get`:  Позволяет получить значение свойства.
*   `set`:  Позволяет установить значение свойства.

**Описание свойства:** Верхняя граница зоны стоимости (Value area high).

#### 2.2. Свойство `ValueAreaLow`

**Тип данных:** `decimal`

**Полное имя типа данных:** `decimal`

**Пространство имен типа данных:** `System` (не указано явно, но подразумевается для `decimal` в C#)

**Доступ:** `[get, set]`

*   `get`:  Позволяет получить значение свойства.
*   `set`:  Позволяет установить значение свойства.

**Описание свойства:** Нижняя граница зоны стоимости (Value area low).

### 3. Подробное описание класса

Класс `ATAS.Indicators.ValueArea` предназначен для хранения и предоставления данных о границах зоны стоимости. Зона стоимости является важным понятием в Volume Analysis и Market Profile, обозначая диапазон цен, в котором был проторгован определенный процент от общего объема за торговый период.  Этот класс предоставляет доступ к верхней (`ValueAreaHigh`) и нижней (`ValueAreaLow`) границам этой зоны.

### 4. Расположение документации класса

Документация для данного класса `ATAS.Indicators.ValueArea` была сгенерирована из файла:

*   `PriceVolumeInfo.cs`









## Методичка по классу `ValueChangingEventArgs<TValue>` для ATAS Indicators

### Пространство имен: `ATAS.Indicators`

### Описание класса `ValueChangingEventArgs<TValue>`

**`ATAS.Indicators.ValueChangingEventArgs<TValue>`** -  ссылочный шаблон класса.

Предоставляет аргументы событий для события изменения значения.

**Подробнее...** *(отсылка к более детальному описанию, если такое есть в оригинальной документации)*

### Открытые члены класса (Public Member Functions)

#### `ValueChangingEventArgs(TValue oldValue, TValue newValue)`

* **Описание:** Инициализирует новый экземпляр класса `ValueChangingEventArgs<TValue>` с указанными старым и новым значениями.

* **Параметры:**
    * `TValue oldValue`: Старое значение *до* изменения.
    * `TValue newValue`: Новое значение *после* изменения.

### Свойства (Properties)

#### `OldValue`  [get]

* **Тип:** `TValue`
* **Описание:** Возвращает старое значение *до* изменения.

#### `NewValue`  [get]

* **Тип:** `TValue`
* **Описание:** Возвращает новое значение *после* изменения.

### Подробное описание

Предоставляет аргументы событий для события изменения значения.

#### Параметры шаблона (Template Parameters)

* `TValue`: Тип значения.

### Документация конструктора и деструктора

#### `ValueChangingEventArgs()`

* **Описание:** *Конструктор класса `ValueChangingEventArgs`. (Если в оригинале есть дополнительная информация о конструкторе, ее нужно добавить здесь)*

---
**Файл, в котором сгенерирована документация:**

* `ValueChangingEventArgs.cs`







## Методичка по классу `ATAS.Indicators.ValueDataSeries`

**Описание класса**

`ATAS.Indicators.ValueDataSeries` представляет собой ряд данных десятичных значений, где каждый элемент является десятичным числом.

**Диаграмма наследования**

```
BaseDataSeries< decimal >
    |
    ATAS.Indicators.ValueDataSeries
```

**Диаграмма взаимодействия**

```
BaseDataSeries< decimal >
    |
    ATAS.Indicators.ValueDataSeries
```

**Классы**

*   `BarColors` - Управляет цветами баров для связанного `ValueDataSeries`.

**Открытые члены-функции**

*   `ValueDataSeries(string id, string name)`
    *   Инициализирует новый экземпляр класса `ValueDataSeries` с указанным уникальным и постоянным идентификатором ряда данных для сериализации данных и уникальным именем.
    *   **Параметры:**
        *   `id` - Уникальный и постоянный идентификатор ряда данных для сериализации.
        *   `name` - Уникальное имя ряда данных.

*   `ValueDataSeries(string id)`
    *   Инициализирует новый экземпляр класса `ValueDataSeries` с указанным уникальным и постоянным идентификатором ряда данных для сериализации данных.
    *   **Параметры:**
        *   `id` - Уникальный и постоянный идентификатор ряда данных для сериализации.

*   `void SetPointOfEndLine(int bar)`
    *   Устанавливает указанный индекс бара в качестве точки конца для линии в ряде данных значений.
    *   **Параметры:**
        *   `bar` - Индекс бара для установки в качестве точки конца.

*   `void RemovePointOfEndLine(int bar)`
    *   Удаляет указанный индекс бара как точку конца для линии в ряде данных значений.
    *   **Параметры:**
        *   `bar` - Индекс бара для удаления в качестве точки конца.

*   `bool IsThisPointOfStartBar(int bar)`
    *   Проверяет, является ли указанный индекс бара точкой начала для линии в ряде данных значений.
    *   **Параметры:**
        *   `bar` - Индекс бара для проверки.
    *   **Возвращает:**
        *   `true`, если указанный индекс бара является точкой начала для линии; иначе, `false`.

*   `override void Clear()`
    *   Очищает ряд данных значений.
    *   *Реализовано повторно из `ATAS.Indicators.BaseDataSeries< decimal >`.*

*   `decimal LastNonZeroValue(int bar)`
    *   Получает последнее ненулевое значение в диапазоне от 0 до указанного бара.
    *   **Параметры:**
        *   `bar` - Индекс последнего бара диапазона.
    *   **Возвращает:**
        *   Последнее ненулевое значение в указанном диапазоне баров. Если ненулевое значение не найдено, возвращается значение на начальном индексе бара.

**Открытые члены-функции, унаследованные от `ATAS.Indicators.BaseDataSeries< decimal >`**

*   *(Список унаследованных функций не приведен в предоставленной документации.)*

**Свойства**

*   `decimal ZeroValue { get; set; }`
    *   Значение нулевой линии для режима 'Гистограмма'.

*   `BarColors Colors { get; }`
    *   Позволяет изменять цвет для каждого бара. Значение цвета будет использоваться для всех баров по умолчанию.

*   `System.Drawing.Color RenderColor { get; set; }`
    *   Получает или задает цвет отрисовки ряда данных значений.

*   `int Digits { get; set; }`
    *   Получает или задает количество цифр после десятичной точки для форматирования ряда данных значений.

*   `string StringFormat { get; set; }`
    *   Получает или задает строку формата цены для форматирования ряда данных значений.

*   `bool ShowOnlyNonZeroLabels { get; set; }`
    *   Всегда отображает последнее ненулевое значение на ценовой оси.

*   `VisualMode VisualType { get; set; }`
    *   Получает или задает режим визуализации для рисования ряда данных значений.

*   `CrossColor Color { get; set; }`
    *   Получает или задает цвет для рисования ряда данных значений.

*   `System.Drawing.Color ValuesColor { get; set; }`
    *   Получает или задает цвет текста значений для ряда данных значений.

*   `int Width { get; set; }`
    *   Получает или задает ширину для рисования ряда данных значений.

*   `LineDashStyle LineDashStyle { get; set; }`
    *   Получает или задает стиль пунктирной линии для рисования ряда данных значений.

*   `bool ShowZeroValue { get; set; }`
    *   Получает или задает, следует ли показывать нулевое значение на ценовой оси для ряда данных значений.

*   `bool ShowCurrentValue { get; set; }`
    *   Получает или задает, следует ли показывать текущее значение на ценовой панели для ряда данных значений.

*   `bool ScaleIt { get; set; }`
    *   Получает или задает, следует ли использовать масштабирование для ряда данных значений.

*   `override int Count { get; }`
    *   Возвращает количество элементов в ряде данных значений.

*   `override bool IsVisible { get; }`
    *   Получает или задает видимость ряда данных значений.

*   `override decimal this[int index] { get; set; }`
    *   Индексатор для доступа к значениям ряда данных значений по индексу.

**Свойства, унаследованные от `ATAS.Indicators.BaseDataSeries< decimal >`**

*   *(Список унаследованных свойств не приведен в предоставленной документации.)*

**Дополнительные унаследованные члены**

*   **Защищенные члены-функции, унаследованные от `ATAS.Indicators.BaseDataSeries< decimal >`**
    *   *(Список унаследованных защищенных функций не приведен в предоставленной документации.)*
*   **События, унаследованные от `ATAS.Indicators.BaseDataSeries< decimal >`**
    *   *(Список унаследованных событий не приведен в предоставленной документации.)*

**Подробное описание**

Представляет ряд данных десятичных значений, где каждый элемент является десятичным числом.

**Документация конструктора и деструктора**

*   `ValueDataSeries() [1/2]`
    *   `ATAS.Indicators.ValueDataSeries.ValueDataSeries(string id, string name)`
        *   Инициализирует новый экземпляр класса `ValueDataSeries` с указанным уникальным и постоянным идентификатором ряда данных для сериализации данных и уникальным именем.
        *   **Параметры:**
            *   `id` - Уникальный и постоянный идентификатор ряда данных для сериализации.
            *   `name` - Уникальное имя ряда данных.

*   `ValueDataSeries() [2/2]`
    *   `ATAS.Indicators.ValueDataSeries.ValueDataSeries(string id)`
        *   Инициализирует новый экземпляр класса `ValueDataSeries` с указанным уникальным и постоянным идентификатором ряда данных для сериализации данных.
        *   **Параметры:**
            *   `id` - Уникальный и постоянный идентификатор ряда данных для сериализации.

**Документация свойств**

*   `Color`
    *   `CrossColor ATAS.Indicators.ValueDataSeries.Color { get; set; }`
        *   Получает или задает цвет для рисования ряда данных значений.

*   `Colors`
    *   `BarColors ATAS.Indicators.ValueDataSeries.Colors { get; }`
        *   Позволяет изменять цвет для каждого бара. Значение цвета будет использоваться для всех баров по умолчанию.

*   `Count`
    *   `override int ATAS.Indicators.ValueDataSeries.Count { get; }`
        *   Возвращает количество элементов в ряде данных значений.

*   `Digits`
    *   `int ATAS.Indicators.ValueDataSeries.Digits { get; set; }`
        *   Получает или задает количество цифр после десятичной точки для форматирования ряда данных значений.

*   `IsVisible`
    *   `override bool ATAS.Indicators.ValueDataSeries.IsVisible { get; }`
        *   Получает или задает видимость ряда данных значений.

*   `LineDashStyle`
    *   `LineDashStyle ATAS.Indicators.ValueDataSeries.LineDashStyle { get; set; }`
        *   Получает или задает стиль пунктирной линии для рисования ряда данных значений.

*   `RenderColor`
    *   `System.Drawing.Color ATAS.Indicators.ValueDataSeries.RenderColor { get; set; }`
        *   Получает или задает цвет отрисовки ряда данных значений.

*   `ScaleIt`
    *   `bool ATAS.Indicators.ValueDataSeries.ScaleIt { get; set; }`
        *   Получает или задает, следует ли использовать масштабирование для ряда данных значений.

*   `ShowCurrentValue`
    *   `bool ATAS.Indicators.ValueDataSeries.ShowCurrentValue { get; set; }`
        *   Получает или задает, следует ли показывать текущее значение на ценовой панели для ряда данных значений.

*   `ShowOnlyNonZeroLabels`
    *   `bool ATAS.Indicators.ValueDataSeries.ShowOnlyNonZeroLabels { get; set; }`
        *   Всегда отображает последнее ненулевое значение на ценовой оси.

*   `ShowZeroValue`
    *   `bool ATAS.Indicators.ValueDataSeries.ShowZeroValue { get; set; }`
        *   Получает или задает, следует ли показывать нулевое значение на ценовой оси для ряда данных значений.

*   `StringFormat`
    *   `string ATAS.Indicators.ValueDataSeries.StringFormat { get; set; }`
        *   Получает или задает строку формата цены для форматирования ряда данных значений.

*   `this[int index]`
    *   `override decimal ATAS.Indicators.ValueDataSeries.this[int index] { get; set; }`
        *   Индексатор для доступа к значениям ряда данных значений по индексу.

*   `ValuesColor`
    *   `System.Drawing.Color ATAS.Indicators.ValueDataSeries.ValuesColor { get; set; }`
        *   Получает или задает цвет текста значений для ряда данных значений.

*   `VisualType`
    *   `VisualMode ATAS.Indicators.ValueDataSeries.VisualType { get; set; }`
        *   Получает или задает режим визуализации для рисования ряда данных значений.

*   `Width`
    *   `int ATAS.Indicators.ValueDataSeries.Width { get; set; }`
        *   Получает или задает ширину для рисования ряда данных значений.

*   `ZeroValue`
    *   `decimal ATAS.Indicators.ValueDataSeries.ZeroValue { get; set; }`
        *   Значение нулевой линии для режима 'Гистограмма'.

**Файл документации для класса:**

*   `ValueDataSeries.cs`





## Методичка для создания индикаторов ATAS на основе PDF

### Индикатор "Volume on the chart"

**Описание из PDF:**

Индикатор, унаследованный от стандартного индикатора Volume. Метод `OnCalculate` был переопределен и оставлен пустым (чтобы избежать ненужных вычислений). Логика рендеринга описана в методе `OnRender`. Цвета берутся из базового индикатора `DataSeries`.

**Код индикатора:**

```csharp
using ATAS.Indicators;

[DisplayName("Volume on the chart")] // Атрибут для отображения имени индикатора в платформе ATAS
public class VolumeOnChart : Volume // Объявление класса VolumeOnChart, наследующегося от класса Volume
{
    private decimal _height = 15; // Приватное поле для хранения высоты объема, по умолчанию 15

    public decimal Height // Публичное свойство Height для доступа и изменения высоты объема
    {
        get { return _height; } // Геттер возвращает текущее значение _height
        set                 // Сеттер для установки значения _height
        {
            if (value < 10 || value > 100) // Проверка, чтобы значение высоты было в диапазоне от 10 до 100
            return;             // Если значение вне диапазона, выход из сеттера без изменения _height

            _height = value;    // Установка нового значения _height, если оно в допустимом диапазоне
            return;             // Выход из сеттера
        }
    }

    public VolumeOnChart() // Конструктор класса VolumeOnChart
    {
        EnableCustomDrawing = true; // Разрешение пользовательской отрисовки индикатора
        SubscribeToDrawingEvents (DrawingLayouts.LatestBar); // Подписка на событие отрисовки на последнем баре
        Panel = IndicatorDataProvider.CandlesPanel; // Указание панели для отрисовки индикатора - панель свечей
    }

    protected override void OnCalculate (int bar, decimal value) // Переопределенный метод OnCalculate (пустой)
    {
        // Метод оставлен пустым для оптимизации производительности, вычисления не требуются
    }

    protected override void OnRender (RenderContext context, DrawingLayouts layout) // Переопределенный метод OnRender для отрисовки индикатора
    {
        var maxValue = 0m; // Инициализация переменной для хранения максимального значения объема, 0 по умолчанию
        var maxHeight = ChartInfo.Region. Height * _height / 100m; // Вычисление максимальной высоты для отрисовки объема, исходя из высоты региона графика и свойства Height
        var positiveColor = ((ValueDataSeries) DataSeries[0]).Color.Convert(); // Получение цвета для положительного объема из DataSeries[0]
        var negativeColor = ((ValueDataSeries)DataSeries[1]).Color.Convert(); // Получение цвета для отрицательного объема из DataSeries[1]
        var neutralColor = ((ValueDataSeries) DataSeries[2]).Color.Convert(); // Получение цвета для нейтрального объема из DataSeries[2]
        var filterColor = ((ValueDataSeries) DataSeries[3]).Color.Convert(); // Получение цвета для фильтрованного объема из DataSeries[3]

        for (int i = FirstVisibleBarNumber; i <= LastVisibleBarNumber; i++) // Цикл по видимым барам на графике
        {
            var candle = GetCandle(i); // Получение данных свечи для текущего бара
            var volumeValue = Input == InputType. Volume ? candle. Volume : candle.Ticks; // Определение значения объема: Volume или Ticks в зависимости от InputType

            maxValue = Math.Max (volumeValue, maxValue); // Обновление максимального значения объема, если текущее значение больше
        }

        for (int i = FirstVisibleBarNumber; i <= LastVisibleBarNumber; i++) // Второй цикл по видимым барам для отрисовки
        {
            var candle = GetCandle(i); // Получение данных свечи для текущего бара
            var volumeValue = Input == InputType. Volume ? candle. Volume : candle.Ticks; // Определение значения объема: Volume или Ticks в зависимости от InputType
            Color volumeColor; // Объявление переменной для цвета объема

            if (UseFilter && volumeValue > FilterValue) // Проверка использования фильтра и превышения значения фильтра
            {
                volumeColor = filterColor; // Если условие выполняется, цвет объема - фильтрованный цвет
            }
            else // Иначе (фильтр не используется или значение объема не превышает фильтр)
            {
                if (DeltaColored) // Проверка, включена ли цветовая дифференциация дельты
                {
                    if (candle. Delta > 0) // Если дельта положительная
                    {
                        volumeColor = positiveColor; // Цвет объема - положительный цвет
                    }
                    else if (candle. Delta < 0) // Если дельта отрицательная
                    {
                        volumeColor = negativeColor; // Цвет объема - отрицательный цвет
                    }
                    else // Если дельта равна 0
                    {
                        volumeColor = neutralColor; // Цвет объема - нейтральный цвет
                    }
                }
                else // Если цветовая дифференциация дельты не включена
                {
                    if (candle.Close > candle.Open) // Если цена закрытия выше цены открытия (растущая свеча)
                    {
                        volumeColor = positiveColor; // Цвет объема - положительный цвет
                    }
                    else if (candle.Close < candle.Open) // Если цена закрытия ниже цены открытия (падающая свеча)
                    {
                        volumeColor = negativeColor; // Цвет объема - отрицательный цвет
                    }
                    else // Если цена закрытия равна цене открытия (нейтральная свеча)
                    {
                        volumeColor = neutralColor; // Цвет объема - нейтральный цвет
                    }
                }
            }

            var x = ChartInfo.GetXByBar(i); // Получение X-координаты бара на графике
            var height = (int) (maxHeight * volumeValue / maxValue); // Вычисление высоты прямоугольника объема пропорционально максимальной высоте и значению объема
            var rectangle = new Rectangle(x, ChartInfo.Region.Height - height, (int)ChartInfo.PriceChartContainer. BarsWidth, height); // Создание прямоугольника для отрисовки объема
            context.FillRectangle(volumeColor, rectangle); // Отрисовка заполненного прямоугольника объема заданным цветом
        }
    }

    public override string ToString() // Переопределение метода ToString для отображения имени индикатора
    {
        return "Volume on the chart"; // Возвращает имя индикатора
    }
}
```

### Индикатор "Current price"

**Описание из PDF:**

Пример индикатора, который отображает текущую цену и текущее время на графике.

**Код индикатора:**

```csharp
using ATAS.Indicators; // Пространство имен индикаторов ATAS

[DisplayName("Current price")] // Атрибут для отображения имени индикатора
public class CurrentPrice : Indicator // Объявление класса CurrentPrice, наследующегося от Indicator
{
    RenderFont_font = new RenderFont("Roboto", 14); // Создание шрифта Roboto размером 14 для текста
    private Color_background = Color.Blue; // Приватное поле для цвета фона, по умолчанию синий
    private Color _textColor = Color.AliceBlue; // Приватное поле для цвета текста, по умолчанию AliceBlue
    private RenderStringFormat _stringFormat = new RenderStringFormat() { LineAlignment = StringAlignment.Center }; // Формат для выравнивания текста по центру

    public System.Windows.Media. Color Background // Публичное свойство Background для цвета фона
    {
        get => _background.Convert(); // Геттер возвращает цвет фона, конвертируя его в System.Windows.Media.Color
        set => _background = value.Convert(); // Сеттер устанавливает цвет фона, конвертируя входное значение
    }

    public System.Windows.Media.Color TextColor // Публичное свойство TextColor для цвета текста
    {
        get => _textColor.Convert(); // Геттер возвращает цвет текста, конвертируя его
        set => _textColor = value.Convert(); // Сеттер устанавливает цвет текста, конвертируя входное значение
    }

    public float FontSize // Публичное свойство FontSize для размера шрифта
    {
        get => _font.Size; // Геттер возвращает текущий размер шрифта
        set                 // Сеттер для установки размера шрифта
        {
            if (value < 5) // Проверка, чтобы размер шрифта не был меньше 5
            return;     // Если размер шрифта меньше 5, выход из сеттера без изменений

            _font = new RenderFont("Roboto", value); // Создание нового шрифта Roboto с заданным размером
        }
    }

    public bool ShowTime { get; set; } = true; // Публичное свойство ShowTime, определяющее, показывать ли время, по умолчанию true
    public string TimeFormat { get; set; } = "HH:mm:ss"; // Публичное свойство TimeFormat, формат отображения времени, по умолчанию "HH:mm:ss"

    public CurrentPrice() : base(true) // Конструктор класса CurrentPrice, вызывающий конструктор базового класса Indicator
    {
        SubscribeToDrawingEvents (DrawingLayouts.Final); // Подписка на событие отрисовки на финальном слое
        EnableCustomDrawing = true; // Разрешение пользовательской отрисовки
        DataSeries[0]. IsHidden = true; // Скрытие DataSeries[0] (не используется в данном индикаторе)
    }

    #region Overrides of BaseIndicator // Регион для переопределенных методов базового класса

    protected override void OnCalculate(int bar, decimal value) // Переопределенный метод OnCalculate (пустой)
    {
        // Метод оставлен пустым, так как вычисления не требуются для данного индикатора
    }

    #endregion

    protected override void OnRender (RenderContext context, Drawing Layouts layout) // Переопределенный метод OnRender для отрисовки
    {
        if (LastVisibleBarNumber != CurrentBar - 1) // Проверка, является ли текущий бар последним видимым баром
        return;                                  // Если нет, выход из метода (отрисовка только на последнем баре)

        var candle = GetCandle(LastVisibleBarNumber); // Получение данных свечи для последнего видимого бара
        var priceString = candle.Close.ToString(); // Получение цены закрытия последнего бара в виде строки
        var size = context.MeasureString(priceString, _font); // Измерение размера строки цены с использованием заданного шрифта

        var x = (int)(ChartInfo.GetXByBar(LastVisibleBarNumber) + ChartInfo. PriceChartContainer.BarsWidth); // Вычисление X-координаты для отрисовки прямоугольника
        var y = ChartInfo.GetYByPrice (candle.Close, false); // Вычисление Y-координаты для отрисовки прямоугольника (по цене закрытия)
        var rectangle = new Rectangle(x + 10, у - (int)size.Height / 2, (int)size.Width + 10, (int)size.Height); // Создание прямоугольника для фона цены

        var points = new List<point> // Создание списка точек для полигона фона
        {
            new Point(x, y), // Левая точка
            new Point(rectangle.X, rectangle.Y), // Верхняя левая точка прямоугольника
            new Point(rectangle.X + rectangle.Width, rectangle.Y), // Верхняя правая точка прямоугольника
            new Point(rectangle.X + rectangle.Width, rectangle. Y + rectangle.Height), // Нижняя правая точка прямоугольника
            new Point(rectangle.X, rectangle. Y + rectangle.Height), // Нижняя левая точка прямоугольника
        };

        context.FillPolygon(_background, points.ToArray()); // Заполнение полигона фона цветом _background

        rectangle.Y++; // Смещение Y-координаты прямоугольника на 1 пиксель вниз (возможно, для визуальной корректировки)
        context.DrawString(priceString, _font, _textColor, rectangle, _stringFormat); // Отрисовка строки цены внутри прямоугольника

        if (ShowTime) // Проверка, нужно ли показывать время
        {
            var time = DateTime.Now.ToString(TimeFormat); // Получение текущего времени в заданном формате
            size = context.MeasureString(time, _font); // Измерение размера строки времени
            context.DrawString(time, _font, _textColor, new Rectangle(rectangle.X + rectangle.Width - size.Width, rectangle. Y - size. He, rectangle.Width, rectangle.Height), _stringFormat); // Отрисовка строки времени справа от цены
        }
    }
}
```

### Индикатор "Tables"

**Описание из PDF:**

Ниже приведен пример индикатора, который отображает данные в табличной форме (аналог Cluster Statistic).

**Код индикатора:**

```csharp
using ATAS.Indicators; // Пространство имен индикаторов ATAS

public class ClusterStatisticSample : Indicator // Объявление класса ClusterStatisticSample, наследующегося от Indicator
{
    public ClusterStatisticSample() // Конструктор класса ClusterStatisticSample
    {
        EnableCustomDrawing = true; // Разрешение пользовательской отрисовки
        SubscribeToDrawingEvents (DrawingLayouts.LatestBar); // Подписка на событие отрисовки на последнем баре
        Panel = IndicatorDataProvider.NewPanel; // Указание панели для отрисовки индикатора - новая панель
        DenyToChangePanel = true; // Запрет изменения панели индикатора пользователем
        DataSeries[0]. IsHidden = true; // Скрытие DataSeries[0]
    }

    protected override void OnCalculate (int bar, decimal value) // Переопределенный метод OnCalculate (пустой)
    {
        // Метод оставлен пустым, так как вычисления выполняются в OnRender
    }

    protected override void OnRender (RenderContext context, DrawingLayouts layout) // Переопределенный метод OnRender для отрисовки
    {
        var rowHeight = Container.Region.Height / 3; // Вычисление высоты строки таблицы, 1/3 от высоты региона графика
        var drawText = ChartInfo. PriceChartContainer. BarsWidth > 50; // Условие для отрисовки текста: ширина бара должна быть больше 50 пикселей
        var font = new RenderFont("Roboto", 13); // Создание шрифта Roboto размером 13 для текста в таблице

        Rectangle rectangle; // Объявление переменной для прямоугольника ячейки таблицы

        for (int bar = FirstVisibleBarNumber; bar <= LastVisibleBarNumber; bar++) // Цикл по видимым барам
        {
            var candle = GetCandle(bar); // Получение данных свечи для текущего бара
            var x = ChartInfo.GetXByBar(bar); // Получение X-координаты бара
            var y = Container.Region.Y; // Y-координата начала региона графика (верхняя граница таблицы)

            rectangle = new Rectangle(x, y, (int) ChartInfo. PriceChartContainer.BarsWidth, rowHeight); // Создание прямоугольника для ячейки "Volume"
            context.FillRectangle(Color.LightBlue, rectangle); // Заполнение ячейки светло-голубым цветом
            if (drawText) // Если условие drawText выполняется (ширина бара > 50)
            context.DrawString(candle. Volume.ToString(), font, Color.Black, rectangle); // Отрисовка значения Volume в ячейке

            rectangle.Y += rowHeight; // Смещение Y-координаты для следующей строки таблицы
            context.FillRectangle (Color. IndianRed, rectangle); // Заполнение ячейки "Bid" индийским красным цветом
            if (drawText) // Если условие drawText выполняется
            context.DrawString(candle. Bid.ToString(), font, Color.Black, rectangle); // Отрисовка значения Bid в ячейке

            rectangle.Y += rowHeight; // Смещение Y-координаты для следующей строки таблицы
            context.FillRectangle (Color. Lime, rectangle); // Заполнение ячейки "Ask" лаймовым цветом
            if (drawText) // Если условие drawText выполняется
            context.DrawString(candle.Ask.ToString(), font, Color.Black, rectangle); // Отрисовка значения Ask в ячейке
        }

        #region draw headers // Регион для отрисовки заголовков таблицы

        rectangle = new Rectangle(0, Container.Region.Y, 100, rowHeight); // Создание прямоугольника для заголовка "Volume"
        context.FillRectangle(Color.Gray, rectangle); // Заполнение заголовка серым цветом
        context.DrawString("Volume", font, Color.Black, rectangle); // Отрисовка текста "Volume" в заголовке

        rectangle.Y += rowHeight; // Смещение Y-координаты для следующего заголовка
        context.FillRectangle(Color.Gray, rectangle); // Заполнение заголовка серым цветом
        context.DrawString("Bid", font, Color.Black, rectangle); // Отрисовка текста "Bid" в заголовке

        rectangle.Y += rowHeight; // Смещение Y-координаты для следующего заголовка
        context.FillRectangle (Color.Gray, rectangle); // Заполнение заголовка серым цветом
        context.DrawString("Ask", font, Color.Black, rectangle); // Отрисовка текста "Ask" в заголовке

        #endregion
    }
}
```

### Индикатор "Watermark"

**Описание из PDF:**

Пример индикатора, который показывает водяной знак на графике.

**Код индикатора:**

```csharp
namespace ATAS. Indicators. Technical // Пространство имен для технических индикаторов ATAS
{
    using System; // Пространство имен System
    using System.ComponentModel; // Пространство имен ComponentModel для атрибутов
    using System.ComponentModel.DataAnnotations; // Пространство имен DataAnnotations для атрибутов валидации
    using System.Drawing; // Пространство имен Drawing для работы с графикой

    using ATAS.Indicators.Editors; // Пространство имен редакторов индикаторов ATAS
    using ATAS. Indicators.Properties; // Пространство имен свойств индикаторов ATAS

    using OFT.Rendering.Context; // Пространство имен контекста рендеринга OFT
    using OFT.Rendering. Tools; // Пространство имен инструментов рендеринга OFT

    using Color = System.Windows.Media.Color; // Псевдоним Color для System.Windows.Media.Color

    public class Watermark : Indicator // Объявление класса Watermark, наследующегося от Indicator
    {
        #region Nested types // Регион для вложенных типов

        public enum Location // Объявление перечисления Location для определения положения водяного знака
        {
            [Display (Name = "Center")] // Атрибут Display для отображения "Center" в пользовательском интерфейсе
            Center, // Элемент перечисления Center - центр графика
            [Display (Name = "TopLeft")] // Атрибут Display для "TopLeft"
            TopLeft, // Элемент перечисления TopLeft - верхний левый угол
            [Display (Name = "TopRight")] // Атрибут Display для "TopRight"
            TopRight, // Элемент перечисления TopRight - верхний правый угол
            [Display (Name = "BottomLeft")] // Атрибут Display для "BottomLeft"
            BottomLeft, // Элемент перечисления BottomLeft - нижний левый угол
            [Display (Name = "BottomRight")] // Атрибут Display для "BottomRight"
            BottomRight // Элемент перечисления BottomRight - нижний правый угол
        }

        #endregion

        #region Properties // Регион для свойств индикатора

        [Display (Name = "Color", GroupName = "Common", Order = 10)] // Атрибут Display для свойства Color, группа "Common", порядок 10
        public Color TextColor { get; set; } = Color.FromArgb(255, 225, 225, 225); // Публичное свойство TextColor, цвет текста, по умолчанию светло-серый

        [Display (Name = "TextLocation", GroupName = "Common", Order = 20)] // Атрибут Display для свойства TextLocation, группа "Common", порядок 20
        public Location TextLocation { get; set; } // Публичное свойство TextLocation, положение текста, по умолчанию не задано

        [Display (Name = "Horizontaloffset", GroupName = "Common", Order = 30)] // Атрибут Display для Horizontaloffset, группа "Common", порядок 30
        public int HorizontalOffset { get; set; } // Публичное свойство HorizontalOffset, горизонтальное смещение, по умолчанию 0

        [Display (Name = "VerticalOffset", GroupName = "Common", Order = 40)] // Атрибут Display для VerticalOffset, группа "Common", порядок 40
        public int VerticalOffset { get; set; } // Публичное свойство VerticalOffset, вертикальное смещение, по умолчанию 0

        [Display (Name = "ShowInstrument", GroupName = "FirstLine", Order = 50)] // Атрибут Display для ShowInstrument, группа "FirstLine", порядок 50
        public bool ShowInstrument { get; set; } = true; // Публичное свойство ShowInstrument, показывать ли инструмент, по умолчанию true

        [Display (Name = "ShowPeriod", GroupName = "FirstLine", Order = 60)] // Атрибут Display для ShowPeriod, группа "FirstLine", порядок 60
        public bool ShowPeriod { get; set; } = true; // Публичное свойство ShowPeriod, показывать ли период, по умолчанию true

        [Display (Name = "Font", GroupName = "FirstLine", Order = 70)] // Атрибут Display для Font, группа "FirstLine", порядок 70
        [Editor(typeof(FontEditor), typeof(FontEditor))] // Атрибут Editor для использования FontEditor для редактирования свойства Font
        public FontSetting Font { get; set; } = new FontSetting { Size = 60, Bold = true }; // Публичное свойство Font, шрифт для первой строки, по умолчанию Roboto, размер 60, жирный

        [Display (Name = "Text", GroupName = "SecondLine", Order = 80)] // Атрибут Display для Text, группа "SecondLine", порядок 80
        public string AdditionalText { get; set; } = ""; // Публичное свойство AdditionalText, дополнительный текст для второй строки, по умолчанию пустая строка

        [Display (Name = "Font", GroupName = "SecondLine", Order = 90)] // Атрибут Display для Font для второй строки, группа "SecondLine", порядок 90
        [Editor(typeof(FontEditor), typeof(FontEditor))] // Атрибут Editor для использования FontEditor для редактирования свойства AdditionalFont
        public FontSetting AdditionalFont { get; set; } = new FontSetting { Size = 55 }; // Публичное свойство AdditionalFont, шрифт для второй строки, по умолчанию Roboto, размер 55

        [Display (Name = "VerticalOffset", GroupName = "SecondLine", Order = 100)] // Атрибут Display для VerticalOffset для второй строки, группа "SecondLine", порядок 100
        public int AdditionalTextYOffset { get; set; } = -40; // Публичное свойство AdditionalTextYOffset, вертикальное смещение второй строки, по умолчанию -40

        #endregion

        #region ctor // Регион для конструктора

        public Watermark() // Конструктор класса Watermark
        : base(true) // Вызов конструктора базового класса Indicator с параметром true
        {
            Font.PropertyChanged += (a, b) => RedrawChart(); // Подписка на событие изменения свойства Font для перерисовки графика
            AdditionalFont.PropertyChanged += (a, b) => RedrawChart(); // Подписка на событие изменения свойства AdditionalFont для перерисовки

            DataSeries[0]. IsHidden = true; // Скрытие DataSeries[0]
            DenyToChangePanel = true; // Запрет изменения панели индикатора
            EnableCustomDrawing = true; // Разрешение пользовательской отрисовки
            SubscribeToDrawingEvents(DrawingLayouts.Historical); // Подписка на событие отрисовки на историческом слое
            DrawAbovePrice = false; // Отрисовка под ценой (не над ценой)
        }

        #endregion

        #region Overrides of BaseIndicator // Регион для переопределенных методов базового класса

        protected override void OnCalculate (int bar, decimal value) // Переопределенный метод OnCalculate (пустой)
        {
            // Метод оставлен пустым, так как отрисовка выполняется в OnRender
        }

        protected override void OnRender (RenderContext context, Drawing Layouts layout) // Переопределенный метод OnRender для отрисовки
        {
            var showSecondLine = !string.IsNullOrWhiteSpace (AdditionalText); // Проверка, задан ли дополнительный текст для второй строки
            if (!showSecondLine && !ShowInstrument && ! ShowPeriod) // Если не нужно показывать ни вторую строку, ни инструмент, ни период
            return; // Выход из метода

            var textColor = TextColor.Convert(); // Получение цвета текста из свойства TextColor
            var mainTextRectangle = new Rectangle(); // Прямоугольник для первой строки текста
            var additionalTextRectangle = new Rectangle(); // Прямоугольник для второй строки текста
            var firstLine = string.Empty; // Строка для первой линии текста

            if (showSecondLine && !string.IsNullOrEmpty(AdditionalText)) // Если нужно показать вторую строку и текст задан
            {
                var size = context.MeasureString(AdditionalText, AdditionalFont.Font); // Измерение размера строки дополнительного текста
                additionalTextRectangle = new Rectangle(0, 0, (int)size.Width, (int)size.Height); // Создание прямоугольника для второй строки
            }

            if (ShowInstrument || ShowPeriod) // Если нужно показывать инструмент или период
            {
                if (ShowInstrument) // Если нужно показывать инструмент
                firstLine = InstrumentInfo. Instrument; // Получение имени инструмента

                if (ShowPeriod) // Если нужно показывать период
                {
                    var period = ChartInfo.ChartType == "TimeFrame" ? ChartInfo. TimeFrame : $"{ChartInfo.ChartType} {ChartInfo. Ti"; // Определение периода графика
                    if (ShowInstrument) // Если также показывается инструмент
                    firstLine += $", {period}"; // Добавление периода к строке инструмента
                    else // Если инструмент не показывается
                    firstLine += "{period}"; // Иначе просто период
                }
                var size = context.MeasureString(firstLine, Font.Font); // Измерение размера строки первой линии
                mainTextRectangle = new Rectangle(0, 0, (int)size.Width, (int)size.Height); // Создание прямоугольника для первой строки
            }

            if (mainTextRectangle.Height > 0 && additionalTextRectangle. Height > 0) // Если обе строки текста имеют высоту (нужно рисовать обе)
            {
                int firstLineX; // X-координата для первой строки
                int secondLineX; // X-координата для второй строки
                var y = 0; // Y-координата для обеих строк
                var totalHeight = mainTextRectangle. Height + additionalTextRectangle.Height + AdditionalTextYOffset; // Общая высота двух строк и смещения

                switch (TextLocation) // Выбор положения текста на графике
                {
                    case Location.Center: // Положение - центр
                    {
                        firstLineX = ChartInfo.PriceChartContainer. Region.Width / 2 - mainTextRectangle.Width / 2 + HorizontalOffset; // Вычисление X-координаты первой строки (центр по горизонтали + смещение)
                        secondLineX = ChartInfo. PriceChartContainer. Region.Width / 2 - additionalTextRectangle.Width / 2 + HorizontalOffset; // Вычисление X-координаты второй строки (центр по горизонтали + смещение)
                        y = ChartInfo. PriceChartContainer.Region.Height / 2 - totalHeight / 2 + VerticalOffset; // Вычисление Y-координаты (центр по вертикали + смещение)
                        break;
                    }
                    case Location.TopLeft: // Положение - верхний левый угол
                    {
                        firstLineX = secondLineX = Horizontaloffset; // X-координаты обеих строк - горизонтальное смещение
                        break;
                    }
                    case Location.TopRight: // Положение - верхний правый угол
                    {
                        firstLineX = ChartInfo. PriceChartContainer.Region.Width - mainTextRectangle.Width + Horizontaloffset; // Вычисление X-координаты первой строки (правый край - ширина текста + смещение)
                        secondLineX = ChartInfo. PriceChartContainer. Region.Width - additionalTextRectangle.Width + Horizontaloffset; // Вычисление X-координаты второй строки (правый край - ширина текста + смещение)
                        break;
                    }
                    case Location.BottomLeft: // Положение - нижний левый угол
                    {
                        firstLineX = secondLineX = Horizontaloffset; // X-координаты обеих строк - горизонтальное смещение
                        y = ChartInfo.PriceChartContainer.Region.Height - totalHeight + VerticalOffset; // Вычисление Y-координаты (нижний край - общая высота + смещение)
                        break;
                    }
                    case Location.BottomRight: // Положение - нижний правый угол
                    {
                        firstLineX = ChartInfo. PriceChartContainer.Region.Width - mainTextRectangle.Width + Horizontaloffset; // Вычисление X-координаты первой строки (правый край - ширина текста + смещение)
                        secondLineX = ChartInfo. PriceChartContainer. Region.Width - additionalTextRectangle.Width + Horizontaloffset; // Вычисление X-координаты второй строки (правый край - ширина текста + смещение)
                        y = ChartInfo. PriceChartContainer. Region. Height - totalHeight + Verticaloffset; // Вычисление Y-координаты (нижний край - общая высота + смещение)
                        break;
                    }
                    default: // Неизвестное положение
                    throw new ArgumentOutOfRangeException(); // Выброс исключения
                }
                context.DrawString(firstLine, Font.Font, textColor, firstLineX, y); // Отрисовка первой строки текста
                context.DrawString(AdditionalText, AdditionalFont.Font, textColor, secondLineX, y + mainTextRectangle.Height); // Отрисовка второй строки текста под первой
            }
            else if (mainTextRectangle. Height > 0) // Если нужно рисовать только первую строку
            {
                DrawString(context, firstLine, Font.Font, textColor, mainTextRectangle); // Вызов метода отрисовки для первой строки
            }
            else if (additionalTextRectangle.Height > 0) // Если нужно рисовать только вторую строку
            {
                DrawString(context, AdditionalText, AdditionalFont.Font, textColor, additionalTextRectangle); // Вызов метода отрисовки для второй строки
            }
        }

        private void DrawString (RenderContext context, string text, RenderFont font, System.Drawing.Color color, Rectangle rectangle) // Приватный метод для отрисовки строки текста
        {
            switch (TextLocation) // Выбор положения текста
            {
                case Location.Center: // Положение - центр
                {
                    context.DrawString(text, font, color, ChartInfo.PriceChartContainer. Region.Width / 2 - rectangle.Width / 2 + HorizontalOffset, // Отрисовка текста в центре по горизонтали
                    ChartInfo.PriceChartContainer. Region. Height / 2 - rectangle.Height / 2 + VerticalOffset); // и вертикали
                    break;
                }
                case Location.TopLeft: // Положение - верхний левый угол
                {
                    context.DrawString (text, font, color, Horizontaloffset, VerticalOffset); // Отрисовка текста в верхнем левом углу со смещением
                    break;
                }
                case Location.TopRight: // Положение - верхний правый угол
                {
                    context.DrawString (text, font, color, ChartInfo.PriceChartContainer. Region.Width - rectangle.Width + Horizont, VerticalOffset); // Отрисовка текста в верхнем правом углу со смещением
                    break;
                }
                case Location.BottomLeft: // Положение - нижний левый угол
                {
                    context.DrawString(text, font, color, HorizontalOffset, ChartInfo.PriceChartContainer.Region.Height - rectang + VerticalOffset); // Отрисовка текста в нижнем левом углу со смещением
                    break;
                }
                case Location.BottomRight: // Положение - нижний правый угол
                {
                    context.DrawString (text, font, color, ChartInfo.PriceChartContainer.Region.Width - rectangle.Width + Horizont, // Отрисовка текста в нижнем правом углу со смещением
                    ChartInfo.PriceChartContainer. Region.Height - rectangle.Height + VerticalOffset);
                    break;
                }
                default: // Неизвестное положение
                throw new ArgumentOutOfRangeException(); // Выброс исключения
            }
        }
        #endregion
    }
}
```

### Индикатор "Maximum Levels"

**Описание из PDF:**

Индикатор "Maximum Levels" отображает максимальные уровни цены для выбранного типа данных (Volume, Bid, Ask, Delta, Ticks, Time) за определенный период.

**Код индикатора:**

```csharp
namespace ATAS. Indicators. Technical // Пространство имен для технических индикаторов ATAS
{
    using System; // Пространство имен System
    using System.ComponentModel; // Пространство имен ComponentModel для атрибутов
    using System.ComponentModel.DataAnnotations; // Пространство имен DataAnnotations для атрибутов валидации
    using System.Drawing; // Пространство имен Drawing для работы с графикой

    using ATAS. Indicators.Properties; // Пространство имен свойств индикаторов ATAS

    using OFT.Rendering.Context; // Пространство имен контекста рендеринга OFT
    using OFT. Rendering. Tools; // Пространство имен инструментов рендеринга OFT

    using Utils.Common.Attributes; // Пространство имен атрибутов Utils.Common
    using Utils.Common.Localization; // Пространство имен локализации Utils.Common

    [DisplayName("Maximum Levels")] // Атрибут DisplayName для отображения имени индикатора
    [Category("Clusters, Profiles, Levels")] // Атрибут Category для группировки индикатора в категории
    public class MaxLevels: Indicator // Объявление класса MaxLevels, наследующегося от Indicator
    {
        #region Nested types // Регион для вложенных типов

        public enum MaxLevelType // Объявление перечисления MaxLevelType для типов максимальных уровней
        {
            Bid, // Максимальный уровень Bid
            Ask, // Максимальный уровень Ask
            PositiveDelta, // Максимальный уровень PositiveDelta
            NegativeDelta, // Максимальный уровень NegativeDelta
            Volume, // Максимальный уровень Volume
            Tick, // Максимальный уровень Tick
            Time // Максимальный уровень Time
        };

        #endregion

        #region Private // Регион для приватных полей и методов

        private RenderStringFormat _stringRightFormat = new RenderStringFormat // Формат для выравнивания текста справа
        {
            Alignment = StringAlignment.Far, // Выравнивание по правому краю
            LineAlignment = StringAlignment.Center, // Выравнивание по центру по вертикали
            Trimming = StringTrimming. EllipsisCharacter, // Обрезка текста с многоточием
            FormatFlags = StringFormatFlags.NoWrap // Запрет переноса строк
        };

        private bool_candleRequested; // Флаг запроса свечи профиля
        private IndicatorCandle _candle; // Переменная для хранения свечи профиля
        private Color _lineColor = System.Drawing.Color.CornflowerBlue; // Цвет линии уровня, по умолчанию CornflowerBlue
        private Color _axisTextColor = System.Drawing.Color.White; // Цвет текста на оси, по умолчанию White
        private Color _textColor = System.Drawing.Color.Black; // Цвет текста уровня, по умолчанию Black
        private RenderPen _renderPen = new RenderPen(System.Drawing.Color.CornflowerBlue, 2); // Перо для отрисовки линии уровня, цвет CornflowerBlue, толщина 2
        private int _width = 2; // Толщина линии уровня, по умолчанию 2
        private FixedProfilePeriods _period = FixedProfilePeriods.CurrentDay; // Период профиля, по умолчанию CurrentDay
        private RenderFont_font = new RenderFont("Arial", 8); // Шрифт для текста уровня, по умолчанию Arial, размер 8
        private string _description = "Current Day"; // Описание периода, по умолчанию "Current Day"

        #endregion

        #region Properties // Регион для публичных свойств

        public Fixed ProfilePeriods Period // Публичное свойство Period для выбора периода профиля
        {
            get => _period; // Геттер возвращает текущий период
            set             // Сеттер для установки периода
            {
                _period = value; // Установка нового значения периода
                _description = GetPeriodDescription(_period); // Обновление описания периода
                RecalculateValues(); // Пересчет значений индикатора
            }
        }

        public MaxLevelType Type { get; set; } = MaxLevelType.Volume; // Публичное свойство Type для выбора типа максимального уровня, по умолчанию Volume

        public System.Windows.Media.Color Color // Публичное свойство Color для цвета линии уровня
        {
            get => _lineColor.Convert(); // Геттер возвращает цвет линии, конвертируя его в System.Windows.Media.Color
            set             // Сеттер для установки цвета линии
            {
                _lineColor = value. Convert(); // Установка нового цвета линии
                _renderPen = new RenderPen(_lineColor, _width); // Обновление пера с новым цветом
            }
        }

        public int Width // Публичное свойство Width для толщины линии уровня
        {
            get => _width; // Геттер возвращает текущую толщину линии
            set             // Сеттер для установки толщины линии
            {
                _width = Math. Max (1, value); // Установка новой толщины линии, минимальное значение 1
                _renderPen = new RenderPen(_lineColor, _width); // Обновление пера с новой толщиной
            }
        }

        public System.Windows.Media.Color AxisTextColor // Публичное свойство AxisTextColor для цвета текста на оси
        {
            get => _axisTextColor.Convert(); // Геттер возвращает цвет текста оси
            set => _axisTextColor = value. Convert(); // Сеттер устанавливает цвет текста оси
        }

        public bool ShowText { get; set; } = true; // Публичное свойство ShowText, показывать ли текст уровня, по умолчанию true

        public System.Windows.Media.Color TextColor // Публичное свойство TextColor для цвета текста уровня
        {
            get => _textColor.Convert(); // Геттер возвращает цвет текста уровня
            set => _textColor = value.Convert(); // Сеттер устанавливает цвет текста уровня
        }

        public int FontSize // Публичное свойство FontSize для размера шрифта текста уровня
        {
            get => (int)_font.Size; // Геттер возвращает текущий размер шрифта
            set => _font = new RenderFont("Arial", Math. Max (7, value)); // Сеттер устанавливает новый размер шрифта, минимальный размер 7
        }

        #endregion

        #region Overrides of BaseIndicator // Регион для переопределенных методов базового класса

        public MaxLevels() // Конструктор класса MaxLevels
        : base(true) // Вызов конструктора базового класса Indicator с параметром true
        {
            DataSeries[0]. IsHidden = true; // Скрытие DataSeries[0]
            DenyToChangePanel = true; // Запрет изменения панели индикатора
            EnableCustomDrawing = true; // Разрешение пользовательской отрисовки
            SubscribeToDrawingEvents(DrawingLayouts.LatestBar); // Подписка на событие отрисовки на последнем баре
            DrawAbovePrice = true; // Отрисовка над ценой
            Width = Width; // Установка толщины линии уровня (возможно, избыточно, так как Width уже инициализировано)
        }

        protected override void OnCalculate(int bar, decimal value) // Переопределенный метод OnCalculate
        {
            if (bar == 0) // Если бар нулевой (первый бар)
            _candleRequested = false; // Сброс флага запроса свечи профиля

            if (!_candleRequested && bar == CurrentBar - 1) // Если свеча профиля не запрошена и текущий бар - последний бар
            {
                _candleRequested = true; // Установка флага запроса свечи профиля
                GetFixedProfile(new FixedProfileRequest(Period)); // Запрос свечи профиля для выбранного периода
            }
        }

        protected override void OnFixedProfilesResponse (IndicatorCandle fixedProfile, FixedProfilePeriods peri // Переопределенный метод OnFixedProfilesResponse, вызываемый после получения свечи профиля
        {
            _candle = fixedProfile; // Сохранение полученной свечи профиля
            RedrawChart(); // Перерисовка графика
        }

        protected override void OnRender (RenderContext context, Drawing Layouts layout) // Переопределенный метод OnRender для отрисовки индикатора
        {
            if (_candle == null) // Если свеча профиля не получена
            return; // Выход из метода

            var priceInfo = GetPriceVolumeInfo(_candle, Type); // Получение информации о цене и объеме для выбранного типа уровня
            if (priceInfo == null) // Если информация не получена
            return; // Выход из метода

            var y = ChartInfo.GetYByPrice (priceInfo. Price); // Получение Y-координаты для отрисовки линии уровня по цене
            var firstX = ChartInfo. PriceChartContainer. Region.Width / 2; // X-координата начала линии (середина ширины контейнера)
            var secondX = ChartInfo. PriceChartContainer. Region.Width; // X-координата конца линии (правый край контейнера)

            context.DrawLine(_renderPen, firstX, y, secondX, y); // Отрисовка горизонтальной линии уровня

            if (ShowText) // Если нужно показывать текст уровня
            {
                var size = context.MeasureString(_description, _font); // Измерение размера строки описания периода
                var textRect = new Rectangle(new Point (ChartInfo. PriceChartContainer.Region.Width - (int)size.Width - 20, y - new Size((int)size.Width + 20, (int)size.Height).Height), // Создание прямоугольника для текста справа от линии уровня
                new Size((int)size.Width + 20, (int)size.Height)); // Размер прямоугольника

                context.DrawString(_description, font, _textColor, textRect, _stringRightFormat); // Отрисовка текста описания периода справа от линии уровня
            }
            this.DrawLabelOnPriceAxis(context, string.Format(ChartInfo.StringFormat, priceInfo. Price), y, font, _lineCol); // Отрисовка метки уровня на ценовой оси
        }

        #endregion

        private string GetPeriodDescription(FixedProfilePeriods period) // Приватный метод для получения текстового описания периода
        {
            switch (period) // Выбор описания в зависимости от периода
            {
                case FixedProfilePeriods. CurrentDay: // Текущий день
                return "Current Day"; // Возврат "Current Day"
                case FixedProfilePeriods. LastDay: // Прошлый день
                return "Last Day"; // Возврат "Last Day"
                case Fixed ProfilePeriods. CurrentWeek: // Текущая неделя
                return "Current Week"; // Возврат "Current Week"
                case Fixed ProfilePeriods. LastWeek: // Прошлая неделя
                return "Last Week"; // Возврат "Last Week"
                case Fixed ProfilePeriods. CurrentMonth: // Текущий месяц
                return "Current Month"; // Возврат "Current Month"
                case Fixed ProfilePeriods. LastMonth: // Прошлый месяц
                return "Last Month"; // Возврат "Last Month"
                case Fixed ProfilePeriods. Contract: // Контракт
                return "Contract"; // Возврат "Contract"
                default: // Неизвестный период
                throw new ArgumentOutOfRangeException(nameof(period), period, null); // Выброс исключения
            }
        }

        private PriceVolumeInfo GetPriceVolumeInfo(IndicatorCandle candle, MaxLevelType levelType) // Приватный метод для получения информации о цене и объеме для выбранного типа уровня
        {
            switch (Type) // Выбор типа уровня
            {
                case MaxLevelType.Bid: // Тип - Bid
                {
                    return_candle.MaxBidPriceInfo; // Возврат информации о максимальном Bid
                }
                case MaxLevelType.Ask: // Тип - Ask
                return_candle.MaxAskPriceInfo; // Возврат информации о максимальном Ask
                case MaxLevelType.PositiveDelta: // Тип - PositiveDelta
                {
                    return_candle. MaxPositiveDeltaPriceInfo; // Возврат информации о максимальной положительной дельте
                }
                case MaxLevelType.NegativeDelta: // Тип - NegativeDelta
                {
                    return_candle.MaxNegativeDeltaPriceInfo; // Возврат информации о максимальной отрицательной дельте
                }
                case MaxLevelType. Volume: // Тип - Volume
                {
                    return_candle.MaxVolumePriceInfo; // Возврат информации о максимальном объеме
                }
                case MaxLevelType.Tick: // Тип - Tick
                {
                    return_candle.MaxTickPriceInfo; // Возврат информации о максимальном количестве тиков
                }
                case MaxLevelType.Time: // Тип - Time
                {
                    return_candle. MaxTimePriceInfo; // Возврат информации о максимальном времени
                }
                default: // Неизвестный тип уровня
                throw new ArgumentOutOfRangeException(); // Выброс исключения
            }
        }
    }
}
```









### Package Members Overview  
#### ATAS Package Members List  

### Namespace Members  
**ATAS.Strategies**  
- **BaseLoggerSource**: Базовый класс для работы с логированием в стратегиях.  
- **IStrategy**: Интерфейс торговой стратегии.  
- **Strategy**: Базовый класс для создания торговых стратегий.  
- **StrategyErrorTypes**: Типы ошибок стратегии (например, превышение лимита ошибок).  
- **StrategyStates**: Состояния стратегии (Stopped, Started, Error и др.).  
- **StrategyStateChangedEventArgs**: Аргументы события изменения состояния стратегии.  
- **StrategyNotificationEventArgs**: Аргументы уведомлений стратегии.  

**ATAS.Indicators**  
- **DataSeriesType**: Перечисление типов данных серий (Candles, Bars, Indicator и др.).  
- **CandleVisualMode**: Режимы визуализации свечей (Candles/Bars).  
- **ChartVisualModes**: Режимы отображения графика (Candles, Clusters, Line и др.).  
- **FootprintColorSchemes**: Схемы окраски следов активности (Delta, Volume, HeatMap и др.).  
- **FootprintContentModes**: Типы содержимого следов (Volume, Trades, Delta и др.).  
- **TradeDirection**: Направление сделки (Buy, Sell, Between).  
- **VisualMode**: Визуальные режимы для данных (Line, Histogram, Arrow и др.).  

**ATAS.Indicators.Drawing**  
- **DefaultColors**: Стандартные цвета для графических объектов.  
- **TrendLine**: Класс для создания линий тренда.  

**ATAS.Indicators.Filters**  
- **Filter**: Базовый класс фильтров.  
- **FilterBool, FilterColor, FilterInt**: Фильтры для логических, цветовых и целочисленных значений.  
- **IFilter**: Интерфейс фильтра.  

**ATAS.DataFeedsCore**  
- **MarketDataType**: Типы рыночных данных (Bid, Ask, Trade).  
- **InstrumentInfo**: Информация о торговом инструменте.  
- **IMarketTimeProvider**: Интерфейс для работы с времевыми данными рынка.  

**ATAS.Indicators.Attributes**  
- **ParameterAttribute**: Атрибут для описания параметров индикаторов.  
- **FeatureId**: Идентификатор функциональных возможностей.  

**ATAS.Indicators.Strategies**  
- **IATMStrategy**: Интерфейс для автоматизированных стратегий (ATM).  
- **BaseStopProfitStrategy**: Базовый класс для стратегий Stop-Loss/Take-Profit.  

### Аннотация:  
- **Package Members**  
  *Полный список всех элементов пакета ATAS, включая классы, интерфейсы и перечисления из разных пространств имен.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**  
- **namespace ATAS.Indicators.Drawing**  
- **namespace ATAS.Indicators.Filters**  
- **namespace ATAS.Strategies**  
- **namespace ATAS.Strategies.ATM**  
- **namespace ATAS.DataFeedsCore**  
- **namespace ATAS.Indicators.Attributes**














### Indicators Directory Reference

#### Directories
- **Attributies**
- **Drawing**
- **Filters**
- **Properties**

#### Files
- **BaseDataSeries.cs**
- **BaseIndicator.cs**
- **Candle.cs**
- **CandleDataSeries.cs**
- **CandlePartSeries.cs**
- **CandleVisualMode.cs**
- **ChartObject.cs**
- **ChartVisualModes.cs**
- **CumulativeTrade.cs**
- **CumulativeTradesRequest.cs**
- **CustomValue.cs**
- **CustomValueDataSeries.cs**
- **DrawingLayouts.cs**
- **Indicators/Extensions.cs**
- **FootprintColorSchemes.cs**
- **FootprintContentModes.cs**
- **FootprintVisualModes.cs**
- **Indicators/GlobalUsings.cs**
- **ICandleCreator.cs**
- **IIndicatorContainer.cs**
- **IMarketTimeProvider.cs**
- **Indicator.cs**
- **IndicatorCandle.cs**
- **IndicatorDataProvider.cs**
- **IndicatorSeries.cs**
- **INotifyPanelPropertyChanged.cs**
- **InstrumentInfo.cs**
- **IOnlineDataProvider.cs**
- **IPlatformSettings.cs**
- **IPropertiesEditor.cs**
- **IPropertiesEditorOwner.cs**
- **ITradingManager.cs**
- **LineSeries.cs**
- **MarketDataArg.cs**
- **Indicators/MarketDataType.cs**
- **MarketDepthInfoProvider.cs**
- **MarketDepthSnapshot.cs**
- **MarketDepthSnapshotRequest.cs**
- **NotifyPropertyChangedBase.cs**
- **ObjectDataSeries.cs**
- **ObjectType.cs**
- **PaintbarsDataSeries.cs**
- **PriceSelectionDataSeries.cs**
- **PriceSelectionValue.cs**
- **PriceVolumeInfo.cs**
- **RangeDataSeries.cs**
- **RangeValue.cs**
- **RedrawArg.cs**
- **SelectionType.cs**
- **Indicators/TradeDirection.cs**
- **ValueDataSeries.cs**
- **VisualMode.cs**






### Attributies Directory Reference

#### Directory Dependency Graph
- **Indicators**
- **Attributies**

#### Files
- **FeatureId.cs**
- **Indicators/Attributies/ParameterAttribute.cs**






### Classes | Namespaces
#### FeatureId.cs File Reference





### Classes | Namespaces
#### Indicators/Attributes/ParameterAttribute.cs
##### File Reference







### Drawing Directory Reference

#### Directory Dependency Graph
- **Indicators**
- **Drawing**

#### Files
- **DefaultColors.cs**
- **TrendLine.cs**




### Filters Directory Reference

#### Directory Dependency Graph
- **Indicators**
- **Filters**
- **Converters**

#### Directories
- **Converters**

#### Files
- **Filter.cs**
- **FilterBase.cs**
- **FilterBool.cs**
- **FilterColor.cs**
- **FilterEnum.cs**
- **FilterExtensions.cs**
- **FilterHeatmapTypes.cs**
- **FilterInt.cs**
- **FilterKey.cs**
- **FilterRangeBase.cs**
- **FilterRangeInt.cs**
- **FilterRangeValue.cs**
- **FilterRenderPen.cs**
- **FilterString.cs**
- **FilterTimeSpan.cs**
- **IFilter.cs**
- **IFilterEnum.cs**
- **IFilterValue.cs**
- **TrackedPropertyBase.cs**
- **ValueChangingEventArgs.cs**






### Properties Directory Reference

#### Directory Dependency Graph
- **Indicators**
- **Properties**

#### Files
- **Indicators/Properties/AssemblyInfo.cs**






### Classes | Namespaces | Enumerations  
#### BaseDataSeries.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IDataSeries< T>**  
  *Интерфейс для работы с данными серий, предоставляющий базовые свойства и методы.*  
- **class ATAS.Indicators.BaseDataSeries< T>**  
  *Базовый обобщённый класс данных серий, реализующий общую функциональность.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**  

### Enumerations  
- **enum ATAS.Indicators.DataSeriesType**  
  ```  
  ATAS.Indicators.Bars,  
  ATAS.Indicators.Open,  
  ATAS.Indicators.High,  
  ATAS.Indicators.Low,  
  ATAS.Indicators.Close,  
  ATAS.Indicators.Volume,  
  ATAS.Indicators.HL2,  
  ATAS.Indicators.HLC3,  
  ATAS.Indicators.OHLC4,  
  ATAS.Indicators.HLCC4,  
  ATAS.Indicators.Indicator,  
  ATAS.Indicators.Line,  
  ATAS.Indicators.Band,  
  ATAS.Indicators.Value,  
  ATAS.Indicators.Candle,  
  ATAS.Indicators.PriceSelection,  
  ATAS.Indicators.PaintBars,  
  ATAS.Indicators.CustomValue,  
  ATAS.Indicators.Object  
  ```  
  *Перечисление типов данных серий:  
  - **Bars, Open, High, Low, Close, Volume** — базовые данные свечей;  
  - **HL2, HLC3, OHLC4, HLCC4** — вычисляемые средние цен;  
  - **Indicator, Line, Band, Value, Candle, PriceSelection, PaintBars, CustomValue, Object** — типы индикаторов и объектов.*







### Classes | Namespaces | Typedefs  
#### BaseIndicator.cs File Reference  

### Classes  
- **class ATAS.Indicators.BaseIndicator**  
  *Базовый класс для создания пользовательских индикаторов на графике.*  
- **class ATAS.Indicators.ExtendedIndicator**  
  *Расширенный базовый класс для индикаторов с дополнительной функциональностью: отображение, оповещения, работа с рыночными данными и т.д.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**  

### Typedefs  
- **using ATAS.Indicators.TimerSubscriptionKey = (System.TimeSpan period, System.Action callback)**  
  *Тип для подписки на таймер: период (TimeSpan) и действие (Action), которое выполняется при срабатывании.*






### Classes | Namespaces  
#### Candle.cs File Reference  

### Classes  
- **class ATAS.Indicators.Candle**  
  *Класс для представления свечи на графике: содержит цены открытия (Open), максимума (High), минимума (Low), закрытия (Close).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Classes | Namespaces  
#### CandleDataSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.CandleDataSeries**  
  *Класс для представления серии свечей. Каждый элемент в серии — объект Candle (свеча с ценами Open/High/Low/Close).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Classes | Namespaces  
#### CandlePartSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.CandlePartSeries**  
  *Класс для представления серии значений decimal, полученных из определённых частей свечи IndicatorCandle, созданной через ICandleCreator.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Namespaces | Enumerations  
#### CandleVisualMode.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.CandleVisualMode**  
  ```  
  ATAS.Indicators.Candles = 0,  
  ATAS.Indicators.Bars = 1  
  ```  
  *Перечисление для выбора визуализации свечей:  
  - **Candles** — традиционные японские свечи;  
  - **Bars** — простые вертикальные линии (бэры) с отметками цены.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Classes | Namespaces  
#### ChartObject.cs File Reference  

### Classes  
- **class ATAS.Indicators.ChartObject**  
  *Базовый класс для объектов на графике (например, линии тренда, уровни, метки).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Namespaces | Enumerations  
#### ChartVisualModes.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.ChartVisualModes**  
  ```  
  ATAS.Indicators.Candles = 0,  
  ATAS.Indicators.Clusters = 1,  
  ATAS.Indicators.TransparentCandles = 2,  
  ATAS.Indicators.Line = 3,  
  ATAS.Indicators.Bars = 4,  
  ATAS.Indicators.Hidden = 5  
  ```  
  *Перечисление режимов визуализации ценовых данных на графике:  
  - **Candles** — традиционные японские свечи;  
  - **Clusters** — кластеризованные свечи;  
  - **TransparentCandles** — прозрачные свечи;  
  - **Line** — линейный график;  
  - **Bars** — вертикальные бэры;  
  - **Hidden** — скрытый режим.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### CumulativeTrade.cs File Reference  

### Classes  
- **class ATAS.Indicators.CumulativeTrade**  
  *Класс для представления накопительной сделки, объединяющей несколько исполнений (prints) в одну позицию.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### CumulativeTradesRequest.cs File Reference  

### Classes  
- **class ATAS.Indicators.CumulativeTradesRequest**  
  *Класс для запроса данных накопительных сделок в заданном времевом диапазоне или для определённой даты.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### CustomValue.cs File Reference  

### Classes  
- **class ATAS.Indicators.CustomValue**  
  *Класс для представления пользовательского значения с дополнительными свойствами (например, метки, параметры).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces  
#### CustomValueDataSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.CustomValueDataSeries**  
  *Класс для представления пользовательской серии данных, хранящей объекты CustomValue.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Namespaces | Enumerations  
#### DrawingLayouts.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.DrawingLayouts**  
  ```  
  ATAS.Indicators.None = 1,  
  ATAS.Indicators.Historical = 2,  
  ATAS.Indicators.LatestBar = 4,  
  ATAS.Indicators.Final = 8  
  ```  
  *Перечисление режимов размещения графических объектов на графике:  
  - **None** — отключено;  
  - **Historical** — отображение исторических данных;  
  - **LatestBar** — отображение последней свечи;  
  - **Final** — финальный режим (например, закрытие позиции).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces  
#### Extensions.cs File Reference  

### Classes  
- **class ATAS.Indicators.Extensions**  
  *Содержит расширения методов для различных типов (например, обработка данных, конвертация и т.д.).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Namespaces | Enumerations  
#### FootprintColorSchemes.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.FootprintColorSchemes**  
  ```  
  ATAS.Indicators.Delta = 0,  
  ATAS.Indicators.Solid = 10,  
  ATAS.Indicators.VolumeProportion = 20,  
  ATAS.Indicators.TradesProportion = 30,  
  ATAS.Indicators.VolumeBasedBidAsk = 50,  
  ATAS.Indicators.HeatMapVolume = 60,  
  ATAS.Indicators.HeatMapTrades = 70,  
  ATAS.Indicators.HeatMapDelta = 80,  
  ATAS.Indicators.None = 90  
  ```  
  *Перечисление схем окраски "Footprint" (следов рыночной активности):  
  - **Delta** — цвет по дельте цены;  
  - **Solid** — однородный цвет;  
  - **VolumeProportion** — цвет по пропорции объёма;  
  - **TradesProportion** — цвет по количеству сделок;  
  - **VolumeBasedBidAsk** — цвет по спросу/предложению;  
  - **HeatMapVolume, HeatMapTrades, HeatMapDelta** — тепловые карты для объёма, сделок и дельты;  
  - **None** — отключено.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Namespaces | Enumerations  
#### FootprintContentModes.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.FootprintContentModes**  
  ```  
  ATAS.Indicators.Volume = 0,  
  ATAS.Indicators.Trades = 10,  
  ATAS.Indicators.VolumeTrades = 30,  
  ATAS.Indicators.VolumeDelta = 40,  
  ATAS.Indicators.Delta = 50,  
  ATAS.Indicators.DeltaCentered = 51,  
  ATAS.Indicators.BidXAsk = 60,  
  ATAS.Indicators.BidAskCentered = 70,  
  ATAS.Indicators.BidAsk = 80,  
  ATAS.Indicators.None = 100  
  ```  
  *Перечисление режимов содержимого "Footprint" (следов рыночной активности):  
  - **Volume** — отображение объёма;  
  - **Trades** — количество сделок;  
  - **VolumeTrades** — объём и сделки;  
  - **VolumeDelta** — объём и дельта цены;  
  - **Delta** — дельта цены;  
  - **DeltaCentered** — дельта, центрированная;  
  - **BidXAsk** — спрос/предложение (Bid/Ask);  
  - **BidAskCentered** — Bid/Ask, центрированный;  
  - **BidAsk** — Bid/Ask без центрирования;  
  - **None** — отключено.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Namespaces | Enumerations  
#### FootprintVisualModes.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.FootprintVisualModes**  
  ```  
  ATAS.Indicators.FullRow,  
  ATAS.Indicators.BidAskHistogram,  
  ATAS.Indicators.VolumeHistogram,  
  ATAS.Indicators.TradesHistogram,  
  ATAS.Indicators.DeltaHistogram,  
  ATAS.Indicators.BidAskLadder,  
  ATAS.Indicators.PositiveNegativeDeltaProfile  
  ```  
  *Перечисление режимов визуализации "Footprint" (следов рыночной активности):  
  - **FullRow** — полная строка данных;  
  - **BidAskHistogram** — гистограмма Bid/Ask;  
  - **VolumeHistogram** — гистограмма объёма;  
  - **TradesHistogram** — гистограмма сделок;  
  - **DeltaHistogram** — гистограмма дельты;  
  - **BidAskLadder** — лестница Bid/Ask;  
  - **PositiveNegativeDeltaProfile** — профиль положительной/отрицательной дельты.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Typedefs  
#### GlobalUsings.cs File Reference  

### Typedefs  
- **using CrossColor = System.Windows.Media.Color**  
  *Тип для работы с цветами в графических элементах.*  
- **using CrossKey = System.Windows.Input.Key**  
  *Тип для обработки клавиш клавиатуры.*  
- **using CrossColors = System.Windows.Media.Colors**  
  *Статический класс с предопределенными цветами.*  
- **using CrossKeyEventArgs = System.Windows.Input.KeyEventArgs**  
  *Аргументы события для нажатия клавиш.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### ICandleCreator.cs File Reference  

### Classes  
- **interface ATAS.Indicators.ICandleCreator**  
  *Интерфейс для создания и управления свечами индикаторов.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces | Functions  
#### IIndicatorContainer.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IChartColorsStore**  
  *Интерфейс для доступа к цветам и пером (pen) для отрисовки графика.*  
- **interface ATAS.Indicators.IChartCoordinatesManager**  
  *Интерфейс для управления координатами и масштабированием графика.*  
- **interface ATAS.Indicators.IContainer**  
  *Интерфейс контейнера, определяющий прямоугольную область на графике.*  
- **interface ATAS.Indicators.IIndicatorContainer**  
  *Интерфейс контейнера индикатора, хранящий данные и методы для работы с индикаторами.*  
- **interface ATAS.Indicators.IChartContainer**  
  *Интерфейс контейнера графика, хранящий данные и методы для работы с графиком.*  
- **interface ATAS.Indicators.IChart**  
  *Интерфейс основного графика с методами и свойствами для взаимодействия.*  
- **interface ATAS.Indicators.IMouseLocationInfo**  
  *Интерфейс для получения информации о положении мыши на графике.*  
- **interface ATAS.Indicators.IKeyboardInfo**  
  *Интерфейс для отслеживания состояния клавиатуры.*  
- **interface ATAS.Indicators.IDrawingObjectsListInfo**  
  *Интерфейс для управления списком графических объектов на графике.*  
- **class ATAS.Indicators.Container**  
  *Класс контейнера, определяющий прямоугольную область на графике.*  
- **interface ATAS.Indicators.ITradingVolumeInfo**  
  *Интерфейс для получения информации о торговом объёме.*  
- **interface ATAS.Indicators.IVolumeSelectorItem**  
  *Интерфейс элемента выбора объёма (например, временной шкалы).*  

### Functions  
- **record ATAS.Indicators.TradingSessionDescription**  
  *Структура для описания торговой сессии (ID, название).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces  
#### IMarketTimeProvider.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IMarketTimeProvider**  
  *Интерфейс для получения текущего времени и данных о торговых сессиях (например, начало/окончание сессии).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### Indicator.cs File Reference  

### Classes  
- **class ATAS.Indicators.IndicatorCategories**  
  *Класс для категоризации индикаторов (например, Осцилляторы, Волатильность и т.д.).*  
- **class ATAS.Indicators.Indicator**  
  *Базовый класс для создания пользовательских индикаторов, включает методы для расчётов, отображения и настройки.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### IndicatorCandle.cs File Reference  

### Classes  
- **class ATAS.Indicators.IndicatorCandle**  
  *Represents an Indicator Candle. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### IndicatorDataProvider.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IIndicatorDataProvider**  
  *Интерфейс для предоставления данных и сервисов, связанных с индикатором.*  
- **class ATAS.Indicators.IndicatorDataProvider**  
  *Реализация интерфейса IIndicatorDataProvider, предоставляющая доступ к данным и функциям индикатора.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces  
#### IndicatorSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.IndicatorSeries**  
  *Represents a custom data series for an indicator, derived from BaseDataSeries<decimal>. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### INotifyPanelPropertyChanged.cs File Reference  

### Classes  
- **interface ATAS.Indicators.INotifyPanelPropertyChanged**  
  *Интерфейс для уведомления клиентов о изменении значения свойства панели.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### InstrumentInfo.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IInstrumentInfo**  
  *Интерфейс для представления информации о торговом инструменте (например, название, тип, тик-размер).*  
- **class ATAS.Indicators.InstrumentInfo**  
  *Реализация интерфейса IInstrumentInfo, хранящая детали инструмента (код, описание, параметры торговли).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### IOnlineDataProvider.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IOnlineDataProvider**  
  *Интерфейс для работы с реальными данными рынка в режиме онлайн.*  
- **interface ATAS.Indicators.ITradesCache**  
  *Интерфейс для доступа к кешу сделок (trades).*  
- **interface ATAS.Indicators.IMarketByOrdersDataProvider**  
  *Интерфейс для получения данных о рыночных заказах (depth-книга).*  
- **interface ATAS.Indicators.IMarketByOrdersCache**  
  *Интерфейс для хранения кеша рыночных заказов.*  
- **interface ATAS.Indicators.IMarketByOrdersWithTradesCache**  
  *Интерфейс для объединённого кеша рыночных заказов и сделок.*  
- **interface ATAS.Indicators.ITimeMarketDataCache< out T>**  
  *Интерфейс для временного кеша данных (хранит данные за указанный период времени).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### IPlatformSettings.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IPlatformSettings**  
  *Интерфейс для доступа к настройкам платформы.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces  
#### IPropertiesEditor.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IPropertiesEditor**  
  *Интерфейс для редактирования свойств индикаторов или объектов на графике.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces  
#### IPropertiesEditorOwner.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IPropertiesEditorOwner**  
  *Интерфейс для объекта, который владеет редактором свойств (например, индикатор или график).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### ITradingManager.cs File Reference  

### Classes  
- **interface ATAS.Indicators.ITradingManager**  
  *Интерфейс для управления торговыми операциями (отправка ордеров, работа с позициями и т.д.).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### LineSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.LineSeries**  
  *Represents a horizontal line with a single value. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### MarketDataArg.cs File Reference  

### Classes  
- **class ATAS.Indicators.MarketDataArg**  
  *Represents a data point in the market. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Namespaces | Enumerations  
#### MarketDataType.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.MarketDataType**  
  ```  
  ATAS.Indicators.Bid = 0,  
  ATAS.Indicators.Ask = 1,  
  ATAS.Indicators.Trade = 2  
  ```  
  *Перечисление типов рыночных данных:  
  - **Bid** — цена покупки (спрос);  
  - **Ask** — цена продажи (предложение);  
  - **Trade** — реальные сделки.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Classes | Namespaces  
#### MarketDepthInfoProvider.cs File Reference  

### Classes  
- **interface ATAS.Indicators.IMarketDepthInfoProvider**  
  *Интерфейс для доступа к данным глубины рынка (market depth).*  
- **class ATAS.Indicators.MarketDepthInfoProvider**  
  *Реализация интерфейса IMarketDepthInfoProvider, предоставляющая информацию о стакане заявок.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Classes | Namespaces  
#### MarketDepthSnapshot.cs File Reference  

### Classes  
- **class ATAS.Indicators.MarketDepthSnapshot**  
  *Represents the end state of market depth over a specified time period. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Classes | Namespaces  
#### MarketDepthSnapshotRequest.cs File Reference  

### Classes  
- **class ATAS.Indicators.MarketDepthSnapshotRequest**  
  *Represents a request to retrieve a snapshot of the market depth for a specified time range. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Classes | Namespaces  
#### NotifyPropertyChangedBase.cs File Reference  

### Classes  
- **class ATAS.Indicators.NotifyPropertyChangedBase**  
  *Базовый класс для реализации интерфейса INotifyPropertyChanged (уведомление о изменении свойств).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces  
#### ObjectDataSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.ObjectDataSeries**  
  *Represents a data series of objects, allowing storing any type of data elements. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Namespaces | Enumerations  
#### ObjectType.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.ObjectType**  
  ```  
  ATAS.Indicators.Ellipse,  
  ATAS.Indicators.Triangle,  
  ATAS.Indicators.Rectangle,  
  ATAS.Indicators.Diamond,  
  ATAS.Indicators.OnlyCluster  
  ```  
  *Перечисление типов графических объектов для PriceSelectionDataSeries (например, эллипсы, треугольники, прямоугольники).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Classes | Namespaces  
#### PaintbarsDataSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.PaintbarsDataSeries**  
  *Represents a data series of paintbars, each element is a nullable Color value. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### PriceSelectionDataSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.PriceSelectionDataSeries**  
  *Represents a data series of price selection values, each element is a synchronized list of PriceSelectionValue. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### PriceSelectionValue.cs File Reference  

### Classes  
- **class ATAS.Indicators.PriceSelectionValue**  
  *Represents a class for defining price level selection in clusters and bars. Used in PriceSelectionDataSeries. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces | Enumerations | Functions  
#### PriceVolumeInfo.cs File Reference  

### Classes  
- **class ATAS.Indicators.PriceVolumeInfo**  
  *Represents information on volumes at a specific price. More...*  
- **class ATAS.Indicators.ValueArea**  
  *Represents information on Value area high/low. More...*  
- **class ATAS.Indicators.FixedProfileRequest**  
  *Represents a request for a fixed profile with a specific period. More...*  

### Interfaces  
- **interface ATAS.Indicators.ISupportedPriceInfo**  
  *Represents an interface for supporting price information. More...*  
- **interface ATAS.Indicators.IIntCandle**  
  *Represents an interface for an integer-based candle. More...*  
- **interface ATAS.Indicators.IKnowFixedProfiles**  
  *Represents an interface for objects that know fixed profiles and can request them. More...*  

### Enumerations  
- **enum ATAS.Indicators.FixedProfilePeriods**  
  ```  
  ATAS.Indicators.CurrentDay = 0,  
  ATAS.Indicators.LastDay = 1,  
  ATAS.Indicators.CurrentWeek = 2,  
  ATAS.Indicators.LastWeek = 3,  
  ATAS.Indicators.CurrentMonth = 4,  
  ATAS.Indicators.LastMonth = 5,  
  ATAS.Indicators.Contract = 6  
  ```  
  *Enumeration representing fixed profile periods.*  

### Functions  
- **record struct ATAS.Indicators.FixedProfileResponse(IndicatorCandle Scaled, IndicatorCandle Original)**  
  *Structure for fixed profile responses, containing scaled and original candle data.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Classes | Namespaces  
#### RangeDataSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.RangeDataSeries**  
  *Represents a data series of range values, each element is a RangeValue. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces  
#### RangeValue.cs File Reference  

### Classes  
- **class ATAS.Indicators.RangeValue**  
  *Represents an element of RangeDataSeries, containing a range of values (e.g., high/low). More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Classes | Namespaces  
#### RedrawArg.cs File Reference  

### Classes  
- **class ATAS.Indicators.RedrawArg**  
  *Represents the arguments for requesting a redraw of a chart. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Namespaces | Enumerations  
#### SelectionType.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.SelectionType**  
  ```  
  ATAS.Indicators.Full,  
  ATAS.Indicators.Bid,  
  ATAS.Indicators.Ask  
  ```  
  *Перечисление типов выбора для уровня цены:  
  - **Full** — полное выделение;  
  - **Bid** — цена покупки (спрос);  
  - **Ask** — цена продажи (предложение).*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Namespaces | Enumerations  
#### TradeDirection.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.TradeDirection**  
  ```  
  ATAS.Indicators.Between = 0,  
  ATAS.Indicators.Buy = 1,  
  ATAS.Indicators.Sell = 2  
  ```  
  *Перечисление направлений сделок:  
  - **Between** — нейтральное состояние;  
  - **Buy** — покупка;  
  - **Sell** — продажа.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**



### Classes | Namespaces  
#### ValueDataSeries.cs File Reference  

### Classes  
- **class ATAS.Indicators.ValueDataSeries**  
  *Represents a data series of decimal values, each element is a decimal. More...*  
- **class ATAS.Indicators.ValueDataSeries.BarColors**  
  *Manages colors per bar for the associated ValueDataSeries. More...*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**




### Namespaces | Enumerations  
#### VisualMode.cs File Reference  

### Enumerations  
- **enum ATAS.Indicators.VisualMode**  
  ```  
  ATAS.Indicators.Line,  
  ATAS.Indicators.Histogram,  
  ATAS.Indicators.Hash,  
  ATAS.Indicators.Block,  
  ATAS.Indicators.Cross,  
  ATAS.Indicators.Square,  
  ATAS.Indicators.Dots,  
  ATAS.Indicators.UpArrow,  
  ATAS.Indicators.DownArrow,  
  ATAS.Indicators.OnlyValueOnAxis,  
  ATAS.Indicators.Hide  
  ```  
  *Перечисление режимов визуализации данных на графике:  
  - **Line** — линейный график;  
  - **Histogram** — гистограмма;  
  - **Hash, Block** — заполненные/простые блоки;  
  - **Cross, Square, Dots** — маркеры (крестики, квадраты, точки);  
  - **UpArrow, DownArrow** — стрелки вверх/вниз;  
  - **OnlyValueOnAxis** — отображение значения на оси;  
  - **Hide** — скрытие серии.*  

### Namespaces  
- **namespace ATAS**  
- **namespace ATAS.Indicators**





### Typedefs  
#### File Members.pdf File Reference  

### Typedefs  
- **typedef CrossColor**  
  *Defined in GlobalUsings.cs: System.Windows.Media.Color*  
- **typedef CrossColors**  
  *Defined in GlobalUsings.cs: System.Windows.Media.Colors*  
- **typedef CrossKey**  
  *Defined in GlobalUsings.cs: System.Windows.Input.Key*  
- **typedef CrossKeyEventArgs**  
  *Defined in GlobalUsings.cs: System.Windows.Input.KeyEventArgs*  
- **typedef PortfolioSecurity**  
  *Defined in DataFeedsCore/TradeStatistics/Matching/GlobalUsings.cs*  
- **typedef PortfolioSecurityKey**  
  *Defined in DataFeedsCore/TradeStatistics/Matching/GlobalUsings.cs*  

### Аннотация:  
- **File Members**  
  *Список глобальных типов (typedef) и их местоположение в файлах для упрощения работы с цветами, клавишами и другими структурами.*












































































































































































































































































