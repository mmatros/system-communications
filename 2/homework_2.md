# домашняя работа #2

## концептуальная модель данных

![концептуальная модель данных](/2/concept.png)

## общая модель формальных и функциональных связей

![общую модель формальных и функциональных связей](/2/common.png)

## изменения связей

|Номер связи|Как связь сделана на текущий момент|Как изменилась связь после первого урока|Какая теперь будет связь|Номера проблем бизнеса, которые потенциально решатся|Почему связь необходимо изменить|
|-----------|-----------------------------------|---------------------------|------------------------|-------------------------------------|--------------------------------|
|COMM-010|HTTP-вызов|Трансляция через kafka новых менеджеров в сервис учителей|связь не нужна|Problem-050, Problem-060, Problem-080, Problem-090|судя по общей схеме формальных и функциональных связей она не нужна|
|COMM-020|HTTP-вызов|-|-|-|так как задание запрашивается каждый раз по новому, транслировать их из сервиса создания заданий не получится|
|COMM-030|HTTP-вызов|Асинхронная отправка события, что задание выполнено N раз|связь не нужна|Problem-050, Problem-060, Problem-080, Problem-090, Problem-100| Подсчет рейтинга и событий успешного выполнения заданий происходит в сервисе заданий, поэтому связь не нужна|
|COMM-040|HTTP-вызов|Асинхронная отправка события через kafka|-|Problem-040, Problem-050, Problem-070, Problem-080, Problem-090, Problem-100|фактически это команда, в EDA лучше заменить на событие|
|COMM-050|HTTP-вызов|Асинхронная отправка события через kafka|-|Problem-040, Problem-050, Problem-070, Problem-080, Problem-090, Problem-100|фактически это команда, в EDA лучше заменить на событие|
|COMM-060|Асинхронная отправка события через kafka|HTTP-вызов|-|Problem-030, Problem-050, Problem-090, Problem-100|Рейтинг меняется быстро, лучше быстро делать синхронный запрос на получение точного значения|
|COMM-070|HTTP-вызов|Трансляция через kafka новых менеджеров в сервис бонусов|-|Problem-080, Problem-090|Снизит нагрузку на сервис бонусов|
|COMM-080|HTTP-вызов|Асинхронная отправка события через kafka|-|Problem-040, Problem-080|фактически это команда, в EDA лучше заменить на событие|
|COMM-090|HTTP-вызов|Трансляция через kafka новых кандидатов в сервис создания заданий|-|Problem-040,Problem-090|Снизит нагрузку на сервис найма учителей|
|COMM-100|-|-|Асинхронная отправка события через kafka||Нужно добавить отправку решения на проверку|
|COMM-110|-|-|Асинхронная отправка события через kafka||Нужно добавить получение оценки после проверки|

## топики и события

|Название очереди/топика|Названия событий и номеров связи, которые окажутся в топике|Почему вы поместили в топик именно эти события|Почему так назвали|
|-----------------------|-----------------------------------------------------------|------------------------------------|------------------|
|datareplication.manager|created COMM-070| - | Формальная связь содержит информацию о менеджерах|
|domain.tasks|completed COMM-040| - | Функциональная связь о выполненном задании|
|domain.tasks|failed COMM-050| - | Функциональная связь о не выполненном задании|
|domain.tasks|created COMM-080| - | Функциональная связь о создании задания|
|datareplication.candidate|created COMM-090| - |Формальная связь с информацией о кандидате|
|domain.candidate.solution|submit COMM-100| - | Функциональная связь с решением|
|domain.tasks|result COMM-110| - | Функциональная связь с оценкой|

[доска в holst](https://app.holst.so/share/b/d6136a24-0892-4dcc--8391ee545978)
