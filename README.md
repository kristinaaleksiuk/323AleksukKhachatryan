# Практическая работа № 8.2 "Тестирование базы данных"

**Дисциплина:** Поддержка и тестирование программных модулей  

**Выполнили:**  
$${\color{purple}Алексюк\ Кристина\ и\ Хачатрян\ Асмик}$$ 
**Группа:** 3ИСиП-323  

### Цель работы: провести проектирование, разработку и комплексное тестирование базы данных в СУБД Microsoft SQL Server с использованием языка запросов T-SQL, платформы модульного тестирования tSQLt, инструмента SQLQueryStress, а также встроенных средств операционной системы Windows.

##  $${\color{purple}Краткие\ теоретические\ сведения:}$$
### В мире тестирования программного обеспечения знание языка запросов SQL является важным навыком для любого тестировщика и QA-инженера. SQL-запросы помогают тестировщикам проверять данные, находить ошибки и обеспечивать качество продукта.
Рассмотрим топ 5 SQL-запросов для тестировщика:
### 1. SELECT: запрос используется для извлечения данных из базы данных. Это основной инструмент для получения информации.
### SELECT * FROM users
Этот запрос извлекает все данные из таблицы users.
### Операторы, расширяющие возможности запроса SELECT:
WHERE: позволяет фильтровать данные по определенным условиям.
SELECT * FROM users WHERE age > 30
Этот запрос извлекает всех пользователей старше 30 лет.
JOIN: используется для объединения данных из двух или более таблиц.
SELECT orders.id, users.name
FROM orders
JOIN users ON orders.user_id = users.id
Этот запрос объединяет таблицы orders и users по полю user_id и извлекает идентификаторы заказов и имена пользователей.
GROUP BY: используется для группировки данных по одному или нескольким столбцам.
SELECT COUNT(*), country
FROM users
GROUP BY country
Этот запрос подсчитывает количество пользователей в каждой стране.
HAVING: используется для фильтрации данных после группировки.
SELECT COUNT(*), country
FROM users
GROUP BY country
HAVING COUNT(*) > 10
Этот запрос извлекает страны, в которых количество пользователей больше 10.
ORDER BY: используется для сортировки данных.
SELECT * FROM users
ORDER BY age DESC
Этот запрос сортирует пользователей по возрасту в порядке убывания.
UNION: используется для объединения результатов двух или более SELECT запросов.
SELECT name FROM users
UNION
SELECT name FROM admins
Этот запрос объединяет имена пользователей из таблиц users и admins.

### 2. INSERT: запрос используется для добавления новых данных в таблицу.
INSERT INTO users (name, age, country)
VALUES ('John Doe', 28, 'USA')
Этот запрос добавляет нового пользователя в таблицу users.

### 3. UPDATE: запрос используется для обновления существующих данных.
UPDATE users
SET age = 29
WHERE name = 'John Doe'
Этот запрос обновляет возраст пользователя с именем 'John Doe'.

### 4. DELETE: запрос используется для удаления данных из таблицы.
DELETE FROM users
WHERE age < 18
Этот запрос удаляет всех пользователей младше 18 лет.

### 5. CREATE (DATABASE, TABLE, PROCEDURE, VIEW, INDEX): запрос используется для создания соответственно базы данных, таблицы, хранимой процедуры, представления, индекса и др.
Перечисленные запросы помогают ИТ-специалистам эффективно работать с базами данных, обеспечивать высокое качество тестирования и соответствуют CRUD-операциям (Create, Read, Update, Delete), представляющим собой основные методы работы с базами данных.

##  $${\color{purple}1.\ ER-Диаграмма}$$
 <img width="974" height="521" alt="Image" src="https://github.com/user-attachments/assets/99688fba-340c-4c08-aa8a-e8b9bf307645" />

## $${\color{purple}2.\ Запросы}$$                                                                
### Задание №1: Найти тренировки с членами клуба, у которых нет активного абонемента

<img width="1322" height="762" alt="Image" src="https://github.com/user-attachments/assets/1b695f73-e9d6-47c4-9ee4-0de2cc601d9a" />

### Задание №2: Проверить загрузку тренера (более 8 тренировок в день)

<img width="726" height="359" alt="Image" src="https://github.com/user-attachments/assets/755f563a-07ac-436e-9b87-d11871979750" />




### Задание №3: Найти «призраков» (MemberID и TrainerID - один и тот же человек)

<img width="1085" height="789" alt="Image" src="https://github.com/user-attachments/assets/d30d2f7f-bc3e-4037-bf3b-347160e8643c" />

##  $${\color{purple}3.\  Создание\ хранимых\ процедур\ для\ тестирования}$$    
### 3.1 Процедура проверки активности абонемента

<img width="736" height="469" alt="Image" src="https://github.com/user-attachments/assets/b760a4ff-0209-425d-999c-377b04cb81b3" />

### 3.2 Процедура проверки загрузки тренера

<img width="867" height="563" alt="Image" src="https://github.com/user-attachments/assets/a14c2722-9db9-4df5-b7c9-cb74ee420898" />

## $${\color{purple}4.\   Тестирование\ с\ использованием\ tSQLt}$$    

<img width="1005" height="499" alt="Image" src="https://github.com/user-attachments/assets/99c5df6c-fd57-4f74-bb2b-f240676be1c7" />


### $${\color{purple} Хранимая\ процедура\ AddMember}$$     

<img width="948" height="717" alt="Image" src="https://github.com/user-attachments/assets/1e40fd8f-88da-446a-a801-e4b20ddfe9e5" />

### $${\color{purple} Хранимая\ процедура\ BuyMembership}$$   

<img width="514" height="142" alt="Image" src="https://github.com/user-attachments/assets/5d59a6e4-f4c4-4c54-ad16-1da71d19e927" />

##  $${\color{purple}Примеры\ использования\ процедур}$$   

### Добавление нового члена клуба

<img width="530" height="299" alt="Image" src="https://github.com/user-attachments/assets/93278e64-75be-453e-9e68-93a53a0acdac" />

### Покупка нового абонемента
<img width="631" height="290" alt="Image" src="https://github.com/user-attachments/assets/7106ab21-99bf-416e-b0bc-4e6ebd349a64" />

## $${\color{purple} 4.\ Стресс-тестирование\ с\ SQLQueryStress}$$ 
### 4.1 Тестирование процедуры проверки абонемента
<img width="1155" height="536" alt="Image" src="https://github.com/user-attachments/assets/0f6ae6a5-c1f1-4161-b677-b8694a18ca0e" />

### 4.2 Тестирование поиска проблемных тренировок
<img width="1157" height="529" alt="Image" src="https://github.com/user-attachments/assets/d2a59e09-f9a5-4301-9f21-09ddaad2eec4" />

###  Анализ загрузки процессора
<img width="1492" height="536" alt="Image" src="https://github.com/user-attachments/assets/5d4f6a81-2906-42f6-87fa-c3412b57f542" />

 $${\color{purple} Cловарь\ к\ Фитнес клубу}$$ 
[Словарь к Фитнес клубу.xlsx](https://github.com/user-attachments/files/27235166/default.xlsx)
