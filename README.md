# Лабораторная работа №6 — Инструментирование кода (Аннотации + Reflection)
Проект полностью решает лабораторную работу №6 по дисциплине «Инструментирование кода».
## Выполнены все требования:

Решение всех 6 задач задания 1 — в отдельных классах
Все аннотации доступны во время выполнения (@Retention(RUNTIME))
Реализованы обработчики через Reflection API
В Main.java — дружественный консольный интерфейс с последовательным запуском всех задач
Полная XML-документация (Javadoc) у каждого класса и метода → +2 балла
Проверка на null, обработка исключений
Google Java Style, каждый класс в отдельном файле, логичное разделение по пакетам
Задание 2: тесты JUnit 5 для @Default (параметризованный) и @Cache (с проверкой пустого массива)


### JavaСостав проекта (каждый класс решает конкретную часть задания)
```java
src/main/java/org/example/
├── annotations/                     ← Задание 1: сами аннотации
│   ├── Invoke.java                  ← п.1 — только для методов, без свойств
│   ├── Default.java                 ← п.2 — обязательное свойство Class<?>
│   ├── ToString.java                ← п.3 — Mode YES/NO, по умолчанию YES
│   ├── Validate.java                ← п.4 — Class[] value
│   ├── Two.java                     ← п.5 — два обязательных свойства
│   └── Cache.java                   ← п.6 — String[] value, default {}
│
├── model/                           ← примеры применения аннотаций
│   ├── DemoInvoke.java              ← п.1 — класс с несколькими @Invoke-методами
│   ├── Person.java                  ← п.2 и п.3 — @Default + @ToString + поле с NO
│   ├── ValidatedClass.java          ← п.4 — @Validate с тремя классами
│   ├── WithTwo.java                 ← п.5 — @Two(first="Hello", second=2025)
│   └── CachedClass.java             ← п.6 — @Cache с несколькими областями
│
├── processor/                       ← обработчики через Reflection
│   ├── InvokeProcessor.java         ← п.1 — автоматический вызов методов
│   ├── DefaultProcessor.java        ← п.2 — выводит тип по умолчанию
│   ├── ToStringProcessor.java       ← п.3 — формирует строку с учётом Mode
│   ├── ValidateProcessor.java       ← п.4 — выводит список классов
│   ├── TwoProcessor.java            ← п.5 — читает first и second
│   └── CacheProcessor.java          ← п.6 — выводит области или сообщение о пустом кэше
│
└── Main.java                        ← дружественный интерфейс: запуск всех задач подряд

src/test/java/org/example/
    ├── DefaultAnnotationTest.java   ← Задание 2 п.3 — параметризованный тест @Default
    └── CacheAnnotationTest.java     ← Задание 2 п.5 — тесты @Cache (обычный и пустой массив)
```
```java
Блок 1 — @Invoke (п.1 задания)
JavaЗадача: аннотация только для методов, без свойств. Обработчик автоматически вызывает все помеченные методы.
Пример работы:
1. @Invoke
Привет из @Invoke!
Всё работает!

Описание реализации:
Аннотация: @Target(METHOD), @Retention(RUNTIME), без свойств.
Класс DemoInvoke содержит два метода с @Invoke.
InvokeProcessor.process(obj) — сканирует методы, вызывает invoke() у помеченных.
Блок 2 — @Default (п.2 задания)
JavaЗадача: обязательное свойство value типа Class<?>. Применяется к классу.
Пример работы:
2. @Default
Тип по умолчанию: java.lang.String

Описание реализации:
Аннотация: @Target({TYPE, FIELD}), обязательное свойство value.
Класс Person помечен @Default(String.class).
DefaultProcessor выводит ann.value().getName().
Блок 3 — @ToString (п.3 задания)
JavaЗадача: Mode YES/NO, по умолчанию YES. Поле с NO не попадает в результат.
Пример работы:
3. @ToString
Person{name=Алексей, age=25}

Описание реализации:
Аннотация: перечисление Mode, default YES.
Класс Person помечен @ToString, поле password — @ToString(NO).
ToStringProcessor сканирует поля, пропускает те, где Mode.NO.
Блок 4 — @Validate (п.4 задания)
JavaЗадача: обязательное свойство Class[].
Пример работы:
4. @Validate
Валидация: String Integer Person

Описание реализации:
Аннотация: @Target({TYPE, ANNOTATION_TYPE}), свойство value().
Класс ValidatedClass содержит @Validate({String.class, Integer.class, Person.class}).
ValidateProcessor выводит getSimpleName() каждого класса.
Блок 5 — @Two (п.5 задания)
JavaЗадача: два обязательных свойства first (String) и second (int).
Пример работы:
5. @Two
first = "Hello", second = 2025

Описание реализации:
Аннотация: обязательные свойства без default.
Класс WithTwo помечен @Two(first="Hello", second=2025).
TwoProcessor читает и выводит оба значения.
Блок 6 — @Cache (п.6 задания)
JavaЗадача: String[] value, по умолчанию пустой массив.
Пример работы:
6. @Cache
Кешируемые области: users orders products
Кеш не настроен

Описание реализации:
Аннотация: default {}.
Класс CachedClass содержит три области.
CacheProcessor проверяет длину массива и выводит соответствующее сообщение.
Блок 7 — Тестирование (задание 2)
DefaultAnnotationTest.java (п.3 задания)
JavaПараметризованный тест (@ParameterizedTest + @MethodSource)
Проверяет наличие аннотации и корректность значения value
Покрывает требование «проверка нескольких классов с разными типами по умолчанию»
Статус: PASSED
CacheAnnotationTest.java (п.5 задания)
JavaТесты:
1) корректное чтение непустого массива
2) случай пустого массива (по умолчанию)
Покрывает требования «если массив пуст — кэширование не производится» и «несколько именованных областей»
Статус: 2/2 PASSED
``` 
## Заключение
Проект полностью соответствует условию лабораторной работы №6:
```java
все 6 аннотаций реализованы строго по требованиям;
обработчики используют Reflection API;
присутствует дружественный интерфейс в Main.java;
каждый класс в отдельном файле, логичная пакетная структура;
полная Javadoc-документация;
реализованы оба теста задания 2 (один параметризованный);
код чистый, валидация null, Google Java Style.
```
### Автор: Белоногов Всеволод Алексеевич
Группа: ИТ-9
Год: 2025
