# Задача

![](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a1/Homer_Simpson_graff.jpg/640px-Homer_Simpson_graff.jpg 'jaroh')

Сегодня нам предстоить помочь телекомпании *FOX* в обработке их контента. Как известно, сериал "Симпсоны" идет на телеэкранах более 25 лет, и за это время скопилось очень много видеоматериала. Персоонажи менялись вместе с изменяющимися графическими технологиями, и Гомер Симпсон-2018 не очень похож на Гомера Симпсона-1989. В этом задании нам необходимо классифицировать персонажей, проживающих в Спрингфилде.

# Выводы

В данном проекте от нас требовалось обучить классификатор изображений персонажей мультсериала Симпсоны. В нашем распоряжении был набор из более чем 20 тысяч картинок. Метрика, по которой оценивается результат работы --- *F1*.

Мы разделили набор данных на обучающую и валидационную выборки в соотношении 80% к 20% соответственно. Для удобства работы мы создали *DataModule* (класс, объединяющий загрузчики обучающих и валидационных изображений). 

К изображениям обучающей выборки мы применяем следующие трансформации (аугментации):

* уменьшение до размера 224 на 224 (чтобы затем использовать предобученные модели)
* случайное отображение по горизонтали
* случайное вращение изображения на угол до 20 градусов
* небольшое размытие
* нормализация.

К валиадционной выборке применялись лишь уменьшение изображение и нормализация.

В качестве модели мы выбрали [*ResNext*](https://arxiv.org/abs/1611.05431). Данная модификация архитектуры *RexNet* была предложена в 2017 году и продемонстрировала хорошие результаты на соревновании *ImageNet*. В качестве направления дальнейшего развития данного проекта можно попробовать обучить модели с различными архитектурами (*DenseNet*, *ElasticNet* и т.д.) и сравнить их результаты.

Обучение происходило с оптимизатором *AdamW* в течение 20 эпох. В качестве функции потерь была выбрана кросс-энтропия. 

На каждой эпохе обучения мы фиксировали значение функции потерь и метрики *F1*. 

После обучения мы получили предсказания на тестовых данных и загрузили их на платформу *kaggle*. Результирующее значение метрики *F1* на тестовой выборке (0.993) соответствует максимальному балу за данную задачу.
