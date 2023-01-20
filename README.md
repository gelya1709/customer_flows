# customer_flows
Проект посвящен сегментации клиентов и выявлению наиболее эффективных видов рекламных акций на основе перетоков клиентов между кластерами во времени. Набор данных содержит более **20 млн наблюдений**. Анализ проводился на Python и R.

## Мотивация
Крупные ритейл компании запускают множество промоаций для поддержания активности покупателей. 
Задача маркетологов - планировать промокалендари таким образом, чтобы 
промоакции приносили максимальную пользу бизнесу. В связи с этим,
есть необходимость максимально эффективно использовать транзакционные данные для понимания поведения потребителей. 

**Задача этого проекта** - разработать новый метод анализа для принятия следующих решений:

- Какие сегменты потребителей можно выделить по их ценности для бизнеса?
- Какие типы промоакций самые эффективные и на каких сегментах потребителей?
- Какая должна быть продолжительность промоакций и когда их следует запускать?

## Данные
Период: 09.2019 - 09.2020

**Датасет №1 содержит информацию о транзакционных данных покупателях.**

Основные переменные: 
- id покупателя 
- дата покупки
- id чека
- кол-во продуктов
- цена

**Датасет №2 содержит информацию о промоакциях.**

Основные переменные: 
- тип промоакции (билборды, фасады, сезонные и двухнедельные) 
- id промоакции
- дата начала
- дата окончания

## Предобработка


## Сегментация 

1. Деление датасета на 52 периода (1 неделя = 1 период)
2. Выделение кластеров потребителей алгоритмом K-means **в каждом периоде**
    В качестве метрик для сегментации были выбраны **frequency** и **monetary** метрики (из RFM модели, подробнее см. [здесь](https://www.investopedia.com/terms/r/rfm-recency-frequency-monetary-value.asp))

**В результате получено 4 кластера потребителей:** 

- **churn** - не активны в рассматриваемый период
- **sleeping** - средний чек 2 000, 1 покупка в неделю
- **loyal** - средний чек 5 000, 3 покупки в неделю
- **champions** - средний чек 11 000, 3 покупки в неделю

### Подсчет перетоков 
Из 4-х кластеров, полученных в каждую из 52 недель, вычислены **16 перетоков** (например, from_churn_to_sleeping для 51 периода)

### Анализ промо датасета 
Среднее количество акций каждого типа для каждого периода:

- Билборды - 50
- Фасады - 70
- Сезонные промо - 150
- Двухнедельные промо - 130

## Вычисление перетоков между кластерами

1. SSA анализ, чтобы обнаружить трендовую составляющую и избавиться от шума
2. Метод первых разностей, чтобы привести распределение в норму
3. Обучение модели DBN для выявления причинно-следственных связей

## Выводы и рекомендации

1. Сегмент покупателей с низкой активностью (sleeping) требует постоянного стимулирования, иначе эти покупатели оттекают
-> **следует на постоянной основе запускать двухнедельные промоакции, рассчитанные на покупателя с низким средним чеком**

2. Билборды и фасады воздействуют на клиентов быстрее всего (в течение 1 недели). 
Наибольший эффект они оказывают на покупателей со средней и высокой активностью (loyal и champions сегменты)
-> билборды и фасады должны постоянно присутствовать для поддержания покупательской активности, однако их кол-во следует оптимизировать

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
3. Сезонные и двухнедельные промоакции имеют отложенный эффект и начинают работать спустя 2 недели с момента запуска
-> эти типы промоакций следует запускать за 2 недели до предполагаемого падения спроса
```






    
