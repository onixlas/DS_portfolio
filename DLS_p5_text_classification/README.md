# Задача

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/NLP_-_isometric.svg/640px-NLP_-_isometric.svg.png "https://freepik.com")

В этом проекте нам предстоит решить задачу классификации текстов. Мы будем использовать набор данных [`ag_news`](https://paperswithcode.com/dataset/ag-news). Это датасет для классификации новостей на 4 темы: *World, Sports, Business, Sci/Tech.*

Классификацию мы будем производить с помощью рекуррентных нейросетей. Мы попробуем применить различные архитектуры (`RNN`, `LSTM`, `GRU`), а также подберём гипер-параметры, чтобы повысить качество моделей. Сравнивать модели между собой будем по метрике `accuracy`.

# Выводы

В ходе данной работы мы обучили несколько нейростевых моделей, относящих англоязычные тексты новостей к одному из четырёх классов: `World, Sports, Business, Sci/Tech`. Для этого мы использовали набор данных `ag-news`. В данном наборе примеры всех классов сбалансированы (в обучающей выборке приведено по 30 тысяч примеров новостей каждого класса).

Все тексты из набора мы разделили на отдельные слова-токены с помощью библиотеки `nltk`, привели к нижнему регистру и удалили символы пунктуации. Чтобы уменьшить размер словаря мы исключили из него все слова, встречающиеся в тексте менее 25 раз. В результате в словарь нашей модели попало 11842 слова.

Нашей целью было получение модели, классифицирующей тексты с дастаточно высоким значением метрики `accuracy`. Для этого мы провели несколько экспериментов с различными типами рекурентных архитектур (`RNN`, `LSTM`, `GRU`), различными значениями гипер-параметров (количество слоёв нейросети, размер скрытого представления, использование двунаправленных нейросетей, значение `dropout rate`) и различными видами обработки исходных данных (слой `embedding` без предобучения и на основе векторов `FastText`).

Для сравнения эффективности моделей для каждого эксперимента мы строили графики значений функции потерь и метрики `accuracy` на всём периоде обучения. Во всех экспериментах модели обучались по 50 эпох. Если в ходе эксперимента удавалось достичь улучшения целевой метрики, значения гипер-параметров переносились в следующий эксперимент.

Сведём данные по значениям метрики на валидационных данных на последних эпохах в таблицу.

| Модель | `Accuracy` на валидации |
| ------ | ----------------------- |
| `RNN`, аггрегация max | 0.8869 |
| `RNN`, аггрегация mean | 0.8847 |
| `LSTM` | 0.8843 |
| `GRU` | 0.8923 |
| `GRU`, 2 слоя | 0.8872 |
| `GRU`, 3 слоя | 0.8921 |
| `GRU`, 4 слоя | 0.8904 |
| `GRU`, 3 слоя, dropout 0.1 | 0.8981 |
| `GRU`, 3 слоя, dropout 0.3 | 0.8860 |
| `GRU`, скрытое представление 256 | 0.9012 |
| `GRU`, скрытое представление 512 | 0.8994 |
| `Bidirectional GRU` | 0.8987 |
| `GRU` с векторами `FastText` | 0.9158 |

Из таблицы видно, что наиболее заметных улучшений целевой метрики удавалось добиться при переходе на архитектуру `GRU`, увеличении скрытого представления и при использовании предобученных эмбеддингов на основе векторов `FastText`.

Наилучшие результаты (значение метрики на валидации 0.9158) удалось достичь для последней модели. Мы также рассчитали для неё значение метрики на тестовых данных (0.9118).

Для дальнейшего улучшения метрики можно попробовать использовать более новые нейросетевые архитектуры, например, трансформеры.
