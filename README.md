# Практическая работа № 8.1 "Тестирование базы данных"

**Дисциплина:** Поддержка и тестирование программных модулей  

**Выполнили:**  
 Алексюк Кристина и Хачатрян Асмик  

**Группа:** 3ИСиП-323  

---

## Цель работы

Провести нагрузочное тестирование и тестирование производительности баз данных в СУБД Microsoft SQL Server с использованием:
- языка запросов T-SQL,
- платформы модульного тестирования tSQLt,
- инструмента SQLQueryStress,
- встроенных средств операционной системы Windows.

---

## 1. Проектирование базы данных

### 1.1 ER-диаграмма (dbdiagram.io)

Спроектирована база данных интернет-магазина техники, содержащая следующие таблицы:
- `Customers` — клиенты
- `Products` — товары
- `Orders` — заказы
- `OrderDetails` — детали заказов

#### Скриншот ER-диаграммы

![ER-диаграмма](https://github.com/user-attachments/assets/8a16ce62-210a-4981-a2cc-75ce411f791d)

#### Код для построения диаграммы (dbml)

Table Orders {
  OrderID int [pk, not null]
  CustomerID int [not null]
  OrderDate datetime [not null]
  CustomerNotes nvarchar(max) [null]
  IsProcessed bit [not null]
}

Table Products {
  ProductID int [pk, not null]
  Name nvarchar(150) [not null]
  Price money [not null]
  Image varchar(max) [null]
  Description nvarchar(max) [null]
}

Table Customers {
  CustomerID int [pk, not null]
  FullName nvarchar(150) [null]
  Email varchar(50) [null]
  Phone varchar(50) [null]
}

Table OrderDetails {
  OrderID int [pk, not null]
  ProductID int [pk, not null]
  Quantity int [not null]
}

Ref: Orders.CustomerID > Customers.CustomerID
Ref: OrderDetails.OrderID > Orders.OrderID
Ref: OrderDetails.ProductID > Products.ProductID

### Заполнение таблиц тестовыми данными

![](https://github.com/user-attachments/assets/d6d83ed4-b152-4f02-8b1a-bf11b1119c74)

### Выполнение запросов и хранимых процедур

![](https://github.com/user-attachments/assets/7b9f48ee-2d4b-4481-bd73-c68fa80326eb)

### Хранимая процедура: Добавление нового заказа

![](https://github.com/user-attachments/assets/1456d1e1-3895-4487-b051-ef8b99899794)

### Нагрузочное тестирование (SQLQueryStress)

![](https://github.com/user-attachments/assets/18fc84d0-a425-4e07-ab05-bc5b97e74efa)

![](https://github.com/user-attachments/assets/f23438e4-10a4-4273-a56b-767db25d9760)

### Результаты нагрузочного тестирования

![](https://github.com/user-attachments/assets/7ba24c4b-24c8-4566-930f-e3a47879087d)

Модульное тестирование (tSQLt)
Установка tSQLt

EXEC tSQLt.NewTestClass 'TestOnlineStore';

### Пример модульного теста
CREATE PROCEDURE TestOnlineStore.[test that order can be inserted]
AS
BEGIN
    -- Arrange
    INSERT INTO Customers (FullName, Email, Phone) 
    VALUES ('Test User', 'test@test.com', '123456789');
    DECLARE @customerId INT = SCOPE_IDENTITY();
    
    -- Act
    INSERT INTO Orders (CustomerID, OrderDate, IsProcessed) 
    VALUES (@customerId, GETDATE(), 0);
    DECLARE @orderId INT = SCOPE_IDENTITY();
    
    -- Assert
    SELECT @orderId INTO expected;
    SELECT OrderID INTO actual FROM Orders WHERE OrderID = @orderId;
    
    EXEC tSQLt.AssertEqualsTable 'expected', 'actual'; END;

### Скриншот Performance Monitor
![](https://github.com/user-attachments/assets/a3828c38-ca68-43a0-a57d-d39010155059)

### В ходе выполнения практической работы были достигнуты следующие результаты:

- Функциональное тестирование
Проверена корректность работы FOREIGN KEY и PRIMARY KEY ограничений.

- Написаны и успешно выполнены SELECT-запросы различной сложности.

- Создана и протестирована хранимая процедура добавления заказа.

- Нагрузочное тестирование (SQLQueryStress)
База данных выдержала нагрузку 50 параллельных потоков по 1000 запросов.

Среднее время ответа составило 24 мс, ошибок не зафиксировано.
Это свидетельствует о хорошей производительности при средней нагрузке.

-  Модульное тестирование (tSQLt)
Написаны тесты для проверки вставки записей и работы ограничений целостности.

- Все тесты пройдены успешно.
- Мониторинг системных ресурсов
Утилизация процессора в момент нагрузочного тестирования не превышала 65%.

### Приложение: Словарь данных (Data Dictionary)

[Словарь к БД.xlsx](https://github.com/user-attachments/files/27007693/default.xlsx)
