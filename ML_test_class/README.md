# Задача

![](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c8/Corporate_Woman_Shaking_Hands_With_a_Corporate_Man.svg/640px-Corporate_Woman_Shaking_Hands_With_a_Corporate_Man.svg.png "")

Стоит задача сегментировать клиентов по трем категориям (0, 1, 2). Необходимо разработать модель, предсказывающую к какому из трех сегментов относится каждый клиент.

Обучающая выборка в файле `contest_train.csv` состоит из следующих столбцов:
* `ID` — идентификаторы клиентов
* `TARGET` — соответствующий клиенту сегмент 
* `FEATURE_0`…`FEATURE_259` — данные клиента.

Точность предсказания оценивается метрикой [`macro f1 score`](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html). При этом для каждого класса (сегмента) рассчитывается значение метрики `F1`, а затем определяется их невзвешенное среднее.

# Выводы

В данном проекте перед нами стояла задача сегментировать клиентов по трем категориям (0, 1, 2). Необходимо было разработать модель, предсказывающую к какому из трех сегментов относится каждый клиент.

Точность предсказания оценивалась метрикой [`macro f1 score`](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html). 

Мы загрузили файл обучающей выборки, содержащий более чем 18 тысяч записей. Все обучающие признаки обезличены (названия колонок не позволяют судить о содержащихся в них данных). В некоторых колонках содержались пропуски данных. 

Мы провели небольшой исследовательский анализ данных, в ходе которого установили, что:
* все идентификаторы клиентов в поле `ID` уникальны и не содержат какой-либо дополнительной информации
* в поле `TARGET` наблюдается дисбаланс классов: примерно 70 процентов клиентов относятся к сегменту 0, а к сегменту 2 относятся всего 6 процентов клиентов

Мы построили гистограммы для некоторых столбцов с большим количеством пропущенных данных, но не смогли обнаружить каких-либо закономерностей, которые позволили бы заполнять пропуски более информативно.

Мы построили графики попарных зависимостей первых и последних десяти признаков, при этом разделили точки, относящиеся к различным сегментам, цветами. Визуальный анализ полученных графиков не позволил нам выявить каких-либо полезных закономерностей.

Для получения предсказаний мы обучили и сравнили несколько моделей:
* `Ridge Classifier`
* `CatBoost`
* `LightGBM`

Проблема дисбаланса решалась тремя методами:
* мы использовали коррекцию весов классов
* искусственно уменьшали размер выборки
* искусственно увеличивали размер выборки

Для различных комбинаций моделей и методов борьбы с дисбалансом мы рассчитали значения метрики `F1 macro`.

Чтобы убедиться в адекватности предсказаний модели, мы обучили "глупый" классификатор, который предсказывал классы случайным образом, но с сохранением дисбаланса.

Результирующие значения метрики `F1 macro` на валидационной выборке представлены в таблицах ниже.

| Модель | F1 (нач.) | F1 (баланс.) |
| -- | -- | -- |
| CatBoost | 0.510 | 0.550 |

| Модель | F1 (нач., mean) | F1 (нач., knn) | F1 (class_weights, mean) | F1 (class_weights, knn)| F1 (undersampling, mean) | F1 (undersampling, knn) | F1 (oversampling, mean) | F1 (oversampling, knn) |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | 
| Ridge | 0.389 | 0.390 | 0.462 | 0.459 | 0.235 | 0.232 | 0.459 | 0.467 |
| LGBM | 0.492 | 0.497 | 0.542 | 0.542 | 0.192 | 0.191 | 0.493 | 0.491 |
| Dummy | 0.327 | --- | --- | --- | --- | --- | --- | --- |

Из данных таблиц видно, что наилучшее значение метрики удалось достичь для модели `CatBoost` с автоматической балансировкой классов.

Так как модель `CatBoost` показала наилучшие значения метрики, мы попытались дополнительно улучшить предсказания за счёт подбора гипер-параметров на кросс-валидации. С ожалению, нам не удалось добиться сколько-нибудь серьёзного улучшения результата.

Наконец, мы загрузили файл с тестовыми данными и подготовили по нему предсказания. Результаты работы мы сохранили в файле `contest_answer.csv`.