# Xenon V3 — Lua API

Документация составлена по текущим исходникам `XenonV3.lua` и `libXenonV3GUI.lua`. Это описание фактически обнаруженных функций, а не отдельный стабильный SDK. Методы зависят от Roblox API, игрового состояния и функций executor.

## 1. Создание контроллера

```lua
local Util = Xenon.Utils.MakeUtilController({
    ConfigName = "XenonV3/XenonConfig.json"
})
```

`MakeUtilController(Settings)` создаёт объект утилит с кэшами сервисов, состояниями, числами, строками, таблицами и настройками конфигурации.

Если `Settings.ConfigName` не задан, используется `XenonV3/XenonConfig.json`.

## 2. Конфигурация и хранилище

| Метод | Назначение |
|---|---|
| `MakeFolder()` | Создаёт каталоги `XenonV3` и `XenonV3/Logs`. |
| `ReadData()` | Читает JSON-конфигурацию; при неудаче использует удалённую конфигурацию Pastebin. Возвращает таблицу с `Data`, `Success`, `Error` и функцией `LoadData()`. |
| `WriteData(Data)` | Кодирует таблицу в JSON и записывает её в файл. Возвращает `Success, Error`. |
| `DeleteData()` | Удаляет файл конфигурации, если он существует. |
| `ConvertConfig(Config)` | Преобразует значения конфигурации в формат, используемый хранилищем. |
| `AddValues(Values)` | Загружает разделы `Int`, `State`, `String` и `Table` во внутренние хранилища. |
| `SaveConfig()` | Сохраняет значения, у которых `SaveValue == true`. |
| `Log(Text)` | Добавляет строку в очередной файл журнала. |
| `GetLogPath()` | Возвращает путь текущего файла журнала. |
| `Beautify(String)` | Выполняет простое форматирование строки конфигурации. |

### Типы хранилищ

- **Int** — числовые значения.
- **State** — булевы состояния.
- **String** — строковые значения.
- **Table** — таблицы и списки.

Для каждого типа доступны соответствующие методы чтения и записи:

| Тип | Чтение | Запись |
|---|---|---|
| Int | `GetInt(Key)` | `SetInt(Key, Value)` |
| State | `GetState(Key)` | `SetState(Key, Value)` |
| String | `GetString(Key)` | `SetString(Key, Value)` |
| Table | `GetTable(Key)` | `SetTable(Key, Value)` |

Операции над таблицами:

- `ChangeTable(Key, Index, Value)` — заменяет элемент по индексу/ключу.
- `InsertTable(Key, Value)` — добавляет элемент.
- `RemoveTable(Key, Value)` — удаляет первое совпадение.
- `FindTable(Key, Value)` — возвращает результат `table.find`.

## 3. Roblox и персонаж

| Метод | Назначение |
|---|---|
| `GetService(Service)` | Получает сервис через кэш сервисов. |
| `GetPlayer()` | Возвращает `Players.LocalPlayer`. |
| `GetCharacter()` | Возвращает текущего персонажа или ожидает `CharacterAdded`. |
| `GetHumanoid()` | Возвращает Humanoid персонажа. |
| `GetHRP()` | Возвращает `HumanoidRootPart`. |
| `GetRoot()` | Возвращает `LowerTorso.Root`, если структура персонажа это поддерживает. |
| `GetHorse()` | Ищет лошадь локального игрока в Workspace. |
| `Teleport(CF, Offset)` | Перемещает персонажа к CFrame или Vector3 с необязательным смещением. |
| `Freeze(State)` | Устанавливает WalkSpeed в 0 или 16. |
| `GetDistance(Part)` | Возвращает округлённое расстояние от персонажа до Part. |
| `HasProperty(Part, Property)` | Проверяет доступность свойства через pcall. |
| `Compare(A, B)` | Сравнивает дочерние объекты по набору визуальных и физических свойств. |

## 4. Предметы и игровые действия

| Метод | Назначение |
|---|---|
| `Identify(Item)` | Определяет предмет по ProximityPrompt.ObjectText. |
| `WaitTillBackpackLoaded()` | Ожидает стабилизации количества предметов в Backpack. |
| `CountItem(Item)` | Считает предметы в Backpack и экипированном Tool. |
| `Has2X()` | Проверяет владение указанным GamePass. |
| `IsMax(Item)` | Сравнивает количество предмета с заданным лимитом. |
| `AddToQueue(Item)` | Добавляет предмет в очередь и создаёт ESP-метку. |
| `Collect(Item)` | Пытается подобрать предмет игровым взаимодействием. |
| `FarmItems(List)` | Запускает сбор предметов из списка. |
| `Click(Button, Manual)` | Вызывает обработчики MouseButton1Click через executor connections API. |
| `LearnSkills(Skills)` | Отправляет запросы изучения перечисленных навыков. |
| `UseRoka()` | Использует Rokakaka через игровой интерфейс. |
| `UseArrow()` | Использует Mysterious Arrow. |
| `UseRib()` | Использует Rib Cage. |
| `UseMove(Move)` | Активирует указанное движение/атаку. |
| `Stats()` | Выполняет распределение характеристик по логике скрипта. |

