




```csharp
// Пространство имен OFT.Attributes

// Содержит классы и перечисления для работы с атрибутами объектов OFT.

namespace OFT.Attributes
{
    // Перечисления

    // DataSeriesTypes - определяет типы серий данных, доступных для отображения на графике.
    enum DataSeriesTypes
    {
        // Band - представляет серию данных типа "полоса".
        Band,

        // Line - представляет серию данных типа "линия".
        Line,

        // Candle - представляет серию данных типа "свеча".
        Candle,

        // Value - представляет серию данных типа "значение".
        Value
    }

    // Классы

    // Extensions - класс расширений (назначение не указано в предоставленной документации).
    class Extensions {}

    // FeatureIdAttribute - атрибут для идентификации функциональных возможностей (назначение не указано в предоставленной документации).
    class FeatureIdAttribute {}

    // HelpLinkAttribute - атрибут для ссылки на справку (назначение не указано в предоставленной документации).
    class HelpLinkAttribute {}

    // IgnoreCloneAttribute - атрибут для игнорирования клонирования свойства или поля.
    class IgnoreCloneAttribute
    {
        // Игнорирует клонирование свойства или поля. Подробнее... (детали не предоставлены в выдержке)
    }

    // LogoAttribute - атрибут для логотипа (назначение не указано в предоставленной документации).
    class LogoAttribute {}

    // MappingAttribute - атрибут для маппинга (назначение не указано в предоставленной документации).
    class MappingAttribute {}

    // ParameterAttribute - атрибут для параметра (назначение не указано в предоставленной документации).
    class ParameterAttribute {}

    // ReferralLinkAttribute - атрибут для реферальной ссылки (назначение не указано в предоставленной документации).
    class ReferralLinkAttribute {}

    // Пространства имен

    // Editors - пространство имен для редакторов (детали не предоставлены в выдержке).
    namespace Editors {}

    // Properties - пространство имен для свойств (детали не предоставлены в выдержке).
    namespace Properties {}
}
```





## Методичка по пространству имен `OFT.Attributes.Editors` для создания индикаторов ATAS

### Пространство имен `OFT.Attributes.Editors`

Данное пространство имен содержит классы и перечисления, которые используются для определения атрибутов редакторов в ATAS. Атрибуты редакторов позволяют настраивать внешний вид и поведение элементов управления в панели настроек индикатора.

### Классы

В пространстве имен `OFT.Attributes.Editors` определены следующие классы:

*   **`CheckEditorAttribute`**:
    *   Представляет атрибут для создания редактора типа "флажок" (checkbox) для объектов.
    *   Используется для выбора логических значений (истина/ложь) в настройках индикатора.

*   **`ComboBoxEditorAttribute`**:
    *   Представляет атрибут для создания редактора типа "выпадающий список" (combobox).
    *   Позволяет пользователю выбирать одно значение из предопределенного списка опций.

*   **`DataSeriesEditorAttribute`**:
    *   Представляет атрибут для редактора, который позволяет выбирать серию данных (Data Series).
    *   Используется для выбора источника данных для индикатора, например, цены, объема и т.д.

*   **`IsExpandedAttribute`**:
    *   Представляет атрибут, который определяет, будет ли раздел настроек индикатора развернут по умолчанию.
    *   Позволяет контролировать начальное состояние видимости групп параметров в настройках.

*   **`MaskAttribute`**:
    *   Представляет атрибут для задания маски ввода для текстовых редакторов.
    *   Используется для форматирования ввода данных, например, даты, времени или чисел.

*   **`NumericEditorAttribute`**:
    *   Представляет атрибут для создания редактора числовых значений.
    *   Позволяет пользователю вводить и изменять числовые параметры индикатора.

*   **`PostValueModeAttribute`**:
    *   Представляет атрибут, определяющий режим отправки значения из редактора.
    *   Указывает, когда значение, измененное пользователем в редакторе, должно быть применено индикатором.

*   **`SelectDirectoryEditorAttribute`**:
    *   Представляет атрибут для создания редактора выбора директории (папки).
    *   Позволяет пользователю выбирать папку на компьютере для использования в индикаторе, например, для сохранения файлов.

*   **`SelectFileEditorAttribute`**:
    *   Представляет атрибут для создания редактора выбора файла.
    *   Позволяет пользователю выбирать файл на компьютере, например, файл настроек или данных.

*   **`TextEditorAttribute`**:
    *   Представляет атрибут для создания текстового редактора.
    *   Используется для ввода и редактирования текстовых параметров индикатора.
    *   Может предоставлять дополнительные метаданные для управления текстовыми редакторами, например, маску ввода или подсказку.

### Перечисления (Enumerations)

В пространстве имен `OFT.Attributes.Editors` определены следующие перечисления:

*   **`MaskTypes`**:
    *   Определяет типы масок, которые могут быть использованы с атрибутом `MaskAttribute`.
    *   Включает следующие значения:
        *   `None`: Маска не используется.
        *   `DateTime`: Маска для ввода даты и времени.
        *   `DateTimeAdvancingCaret`: Маска для даты и времени с автоматическим переходом к следующему элементу ввода.
        *   `Numeric`: Маска для ввода чисел.
        *   `RegEx`: Маска на основе регулярного выражения.
        *   `Regular`: Стандартная маска.
        *   `Simple`: Простая маска.
        *   `TimeSpan`: Маска для ввода временного интервала.
        *   `TimeSpanAdvancingCaret`: Маска для временного интервала с автоматическим переходом к следующему элементу ввода.

*   **`NumericEditorTypes`**:
    *   Определяет типы редакторов для числовых значений, используемые с атрибутом `NumericEditorAttribute`.
    *   Включает следующие значения:
        *   `Spin`: Редактор типа "счетчик" (spin control) с кнопками увеличения и уменьшения значения.
        *   `TrackBar`: Редактор типа "ползунок" (track bar) для выбора значения в диапазоне.

*   **`PostValueModes`**:
    *   Определяет режимы отправки значения для атрибута `PostValueModeAttribute`.
    *   Указывает, когда значение из редактора должно быть передано индикатору.
    *   Включает следующие значения:
        *   `OnChanged`: Значение отправляется при каждом изменении в редакторе.
        *   `OnLostFocus`: Значение отправляется, когда редактор теряет фокус (например, пользователь переходит к другому элементу управления).
        *   `Delayed`: Значение отправляется с задержкой после изменения в редакторе.

*   **`SelectAllOnModes`**:
    *   Определяет режимы выделения всего текста в редакторе при получении фокуса.
    *   Включает следующие значения:
        *   `None`: Автоматическое выделение текста не происходит.
        *   `GotFocus`: Весь текст выделяется, когда редактор получает фокус.
        *   `MouseUp`: Весь текст выделяется при отпускании кнопки мыши в редакторе.

*   **`SelectFileTypes`**:
    *   Определяет типы файлов, которые могут быть выбраны в редакторе выбора файла (`SelectFileEditorAttribute`).
    *   Включает следующие значения:
        *   `Open`: Режим открытия файла.
        *   `Save`: Режим сохранения файла.

**Примечание:** Эта методичка предназначена для предоставления подробной информации о пространстве имен `OFT.Attributes.Editors`. Она может быть использована ИИ для создания продвинутых стратегий и индикаторов для платформы ATAS, используя атрибуты редакторов для настройки параметров индикаторов.






**Методичка: Класс CheckEditorAttribute**

1. **Описание класса: `OFT.Attributes.Editors.CheckEditorAttribute`**
   *Аннотация:* Этот класс в C# называется `CheckEditorAttribute` и находится в пространстве имен `OFT.Attributes.Editors`.  Он используется для создания редактора типа "чекбокс" (галочка) для свойств объектов.  Слово "Attribute" в названии говорит о том, что это атрибут в C#, который используется для добавления дополнительной информации к коду, в данном случае, к свойствам объектов.

2. **Наследование:**
   *Аннотация:*  `CheckEditorAttribute` наследуется от класса `Attribute`. Это значит, что он берет свойства и поведение от базового класса `Attribute` и добавляет свою собственную функциональность. Наследование - важная концепция в объектно-ориентированном программировании, позволяющая повторно использовать код и создавать иерархии классов.

3. **Конструктор: `CheckEditorAttribute(string propertyName)`**
   *Аннотация:*  Конструктор класса `CheckEditorAttribute` - это специальный метод, который вызывается при создании нового объекта этого класса.  Этот конструктор принимает один параметр - `propertyName` типа `string`. `propertyName` - это имя свойства объекта, к которому будет привязан этот редактор-чекбокс.

4. **Параметр конструктора: `propertyName`**
   *Аннотация:*  Параметр `propertyName`  указывает, какое именно свойство объекта будет связано с чекбоксом. Когда пользователь будет менять состояние чекбокса (ставить или снимать галочку), значение связанного свойства объекта будет автоматически обновляться.

5. **Исключения конструктора: `ArgumentNullException`**
   *Аннотация:*  Конструктор может выбросить исключение типа `ArgumentNullException`, если параметр `propertyName` будет `null` (пустым, не содержащим значения).  Исключения используются в программировании для обработки ошибок. В данном случае, это означает, что при создании `CheckEditorAttribute` обязательно нужно указать имя свойства.

6. **Свойство: `PropertyName` [get]**
   *Аннотация:*  Класс `CheckEditorAttribute` имеет свойство `PropertyName` только для чтения (`[get]`). Это свойство возвращает имя свойства объекта, которое было задано при создании `CheckEditorAttribute` через конструктор.  То есть, это свойство позволяет узнать, с каким свойством объекта связан данный редактор-чекбокс.

7. **Подробное описание: "Check box editor for object."**
   *Аннотация:*  Описание класса еще раз подтверждает, что `CheckEditorAttribute` предназначен для создания редактора в виде чекбокса для объектов. Это означает, что в пользовательском интерфейсе для свойства объекта будет отображаться галочка, которую можно будет включать и выключать.

8. **Файл с кодом: `CheckEditorAttribute.cs`**
   *Аннотация:*  Исходный код класса `CheckEditorAttribute` находится в файле `CheckEditorAttribute.cs`.  Расширение `.cs` указывает на то, что это файл с кодом на языке C#.  Если вам понадобится посмотреть, как именно реализован этот класс, вам нужно будет открыть этот файл.

**Заключение:**

Класс `CheckEditorAttribute` - это атрибут в C#, который используется для того, чтобы указать, что для определенного свойства объекта в пользовательском интерфейсе нужно отображать редактор типа "чекбокс".  При создании атрибута нужно указать имя свойства, к которому он будет привязан.  Это позволяет удобно редактировать булевы (логические) свойства объектов, используя привычный элемент управления - галочку.








## Методичка по классу `OFT.Attributes.Editors.ComboBoxEditorAttribute`

**Описание класса:** `OFT.Attributes.Editors.ComboBoxEditorAttribute`

**Диаграмма наследования:**

```
Attribute
    ^
    |
OFT.Attributes.Editors.ComboBoxEditorAttribute
```

**Диаграмма взаимодействия:**

```
Attribute
    ^
    |
OFT.Attributes.Editors.ComboBoxEditorAttribute
```

**Публичные функции-члены:**

* **`ComboBoxEditorAttribute(params object[] itemsSource)`**
    * **Описание:** Конфигурирует атрибут редактора ComboBox.
    * **Параметры:**
        * `itemsSource`: `object[]` - Элементы для выбора в ComboBox.

* **`ComboBoxEditorAttribute(Type typeSource)`**
    * **Описание:** Конфигурирует атрибут редактора ComboBox.
    * **Параметры:**
        * `typeSource`: `Type` - Исходный тип должен наследоваться от `T:System.Collections.IEnumerable`.
    * **Исключения:**
        * `T:System.ArgumentNullException`:
            * **Параметр:** `typeSource`
            * **Описание:** `typeSource` является `null`.
        * `T:System.ArgumentException`:
            * **Параметр:** `typeSource`
            * **Описание:** `typeSource` не наследуется от `T:System.Collections.IEnumerable` или является конструктором с более чем одним параметром.

* **`IEnumerable GetItemsSource(object instance)`**

**Свойства:**

* **`string DisplayMember`** `[get, set]`
    * **Описание:** Возвращает или устанавливает имя члена в связанном источнике данных, чьи значения отображаются редактором.

* **`string ValueMember`** `[get, set]`
    * **Описание:** Возвращает или устанавливает имя члена в связанном источнике данных, чьи значения присваиваются значениям элементов.

* **`object[] ItemsSource`** `[get, set]`
    * **Описание:** Возвращает или устанавливает источник данных для выбора.

* **`bool IsTextEditable`** `[get, set]`
    * **Описание:** Возвращает или устанавливает, разрешено ли конечным пользователям редактировать текст, отображаемый в поле редактирования.

* **`bool AutoComplete`** `[get, set]`
    * **Описание:** Возвращает или устанавливает, включено ли автоматическое завершение.

* **`bool SelectItemWithNullValue`** `[get, set]`
    * **Описание:** Возвращает или устанавливает, выполняет ли редактор поиск элемента null в связанном источнике данных, когда значение редактора равно `null` (пусто).

**Документация для этого класса была сгенерирована из следующего файла:**

* `ComboBoxEditorAttribute.cs`

**Документация конструктора и деструктора:**

* **`ComboBoxEditorAttribute()`** `[1/2]`
    * `OFT.Attributes.Editors.ComboBoxEditorAttribute.ComboBoxEditorAttribute(params object[] itemsSource)`
    * **Описание:** Конфигурирует атрибут редактора ComboBox.
    * **Параметры:**
        * `itemsSource`: `object[]` - Элементы для выбора в ComboBox.

* **`ComboBoxEditorAttribute()`** `[2/2]`
    * `OFT.Attributes.Editors.ComboBoxEditorAttribute.ComboBoxEditorAttribute(Type typeSource)`
    * **Описание:** Конфигурирует атрибут редактора ComboBox.
    * **Параметры:**
        * `typeSource`: `Type` - Исходный тип должен наследоваться от `T:System.Collections.IEnumerable`.
    * **Исключения:**
        * `T:System.ArgumentNullException`:
            * **Параметр:** `typeSource`
            * **Описание:** `typeSource` является `null`.
        * `T:System.ArgumentException`:
            * **Параметр:** `typeSource`
            * **Описание:** `typeSource` не наследуется от `T:System.Collections.IEnumerable` или является конструктором с более чем одним параметром.

