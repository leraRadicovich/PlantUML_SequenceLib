# Библиотека PlantUML_SequenceLib

## Краткое описание библиотеки:
Библиотека SequenceLib v.4.0 представляет собой набор макросов для PlantUML, упрощающих создание sequence-диаграмм с набором функций:

* Автогенерация документации напрямую из кода диаграммы, исключая расхождения.
* Стандартизированное оформление (цвета, шрифты, активации)
* Связь с техническими требованиями через автопосторение таблиц с задачами для разработки
* Автоматизацией рутинных действий

Библиотека SequenceLib превращает PlantUML в инструмент для end-to-end документирования, связывая визуальные диаграммы с техническими требованиями и метаданными.
Модель становится основой для ТЗ разработчикам (ошибки, параметры, сценарии). Сокращается объем рукописного кода на 40-60% за счет макросов, как следствие сокращается время на создание диаграммы и постановки.
Генерация ТЗ стала возможна благодаря автоматической генерации таблиц с описанием элементов диаграммы: параметры, конфиги, события аудита, метрик и ошибки.

Пример диаграммы последовательности AS IS (оригинальный синтаксис)   
![AS IS.png](SequenceLib%2FRESOURCES%2FAS%20IS.png)

Пример диаграммы последовательности TO BE (с применением библиотеки)  
![TO BE.png](SequenceLib%2FRESOURCES%2FTO%20BE.png)

Благодаря встроенной функции PlanntUML, позволяющей сгенерировать диаграмму в формате unicode ASCII,  
мы можем получить доступ к сгенерированной автоматический документации.  
![get ASCII.png](SequenceLib%2FRESOURCES%2Fget%20ASCII.png)

После извлечения данных вы получаете документацию в формате markdown/adoc   
![Автосписки.png](SequenceLib%2FRESOURCES%2F%D0%90%D0%B2%D1%82%D0%BE%D1%81%D0%BF%D0%B8%D1%81%D0%BA%D0%B8.png)


## С чего начать?
### Установка библиотеки
Библиотека совместима с версией PlantUml 1.2024.7 и выше.   
Для проверки установленной версии введите команду version в любой PlantUML-диаграмме.  
![version.png](SequenceLib%2FRESOURCES%2Fversion.png)


