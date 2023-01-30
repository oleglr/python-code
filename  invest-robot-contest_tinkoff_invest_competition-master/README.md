# конкурс Тинькофф инвестиций
0. Это пример торгового робота для участия в конкурсе. Робот тестировался до 24.02.2022 и имел торговый результат по ордерам находящимся в директории results/real_results.csv в данном файле есть order_id, FIGI и дата записи в файл по данным id вы можете посмотреть как торговал/работал с ордерами робот на реальных данных.
1. В роботе немного изменена логика принятия решений (многие коэффициенты специально сделаны по невыполнимым условиям/ заведомо являются неправильными (например как в daily_buy_functions.py условие index_quantille <= 10 - бессмысленно так как выполняется всегда) сделано это для секьюрности бизнес-логики. Тем не менее это не нарушает структуру кода
2. На проде робот работал каждый день с ограниченным списком торговых бумаг - этот список выбирался из анализа рынка (yfinance (брался индекс VIX и SPY), фундаментальных показателей компаний (finviz), индикаторов рынка и бумаги (ta-lib). Также строилась линейная регрессия по совокупности данных (через Sckit-learn), которая принимала решение торговать или не торговать отдельным инструментом. Этот код выплевывал каждый день нужный датафрейм акций, который непосредственно крутился на рынке. Логика выбора этого списка - не влияет на конкурс - этот код я не выкладываю в открытый доступ. 
3. Далее после формирования нужного датафрейма после 30 минут после начала основной сессии (в первые 30 минут могли быть неопределенные движения на рынке - робот никогда в эти полчаса не работал) начинался серчинг акций по списку (RobotSearcherAndFirstBuy.daily_searcher()). При выполнении локальных условий (на RSI, по квантилям цены по неделям, по квантилю цены по дням, по условию min_price_tendention (для того чтобы не ловить "падающий нож" (формула скрыта), по заранее высчитанному параметру param_frec принималось условие о первоначальной покупке инструмента. 
4.В коде daily_check_funcrions.py написаны условия усреднений, замены ордеров (в разное время ордера имеют разный profit_value, робот в каждом прогоне удалял и заново ставил ордер), а также проставления успешных ордеров. 
5. В коде написаны условия выхода после истечения основной сессии по времени МСК. 
6. В робот должны быть добавлены условия на стоп-лосс. Изначально робот создавался только для бумаг, в которых "можно было бы застрять и сидеть долго". Поэтому стопы не ставились в первой итерации и до 24 февраля не были реализованы. 

По всем вопросам - пояснениям просьба обращаться в TG (@ekasatkin).  