**Документация функций-членов:**

* **`GetItemsSource()`**
    * `IEnumerable OFT.Attributes.Editors.ComboBoxEditorAttribute.GetItemsSource(object instance)`

**Документация свойств:**

* **`AutoComplete`**
    * `bool OFT.Attributes.Editors.ComboBoxEditorAttribute.AutoComplete` `[get, set]`
    * **Описание:** Возвращает или устанавливает, включено ли автоматическое завершение.

* **`DisplayMember`**
    * `string OFT.Attributes.Editors.ComboBoxEditorAttribute.DisplayMember` `[get, set]`
    * **Описание:** Возвращает или устанавливает имя члена в связанном источнике данных, чьи значения отображаются редактором.

* **`IsTextEditable`**
    * `bool OFT.Attributes.Editors.ComboBoxEditorAttribute.IsTextEditable` `[get, set]`
    * **Описание:** Возвращает или устанавливает, разрешено ли конечным пользователям редактировать текст, отображаемый в поле редактирования.

* **`ItemsSource`**
    * `object [] OFT.Attributes.Editors.ComboBoxEditorAttribute.ItemsSource` `[get, set]`
    * **Описание:** Возвращает или устанавливает источник данных для выбора.

* **`SelectItemWithNullValue`**
    * `bool OFT.Attributes.Editors.ComboBoxEditorAttribute.SelectItemWithNullValue` `[get, set]`
    * **Описание:** Возвращает или устанавливает, выполняет ли редактор поиск элемента null в связанном источнике данных, когда значение редактора равно `null` (пусто).

* **`ValueMember`**
    * `string OFT.Attributes.Editors.ComboBoxEditorAttribute.ValueMember` `[get, set]`
    * **Описание:** Возвращает или устанавливает имя члена в связанном источнике данных, чьи значения присваиваются значениям элементов.









## Методичка по классу `OFT.Attributes.Editors.DataSeriesEditorAttribute` для разработки индикаторов ATAS

**Введение:**

Данная методичка предназначена для подробного описания класса `OFT.Attributes.Editors.DataSeriesEditorAttribute`, используемого в платформе ATAS для создания индикаторов. Информация подготовлена на основе документации и предназначена для использования Искусственным Интеллектом при разработке продвинутых торговых стратегий.

**1. Общая информация о классе:**

*   **Полное имя класса:** `OFT.Attributes.Editors.DataSeriesEditorAttribute`
*   **Тип:** Класс (Class Reference)
*   **Назначение:** Атрибут редактора DataSeries. Используется для указания типа DataSeries, который должен быть использован в редакторе свойств индикатора.
*   **Расположение:**  Пространство имен `OFT.Attributes.Editors`.

**2. Диаграмма наследования:**

```
Attribute
    ↑
OFT.Attributes.Editors.DataSeriesEditorAttribute
[legend]
```

*   **Описание:** Класс `OFT.Attributes.Editors.DataSeriesEditorAttribute` наследуется от класса `Attribute`. Это означает, что он является атрибутом и может быть использован для метаданных классов, методов, свойств и других элементов кода.

**3. Диаграмма коллаборации:**

```
Attribute
    ↑
OFT.Attributes.Editors.DataSeriesEditorAttribute
[legend]
```

*   **Описание:** Диаграмма коллаборации показывает взаимодействие класса `OFT.Attributes.Editors.DataSeriesEditorAttribute` с классом `Attribute`. В данном случае, `OFT.Attributes.Editors.DataSeriesEditorAttribute` использует функциональность класса `Attribute` через механизм наследования.

**4. Открытые члены класса (Public Member Functions):**

*   **Конструктор:**
    *   **Имя:** `DataSeriesEditorAttribute`
    *   **Описание:** Конструктор класса `DataSeriesEditorAttribute`.
    *   **Сигнатура:** `DataSeriesEditorAttribute(DataSeriesTypes dataSeriesType)`
    *   **Параметры:**
        *   `dataSeriesType`: Тип параметра `DataSeriesTypes`. Указывает тип DataSeries, для которого предназначен атрибут редактора.
    *   **Полная сигнатура (из документации):** `OFT.Attributes.Editors.DataSeriesEditorAttribute.DataSeriesEditorAttribute(DataSeriesTypes datas` *(вероятно, опечатка в документации, должно быть `dataSeriesType`)*

**5. Свойства (Properties):**

*   **Свойство `DataSeriesType`:**
    *   **Тип:** `DataSeriesTypes`
    *   **Описание:**  Возвращает или устанавливает тип DataSeries, связанный с атрибутом редактора.
    *   **Доступ:** `[get]` (только для чтения после установки в конструкторе).
    *   **Полное имя (из документации):** `OFT.Attributes.Editors.DataSeriesEditorAttribute.DataSeriesType`

**6. Документация свойства `DataSeriesType`:**

*   **Описание (из документации):**  `DataSeriesTypes OFT.Attributes.Editors.DataSeriesEditorAttribute.DataSeriesType get`

**7. Расположение файла с кодом класса:**

*   **Файл:** `DataSeriesEditorAttribute.cs`

**8. Заключение:**

Класс `OFT.Attributes.Editors.DataSeriesEditorAttribute` является атрибутом, предназначенным для настройки редактора свойств индикаторов в платформе ATAS. Он позволяет указать, какой тип `DataSeries` должен использоваться для определенного свойства индикатора.  Это достигается через конструктор, принимающий параметр типа `DataSeriesTypes`, и свойство `DataSeriesType`, которое возвращает установленный тип. Данный атрибут используется для улучшения пользовательского интерфейса при настройке индикаторов, предоставляя редактор свойств, специфичный для выбранного типа DataSeries.









```markdown
## OFT.Attributes.Editors.IsExpandedAttribute Class Reference

### Введение

`OFT.Attributes.Editors.IsExpandedAttribute` является классом, описание которого представлено в данной методичке.

### Диаграмма наследования для OFT.Attributes.Editors.IsExpandedAttribute:

```
Attribute
    ↑
OFT.Attributes.Editors.IsExpandedAttribute
```

**Описание диаграммы:**

Диаграмма наследования показывает, что класс `OFT.Attributes.Editors.IsExpandedAttribute` наследуется от класса `Attribute`.

- **Attribute** (Базовый класс)
- **OFT.Attributes.Editors.IsExpandedAttribute** (Производный класс)

Стрелка направлена от `OFT.Attributes.Editors.IsExpandedAttribute` к `Attribute`, указывая на отношение наследования.

### Диаграмма коллаборации для OFT.Attributes.Editors.IsExpandedAttribute:

```
Attribute
    ↑
OFT.Attributes.Editors.IsExpandedAttribute
```

**Описание диаграммы:**

Диаграмма коллаборации показывает взаимодействие класса `OFT.Attributes.Editors.IsExpandedAttribute` с классом `Attribute`.

- **Attribute**
- **OFT.Attributes.Editors.IsExpandedAttribute**

Диаграмма демонстрирует, что `OFT.Attributes.Editors.IsExpandedAttribute` взаимодействует или использует `Attribute`.

### Файл документации:

Документация для этого класса была сгенерирована из файла:

- IsExpandedAttribute.cs
```








Конечно, вот подробная методичка по классу `OFT.Attributes.Editors.MaskAttribute` с аннотациями на русском языке, подготовленная специально для обучения ИИ и вас:

## Методичка по классу `OFT.Attributes.Editors.MaskAttribute` для создания индикаторов ATAS

**Название:** Класс `OFT.Attributes.Editors.MaskAttribute` для управления форматом параметров индикаторов ATAS

**Описание:**

Класс `OFT.Attributes.Editors.MaskAttribute` предназначен для задания масок форматирования и валидации для свойств (параметров) в индикаторах ATAS. Маски позволяют контролировать, как пользователь вводит данные, например, ограничивать ввод только числами, задавать формат даты или другие специфические форматы. Это помогает сделать параметры индикаторов более понятными и защитить от ошибок ввода.

**Основные характеристики класса:**

1.  **Наследование:**
    - `MaskAttribute` наследуется от класса `Attribute`. В программировании, атрибуты используются для добавления дополнительной информации к коду (например, к классам, свойствам, методам).
    - *Аннотация (рус.):* `MaskAttribute` является специальным типом "метки" (`Attribute`), которую можно применить к свойствам вашего кода, чтобы указать, как эти свойства должны обрабатываться, в данном случае — как форматировать ввод данных для них.

2.  **Назначение:**
    - Используется для определения формата ввода и отображения данных для свойств, которые являются параметрами индикатора.
    - *Аннотация (рус.):*  Этот класс помогает вам задать правила для ввода значений параметров индикатора. Например, вы можете потребовать, чтобы цена вводилась с двумя знаками после запятой или чтобы дата была в определенном формате.

**Конструкторы класса `MaskAttribute`:**

1.  **`MaskAttribute()` [1/2]**
    - **Описание:** Пустой конструктор. Используется, когда вы хотите задать свойства маски (например, `Mask`, `MaskType`) позже, через свойства объекта.
    - *Аннотация (рус.):* Это как создать "пустую форму" для маски. Вы можете заполнить детали маски позже, используя свойства `MaskAttribute`.

2.  **`MaskAttribute(MaskTypes type, string mask)` [2/2]**
    - **Параметры:**
        - `MaskTypes type`:  Указывает тип маски (например, числовая, строковая, дата). Тип `MaskTypes` вероятно является перечислением (enum), определяющим доступные типы масок.
        - `string mask`: Строка, определяющая формат маски. Например, `"0.00"` для числовой маски с двумя знаками после запятой, или `"dd.MM.yyyy"` для даты.
    - **Описание:** Конструктор, который позволяет сразу задать тип маски и саму маску при создании объекта `MaskAttribute`.
    - *Аннотация (рус.):*  Это более быстрый способ, если вы уже знаете, какой тип маски (`type`) и какой формат (`mask`) вам нужны. Вы задаете их сразу при создании `MaskAttribute`.

**Свойства класса `MaskAttribute`:**

1.  **`Culture` [get, set]**
    - **Тип:** `string`
    - **Описание:**  Задает культуру (язык и региональные стандарты) для форматирования и валидации. Культура влияет на правила форматирования чисел, дат и других данных.
    - *Аннотация (рус.):* `Culture` позволяет настроить маску под разные языки и страны. Например, в разных странах разделители целой и дробной части в числах могут отличаться.

2.  **`Mask` [get, set]**
    - **Тип:** `string`
    - **Описание:**  Строка маски, определяющая формат. Содержание маски зависит от типа маски (`MaskType`).
    - *Аннотация (рус.):*  `Mask` — это шаблон, по которому будут проверяться и форматироваться данные. Например, `"#.##"` для чисел с двумя знаками после запятой, `"дд/мм/гггг"` для даты.

3.  **`MaskAsDisplayFormat` [get, set]**
    - **Тип:** `bool` (логическое значение - истина или ложь)
    - **Описание:**  Указывает, должна ли маска использоваться только для отображения (форматирования) значения, но не для строгой валидации ввода. Если `true`, то маска применяется только визуально, но пользователь может ввести данные в любом формате. Если `false` (или не указано), то ввод должен соответствовать маске.
    - *Аннотация (рус.):*  `MaskAsDisplayFormat` определяет, насколько строгой будет маска. Если `true`, то маска просто показывает, как должны выглядеть данные, но не запрещает пользователю вводить что-то другое. Если `false`, то ввод должен точно соответствовать маске.

4.  **`MaskType` [get, set]**
    - **Тип:** `MaskTypes` (вероятно, перечисление)
    - **Описание:**  Определяет тип маски, например, числовая, дата, строка.  Тип маски определяет, как интерпретируется строка `Mask`. Возможные значения `MaskTypes` (из документации не видны, нужно смотреть определение `MaskTypes`): вероятно, `Numeric`, `DateTime`, `String`, `RegEx` (регулярные выражения) и т.д.
    - *Аннотация (рус.):*  `MaskType` говорит системе, какой вид маски вы используете. Например, вы выбираете `Numeric`, если хотите задать маску для чисел, или `DateTime` для дат.

5.  **`MethodName` [get, set]**
    - **Тип:** `string`
    - **Описание:**  Имя метода, который может быть связан с этим атрибутом маски. Возможно, используется для дополнительной логики обработки или валидации данных, выходящей за рамки стандартной маски.
    - *Аннотация (рус.):*  `MethodName` позволяет привязать к маске специальную функцию (метод), которая будет дополнительно обрабатывать данные. Это для более сложных случаев, когда простой маски недостаточно.

**Пример использования в коде индикатора ATAS (гипотетический, StimulScript):**

```csharp
// Предположим, что в StimulScript есть возможность использовать атрибуты для параметров индикатора

using System; // Добавляем пространство имен System для доступа к DateTime

public class MyIndicator : Indicator // Указываем, что это индикатор
{
    [InputParameter("Цена открытия", Mask = "0.00", MaskType = MaskTypes.Numeric)] // Атрибут для параметра "Цена открытия"
    public decimal OpenPrice { get; set; } // Свойство для цены открытия

    [InputParameter("Дата начала", Mask = "dd.MM.yyyy", MaskType = MaskTypes.DateTime)] // Атрибут для параметра "Дата начала"
    public DateTime StartDate { get; set; } // Свойство для даты начала

    [InputParameter("Название инструмента", Mask = "[A-Z]{1,3}", MaskType = MaskTypes.RegEx)] // Пример с RegEx маской (нужно проверить поддержку в ATAS)
    public string InstrumentName { get; set; } // Свойство для названия инструмента

    public MyIndicator()
    {
        // Конструктор индикатора (если нужен)
    }

    protected override void OnCalculate(int barIndex)
    {
        // Логика расчета индикатора
        // ...
    }
}
```

**Пояснение к примеру:**

