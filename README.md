### Лабораторная работа №6 — Инструментирование кода (Аннотации и Reflection)
## Описание проекта
Проект представляет собой учебную реализацию задач по созданию собственных аннотаций и их обработке с помощью Reflection API.
Выполнены все пункты задания 1 (6 аннотаций) и задания 2 (JUnit-тесты для @Default и @Cache).
Все классы находятся в отдельных файлах, организованы по пакетам.
Программа имеет дружественный интерфейс: в Main.java последовательно демонстрируется работа каждой аннотации.
# Технологии:

Java 17
Maven
JUnit Jupiter 5.10.3
Reflection API
Собственные аннотации с @Retention(RUNTIME)

# Состав проекта (файлы)
textsrc/main/java/org/example/
├── annotations/             ← собственные аннотации
│   ├── Invoke.java
│   ├── Default.java
│   ├── ToString.java
│   ├── Validate.java
│   ├── Two.java
│   └── Cache.java
├── model/                   ← тестовые классы с аннотациями
│   ├── DemoInvoke.java
│   ├── Person.java
│   ├── ValidatedClass.java
│   ├── WithTwo.java
│   └── CachedClass.java
├── processor/               ← обработчики аннотаций
│   ├── InvokeProcessor.java
│   ├── DefaultProcessor.java
│   ├── ToStringProcessor.java
│   ├── ValidateProcessor.java
│   ├── TwoProcessor.java
│   └── CacheProcessor.java
└── Main.java                ← точка входа, дружественный вывод

src/test/java/org/example/
    ├── DefaultAnnotationTest.java   ← параметризованный тест
    └── CacheAnnotationTest.java
# Задание 1 — Аннотации
1. @Invoke
textАннотация для методов. Обработчик автоматически вызывает все методы, помеченные @Invoke.
Пример вывода:
Привет из @Invoke!
Всё работает!
2. @Default
textОбязательное свойство value типа Class<?>. Применена к классу Person.
Обработчик выводит тип по умолчанию: java.lang.String
3. @ToString
textНеобязательное свойство Mode (YES/NO), по умолчанию YES.
Класс Person помечен @ToString, поле password — @ToString(NO).
Результат:
Person{name=Алексей, age=25}
(пароль скрыт)
4. @Validate
textОбязательное свойство value типа Class[].
Применена к ValidatedClass с тремя классами.
Результат:
Валидация: String Integer Person
5. @Two
textДва обязательных свойства first (String) и second (int).
Применена к WithTwo("Hello", 2025).
Результат:
first = "Hello", second = 2025
6. @Cache
textНеобязательное свойство value типа String[], по умолчанию пустой массив.
Применена к CachedClass с тремя областями.
Результат:
Кешируемые области: users orders products
(для класса без аннотации — сообщение «Кеш не настроен»)
Задание 2 — Тестирование
Тесты @Default

Параметризованный тест (@ParameterizedTest)
Проверка наличия аннотации и корректности значения value
Статус: PASSED

# Тесты @Cache

Проверка корректного чтения массива кешируемых областей
Проверка случая пустого массива (по умолчанию)
Статус: PASSED (2/2)

# Вывод программы (Main.java)
text=== ЛАБОРАТОРНАЯ РАБОТА №6 — АННОТАЦИИ ===


1. @Invoke
Привет из @Invoke!
Всё работает!

2. @Default
Тип по умолчанию: java.lang.String

3. @ToString
Person{name=Алексей, age=25}

4. @Validate
Валидация: String Integer Person

5. @Two
first = "Hello", second = 2025

6. @Cache
Кешируемые области: users orders products
Кеш не настроен

реализованы все 6 аннотаций с указанными характеристиками;
написаны обработчики через Reflection;
реализованы параметризованные тесты JUnit;
присутствует дружественный консольный интерфейс;
код соответствует Google Java Style;
каждый класс в отдельном файле, логичное разделение по пакетам.

Автор: Белоногов Всеволод Алексеевич
Группа: ИТ-9
Год: 2025
