# Задача

Сетевой супермаркет «Хлеб-Соль» внедряет систему компьютерного зрения для обработки фотографий покупателей. Фотофиксация в прикассовой зоне поможет определять возраст клиентов, чтобы:

*    Анализировать покупки и предлагать товары, которые могут заинтересовать покупателей этой возрастной группы;
*    Контролировать добросовестность кассиров при продаже алкоголя.

Необходимо построить модель, которая по фотографии определит приблизительный возраст человека. В нашем распоряжении набор фотографий людей с указанием возраста.

# Выводы

Мы построили модель, которая по фотографии человека определяет его возраст со средней ошибкой в 7,2 года.

Заказчик планирует использовать её чтобы:

*    Анализировать покупки и предлагать товары, которые могут заинтересовать покупателей этой возрастной группы;
*    Контролировать добросовестность кассиров при продаже алкоголя.

Можно утверждать, что качество модели может быть достаточно для того, чтобы решать первую задачу. Если предположить, что возрастные группы будут определяться примерно так: подростки (до 18 лет), молодёжь (от 18 до 35 лет), средний возраст (от 35 до 50 лет), старшее поколение (от 50 лет и старше), то в большинстве случаев мы будем правильно предсказывать возрастную группу.

С другой стороны, качество модели недостаточно для того, чтобы автоматизировать контроль добросовестности кассиров при продаже алкоголя. Модель можно использовать лишь для предотбора случаев продажи алкоголя с последующим контролем со стороны человека. Например, можно фиксировать все случаи продажи алкоголя покупателям с предсказанным возрастом до 30 лет и отправлять их для контроля ответственному лицу.

Стоит также отметить, что у нас относительно мало фотографий людей старшего возраста (старше 50 лет). Возможно при добавлении в выборку фотографий людей этого возраста метрика модели может улучшиться.