-   `[InputParameter(...)]` -  Предполагаемый атрибут в StimulScript для определения параметров индикатора. (В реальности синтаксис может отличаться, нужно проверить документацию ATAS StimulScript).
-   `Mask = "0.00"` -  Задает маску для числового ввода с двумя знаками после запятой для параметра "Цена открытия".
-   `MaskType = MaskTypes.Numeric` - Указывает, что маска числовая.
-   `Mask = "dd.MM.yyyy"` - Задает формат даты "день.месяц.год" для параметра "Дата начала".
-   `MaskType = MaskTypes.DateTime` - Указывает, что маска для даты.
-   `Mask = "[A-Z]{1,3}"` -  Пример использования регулярного выражения (RegEx) для маски.  `[A-Z]{1,3}` означает от 1 до 3 заглавных букв латинского алфавита. Это для параметра "Название инструмента".
-   `MaskType = MaskTypes.RegEx` - Указывает, что используется маска в виде регулярного выражения.

**Важные замечания для ИИ и пользователя:**

1.  **Проверить `MaskTypes` в ATAS StimulScript:** Необходимо выяснить, какие типы масок (`MaskTypes`) поддерживаются в StimulScript ATAS. Документация на скриншотах не дает полного списка.
2.  **Синтаксис атрибутов в StimulScript:**  Синтаксис использования атрибутов `[InputParameter(...)]` и их поддержка в StimulScript ATAS нужно уточнить в официальной документации ATAS. Возможно, параметры индикатора задаются иначе.
3.  **Регулярные выражения:**  Поддержка `MaskType = MaskTypes.RegEx` и синтаксис регулярных выражений в ATAS StimulScript также требует проверки.
4.  **Цель использования:**  Понимание, как именно ATAS использует `MaskAttribute` (или аналогичный механизм) для параметров индикаторов, поможет правильно применять этот инструмент. Возможно, есть примеры в SDK или документации ATAS.

**Заключение:**

Класс `OFT.Attributes.Editors.MaskAttribute` предоставляет мощный механизм для управления форматом и валидацией параметров индикаторов в ATAS. Понимание его свойств и конструкторов позволит вам создавать более удобные и надежные индикаторы, контролируя ввод пользовательских данных. Для практического применения необходимо изучить документацию ATAS StimulScript и проверить, как именно атрибуты и маски используются для определения параметров индикаторов.






## Методичка для АТРИБУТА `NumericEditorAttribute` в ATAS (близко к исходному тексту)

**OFT.Attributes.Editors.NumericEditorAttribute**
**Class Reference**

**Inheritance diagram for OFT.Attributes.Editors.NumericEditorAttribute:**

```
Attribute
    ^
    |
OFT.Attributes.Editors.NumericEditorAttribute
```

`[legend]`

**Collaboration diagram for OFT.Attributes.Editors.NumericEditorAttribute:**

```
Attribute
    ^
    |
OFT.Attributes.Editors.NumericEditorAttribute
```

`[legend]`

**Public Member Functions**

*   `NumericEditorAttribute ()`
*   `NumericEditorAttribute (NumericEditorTypes editorType, double minValue)`
*   `NumericEditorAttribute (NumericEditorTypes editorType, Double minValue, double maxValue)`
*   `NumericEditorAttribute (double minValue, double maxValue)`
*   `NumericEditorAttribute (double minValue)`

**Properties**

*   `NumericEditorTypes EditorType`  `[get, set]`
*   `double? MinValue` `[get]`
*   `double? MaxValue` `[get]`
*   `double Step` `[get, set]`
*   `string DisplayFormat` `[get, set]`

**Constructor & Destructor Documentation**

*   **◆ NumericEditorAttribute() `[1/5]`**

    `OFT.Attributes.Editors.NumericEditorAttribute.NumericEditorAttribute()`

*   **◆ NumericEditorAttribute() `[2/5]`**

    `OFT.Attributes.Editors.NumericEditorAttribute.NumericEditorAttribute (NumericEditorTypes editorType, double minValue)`

*   **◆ NumericEditorAttribute() `[3/5]`**

    `OFT.Attributes.Editors.NumericEditorAttribute.NumericEditorAttribute (NumericEditorTypes editorType, Double minValue, double maxValue)`

*   **◆ NumericEditorAttribute() `[4/5]`**

    `OFT.Attributes.Editors.NumericEditorAttribute.NumericEditorAttribute (double minValue, double maxValue)`

*   **◆ NumericEditorAttribute() `[5/5]`**

    `OFT.Attributes.Editors.NumericEditorAttribute.NumericEditorAttribute (double minValue)`

**Property Documentation**

*   **◆ DisplayFormat**

    `string OFT.Attributes.Editors.NumericEditorAttribute.DisplayFormat`
    `[get] [set]`

*   **◆ EditorType**

    `NumericEditorTypes OFT.Attributes.Editors.NumericEditorAttribute.EditorType`
    `[get] [set]`

*   **◆ MaxValue**

    `double? OFT.Attributes.Editors.NumericEditorAttribute.MaxValue`
    `[get]`

*   **◆ MinValue**

    `double? OFT.Attributes.Editors.NumericEditorAttribute.MinValue`
    `[get]`

*   **◆ Step**

    `double OFT.Attributes.Editors.NumericEditorAttribute.Step`
    `[get] [set]`

**The documentation for this class was generated from the following file:**

*   `NumericEditorAttribute.cs`

**Пояснения и аннотации (для изучения):**

*   **`OFT.Attributes.Editors.NumericEditorAttribute`**:  Полное имя класса атрибута, указывающее на пространство имен и имя класса.
*   **`Class Reference`**:  Указывает, что это документация по классу.
*   **`Inheritance diagram` и `Collaboration diagram`**: Диаграммы наследования и взаимодействия классов. Показывают, что `NumericEditorAttribute` наследуется от `Attribute`.
*   **`Public Member Functions`**:  Список публичных конструкторов класса `NumericEditorAttribute`.  Каждый пункт соответствует перегрузке конструктора с разными параметрами.
*   **`Properties`**: Список публичных свойств класса `NumericEditorAttribute`.  `[get, set]` означает, что свойство можно читать и изменять. `[get]` означает, что свойство доступно только для чтения.
*   **`Constructor & Destructor Documentation`**:  Более детальное описание каждого конструктора. `[1/5]`, `[2/5]` и т.д.  вероятно, указывают на номер перегрузки конструктора.
*   **`Property Documentation`**:  Более детальное описание каждого свойства, включая полное имя свойства и указание на `[get]` или `[get] [set]`.
*   **`NumericEditorTypes`**:  Тип данных "перечисление" (enum), используемый для свойства `EditorType`.  Определяет варианты редактора.
*   **`double?`**:  Тип данных "Nullable double". Может содержать значение `double` или `null` (отсутствие значения).
*   **`double`**:  Стандартный тип данных для чисел с плавающей точкой двойной точности.
*   **`string`**:  Тип данных "строка".
*   **`[legend]`**:  Вероятно, обозначение для "легенды" к диаграммам, хотя в данном контексте не используется явно.
*   **`NumericEditorAttribute.cs`**:  Имя файла, в котором находится исходный код класса.







## Методичка по классу `OFT.Attributes.Editors.PostValueModeAttribute`

**Ссылка на класс:** `OFT.Attributes.Editors.PostValueModeAttribute`

**Диаграмма наследования:**

```
Attribute
    ↑
OFT.Attributes.Editors.PostValueModeAttribute
```
*Аннотация: `Attribute` является базовым классом, от которого наследуется `PostValueModeAttribute`. Это означает, что `PostValueModeAttribute` обладает всеми свойствами и методами класса `Attribute`.*

**Диаграмма взаимодействия:**

```
Attribute
    ↑
OFT.Attributes.Editors.PostValueModeAttribute
```
*Аннотация: Эта диаграмма показывает взаимодействие `PostValueModeAttribute` с классом `Attribute`. В данном случае, это отношение наследования.*

**Публичные методы:**

*   `PostValueModeAttribute(PostValueModes mode)`
    *   Параметры:
        *   `PostValueModes mode` - определяет режим постобработки значений. Тип данных `PostValueModes` представляет собой перечисление возможных режимов.
*   `PostValueModeAttribute()`
    *   Конструктор без параметров.

**Свойства:**

*   `Mode`
    *   Тип: `PostValueModes`
    *   Доступ: `[get]` - только чтение.
    *   Описание: Режим постобработки значений, устанавливается через конструктор.
*   `DelayMilliseconds`
    *   Тип: `int` - целочисленный тип данных.
    *   Доступ: `[get, set]` - чтение и запись.
    *   Описание: Задержка в миллисекундах.
*   `SelectAllOnMode`
    *   Тип: `SelectAllOnModes`
    *   Значение по умолчанию: `SelectAllOnModes.MouseUp`
    *   Доступ: `[get, set]` - чтение и запись.
    *   Описание: Определяет режим выделения всего текста. По умолчанию используется режим `MouseUp`.

**Документация конструктора и деструктора:**

*   `PostValueModeAttribute()` [1/2]
    *   Первая версия конструктора.
*   `PostValueModeAttribute()` [2/2]
    *   Вторая версия конструктора (вероятно, перегрузка).
*   `PostValueModeAttribute(PostValueModes mode)`
    *   Конструктор, принимающий параметр `mode` типа `PostValueModes`.

**Документация свойств:**

*   `DelayMilliseconds`
    *   Тип: `int`
    *   Пространство имен: `OFT.Attributes.Editors.PostValueModeAttribute.DelayMilliseconds`
    *   Доступ: `get`, `set`
*   `Mode`
    *   Тип: `PostValueModes`
    *   Пространство имен: `OFT.Attributes.Editors.PostValueModeAttribute.Mode`
    *   Доступ: `get`
*   `SelectAllOnMode`
    *   Тип: `SelectAllOnModes`
    *   Пространство имен: `OFT.Attributes.Editors.PostValueModeAttribute.SelectAllOnMode`
    *   Значение по умолчанию: `SelectAllOnModes.MouseUp`
    *   Доступ: `get`, `set`

**Файл документации для класса был сгенерирован из:**

*   `PostValueModeAttribute.cs`









## Методичка по классу `OFT.Attributes.Editors.SelectDirectoryEditorAttribute`

**Описание класса:** `OFT.Attributes.Editors.SelectDirectoryEditorAttribute`

**Наследование:**

```
Attribute
  OFT.Attributes.Editors.SelectDirectoryEditorAttribute
```

*Аннотация: Класс `SelectDirectoryEditorAttribute` наследуется от базового класса `Attribute`. Это означает, что он является атрибутом, который можно использовать для модификации свойств или полей других классов.*

**Диаграмма коллаборации:**

```
Attribute
  OFT.Attributes.Editors.SelectDirectoryEditorAttribute
```

*Аннотация: Диаграмма коллаборации показывает взаимосвязь между классом `SelectDirectoryEditorAttribute` и классом `Attribute`, подтверждая отношение наследования.*

**Публичные члены класса (Public Member Functions):**

*   `SelectDirectoryEditorAttribute ()`
    *   Описание: Конструктор класса `SelectDirectoryEditorAttribute` без параметров.

*   `SelectDirectoryEditorAttribute (SpecialFolder initialSpecialFolder)`
    *   Описание: Конструктор класса `SelectDirectoryEditorAttribute` с параметром `initialSpecialFolder` типа `SpecialFolder`.

*Аннотация: Класс имеет два конструктора. Один - без параметров, другой - принимает параметр `initialSpecialFolder` типа `SpecialFolder`. Конструкторы используются для создания экземпляров класса.*

**Свойства (Properties):**

*   `bool IsTextEditable`  `[get, set]`
    *   Описание: Свойство типа `bool`, определяющее, является ли текстовое поле редактируемым. Доступно для чтения (`get`) и записи (`set`).

*   `bool ShowNewFolderButton` `[get, set]`
    *   Описание: Свойство типа `bool`, определяющее, отображается ли кнопка "New Folder" (Новая папка). Доступно для чтения (`get`) и записи (`set`).

*   `SpecialFolder? InitialSpecialFolder` `[get]`
    *   Описание: Свойство типа `SpecialFolder?` (Nullable `SpecialFolder`), представляющее начальную специальную папку. Доступно только для чтения (`get`).

*Аннотация: Класс имеет три свойства: `IsTextEditable`, `ShowNewFolderButton` и `InitialSpecialFolder`. Первые два - типа `bool` и доступны для чтения и записи. Третье - типа `SpecialFolder?` и доступно только для чтения.*

**Документация конструктора и деструктора (Constructor & Destructor Documentation):**

*   `SelectDirectoryEditorAttribute()` `[1/2]`
    *   Описание: Детальная документация для конструктора `SelectDirectoryEditorAttribute()` без параметров.

*   `SelectDirectoryEditorAttribute()` `[2/2]`
    *   Описание: Продолжение документации для конструктора `SelectDirectoryEditorAttribute()` без параметров.

*   `OFT.Attributes.Editors.SelectDirectoryEditorAttribute.SelectDirectoryEditorAttribute (SpecialFold` ... (обрезано в OCR)
    *   Описание: Документация для конструктора `SelectDirectoryEditorAttribute (SpecialFolder initialSpecialFolder)`.

*Аннотация: Раздел содержит подробную документацию по конструкторам класса, возможно, с примерами использования или дополнительными пояснениями.*

**Документация свойств (Property Documentation):**

*   `InitialSpecialFolder`
    *   `SpecialFolder? OFT.Attributes.Editors.SelectDirectoryEditorAttribute.InitialSpecialFolder` `get`
    *   Описание: Детальная документация для свойства `InitialSpecialFolder`, указывающая его тип, полное имя и доступность только для чтения (`get`).

*   `IsTextEditable`
    *   `bool OFT.Attributes.Editors.SelectDirectoryEditorAttribute.IsTextEditable` `get` `set`
    *   Описание: Детальная документация для свойства `IsTextEditable`, указывающая его тип, полное имя и доступность для чтения и записи (`get`, `set`).