1. Скачайте [архив](https://github.com/leraRadicovich/PlantUML_SequenceLib/archive/refs/tags/REaleas_for_AD.zip) с последним релизом библиотеки. 
2. Распакуйте архив в выбранной папке
3. Создайте диаграмму plantUML с помощью установленного плагина
4. Включить в созданную диаграмму файл-указатель библиотеки:
5. Инициируйте использование библиотеки командой diagramInit
```
!include C:\Path\To..\SequenceLibIncludeFile_v4.puml
diagramInit(final, "Загловок диаграммы") 
```

### Шаблоны liveTemplates (IntellijIDEA)
Если вы используете IntellijIDEA, рекомендую воспользоваться подготовленными live templates.  
Подробнее про live templates: https://www.jetbrains.com/help/idea/using-live-templates.html  
В распакованном релизе, в разделе RESOURCES расположен архив с шаблонами [PlantUmlTemplates.zip](SequenceLib%2FRESOURCES%2FPlantUmlTemplates.zip), повторяющими наименования процедур библиотеки.  
Импортируйте их в свою среду разработки по [инструкции](https://www.jetbrains.com/help/idea/sharing-live-templates.html#export-and-import-live-templates-manually) для более комфортной работы с библиотекой.

## Описание функций и процедур библиотеки
### 1. Инициализация диаграммы

diagramInit(mode, title)
* mode: final / draft
* final — полное оформление с легендами, таблицами, нумерацией.
* draft — черновик (только визуальные элементы).
* title: Заголовок диаграммы (отображается в верхней части).

Пример:
```
!include C:\Path\To\SequenceLibIncludeFile_v4.puml
diagramInit(final, "Диаграмма аутентификации") 
```

### 2. Объявление участников
parties(type, name, alias, order)
* type: Тип участника (participant, actor, database и т.д.).
* name: Отображаемое имя (например, "Пользователь").
* alias: Логическое имя для ссылок (например, user).
* order: Позиция в списке (опционально, для сортировки).

Пример:
```
parties(actor, "Пользователь", user, 1)  /'Создание актора'/
parties(srv, "",,)     /'Создание участника из списка кастомных участников'/
BOX("Группа", #lightblue, "user, srv")    /'Группировка участников'/
```

### 3. Взаимодействие участников
rq(from, to, activation, message, comment)
* from: Отправитель (alias).
* to: Получатель (alias).
* activation: ++ (активация линии жизни получателя).
* message: Текст сообщения (поддерживает Markdown).
* comment: Описание для таблиц документации (опционально).

Пример:
```
rq(to, from, activation, message, comment) /'Сплошная стрелка между участниками'/
rs(to, from, activation, message, comment) /'Пунктирная стрелка между участниками'/
rq(, , activation, message, comment) /'Автоовтет: сплошная стрелка между участниками'/
rs(, , activation, message, comment) /'Автоовтет: пунктирная стрелка между участниками'/
rq(*, N, activation, message, comment) /'Копирование участников, направления и типа стрелки с номером N'/
```

### 4. Служебные функции
audit(code, status, description, target)
* code: Код события
* status: SUCCESS/FAILURE
* description: Описание
* target: Участник-источник

Пример:
```
audit(AUTH_LOGIN, SUCCESS, "Успех", srv)
```

config(name, type)	
* name: имя параметра
* type: тип параметра

Пример:
```
config(passLoginEnabled, boolean)
```

SEPARATOR(text)	Создает разделитель с текстом (например, SEPARATOR("Первый вход")).    
todo("Текст")	Добавляет задачу в таблицу "Список доработок".            
error(code)	Фиксирует ошибку (например, error(login_disabled)).           
anchor(name)	Создает якор - переменную с номером стрелки, после которой он вызван  (например, anchor(st1)).      

### 5. Заметки и ссылки
NOTE(position1, position2, color, "text")
REF(position1, position2, color, "text")
* position: left, right, over (позиция относительно участника).
* target: Участник, к которому привязана заметка (alias).
* color: Цвет фона (например, #azure).
* text: Текст заметки.

Пример:
```
NOTE(left, srv, azure, "Пример заметки слева")
REF(, , azure, "Пример заметки across")                     
```
### 6. Автоматизация
LEGEND()	Генерирует все таблицы (процесс, доработки, ошибки, параметры) в легенде.                  
ORIGINAL()	Добавляет на диаграмму заметку с исходным кодом диаграммы (обратная совместимость, требует валидации положения участников).         
HELP()	    Добавляет раздел с пояснением процедур и функций библиотеки, если передать имя процедуры - предоставит описание только указанной процедуры.

Пример:
```
LEGEND()
ORIGINAL()
HELP()
HELP(LEGEND)
```

## Кастомные настройки
В папке [customerEnviroment](SequenceLib%2Fsrc%2FcustomerEnviroment) расположены файлы, позволяющие задать кастомные настройки пользователя:
1. Кастомизация участников - создание списка участников, часто испльзуемых пользователем.
2. Кастомизация событий аудита - создание списка событий аудита, часто испльзуемых пользователем.
3. Кастомизация конфигурационных параметров  - создание списка параметров и настроек, часто испльзуемых пользователем.
4. Кастомизация ошибок  - создание списка ошибок, часто испльзуемых пользователем.
5. Кастомизация метрик - создание списка метрик, часто испльзуемых пользователем.
6. Кастомизация типов участников - создание списка типов участников для автоопределения типа участника при его инициализации.

Задайте необходимые настройки по аналогии и ускорьте работу с диаграммами.
#### Кастомизация участников и типов участников?   
Ниже на скрине представлена диаграмма, в которой отсутствует объявление участников диаграммы: Ваш сервис и in-memory cashe Redis.   
При этом диаграмма генерируется, с учетом кастомных настроек.   
![Кастомизация участников.png](SequenceLib%2FRESOURCES%2F%D0%9A%D0%B0%D1%81%D1%82%D0%BE%D0%BC%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F%20%D1%83%D1%87%D0%B0%D1%81%D1%82%D0%BD%D0%B8%D0%BA%D0%BE%D0%B2.png)

#### Кастомизация настроек, аудита, ошибок и метрик в связке со списком доработок
Если вы вызываете одну из следующих команд:
* audit()
* config()
* error()
* metric(),

и указываете в качестве параметров те, которые отсутствуют в одноименных файлах кастомизации, библиотека автоматически  
вызовет процедуру todo() и создаст строку с задачей на разработку, а также добавит строку с предзаполненным описанием ошибки     
в списке Новых ошибок. См. пример ниже:   
Текущие кастомные ошибки:    
![customErrorMap.png](SequenceLib%2FRESOURCES%2FcustomErrorMap.png)   

Пример диаграммы с созданием новой ошибки, см. таблицы: Список доработок и Новые ошибки:     
![new Error.png](SequenceLib%2FRESOURCES%2Fnew%20Error.png)     

При этом в Легенде создаются таблицы только с новыми элементами. В примере не создаются новые события аудита или метрики, поэтому эти таблицы отсутствуют в легенде.
