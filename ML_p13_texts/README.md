# Задача

Интернет-магазин «Викишоп» запускает новый сервис. Теперь пользователи могут редактировать и дополнять описания товаров, как в вики-сообществах. То есть клиенты предлагают свои правки и комментируют изменения других. Магазину нужен инструмент, который будет искать токсичные комментарии и отправлять их на модерацию. 

# Выводы

Мы загрузили данные и немного познакомились с ними. В нашем распоряжении файл с почти 160 тысячами комментариев. Пропусков данных нет, явных дупилкатов данных нет. 

Классы не сбалансированы, данных без признака токсичности сильно больше.

Мы попробовали решить задачу несколькими способами.

Для начала мы сравнили модели на основе логистической регрессии, метода опроных векторов и гребневой регрессии. Предварительно мы провели леммаизацию признаков с помощью библиотеки *nltk*. Модель линейной регресии показала наивысшее значение метрики *ROC-AUC*, поэтому мы отобрали её для дальнейшего обучения. После подобора гипер-параметров и выбора оптимального порога модель обеспечила значение метрики *F1*=0.78.

Затем мы применили две модели на основе нейросетей. Для их использования нам понадобилось использовать видеокарты.

Модель на основе библиотеки *fast.ai* после обучения смогла классифицировать комментарии довольно точно, значение метрики *F1* составило 0.82.

Предобученная модель *BERT* из библиотеки *HuggingFace* делелала предсказания довольно долго, но в конце дотянула метрику *F1* до великолепного 0.94.

Сведём результаты моделей в единую таблицу.

| Item         | F1     | 
|--------------|-----------|
| Логистическая регрессия | 0.78      | 
| fast.ai (LSTM)      | 0.82  |
| HuggingFace (BERT)      | 0.94 |

Исходя из значения метрики *F1* мы можем рекомендовать заказчику нейросетевую модель *BERT*. Однако если заказчику важно ещё и время расчётов предсказаний, то можно рассмотреть возможность применения модели на основе библиотеки *fast.ai* (высокая скорость предсказаний и неплохая точность).