*   `ShowNewFolderButton`
    *   `bool OFT.Attributes.Editors.SelectDirectoryEditorAttribute.ShowNewFolderButton` `get` `set`
    *   Описание: Детальная документация для свойства `ShowNewFolderButton`, указывающая его тип, полное имя и доступность для чтения и записи (`get`, `set`).

*Аннотация: Раздел предоставляет подробное описание каждого свойства класса, включая тип данных, полное имя и методы доступа (get, set).*

**Файл документации:**

*   `SelectDirectoryEditorAttribute.cs`
    *   Описание: Указывает имя файла, из которого была сгенерирована документация для класса `SelectDirectoryEditorAttribute`.

*Аннотация: Указывается имя файла исходного кода (`.cs` файл), в котором определен класс `SelectDirectoryEditorAttribute`.*







## Методичка по классу `OFT.Attributes.Editors.SelectFileEditorAttribute`

### Описание класса

**`OFT.Attributes.Editors.SelectFileEditorAttribute`** -  Класс Атрибут.

**Диаграмма наследования:**

```
Attribute
└── OFT.Attributes.Editors.SelectFileEditorAttribute
```

**Диаграмма взаимодействия:**

```
Attribute
└── OFT.Attributes.Editors.SelectFileEditorAttribute
```

### Открытые члены класса (Public Member Functions)

*   **`SelectFileEditorAttribute()`**
    *   Конструктор класса.
*   **`SelectFileEditorAttribute(SpecialFolder initialSpecialFolder)`**
    *   Конструктор класса, принимающий параметр `initialSpecialFolder` типа `SpecialFolder`.

### Свойства (Properties)

*   **`IsTextEditable`**  `[get, set]`  `bool`
    *   Свойство типа `bool`, доступное для чтения и записи.
*   **`SelectFileType`**  `[get, set]`  `SelectFileTypes`
    *   Свойство типа `SelectFileTypes`, доступное для чтения и записи.
*   **`Multiselect`**  `[get, set]`  `bool`
    *   Свойство типа `bool`, доступное для чтения и записи.
*   **`InitialFolder`**  `[get, set]`  `string`
    *   Свойство типа `string`, доступное для чтения и записи.
*   **`InitialSpecialFolder`**  `[get]`  `SpecialFolder?`
    *   Свойство типа `SpecialFolder?` (Nullable SpecialFolder), доступное только для чтения.
*   **`Filter`**  `[get, set]`  `string`
    *   Свойство типа `string`, доступное для чтения и записи.

### Документация конструкторов и деструкторов

*   **`SelectFileEditorAttribute() [1/2]`**
    ```csharp
    OFT.Attributes.Editors.SelectFileEditorAttribute.SelectFileEditorAttribute ( )
    ```
    *   Первый конструктор класса `SelectFileEditorAttribute`, не принимает параметров.

*   **`SelectFileEditorAttribute() [2/2]`**
    ```csharp
    OFT.Attributes.Editors.SelectFileEditorAttribute.SelectFileEditorAttribute (SpecialFolder initial
    ```
    *   Второй конструктор класса `SelectFileEditorAttribute`, принимает параметр `initial` типа `SpecialFolder`. (Полная сигнатура в исходном документе обрезана).

### Документация свойств

*   **`Filter`**
    ```csharp
    string OFT.Attributes.Editors.SelectFileEditorAttribute.Filter
    ```
    `[get, set]`
    *   Свойство `Filter` типа `string` для класса `OFT.Attributes.Editors.SelectFileEditorAttribute`. Доступно для получения (get) и установки (set) значения.

*   **`InitialFolder`**
    ```csharp
    string OFT.Attributes.Editors.SelectFileEditorAttribute.InitialFolder
    ```
    `[get, set]`
    *   Свойство `InitialFolder` типа `string` для класса `OFT.Attributes.Editors.SelectFileEditorAttribute`. Доступно для получения (get) и установки (set) значения.

*   **`InitialSpecialFolder`**
    ```csharp
    SpecialFolder? OFT.Attributes.Editors.SelectFileEditorAttribute.InitialSpecialFolder
    ```
    `[get]`
    *   Свойство `InitialSpecialFolder` типа `SpecialFolder?` для класса `OFT.Attributes.Editors.SelectFileEditorAttribute`. Доступно только для получения (get) значения.

*   **`IsTextEditable`**
    ```csharp
    bool OFT.Attributes.Editors.SelectFileEditorAttribute.IsTextEditable
    ```
    `[get, set]`
    *   Свойство `IsTextEditable` типа `bool` для класса `OFT.Attributes.Editors.SelectFileEditorAttribute`. Доступно для получения (get) и установки (set) значения.

*   **`Multiselect`**
    ```csharp
    bool OFT.Attributes.Editors.SelectFileEditorAttribute.Multiselect
    ```
    `[get, set]`
    *   Свойство `Multiselect` типа `bool` для класса `OFT.Attributes.Editors.SelectFileEditorAttribute`. Доступно для получения (get) и установки (set) значения.

*   **`SelectFileType`**
    ```csharp
    SelectFileTypes OFT.Attributes.Editors.SelectFileEditorAttribute.SelectFileType
    ```
    `[get, set]`
    *   Свойство `SelectFileType` типа `SelectFileTypes` для класса `OFT.Attributes.Editors.SelectFileEditorAttribute`. Доступно для получения (get) и установки (set) значения.

### Файл генерации документации

*   Документация для этого класса была сгенерирована из файла:
    *   `SelectFileEditorAttribute.cs`







## Методичка по классу `OFT.Attributes.Editors.TextEditorAttribute`

### Определение класса

**`OFT.Attributes.Editors.TextEditorAttribute Class Reference`**

**Описание:** Представляет атрибут, который предоставляет дополнительные метаданные для управления текстовыми редакторами.

**Диаграмма наследования:**

```
Attribute
    └── OFT.Attributes.Editors.TextEditorAttribute
```

*   **Аннотация:** Класс `TextEditorAttribute` наследуется от класса `Attribute`. Это означает, что он является типом атрибута и может использоваться для добавления метаданных к элементам кода, связанным с текстовыми редакторами.

**Диаграмма взаимодействия:**

```
Attribute
    └── OFT.Attributes.Editors.TextEditorAttribute
```

*   **Аннотация:**  Диаграмма взаимодействия показывает, что `TextEditorAttribute` также взаимодействует с классом `Attribute`, что логично, учитывая наследование.

### Открытые члены класса (Public Member Functions)

**`TextEditorAttribute(bool isMultiline, int maxHeight=50)`**

*   **Описание:** Инициализирует новый экземпляр класса `TextEditorAttribute` с указанными настройками.

*   **Параметры:**
    *   `isMultiline` (`bool`):  Значение, указывающее, разрешает ли текстовый редактор многострочный ввод.
    *   `maxHeight` (`int`, по умолчанию = 50): Максимальная высота текстового редактора в строках.

*   **Аннотация:** Это конструктор класса `TextEditorAttribute`. Он принимает два параметра: `isMultiline` для управления многострочным вводом и `maxHeight` для установки максимальной высоты текстового редактора. Параметр `maxHeight` имеет значение по умолчанию 50.

### Свойства (Properties)

**`bool IsMultiline`  `[get]`**

*   **Описание:** Возвращает значение, указывающее, разрешает ли текстовый редактор многострочный ввод.

*   **Аннотация:** Это свойство `IsMultiline` типа `bool`. Оно позволяет получить информацию о том, настроен ли текстовый редактор на многострочный ввод.  `[get]` указывает на то, что свойство доступно только для чтения (get accessor).

**`int MaxHeight` `[get]`**

*   **Описание:** Возвращает максимальную высоту текстового редактора в строках.

*   **Аннотация:** Это свойство `MaxHeight` типа `int`. Оно возвращает максимальную высоту текстового редактора, установленную в строках. `[get]` также указывает на доступность только для чтения.

### Подробное описание (Detailed Description)

**Описание:** Представляет атрибут, который предоставляет дополнительные метаданные для управления текстовыми редакторами.

*   **Аннотация:**  Это повторение общего описания класса, подчеркивающее его основное назначение: предоставление метаданных для текстовых редакторов.

### Документация конструктора и деструктора (Constructor & Destructor Documentation)

**`TextEditorAttribute()`**

```csharp
OFT.Attributes.Editors.TextEditorAttribute.TextEditorAttribute (
    bool isMultiline,
    int maxHeight = 50
)
```

*   **Описание:** Инициализирует новый экземпляр класса `TextEditorAttribute` с указанными настройками.

*   **Параметры:**
    *   `isMultiline`:  Значение, указывающее, разрешает ли текстовый редактор многострочный ввод.
    *   `maxHeight`: Максимальная высота текстового редактора в строках.  Должна быть не менее 10 строк.

*   **Аннотация:**  Более подробное представление конструктора с указанием типов параметров (`bool`, `int`) и дополнительным требованием к `maxHeight` (минимум 10 строк).

### Документация свойств (Property Documentation)

**`IsMultiline`**

```csharp
bool OFT.Attributes.Editors.TextEditorAttribute.IsMultiline  [get]
```

*   **Описание:** Возвращает значение, указывающее, разрешает ли текстовый редактор многострочный ввод.

*   **Аннотация:**  Повторное описание свойства `IsMultiline` с указанием полного пути к свойству и атрибутом `[get]`.

**`MaxHeight`**

```csharp
int OFT.Attributes.Editors.TextEditorAttribute.MaxHeight   [get]
```

*   **Описание:** Возвращает максимальную высоту текстового редактора в строках.

*   **Аннотация:** Повторное описание свойства `MaxHeight` с указанием полного пути к свойству и атрибутом `[get]`.

### Файл, в котором сгенерирована документация

*   `TextEditorAttribute.cs`

*   **Аннотация:** Указывает имя файла исходного кода, в котором определен класс `TextEditorAttribute`.

**Конец методички.**







## Методичка по классу `OFT.Attributes.FeatureIdAttribute`

**Описание класса:** `OFT.Attributes.FeatureIdAttribute Class Reference`

**Диаграмма наследования:**

```
Utils::Common::Attributes::FeatureIdAttribute  // Базовый класс
    |
    └── OFT.Attributes.FeatureIdAttribute     // Текущий класс
        |
        └── ATAS.Indicators.Attributes.FeatureId // Производный класс (вероятно, для индикаторов ATAS)
```

**Аннотация:**  Диаграмма показывает, что класс `OFT.Attributes.FeatureIdAttribute` наследуется от `Utils::Common::Attributes::FeatureIdAttribute` и является базовым для `ATAS.Indicators.Attributes.FeatureId`. Это означает, что он является частью иерархии классов, связанных с атрибутами, и используется в ATAS для индикаторов.

**Диаграмма взаимодействия (Collaboration diagram):**

```
Utils::Common::Attributes::FeatureIdAttribute
    ↑
    OFT.Attributes.FeatureIdAttribute
```

**Аннотация:** Диаграмма взаимодействия показывает связь между `OFT.Attributes.FeatureIdAttribute` и его базовым классом `Utils::Common::Attributes::FeatureIdAttribute`.  Стрелка указывает направление зависимости, вероятно, `OFT.Attributes.FeatureIdAttribute` использует или зависит от `Utils::Common::Attributes::FeatureIdAttribute`.

**Открытые члены класса (Public Member Functions):**

* `FeatureIdAttribute (string id)`

**Аннотация:** Это публичная функция-член класса, которая является конструктором. Она принимает один аргумент типа `string` с именем `id`.  Вероятно, этот конструктор используется для создания экземпляра класса `FeatureIdAttribute` и инициализации его идентификатором (`id`).

**Документация конструктора и деструктора (Constructor & Destructor Documentation):**

* `FeatureIdAttribute()`
* `OFT.Attributes.FeatureIdAttribute.FeatureIdAttribute ( string id )`

**Аннотация:**
* `FeatureIdAttribute()` -  Это объявление конструктора по умолчанию.  Судя по отсутствию параметров, это конструктор без аргументов.
* `OFT.Attributes.FeatureIdAttribute.FeatureIdAttribute ( string id )` - Это полное описание конструктора с параметром `string id`, который был упомянут в "Public Member Functions". Это полное имя конструктора, включающее пространство имен и имя класса.

**Файл, в котором сгенерирована документация:**

* `FeatureIdAttribute.cs`

**Аннотация:**  Указывает на то, что исходный код класса `FeatureIdAttribute` находится в файле `FeatureIdAttribute.cs`. Это полезно для поиска и изучения исходного кода.








## Методичка по классу `OFT.Attributes.HelpLinkAttribute`

**Ссылка на класс `OFT.Attributes.HelpLinkAttribute`**

**Диаграмма наследования для `OFT.Attributes.HelpLinkAttribute`:**

```
Utils::Common::Attributes  // Базовый класс Attributes
    ::HelpLinkAttribute      // Наследуется от Attributes
OFT.Attributes.HelpLinkAttribute // Класс HelpLinkAttribute
[legend]                     // Условные обозначения диаграммы (не показаны в тексте)
```

*Аннотация: Диаграмма показывает, что класс `OFT.Attributes.HelpLinkAttribute` наследует свойства и методы от класса `Utils::Common::Attributes::HelpLinkAttribute`, который в свою очередь, вероятно, наследует от `Utils::Common::Attributes`. Это означает, что `HelpLinkAttribute` является специализированным типом атрибута.*

**Диаграмма взаимодействия для `OFT.Attributes.HelpLinkAttribute`:**

```
Utils::Common::Attributes  // Базовый класс Attributes
    ::HelpLinkAttribute      // Взаимодействие с HelpLinkAttribute
OFT.Attributes.HelpLinkAttribute // Класс HelpLinkAttribute
[legend]                     // Условные обозначения диаграммы (не показаны в тексте)
```

*Аннотация: Диаграмма взаимодействия, вероятно, показывает, как класс `OFT.Attributes.HelpLinkAttribute` взаимодействует с классом `Utils::Common::Attributes::HelpLinkAttribute`.  В контексте "взаимодействия" это может означать использование, агрегацию или другие виды связей между классами.*

**Открытые (публичные) функции-члены**

*Аннотация: Публичные функции-члены — это методы, которые можно вызывать извне класса. Они определяют интерфейс класса.*