## 5. Стенды, скины и автоматизация

| Метод | Назначение |
|---|---|
| `HasStand()` | Проверяет наличие стенда. |
| `HasShiny()` | Проверяет, есть ли у текущего стенда скин. |
| `CheckShiny()` | Возвращает название скина текущего стенда. |
| `CheckStand()` | Возвращает название текущего стенда. |
| `GetPity()` | Получает текущее значение pity. |
| `EquipStand()` | Выполняет экипировку стенда по логике скрипта. |
| `Kill(Enemy)` | Выполняет боевую логику против указанной цели. |
| `GetStroke()` | Возвращает параметры движения для dash-механики. |
| `GetQuest(NPC)` | Получает/обрабатывает данные задания для NPC. |
| `IsSBR()` | Проверяет, запущен ли режим Steel Ball Run. |
| `SearchPlayer(Name)` | Ищет игрока по имени или ближайшему совпадению. |

**Примечание:** `Shiny` здесь означает именно скин стенда. `HasShiny()` проверяет наличие скина, а `CheckShiny()` возвращает его название.

## 6. Задачи и соединения

| Метод | Назначение |
|---|---|
| `AddTask(TaskName, Task)` | Сохраняет задачу или соединение под именем. |
| `IsTaskRunning(TaskName)` | Проверяет наличие задачи с указанным именем. |
| `DisconnectTask(TaskName)` | Отключает и удаляет сохранённую задачу/соединение. |
| `ReplaceReset()` | Заменяет обработчик сброса персонажа. |

## 7. Webhook

`SendWebhook(webhook, msg, title)` отправляет Discord embed через HTTP POST и executor-функцию запроса.

**Важно:** встроенные старые webhook URL удалены. Для отправки требуется собственный URL в конфигурационном значении `Webhook`. Не храните действующие URL в публичном репозитории.

## 8. Прочие утилиты

- `Censor(string)` — преобразует имя игрока по внутренним правилам скрипта.
- `LabelChar(Chars)` — добавляет подпись к модели персонажа.
- `GetStroke()` — возвращает направление и идентификатор анимации dash.

## 9. GUI API

GUI-модуль загружается как отдельная библиотека:

```lua
local Library = loadstring(game:HttpGet(
    "https://raw.githubusercontent.com/wut780/xenonV3_extr/main/libXenonV3GUI.lua"
))()
```

### Создание окна и вкладок

```lua
local Window = Library.CreateLib("Заголовок", "Тема")
local Tab = Window:Tab("Player", "rbxassetid://...")
local Section = Tab:Section("Основные настройки")
```

### Элементы интерфейса

| Метод | Сигнатура | Назначение |
|---|---|---|
| Button | `Section:Button(Name, Callback, Description)` | Кнопка. |
| Toggle | `Section:Toggle(Name, StartingState, Callback, Description, RunOnStart)` | Переключатель. |
| Dropdown | `Section:Dropdown(Name, List, Callback, Description)` | Выпадающий список. |
| TextBox | `Section:TextBox(Name, Callback, Description)` | Поле ввода. |
| Keybind | `Section:Keybind(Name, Starting_Key, Callback, Description, Blacklisted_Keys)` | Назначение клавиши. |
| Slider | `Section:Slider(Name, Min, Max, Start, Callback, Precise, Description)` | Ползунок. |
| Label | `Section:Label(Text)` | Текстовая метка. |

### Уведомления и обновление элементов

```lua
Library:Notification("Заголовок", "Текст", 3)
Library:UpdateNotifications()
Element:UpdateAsset(NewValue)
```

`UpdateAsset` обновляет значение существующего элемента. Допустимый тип значения зависит от типа элемента.

### Горячие клавиши окна

- `RightAlt` — показать или скрыть основное окно.
- `RightControl` — показать или скрыть контейнер вкладок/нижнее меню.

### Внешний ресурс GUI

GUI-библиотека использует Roblox asset с идентификатором `12403182534`. Наличие Lua-файла в репозитории не включает сам asset и не гарантирует его доступность.
