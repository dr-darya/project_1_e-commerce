# Проект e-commerce: анализ совершенных покупок
> Стек - pandas, seaborn, matplotlib, requests

____
## Этапы 

1. Определить сколько пользователей, которые совершили покупку только один раз.
2. Найти сколько заказов в месяц в среднем не доставляется по разным причинам (вывести детализацию по причинам).
3. По каждому товару определить, в какой день недели товар чаще всего покупается.
4. Определить сколько у каждого из пользователей в среднем покупок в неделю (по месяцам). Внутри месяца может быть не целое количество недель. Например, в ноябре 2021 года 4,28 недели. И внутри метрики это нужно учесть.
5. Используя pandas, провести когортный анализ пользователей. В период с января по декабрь выяви когорту с самым высоким retention на 3й месяц. 
6. Построить RFM-сегментацию пользователей, чтобы качественно оценить аудиторию. 

**Цель  - для решения задачи нужно провести предварительное исследование данных и сформулировать, что должно считаться покупкой.**
____
## Реализация проекта
1. Проанализировала поведение пользователей, сформулировала, что считается совершенной покупкой. Совершенной покупкой считается заказ, который был доставлен, с указанной датой подтверждения заказа, а также с указанным временем доставки заказа покупателю.
2. Провела анализ заказов, определила две причины, по которым не доставляются заказы: заказ отменен, заказ недоступен.
3. Определила, что **понедельник и пятница** лидируют по количеству проданных товаров.
4. Провела когортный анализ, который показал, что показатель retention 0,43% для онлайн магазина довольно низкий. Обычно хороший показатель retention для онлайн магазина составляет от 20% до 40% и выше, в зависимости от отрасли и конкретного бизнеса. Этот показатель означает, что только очень небольшая часть клиентов возвращается в магазин, что может свидетельствовать о проблемах с удержанием клиентов.
5. Построила RFM-сегментацию пользователей. Выделила следующие сегменты:
   - лояльные пользователи;
   - лояльные, на грани ухода;
   - многообещающие пользователи;
   - новые пользователи, потратили немного;
   - потерянные доходные клиенты;
   - потерянные недоходные пользователи.
     
  *Большая часть клиентов - **потерянные недоходные клиенты и новые пользователи, которые потратили от 180-500 на покупки**. Лояльных клиентов у сервиса можно сказать нет (14 лояльных из 42 004)*
  
**Изучив данные покупок интернет-магазина(предположим). Я выявила основную проблему: сервис не работает над удержанием и вовлечением клиентов.**

**Рекомендации**

1. Нужно внедрить прозвон ушедших клиентов, клиентов, которые покупают редко, с целью: узнать причину их ухода. Может быть они недовольны доставкой, грубое отношение сотрудников, недовольный работой сервиса, может быть сервис подвисает, картинки не грузятся.
2. Знакомить клиентов с товарной линейкой. Напоминать о себе (звонки, уведомления, акции).
3. Разработать программу лояльности, давать персональные предложения. Мотивировать покупателей совершать повторные покупки. Это может быть клубная карта, персональные скидки, накопительные бонусы и другие приятные возможности, которыми могут воспользоваться постоянные клиенты
4. Проанализировать конкурентов.
5. Сделать сайт/приложение максимально удобным для пользователя.

______
#### Описание данных:
**1. olist_customers_datase.csv — таблица с уникальными идентификаторами пользователей**
- customer_id — позаказный идентификатор пользователя
- customer_unique_id —  уникальный идентификатор пользователя  (аналог номера паспорта)
- customer_zip_code_prefix —  почтовый индекс пользователя
- customer_city —  город доставки пользователя
- customer_state —  штат доставки пользователя
**2. olist_orders_dataset.csv —  таблица заказов**
- order_id —  уникальный идентификатор заказа (номер чека)
- customer_id —  позаказный идентификатор пользователя
- order_status —  статус заказа
- order_purchase_timestamp —  время создания заказа
- order_approved_at —  время подтверждения оплаты заказа
- order_delivered_carrier_date —  время передачи заказа в логистическую службу
- order_delivered_customer_date —  время доставки заказа
- order_estimated_delivery_date —  обещанная дата доставки
**3. olist_order_items_dataset.csv —  товарные позиции, входящие в заказы**
- order_id —  уникальный идентификатор заказа (номер чека)
- order_item_id —  идентификатор товара внутри одного заказа
- product_id —  ид товара (аналог штрихкода)
- seller_id — ид производителя товара
- shipping_limit_date —  максимальная дата доставки продавцом для передачи заказа партнеру по логистике
- price —  цена за единицу товара
- freight_value —  вес товара

**Уникальные статусы заказов в таблице olist_orders_dataset:**
- created —  создан
- approved —  подтверждён
- invoiced —  выставлен счёт
- processing —  в процессе сборки заказа
- shipped —  отгружен со склада
- delivered —  доставлен пользователю
- unavailable —  недоступен
- canceled —  отменён