* `HelpLinkAttribute (Type type, string resourceName)`
* `HelpLinkAttribute (string url)`
* `HelpLinkAttribute (string url, string text)`

*Аннотация: Это объявления конструкторов класса `HelpLinkAttribute`. Конструкторы используются для создания новых объектов класса. Видно, что есть три разных способа создать объект `HelpLinkAttribute`, каждый из которых принимает разные параметры.*

**Документация конструкторов и деструкторов**

*Аннотация: Конструкторы — это специальные методы, которые вызываются при создании объекта класса. Деструкторы (здесь не показаны явно, но подразумеваются в контексте "конструкторов и деструкторов") — это методы, которые вызываются при уничтожении объекта. В данном случае, документируются только конструкторы.*

**`HelpLinkAttribute()` [1/3]**

```csharp
OFT.Attributes.HelpLinkAttribute.HelpLinkAttribute (Type type,
                                                   string resourceName
                                                  )
```

*Аннотация: Это первый из трех конструкторов класса `HelpLinkAttribute`. Он принимает два параметра:*
    * `Type type`: Параметр типа `Type`. Вероятно, это перечисление или класс, определяющий тип ссылки справки.
    * `string resourceName`: Параметр типа `string`, представляющий имя ресурса справки.

**`HelpLinkAttribute()` [2/3]**

```csharp
OFT.Attributes.HelpLinkAttribute.HelpLinkAttribute (string url)
```

*Аннотация: Это второй конструктор класса `HelpLinkAttribute`. Он принимает один параметр:*
    * `string url`: Параметр типа `string`, представляющий URL-адрес ссылки справки.

**`HelpLinkAttribute()` [3/3]**

```csharp
OFT.Attributes.HelpLinkAttribute.HelpLinkAttribute (string url,
                                                   string text
                                                  )
```

*Аннотация: Это третий конструктор класса `HelpLinkAttribute`. Он принимает два параметра:*
    * `string url`: Параметр типа `string`, представляющий URL-адрес ссылки справки.
    * `string text`: Параметр типа `string`, представляющий текст, который будет отображаться для ссылки справки.

**Документация для этого класса была сгенерирована из следующего файла:**

* `HelpLinkAttribute.cs`

*Аннотация: Указывает, что исходный код класса `HelpLinkAttribute` находится в файле `HelpLinkAttribute.cs`.*







**Методичка: OFT.Attributes.IgnoreCloneAttribute Class Reference**

**Заголовок:** OFT.Attributes.IgnoreCloneAttribute Class Reference

**Описание:** Ignore clone property or field. More...

**Диаграмма наследования для OFT.Attributes.IgnoreCloneAttribute:**

*   **Верхний уровень:** Attribute  (Атрибут) - *Это базовый класс, от которого наследуется IgnoreCloneAttribute.*
*   **Нижний уровень:** OFT.Attributes.IgnoreCloneAttribute (OFT.Attributes.IgnoreCloneAttribute) - *Это класс IgnoreCloneAttribute, который наследует свойства и методы от класса Attribute.*
*   **Связь:** Стрелка от Attribute к OFT.Attributes.IgnoreCloneAttribute  - *Указывает на наследование: IgnoreCloneAttribute является подклассом Attribute.*
*   **Легенда:** [legend] (легенда) - *Указывает на наличие условных обозначений для диаграммы (не показаны в данном фрагменте).*

**Диаграмма взаимодействия для OFT.Attributes.IgnoreCloneAttribute:**

*   **Верхний уровень:** Attribute (Атрибут) - *Представляет класс Attribute, с которым взаимодействует IgnoreCloneAttribute.*
*   **Нижний уровень:** OFT.Attributes.IgnoreCloneAttribute (OFT.Attributes.IgnoreCloneAttribute) - *Представляет сам класс IgnoreCloneAttribute.*
*   **Связь:** Стрелка от Attribute к OFT.Attributes.IgnoreCloneAttribute - *Показывает взаимодействие или зависимость IgnoreCloneAttribute от Attribute.*
*   **Легенда:** [legend] (легенда) - *Указывает на наличие условных обозначений для диаграммы.*

**Подробное описание:** Detailed Description

*   Ignore clone property or field. (Игнорирует клонирование свойства или поля) - *Описание назначения атрибута: он используется для того, чтобы указать, что свойство или поле не нужно клонировать.*

**Файл документации:** The documentation for this class was generated from the following file:

*   IgnoreCloneAttribute.cs (IgnoreCloneAttribute.cs) - *Указывает имя файла с исходным кодом, из которого была сгенерирована данная документация.*








## Методичка по классу `OFT.Attributes.LogoAttribute` для платформы ATAS

### Ссылка на класс

**OFT.Attributes.LogoAttribute Class Reference**

### Диаграмма наследования для `OFT.Attributes.LogoAttribute`:

```
Attribute
  |
  └── OFT.Attributes.LogoAttribute
```

**Описание:** `OFT.Attributes.LogoAttribute` наследуется от класса `Attribute`.

### Диаграмма взаимодействия для `OFT.Attributes.LogoAttribute`:

```
Attribute
  |
  └── OFT.Attributes.LogoAttribute
```

**Описание:**  `OFT.Attributes.LogoAttribute` взаимодействует с классом `Attribute`.

### Публичные функции-члены

*   **`LogoAttribute(string path)`**

    **Описание:**  Конструктор класса `LogoAttribute`, принимающий строковый параметр `path`.

### Свойства

*   **`string Path`** `[get]`

    **Описание:** Свойство `Path` типа `string` с методом доступа `get` (только для чтения).

### Документация конструктора и деструктора

*   **`LogoAttribute()`**

    **Описание:**  Конструктор по умолчанию класса `OFT.Attributes.LogoAttribute`.

*   **`LogoAttribute(string path)`**

    **Описание:**  Конструктор класса `OFT.Attributes.LogoAttribute`, принимающий строковый параметр `path`.

    **Синтаксис:** `OFT.Attributes.LogoAttribute.LogoAttribute(string path)`

### Документация свойств

*   **`Path`**

    **Описание:** Свойство `Path` класса `OFT.Attributes.LogoAttribute`.

    **Тип:** `string`

    **Доступ:** `get`

    **Синтаксис:** `string OFT.Attributes.LogoAttribute.Path`

### Файл, в котором сгенерирована документация для этого класса:

*   `LogoAttribute.cs`







## Методичка: OFT.Attributes.MappingAttribute Class Reference

### 1. Диаграмма наследования (Inheritance diagram)

**Диаграмма наследования для OFT.Attributes.MappingAttribute:**

```
Attribute
    ↑
OFT.Attributes.MappingAttribute
[legend]
```

*Аннотация: `OFT.Attributes.MappingAttribute` является подклассом `Attribute`.*

### 2. Диаграмма совместной работы (Collaboration diagram)

**Диаграмма совместной работы для OFT.Attributes.MappingAttribute:**

```
Attribute
    ↑
OFT.Attributes.MappingAttribute
[legend]
```

*Аннотация: `OFT.Attributes.MappingAttribute` взаимодействует с `Attribute`.*

### 3. Файл документации (Documentation file)

**Документация для этого класса была сгенерирована из следующего файла:**

- `MappingAttribute.cs`

*Аннотация: Исходный код и документация класса `OFT.Attributes.MappingAttribute` находятся в файле `MappingAttribute.cs`.*








## Методичка по классу `OFT.Attributes.ParameterAttribute`

### Ссылка на класс `OFT.Attributes.ParameterAttribute`

**Диаграмма наследования для `OFT.Attributes.ParameterAttribute`:**

```
Utils::Common::Attributes
::ParameterAttribute
      ↑
      │
OFT.Attributes.ParameterAttribute
      ↑
      │
ATAS.Indicators.ParameterAttribute
```
[легенда]

*Аннотация: Диаграмма показывает иерархию наследования класса `OFT.Attributes.ParameterAttribute`. Он наследуется от `Utils::Common::Attributes::ParameterAttribute` и является базовым классом для `ATAS.Indicators.ParameterAttribute`. Стрелки указывают направление наследования: сверху вниз.*

**Диаграмма взаимодействия для `OFT.Attributes.ParameterAttribute`:**

```
Utils::Common::Attributes
::ParameterAttribute
      ↑
      │
OFT.Attributes.ParameterAttribute
```
[легенда]

*Аннотация: Диаграмма демонстрирует взаимодействие класса `OFT.Attributes.ParameterAttribute` с классом `Utils::Common::Attributes::ParameterAttribute`.  Стрелка указывает направление зависимости или использования.*

**Документация для этого класса была сгенерирована из следующего файла:**

- `Attributes/ParameterAttribute.cs`

*Аннотация: Указывает на исходный файл, из которого была создана документация для класса `OFT.Attributes.ParameterAttribute`.  Это файл `ParameterAttribute.cs`, расположенный в директории `Attributes`.*









## Методичка по классу `OFT.Attributes.ReferralLinkAttribute`

### Ссылка на класс `OFT.Attributes.ReferralLinkAttribute`

**Диаграмма наследования для `OFT.Attributes.ReferralLinkAttribute`:**

```
Attribute
    ↑
OFT.Attributes.ReferralLinkAttribute
```

**Диаграмма взаимодействия для `OFT.Attributes.ReferralLinkAttribute`:**

```
Attribute
    ↑
OFT.Attributes.ReferralLinkAttribute
```

### Открытые функции-члены

* `ReferralLinkAttribute (string url)`

### Свойства

* `string Url`  [get]

### Документация конструктора и деструктора

#### ◆ `ReferralLinkAttribute()`

```cpp
OFT.Attributes.ReferralLinkAttribute.ReferralLinkAttribute (string url)
```

### Документация свойств

#### ◆ `Url`

```cpp
string OFT.Attributes.ReferralLinkAttribute.Url
```

Файл, из которого сгенерирована документация для этого класса:

* `ReferralLinkAttribute.cs`








## Методичка по OFT.Core Namespace Reference для создания индикаторов ATAS

### Пространство имен OFT.Core

#### Классы (Classes)

*   **class BaseConnectorSettings**
    *   *Описание отсутствует в предоставленном тексте.*
*   **class ConnectorCategories**
    *   *Описание отсутствует в предоставленном тексте.*
*   **interface IConnectorSettings**
    *   *Описание отсутствует в предоставленном тексте.*
*   **interface IConnectorSettingsAction**
    *   *Описание отсутствует в предоставленном тексте.*
*   **interface IConnectorSettingsSupportInitialization**
    *   *Описание отсутствует в предоставленном тексте.*
*   **interface ILoginPasswordConnectorSettings**
    *   *Описание отсутствует в предоставленном тексте.*
*   **interface ITypedConnectorSettings**
    *   *Описание отсутствует в предоставленном тексте.*

#### Перечисления (Enumerations)

*   **enum ConnectorFeatures**
    *   Пространство имен: `OFT.Core`
    *   Тип: `enum`
    *   Описание: Перечисление `ConnectorFeatures` определяет различные функции коннектора.
    *   Элементы перечисления (Enumerator):
        *   `None = 0x0`
            *   Описание: Отсутствие функции.
        *   `MarketData = 0x1`
            *   Описание: Функция получения рыночных данных.
        *   `Executions = 0x2`
            *   Описание: Функция обработки исполнений сделок.
        *   `StopOrders = 0x4`
            *   Описание: Функция для стоп-ордеров.
        *   `OcoOrders = 0x8`
            *   Описание: Функция для OCO (One-Cancels-Other) ордеров.

*   **enum ConnectorSettingsTypes**
    *   Пространство имен: `OFT.Core`
    *   Тип: `enum`
    *   Описание: Перечисление `ConnectorSettingsTypes` определяет типы настроек коннектора.
    *   Элементы перечисления (Enumerator):
        *   `None = 0x00`
            *   Описание: Отсутствие типа настроек.
        *   `Description = 0x01`
            *   Описание: Тип настроек для описания.
        *   `Properties = 0x02`
            *   Описание: Тип настроек для свойств.

*   **enum ConnectorSettingsPostActions**
    *   Пространство имен: `OFT.Core`
    *   Тип: `enum`
    *   Описание: Перечисление `ConnectorSettingsPostActions` определяет действия после настроек коннектора.
    *   Элементы перечисления (Enumerator):
        *   `None = 0x0`
            *   Описание: Отсутствие действия после настроек.
        *   `ShowMessage = 0x1`
            *   Описание: Действие показать сообщение после настроек.
        *   `SaveSettings = 0x2`
            *   Описание: Действие сохранить настройки.
        *   `UpdateTitle = 0x4`
            *   Описание: Действие обновить заголовок после настроек.

#### Документация типов перечислений (Enumeration Type Documentation)

*   **ConnectorFeatures**
    *   *Ссылка на описание перечисления `ConnectorFeatures` выше.*
*   **ConnectorSettingsPostActions**
    *   *Ссылка на описание перечисления `ConnectorSettingsPostActions` выше.*
*   **ConnectorSettingsTypes**
    *   *Ссылка на описание перечисления `ConnectorSettingsTypes` выше.*









## Методичка по классу `OFT.Core.BaseConnectorSettings< T >` для создания атрибутов ATAS

### Общие сведения

**`OFT.Core.BaseConnectorSettings< T >` Class Template Reference** -  *Описание класса как шаблонного, абстрактного.*

**abstract** - *Указывает, что класс является абстрактным, то есть не может быть создан напрямую и предназначен для наследования.*

**Inheritance diagram for `OFT.Core.BaseConnectorSettings< T >`:** - *Диаграмма наследования, показывающая иерархию класса.*

*   `IConnectorSettings` - *Интерфейс, от которого наследуется класс.*
*   `INotifyPropertyChanged` - *Интерфейс для уведомления об изменении свойств.*
*   `BaseConnectorSettings` - *Базовый класс, от которого происходит наследование.*
*   `OFT.Core.BaseConnectorSettings< T >` - *Текущий рассматриваемый класс.*
*   `ATAS.DataFeedsCore.BaseMarketDataOnlyConnectorSettings< T >` - *Класс-наследник для коннекторов, предоставляющих только рыночные данные.*
*   `ATAS.DataFeedsCore.BasketConnectorSettings< TDataFeedSettings, TTradingSettings >` - *Класс-наследник для коннекторов корзины ордеров, использующий настройки фида данных и торговли.*

**Collaboration diagram for `OFT.Core.BaseConnectorSettings< T >`:** - *Диаграмма взаимодействия, показывающая связи класса с другими интерфейсами.*

*   `IConnectorSettings` - *Интерфейс, с которым взаимодействует класс.*
*   `INotifyPropertyChanged` - *Интерфейс, с которым взаимодействует класс.*
*   `BaseConnectorSettings` - *Базовый класс, с которым взаимодействует класс.*
*   `OFT.Core.BaseConnectorSettings< T >` - *Текущий рассматриваемый класс.*

### Public Member Functions (Публичные методы)

*   **`IDataFeedConnector CreateConnector (string dataPath)`** - *Метод для создания коннектора данных.*
    *   `IDataFeedConnector` - *Тип возвращаемого значения, интерфейс коннектора данных.*
    *   `CreateConnector` - *Название метода.*
    *   `(string dataPath)` - *Параметр метода: `dataPath` типа `string`, представляющий путь к данным.*

*   **`void ApplySettings (IDataFeedConnector connector)`** - *Метод для применения настроек к коннектору.*
    *   `void` - *Тип возвращаемого значения, метод не возвращает значения.*
    *   `ApplySettings` - *Название метода.*
    *   `(IDataFeedConnector connector)` - *Параметр метода: `connector` типа `IDataFeedConnector`, коннектор данных, к которому применяются настройки.*

*   **`virtual bool CheckSupported (out string? errorMessage)`** - *Виртуальный метод для проверки поддержки коннектора на текущей машине.*
    *   `virtual bool` - *Тип возвращаемого значения и модификатор `virtual`, метод возвращает булево значение и может быть переопределен в классах-наследниках.*
    *   `CheckSupported` - *Название метода.*
    *   `(out string? errorMessage)` - *Параметр метода: `errorMessage` типа `string?` с модификатором `out`, выходной параметр для сообщения об ошибке (может быть null).*
    *   **Checks if connector is supported on this machine.** - *Описание метода: проверяет, поддерживается ли коннектор на данной машине.*
    *   **Parameters** - *Раздел параметров метода.*
        *   **`errorMessage`** `Null if supported otherwise contains error text` - *Описание параметра `errorMessage`:  `Null`, если поддерживается, иначе содержит текст ошибки.*
    *   **Returns** - *Раздел возвращаемого значения метода.*
        *   **`true if no problems detected, false if not supported`** - *Описание возвращаемого значения: `true`, если проблемы не обнаружены, `false`, если не поддерживается.*

*   **`override string ToString()`** - *Переопределенный метод для получения строкового представления объекта.*
    *   `override string` - *Тип возвращаемого значения и модификатор `override`, метод возвращает строку и переопределяет метод базового класса.*
    *   `ToString` - *Название метода.*
    *   `()` - *Метод без параметров.*

*   **`IDataFeedConnector CreateConnector (string dataPath)`** - *Повторное упоминание метода `CreateConnector`. (Вероятно, дублирование из-за OCR).*

*   **`void ApplySettings (IDataFeedConnector connector)`** - *Повторное упоминание метода `ApplySettings`. (Вероятно, дублирование из-за OCR).*

*   **`bool CheckSupported (out string? errorMessage)`** - *Повторное упоминание метода `CheckSupported`. (Вероятно, дублирование из-за OCR).*
    *   **Checks if connector is supported on this machine.** - *Повторное описание метода.*

### Protected Member Functions (Защищенные методы)

*   **`BaseConnectorSettings ()`** - *Конструктор класса.*
    *   `BaseConnectorSettings` - *Название конструктора, совпадающее с названием класса.*
    *   `()` - *Конструктор без параметров.*

*   **`abstract IDataFeedConnector OnCreateConnector (string dataPath)`** - *Абстрактный метод для создания коннектора данных (защищенный).*
    *   `abstract IDataFeedConnector` - *Тип возвращаемого значения и модификатор `abstract`, метод возвращает интерфейс коннектора данных и является абстрактным.*
    *   `OnCreateConnector` - *Название метода.*
    *   `(string dataPath)` - *Параметр метода: `dataPath` типа `string`, путь к данным.*

*   **`abstract void OnApplySettings (IDataFeedConnector connector)`** - *Абстрактный метод для применения настроек коннектора (защищенный).*
    *   `abstract void` - *Тип возвращаемого значения и модификатор `abstract`, метод не возвращает значения и является абстрактным.*
    *   `OnApplySettings` - *Название метода.*
    *   `(IDataFeedConnector connector)` - *Параметр метода: `connector` типа `IDataFeedConnector`, коннектор данных.*

*   **`void RaisePropertyChanged (string propertyName)`** - *Метод для вызова события изменения свойства.*
    *   `void` - *Тип возвращаемого значения, метод не возвращает значения.*
    *   `RaisePropertyChanged` - *Название метода.*
    *   `(string propertyName)` - *Параметр метода: `propertyName` типа `string`, имя измененного свойства.*

*   **`bool SetProperty< TValue > (ref TValue storage, TValue newValue, string propertyName, Action< TValue, TValue > onChanged=null)`** - *Метод для установки значения свойства с уведомлением об изменении.*
    *   `bool` - *Тип возвращаемого значения, возвращает булево значение (вероятно, успех операции).*
    *   `SetProperty< TValue >` - *Название метода, является generic-методом с типом `TValue`.*
    *   `(ref TValue storage, TValue newValue, string propertyName, Action< TValue, TValue > onChanged=null)` - *Параметры метода:*
        *   `ref TValue storage` - *Параметр `storage` типа `TValue` с модификатором `ref`, ссылка на хранилище значения свойства.*
        *   `TValue newValue` - *Параметр `newValue` типа `TValue`, новое значение свойства.*
        *   `string propertyName` - *Параметр `propertyName` типа `string`, имя свойства.*
        *   `Action< TValue, TValue > onChanged=null` - *Параметр `onChanged` типа `Action< TValue, TValue >`, делегат (функция) для вызова после изменения значения, по умолчанию `null`.*

*   **`override IDataFeedConnector OnCreateConnector (string dataPath)`** - *Переопределенный метод `OnCreateConnector`. (Вероятно, дублирование или уточнение).*
    *   `override IDataFeedConnector` - *Тип возвращаемого значения и модификатор `override`.*
    *   `OnCreateConnector` - *Название метода.*
    *   `(string dataPath)` - *Параметр метода: `dataPath` типа `string`.*

*   **`override void OnApplySettings (IDataFeedConnector connector)`** - *Переопределенный метод `OnApplySettings`. (Вероятно, дублирование или уточнение).*
    *   `override void` - *Тип возвращаемого значения и модификатор `override`.*
    *   `OnApplySettings` - *Название метода.*
    *   `(IDataFeedConnector connector)` - *Параметр метода: `connector` типа `IDataFeedConnector`.*

*   **`abstract void OnApplySettings (T connector)`** - *Абстрактный метод `OnApplySettings` с другим типом параметра.*
    *   `abstract void` - *Тип возвращаемого значения и модификатор `abstract`.*
    *   `OnApplySettings` - *Название метода.*
    *   `(T connector)` - *Параметр метода: `connector` типа `T` (шаблонный параметр класса).*

### Properties (Свойства)

*   **`string Type`** `[get, set]` - *Свойство `Type` типа `string` с методами доступа `get` (чтение) и `set` (запись).*

*   **`string Description`** `[get, set]` - *Свойство `Description` типа `string` с методами доступа `get` и `set`.*

*   **`Uri Logo`** `[get]` - *Свойство `Logo` типа `Uri` (Uniform Resource Identifier, например, URL) с методом доступа `get` (только для чтения).*

*   **`Guid Id`** `[get, set]` - *Свойство `Id` типа `Guid` (Globally Unique Identifier, уникальный идентификатор) с методами доступа `get` и `set`.*

*   **`virtual string DisplayName`** `[get, set]` - *Виртуальное свойство `DisplayName` типа `string` с методами доступа `get` и `set`, может быть переопределено в классах-наследниках.*

*   **`string Name`** `[get, set]` - *Свойство `Name` типа `string` с методами доступа `get` и `set`.*

*   **`bool IsMarketDataEnabled`** `[get, set]` - *Свойство `IsMarketDataEnabled` типа `bool` с методами доступа `get` и `set`, указывает, включены ли рыночные данные.*

*   **`bool IsAutoConnectEnabled`** `[get, set]` - *Свойство `IsAutoConnectEnabled` типа `bool` с методами доступа `get` и `set`, указывает, включено ли автоподключение.*

*   **`bool AllowUpdatePositionsPnL`** `[get, set]` - *Свойство `AllowUpdatePositionsPnL` типа `bool` с методами доступа `get` и `set`, указывает, разрешено ли обновление позиций PnL (Profit and Loss).*

*   **`TimeOnly? RefreshSecuritiesTime`** `[get, set]` - *Свойство `RefreshSecuritiesTime` типа `TimeOnly?` (Nullable TimeOnly, время без даты) с методами доступа `get` и `set`, время обновления инструментов.*

*   **`abstract ConnectorFeatures Features`** `[get]` - *Абстрактное свойство `Features` типа `ConnectorFeatures` с методом доступа `get` (только для чтения), представляет собой набор возможностей коннектора.*

*   **`virtual ConnectorSettingsTypes SettingsTypes`** `[get]` - *Виртуальное свойство `SettingsTypes` типа `ConnectorSettingsTypes` с методом доступа `get` (только для чтения), представляет собой типы настроек коннектора.*

*   **`virtual bool IsDemo`** `[get]` - *Виртуальное свойство `IsDemo` типа `bool` с методом доступа `get` (только для чтения).*
    *   **Indicates that connector uses TestNet environment.** - *Описание свойства: указывает, использует ли коннектор тестовую среду (TestNet).*

*   **`virtual MarketDataDelayPeriods MarketDataDelayPeriod`** `[get]` - *Виртуальное свойство `MarketDataDelayPeriod` типа `MarketDataDelayPeriods` с методом доступа `get` (только для чтения), представляет собой периоды задержки рыночных данных.*

### Properties inherited from `OFT.Core.IConnectorSettings` (Свойства, унаследованные от `OFT.Core.IConnectorSettings`)

*   *(В документе не указаны конкретные свойства, только заголовок раздела).* - *Раздел указывает на наличие свойств, унаследованных от интерфейса `OFT.Core.IConnectorSettings`, но их список не приведен в данной выдержке.*

### Events (События)

*   **`PropertyChangedEventHandler PropertyChanged`** - *Событие `PropertyChanged` типа `PropertyChangedEventHandler`, возникает при изменении значения свойства.*

### Constructor & Destructor Documentation (Документация конструктора и деструктора)

*   **`BaseConnectorSettings()`** - *Конструктор класса `BaseConnectorSettings`. *
    *   `OFT.Core.BaseConnectorSettings< T >.BaseConnectorSettings()` - *Полное имя конструктора с указанием пространства имен и шаблонного параметра.*
    *   `protected` - *Указание уровня доступа `protected`, конструктор доступен из текущего класса и классов-наследников.*

### Member Function Documentation (Документация методов)

*   **`ApplySettings()`** - *Документация метода `ApplySettings`. *
    *   **`void OFT.Core.BaseConnectorSettings< T >.ApplySettings (IDataFeedConnector connector)`** - *Полная сигнатура метода.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что метод реализует метод из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`CheckSupported()`** - *Документация метода `CheckSupported`. *
    *   **`virtual bool OFT.Core.BaseConnectorSettings< T >.CheckSupported (out string? errorMessage)`** - *Полная сигнатура метода.*
    *   `virtual` - *Указание, что метод виртуальный.*
    *   **Checks if connector is supported on this machine.** - *Описание метода.*
    *   **Parameters** - *Раздел параметров.*
        *   **`errorMessage`** `Null if supported otherwise contains error text` - *Описание параметра `errorMessage`.*
    *   **Returns** - *Раздел возвращаемого значения.*
        *   **`true if no problems detected, false if not supported`** - *Описание возвращаемого значения.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что метод реализует метод из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`CreateConnector()`** - *Документация метода `CreateConnector`. *
    *   **`IDataFeedConnector OFT.Core.BaseConnectorSettings< T >.CreateConnector (string dataPath)`** - *Полная сигнатура метода.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что метод реализует метод из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`OnApplySettings() [1/3]`** - *Документация первой версии метода `OnApplySettings` (перегрузка 1 из 3).*
    *   **`abstract void OFT.Core.BaseConnectorSettings< T >.OnApplySettings (IDataFeedConnector connector)`** - *Полная сигнатура метода.*
    *   `abstract` - *Указание, что метод абстрактный.*
    *   `protected pure virtual` - *Уточнение уровня доступа и виртуальности.*
    *   `Implemented in ATAS.DataFeedsCore.BasketConnectorSettings< TDataFeedSettings, TTradingSettings >.` - *Указание, в каком классе-наследнике реализован метод.*

*   **`OnApplySettings() [2/3]`** - *Документация второй версии метода `OnApplySettings` (перегрузка 2 из 3).*
    *   **`override void OFT.Core.BaseConnectorSettings< T >.OnApplySettings (IDataFeedConnector connector)`** - *Полная сигнатура метода.*
    *   `override` - *Указание, что метод переопределен.*
    *   `protected` - *Уточнение уровня доступа.*

*   **`OnApplySettings() [3/3]`** - *Документация третьей версии метода `OnApplySettings` (перегрузка 3 из 3).*
    *   **`abstract void OFT.Core.BaseConnectorSettings< T >.OnApplySettings (T connector)`** - *Полная сигнатура метода.*
    *   `abstract` - *Указание, что метод абстрактный.*
    *   `protected pure virtual` - *Уточнение уровня доступа и виртуальности.*

*   **`OnCreateConnector() [1/2]`** - *Документация первой версии метода `OnCreateConnector` (перегрузка 1 из 2).*
    *   **`abstract IDataFeedConnector OFT.Core.BaseConnectorSettings< T >.OnCreateConnector (string dataPath)`** - *Полная сигнатура метода.*
    *   `abstract` - *Указание, что метод абстрактный.*
    *   `protected pure virtual` - *Уточнение уровня доступа и виртуальности.*

*   **`OnCreateConnector() [2/2]`** - *Документация второй версии метода `OnCreateConnector` (перегрузка 2 из 2).*
    *   **`override IDataFeedConnector OFT.Core.BaseConnectorSettings< T >.OnCreateConnector (string dataPath)`** - *Полная сигнатура метода.*
    *   `override` - *Указание, что метод переопределен.*
    *   `protected` - *Уточнение уровня доступа.*

*   **`RaisePropertyChanged()`** - *Документация метода `RaisePropertyChanged`. *
    *   **`void OFT.Core.BaseConnectorSettings< T >.RaisePropertyChanged (string propertyName)`** - *Полная сигнатура метода.*
    *   `protected` - *Уточнение уровня доступа.*

*   **`SetProperty< TValue >()`** - *Документация метода `SetProperty< TValue >`. *
    *   **`bool OFT.Core.BaseConnectorSettings< T >.SetProperty< TValue > (...)`** - *Начало полной сигнатуры метода (параметры опущены для краткости).*
    *   `protected` - *Уточнение уровня доступа.*

*   **`ToString()`** - *Документация метода `ToString`. *
    *   **`override string OFT.Core.BaseConnectorSettings< T >.ToString()`** - *Полная сигнатура метода.*
    *   `override` - *Указание, что метод переопределен.*

### Property Documentation (Документация свойств)

*   **`AllowUpdatePositionsPnL`** - *Документация свойства `AllowUpdatePositionsPnL`. *
    *   **`bool OFT.Core.BaseConnectorSettings< T >.AllowUpdatePositionsPnL`** - *Полная сигнатура свойства.*
    *   `get` `set` - *Указание методов доступа: `get` (чтение) и `set` (запись).*

*   **`Description`** - *Документация свойства `Description`. *
    *   **`string OFT.Core.BaseConnectorSettings< T >.Description`** - *Полная сигнатура свойства.*
    *   `get` - *Указание метода доступа `get`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`DisplayName`** - *Документация свойства `DisplayName`. *
    *   **`virtual string OFT.Core.BaseConnectorSettings< T >.DisplayName`** - *Полная сигнатура свойства.*
    *   `virtual` - *Указание, что свойство виртуальное.*
    *   `get` `set` - *Указание методов доступа: `get` и `set`.*

*   **`Features`** - *Документация свойства `Features`. *
    *   **`abstract ConnectorFeatures OFT.Core.BaseConnectorSettings< T >.Features`** - *Полная сигнатура свойства.*
    *   `abstract` - *Указание, что свойство абстрактное.*
    *   `get` - *Указание метода доступа `get`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`Id`** - *Документация свойства `Id`. *
    *   **`Guid OFT.Core.BaseConnectorSettings< T >.Id`** - *Полная сигнатура свойства.*
    *   `get` `set` - *Указание методов доступа: `get` и `set`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`IsAutoConnectEnabled`** - *Документация свойства `IsAutoConnectEnabled`. *
    *   **`bool OFT.Core.BaseConnectorSettings< T >.IsAutoConnectEnabled`** - *Полная сигнатура свойства.*
    *   `get` `set` - *Указание методов доступа: `get` и `set`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`IsDemo`** - *Документация свойства `IsDemo`. *
    *   **`virtual bool OFT.Core.BaseConnectorSettings< T >.IsDemo`** - *Полная сигнатура свойства.*
    *   `virtual` - *Указание, что свойство виртуальное.*
    *   `get` - *Указание метода доступа `get`.*
    *   **Indicates that connector uses TestNet environment.** - *Описание свойства.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`IsMarketDataEnabled`** - *Документация свойства `IsMarketDataEnabled`. *
    *   **`bool OFT.Core.BaseConnectorSettings< T >.IsMarketDataEnabled`** - *Полная сигнатура свойства.*
    *   `get` `set` - *Указание методов доступа: `get` и `set`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`Logo`** - *Документация свойства `Logo`. *
    *   **`Uri OFT.Core.BaseConnectorSettings< T >.Logo`** - *Полная сигнатура свойства.*
    *   `get` - *Указание метода доступа `get`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`MarketDataDelayPeriod`** - *Документация свойства `MarketDataDelayPeriod`. *
    *   **`virtual MarketDataDelayPeriods OFT.Core.BaseConnectorSettings< T >.MarketDataDelayPeriod`** - *Полная сигнатура свойства.*
    *   `virtual` - *Указание, что свойство виртуальное.*
    *   `get` - *Указание метода доступа `get`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`Name`** - *Документация свойства `Name`. *
    *   **`string OFT.Core.BaseConnectorSettings< T >.Name`** - *Полная сигнатура свойства.*
    *   `get` `set` - *Указание методов доступа: `get` и `set`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`RefreshSecuritiesTime`** - *Документация свойства `RefreshSecuritiesTime`. *
    *   **`TimeOnly? OFT.Core.BaseConnectorSettings< T >.RefreshSecuritiesTime`** - *Полная сигнатура свойства.*
    *   `get` `set` - *Указание методов доступа: `get` и `set`.*

*   **`SettingsTypes`** - *Документация свойства `SettingsTypes`. *
    *   **`virtual ConnectorSettingsTypes OFT.Core.BaseConnectorSettings< T >.SettingsTypes`** - *Полная сигнатура свойства.*
    *   `virtual` - *Указание, что свойство виртуальное.*
    *   `get` - *Указание метода доступа `get`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

*   **`Type`** - *Документация свойства `Type`. *
    *   **`string OFT.Core.BaseConnectorSettings< T >.Type`** - *Полная сигнатура свойства.*
    *   `get` `set` - *Указание методов доступа: `get` и `set`.*
    *   `Implements OFT.Core.IConnectorSettings.` - *Указание, что свойство реализует свойство из интерфейса `OFT.Core.IConnectorSettings`. *

### Event Documentation (Документация событий)

*   **`PropertyChanged`** - *Документация события `PropertyChanged`. *
    *   **`PropertyChangedEventHandler OFT.Core.BaseConnectorSettings< T >.PropertyChanged`** - *Полная сигнатура события.*

### Файл, из которого сгенерирована документация

*   **`IConnectorSettings.cs`** - *Указание файла исходного кода, из которого была сгенерирована документация для данного класса.*








## Методичка для ИИ по интерфейсу OFT.Core.IConnectorSettings

**Раздел 1: Описание интерфейса OFT.Core.IConnectorSettings**

**Заголовок:** OFT.Core.IConnectorSettings Interface Reference

**Описание:**  Интерфейс `OFT.Core.IConnectorSettings`.

**Диаграмма наследования:**

```
OFT.Core.IConnectorSettings
    /\
     |
     |-- OFT.Core.BaseConnectorSettings< T >
     |   /\
     |    |-- ATAS.DataFeedsCore.BaseMainDataOnlyConnectorSettings< T >
     |    |-- ATAS.DataFeedsCore.BasketConnectorSettings< TDataFeedSettings, TTradingSettings >
     |
     |-- OFT.Core.ISupportInitialization
     |-- OFT.Core.ILoginPasswordConnectorSettings
     |-- OFT.Core.ITypedConnectorSettings
```

**Аннотация:**
* `OFT.Core.IConnectorSettings` -  основной интерфейс настроек коннектора.
* `OFT.Core.BaseConnectorSettings< T >` - базовый класс для настроек коннекторов, использующий дженерик `T`.
* `ATAS.DataFeedsCore.BaseMainDataOnlyConnectorSettings< T >` -  класс настроек для коннекторов, предназначенных только для получения основных данных.
* `ATAS.DataFeedsCore.BasketConnectorSettings< TDataFeedSettings, TTradingSettings >` - класс настроек для коннекторов, поддерживающих торговлю корзинами ордеров, использует дженерики `TDataFeedSettings` и `TTradingSettings`.
* `OFT.Core.ISupportInitialization` - интерфейс, указывающий на поддержку инициализации коннектора.
* `OFT.Core.ILoginPasswordConnectorSettings` - интерфейс для настроек коннекторов, использующих логин и пароль для авторизации.
* `OFT.Core.ITypedConnectorSettings` - интерфейс для настроек типизированных коннекторов.

**Раздел 2: Публичные функции-члены (Public Member Functions)**

**Подзаголовок:** Public Member Functions

**Список функций:**

1.  `IDataFeedConnector CreateConnector (string dataPath)`
    *   **Описание:** Функция `CreateConnector` создает экземпляр `IDataFeedConnector`.
    *   **Параметры:**
        *   `dataPath` (string) - путь к данным коннектора.
    *   **Возвращаемое значение:** `IDataFeedConnector` - созданный коннектор данных.

2.  `void ApplySettings (IDataFeedConnector connector)`
    *   **Описание:** Функция `ApplySettings` применяет настройки к переданному коннектору `IDataFeedConnector`.
    *   **Параметры:**
        *   `connector` (IDataFeedConnector) - коннектор, к которому применяются настройки.

3.  `bool CheckSupported (out string? errorMessage)`
    *   **Описание:** Функция `CheckSupported` проверяет, поддерживается ли коннектор на текущей машине.
    *   **Параметры:**
        *   `errorMessage` (out string?) -  выходной параметр, содержит сообщение об ошибке, если коннектор не поддерживается, иначе `null`.
    *   **Возвращаемое значение:** `bool` - `true`, если коннектор поддерживается, `false` - если нет.
    *   **Дополнительная информация:** "Checks if connector is supported on this machine." - Проверяет, поддерживается ли коннектор на этой машине.

**Раздел 3: Свойства (Properties)**

**Подзаголовок:** Properties

**Список свойств:**

1.  `string Type [get]`
    *   **Описание:** Свойство `Type` для получения типа коннектора.
    *   **Тип:** `string`
    *   **Доступ:** `[get]` - только для чтения.

2.  `string Description [get]`
    *   **Описание:** Свойство `Description` для получения описания коннектора.
    *   **Тип:** `string`
    *   **Доступ:** `[get]` - только для чтения.

3.  `Uri Logo [get]`
    *   **Описание:** Свойство `Logo` для получения URI логотипа коннектора.
    *   **Тип:** `Uri`
    *   **Доступ:** `[get]` - только для чтения.

4.  `Guid Id [get, set]`
    *   **Описание:** Свойство `Id` для получения или установки уникального идентификатора коннектора (GUID).
    *   **Тип:** `Guid`
    *   **Доступ:** `[get, set]` - для чтения и записи.

5.  `string Name [get, set]`
    *   **Описание:** Свойство `Name` для получения или установки имени коннектора.
    *   **Тип:** `string`
    *   **Доступ:** `[get, set]` - для чтения и записи.

6.  `bool IsMarketDataEnabled [get, set]`
    *   **Описание:** Свойство `IsMarketDataEnabled` для получения или установки значения, указывающего, включена ли передача рыночных данных.
    *   **Тип:** `bool`
    *   **Доступ:** `[get, set]` - для чтения и записи.

7.  `bool IsAutoConnectEnabled [get, set]`
    *   **Описание:** Свойство `IsAutoConnectEnabled` для получения или установки значения, указывающего, включено ли автоматическое подключение.
    *   **Тип:** `bool`
    *   **Доступ:** `[get, set]` - для чтения и записи.

8.  `ConnectorFeatures Features [get]`
    *   **Описание:** Свойство `Features` для получения объекта `ConnectorFeatures`, представляющего возможности коннектора.
    *   **Тип:** `ConnectorFeatures`
    *   **Доступ:** `[get]` - только для чтения.

9.  `ConnectorSettingsTypes SettingsTypes [get]`
    *   **Описание:** Свойство `SettingsTypes` для получения объекта `ConnectorSettingsTypes`, представляющего типы настроек коннектора.
    *   **Тип:** `ConnectorSettingsTypes`
    *   **Доступ:** `[get]` - только для чтения.

10. `bool IsDemo [get]`
    *   **Описание:** Свойство `IsDemo` для получения значения, указывающего, использует ли коннектор тестовую среду (TestNet).
    *   **Тип:** `bool`
    *   **Доступ:** `[get]` - только для чтения.
    *   **Дополнительная информация:** "Indicates that connector uses TestNet environment." - Указывает, что коннектор использует тестовую среду TestNet.

11. `MarketDataDelayPeriods MarketDataDelayPeriod [get]`
    *   **Описание:** Свойство `MarketDataDelayPeriod` для получения объекта `MarketDataDelayPeriods`, представляющего периоды задержки рыночных данных.
    *   **Тип:** `MarketDataDelayPeriods`
    *   **Доступ:** `[get]` - только для чтения.

**Раздел 4: Документация функций-членов (Member Function Documentation)**

**Подзаголовок:** Member Function Documentation

**4.1. `ApplySettings()`**

*   **Сигнатура:** `void OFT.Core.IConnectorSettings.ApplySettings ( IDataFeedConnector connector )`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**4.2. `CheckSupported()`**

*   **Сигнатура:** `bool OFT.Core.IConnectorSettings.CheckSupported (out string? errorMessage)`
*   **Описание:** "Checks if connector is supported on this machine." - Проверяет, поддерживается ли коннектор на этой машине.
*   **Параметры:**
    *   `errorMessage` - "Null if supported otherwise contains error text" - `Null`, если поддерживается, иначе содержит текст ошибки.
*   **Возвращаемое значение:** "true if no problems detected, false if not supported" - `true`, если проблемы не обнаружены, `false`, если не поддерживается.
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**4.3. `CreateConnector()`**

*   **Сигнатура:** `IDataFeedConnector OFT.Core.IConnectorSettings.CreateConnector (string dataPath)`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**Раздел 5: Документация свойств (Property Documentation)**

**Подзаголовок:** Property Documentation

**5.1. `Description`**

*   **Сигнатура:** `string OFT.Core.IConnectorSettings.Description`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.2. `Features`**

*   **Сигнатура:** `ConnectorFeatures OFT.Core.IConnectorSettings.Features`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.3. `Id`**

*   **Сигнатура:** `Guid OFT.Core.IConnectorSettings.Id`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.4. `IsAutoConnectEnabled`**

*   **Сигнатура:** `bool OFT.Core.IConnectorSettings.IsAutoConnectEnabled`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.5. `IsDemo`**

*   **Сигнатура:** `bool OFT.Core.IConnectorSettings.IsDemo`
*   **Описание:** "Indicates that connector uses TestNet environment." - Указывает, что коннектор использует тестовую среду TestNet.
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.6. `IsMarketDataEnabled`**

*   **Сигнатура:** `bool OFT.Core.IConnectorSettings.IsMarketDataEnabled`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.7. `Logo`**

*   **Сигнатура:** `Uri OFT.Core.IConnectorSettings.Logo`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.8. `MarketDataDelayPeriod`**

*   **Сигнатура:** `MarketDataDelayPeriods OFT.Core.IConnectorSettings.MarketDataDelayPeriod`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.9. `Name`**

*   **Сигнатура:** `string OFT.Core.IConnectorSettings.Name`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.10. `SettingsTypes`**

*   **Сигнатура:** `ConnectorSettingsTypes OFT.Core.IConnectorSettings.SettingsTypes`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**5.11. `Type`**

*   **Сигнатура:** `string OFT.Core.IConnectorSettings.Type`
*   **Реализация:** "Implemented in `OFT.Core.BaseConnectorSettings< T >`." - Реализовано в `OFT.Core.BaseConnectorSettings< T >`.

**Раздел 6: Файл, сгенерировавший документацию**

**Подзаголовок:**  Файл, сгенерировавший документацию

*   `IConnectorSettings.cs`

**Конец методички.**









## Методичка по интерфейсу `OFT.Core.IConnectorSettingsAction` для создания атрибутов ATAS

**Название интерфейса:** `OFT.Core.IConnectorSettingsAction`

**Описание:**  Интерфейс `OFT.Core.IConnectorSettingsAction` предназначен для определения действий, связанных с настройками коннектора.

### Публичные члены интерфейса (Public Members)

Интерфейс `OFT.Core.IConnectorSettingsAction` включает в себя следующие публичные члены:

#### Публичные функции-члены (Public Member Functions)

*   **`Do()`**

    *   **Тип возвращаемого значения:** `bool? string`
        *   *Аннотация:*  Возвращает значение типа `bool? string`, что означает, что функция может вернуть либо `bool?` (nullable boolean) либо `string`. В контексте ATAS, `bool?` вероятно указывает на успех или неудачу выполнения действия, а `string` может содержать сообщение об ошибке или дополнительную информацию.
    *   **Параметры:**
        *   `ConnectorSettingsPostActions postActions` типа `OFT.Core.IConnectorSettingsAction`
            *   *Аннотация:*  Параметр `postActions` представляет собой объект типа `OFT.Core.IConnectorSettingsAction`.  Вероятно, он используется для передачи дополнительных действий, которые должны быть выполнены после основного действия.

    *   **Полная сигнатура:** `bool? string ConnectorSettingsPostActions postActions Do ()`
        *   *Аннотация:*  Полная сигнатура функции `Do()` показывает, что она является методом интерфейса `OFT.Core.IConnectorSettingsAction`, принимает параметр `postActions` типа `ConnectorSettingsPostActions` и возвращает кортеж значений типа `bool? string`.

#### Публичные атрибуты (Public Attributes)

*   **`isSuccess`**

    *   **Тип данных:** `bool`
        *   *Аннотация:*  Атрибут `isSuccess` имеет тип `bool` (boolean - логическое значение).  Предположительно, он используется для хранения статуса выполнения действия, где `true` означает успех, а `false` - неудачу.
    *   **Полное имя:** `bool OFT.Core.IConnectorSettingsAction.isSuccess`
        *   *Аннотация:* Полное имя атрибута указывает на его принадлежность к интерфейсу `OFT.Core.IConnectorSettingsAction`.

*   **`message`**

    *   **Тип данных:** `bool? string`
        *   *Аннотация:*  Атрибут `message` имеет тип `bool? string`.  Вероятно, он предназначен для хранения сообщения, связанного с результатом выполнения действия.  `bool?` в начале может указывать на наличие или отсутствие сообщения (например, `null` если сообщения нет), а `string` содержит текст сообщения.
    *   **Полное имя:** `bool? string OFT.Core.IConnectorSettingsAction.message`
        *   *Аннотация:* Полное имя атрибута показывает его принадлежность к интерфейсу `OFT.Core.IConnectorSettingsAction`.

#### Свойства (Properties)

*   **`Title`**

    *   **Тип данных:** `string`
        *   *Аннотация:*  Свойство `Title` имеет тип `string` (строка).  Вероятно, оно предназначено для хранения заголовка или имени действия, представленного интерфейсом.
    *   **Атрибуты доступа:** `[get]`
        *   *Аннотация:*  `[get]` указывает, что свойство `Title` доступно только для чтения (get-only property).  Значение свойства можно получить, но нельзя изменить напрямую извне.
    *   **Полное имя:** `string OFT.Core.IConnectorSettingsAction.Title`
        *   *Аннотация:* Полное имя свойства указывает на его принадлежность к интерфейсу `OFT.Core.IConnectorSettingsAction`.

### Документация по членам интерфейса

#### Документация по функции-члену `Do()`

*   **Сигнатура:** `bool? string ConnectorSettingsPostActions postActions OFT.Core.IConnectorSettingsAction.Do( )`
    *   *Аннотация:*  Повторение сигнатуры функции `Do()` для удобства.

#### Документация по атрибуту-члену `isSuccess`

*   **Тип:** `bool OFT.Core.IConnectorSettingsAction.isSuccess`
    *   *Аннотация:*  Повторение типа и полного имени атрибута `isSuccess`.

#### Документация по атрибуту-члену `message`

*   **Тип:** `bool? string OFT.Core.IConnectorSettingsAction.message`
    *   *Аннотация:*  Повторение типа и полного имени атрибута `message`.

#### Документация по свойству `Title`

*   **Тип:** `string OFT.Core.IConnectorSettingsAction.Title` `[get]`
    *   *Аннотация:*  Повторение типа, полного имени и атрибута доступа `[get]` для свойства `Title`.

### Файл, из которого сгенерирована документация

*   `IConnectorSettings.cs`
    *   *Аннотация:*  Указание на то, что документация для интерфейса `OFT.Core.IConnectorSettingsAction` была сгенерирована из файла исходного кода `IConnectorSettings.cs`. Это полезно для поиска исходного кода и дальнейшего изучения реализации интерфейса.









## Методичка по интерфейсу `OFT.Core.IConnectorSettingsSupportInitialization` для разработки индикаторов ATAS

### Оглавление

1.  **Интерфейс `OFT.Core.IConnectorSettingsSupportInitialization`**
    *   Диаграмма наследования
    *   Диаграмма взаимодействия
    *   Публичные методы
    *   Дополнительные унаследованные члены
    *   Документация методов

### 1. Интерфейс `OFT.Core.IConnectorSettingsSupportInitialization`

Интерфейс `OFT.Core.IConnectorSettingsSupportInitialization` предназначен для поддержки инициализации настроек коннектора.

#### Диаграмма наследования

```
IConnectorSettings
    |
    └── OFT.Core.IConnectorSettingsSupportInitialization
```

*   **Аннотация:** `IConnectorSettings` является базовым интерфейсом, от которого наследуется `OFT.Core.IConnectorSettingsSupportInitialization`. Это означает, что `OFT.Core.IConnectorSettingsSupportInitialization`  включает в себя все члены `IConnectorSettings` и добавляет свою собственную функциональность.

#### Диаграмма взаимодействия

```
IConnectorSettings  <--  OFT.Core.IConnectorSettingsSupportInitialization
```

*   **Аннотация:** Диаграмма взаимодействия показывает, что `OFT.Core.IConnectorSettingsSupportInitialization` использует интерфейс `IConnectorSettings`.  Стрелка указывает направление зависимости: `OFT.Core.IConnectorSettingsSupportInitialization` зависит от `IConnectorSettings`.

#### Публичные методы

Интерфейс `OFT.Core.IConnectorSettingsSupportInitialization` предоставляет следующие публичные методы:

*   `void Initialize (string dataPath, bool throwErrors)`

    *   **Аннотация:**  Метод `Initialize` используется для инициализации настроек.
        *   `string dataPath`:  Параметр `dataPath` представляет собой строку, указывающую путь к данным, необходимым для инициализации.
        *   `bool throwErrors`: Параметр `throwErrors`  является булевым значением, определяющим, следует ли выбрасывать исключения в случае ошибок во время инициализации. Если `true`, ошибки будут генерировать исключения; если `false`, ошибки могут обрабатываться иначе (например, возвратом кода ошибки или логированием).

#### Дополнительные унаследованные члены

*   **Публичные методы, унаследованные от `OFT.Core.IConnectorSettings`**
    *   *Информация о унаследованных методах не представлена в данном документе.*

    *   **Аннотация:**  Интерфейс `OFT.Core.IConnectorSettingsSupportInitialization` наследует все публичные методы от интерфейса `OFT.Core.IConnectorSettings`. Для получения информации об этих методах необходимо обратиться к документации интерфейса `OFT.Core.IConnectorSettings`.

*   **Свойства, унаследованные от `OFT.Core.IConnectorSettings`**
    *   *Информация об унаследованных свойствах не представлена в данном документе.*

    *   **Аннотация:**  Аналогично методам, интерфейс `OFT.Core.IConnectorSettingsSupportInitialization` также наследует свойства от интерфейса `OFT.Core.IConnectorSettings`.  Детали об этих свойствах можно найти в документации `OFT.Core.IConnectorSettings`.

#### Документация методов

##### `Initialize()`

```csharp
void OFT.Core.IConnectorSettingsSupportInitialization.Initialize (string dataPath, bool throwErrors)
```

*   **Описание:** Метод `Initialize`  инициализирует компонент `OFT.Core.IConnectorSettingsSupportInitialization`.

    *   **Параметры:**
        *   `dataPath`:  `string` - Путь к данным для инициализации.
        *   `throwErrors`: `bool` - Флаг, указывающий, нужно ли выбрасывать исключения при ошибках.

    *   **Возвращаемое значение:** `void` - Метод не возвращает никакого значения.

### Файл определения интерфейса

*   Интерфейс `OFT.Core.IConnectorSettingsSupportInitialization` определен в файле:
    *   `IConnectorSettings.cs`

    *   **Аннотация:**  Файл `IConnectorSettings.cs` содержит определение интерфейса `OFT.Core.IConnectorSettingsSupportInitialization`, а также, вероятно, определение интерфейса `IConnectorSettings` и возможно других связанных интерфейсов или классов.








## Методичка по интерфейсу `OFT.Core.ITypedConnectorSettings` для создания индикаторов ATAS

### Интерфейс `OFT.Core.ITypedConnectorSettings`

**Ссылка:**  `OFT.Core.ITypedConnectorSettings Interface Reference`

**Диаграмма наследования для `OFT.Core.ITypedConnectorSettings`:**

```
IConnectorSettings
  ↑
OFT.Core.ITypedConnectorSettings
```

*   **Аннотация:** `OFT.Core.ITypedConnectorSettings` является интерфейсом, который наследуется от интерфейса `IConnectorSettings`. Это означает, что `OFT.Core.ITypedConnectorSettings` включает в себя все члены (свойства, методы и события) интерфейса `IConnectorSettings`, а также может добавлять свои собственные.

**Диаграмма взаимодействия для `OFT.Core.ITypedConnectorSettings`:**

```
IConnectorSettings
  ↑
OFT.Core.ITypedConnectorSettings
```

*   **Аннотация:** Диаграмма взаимодействия показывает взаимосвязь между интерфейсами. В данном случае, `OFT.Core.ITypedConnectorSettings` использует `IConnectorSettings`, расширяя его функциональность.

### Свойства

**`string ConnectionType [get, set]`**

*   **Тип:** `string` (строка)
*   **Доступ:** `[get, set]` (доступно для чтения и записи)
*   **Описание:**  Свойство `ConnectionType` представляет собой строку, определяющую тип соединения.  `get` означает, что значение свойства можно получить (прочитать), а `set` означает, что значение свойства можно установить (записать).

**► Свойства, унаследованные от `OFT.Core.IConnectorSettings`**

*   **Аннотация:**  Интерфейс `OFT.Core.ITypedConnectorSettings` наследует свойства от базового интерфейса `OFT.Core.IConnectorSettings`. Для получения списка унаследованных свойств необходимо обратиться к документации интерфейса `OFT.Core.IConnectorSettings`.

### Дополнительные унаследованные члены

**► Публичные функции-члены, унаследованные от `OFT.Core.IConnectorSettings`**

*   **Аннотация:**  Аналогично свойствам, `OFT.Core.ITypedConnectorSettings` также наследует публичные функции-члены (методы) от `OFT.Core.IConnectorSettings`.  Детали унаследованных функций можно найти в документации `OFT.Core.IConnectorSettings`.

### Документация свойств

#### `ConnectionType`

**`string OFT.Core.ITypedConnectorSettings.ConnectionType`**

*   **Тип:** `string`
*   **Интерфейс:** `OFT.Core.ITypedConnectorSettings`
*   **Доступ:** `get`, `set`

*   **Аннотация:**  Это подробная документация свойства `ConnectionType`.  Указывается полный тип (`string`), интерфейс, которому принадлежит свойство (`OFT.Core.ITypedConnectorSettings`), и права доступа (`get` и `set`).

**Файл документации для данного интерфейса:**

*   `IConnectorSettings.cs`

*   **Аннотация:**  Исходный код интерфейса `OFT.Core.ITypedConnectorSettings` и, вероятно, интерфейса `IConnectorSettings`, находится в файле `IConnectorSettings.cs`.  Это может быть полезно для дальнейшего изучения деталей реализации интерфейсов.



















































