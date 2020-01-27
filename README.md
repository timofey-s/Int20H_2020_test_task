# Int20H_2020_test_task
Тестовое задание для отбора на Int20H. Суть задания: распознавание и классификация одежды (команда: sudo)

Участники команды: Тимофей Шидловский, Ярослав Гладкий и Никита Воронин

## Задание
Использую архив из 36156 фотографий обучить нейросеть распознавать и классифицировать одежду, а также её отдельные элементы. Затем используя 9039 фотографий проверить точность нейросети, по формуле: 
(угаданные пиксели) / (угаданные пиксели + ошибочно угаданные пиксели + пропущенные пиксели)
![](https://s3.amazonaws.com/ifashionist/Kaggle/Kaggle3.jpg)

## Данные
Имеются 45195 изображений, 80% из которых будут использоваться для тренировки, а оставшиеся 20% для проверки нейросети. Фотографии очень разнообразны, в основном на них люди, но также есть и просто фотографии одежды. Собраны данные из самых разнообразных источников, большой разброс по стилю, освещению, окружению и в позах людей. Также имеется CSV файл, на котором размечены картинки, а именно вещь какой категории и где находится. Также есть JSON файл, который описывает категории. 
![](https://s3.amazonaws.com/ifashionist/Kaggle/dataset_example.jpg)

## Какие были идеи и решения
Мы начали с того, что один из нас полностью прочёл задание и попробовал нам его объяснить, пока мы в спешке изучали нейронные сети и с чем их едят. 
Мы придумали что нам будет удобней, если данные про местонахождение и категории одежды тоже будут в виде изображений. Так как категорий меньше 255, было решено использовать ч-б изображение ради простоты и экономии пространства. Мы написали код для такой трансформации. Также нам это помогло понять, в каком виде предоставлены данные в train.csv. Для теста мы выбрали изображение Криса Эванса. Должен признать, что данные просто отпад, очень точно переданы все границы объектов.
![Отображение всех категорий](https://github.com/timofey-s/Int20H_2020_test_task/blob/master/readme%20img/all%20categories.jpg)
Затем, мы проанализировали, и поняли, что некоторые категории находятся одна над другой. Поэтому после дискуссии было решено оставить только основные категории. Это бы ускорило и упростило бы обучение, но, с другой стороны, мы теряем точность. А так как мы из-за личных обстоятельств приступили к выполнению задания слишком поздно, время для нас было в приоритете. И вот что мы получили:
![Отображение основных категорий](https://github.com/timofey-s/Int20H_2020_test_task/blob/master/readme%20img/main%20categories.jpg)
По типу задания, понятно, что нужно использовать Свёрточные Нейронные Сети. Для того, чтобы подготовить данные для обучения сети, нам нужно было сделать их одного размера. После небольшого анализа мы поняли, что для нахождения основных категорий, будет достаточно 256*256 пикселей. Так что было решено подогнать все изображения под этот размер, при этом сохраняя пропорции
![Как нужно уменьшать картинки](https://github.com/timofey-s/Int20H_2020_test_task/blob/master/readme%20img/crop%20images.jpg)

К сожалению, у нас не хватило времени на обучение сети, так что мы придумали довольно-таки отчаянное решение:
Наш submission (учитывая, что на Kaggle просили учитывать размер картинок 512*512):
> *.jpeg, 65536 131072, 10

Таким образом, наш счёт хоть и маленький, но он больше 0

## Как можно улучшить результат
Ну во-первых, можно повысить размер входных данных. Также у нас были идеи что в качестве входных данных можно использовать не только стандартный способ с RGB каналами, а и HSV. Так как HSV даёт лучшее представление, об оттенках, и освещении. Вот для сравнения, что будет если разбить на HSV и RGB 
![HSV](https://github.com/timofey-s/Int20H_2020_test_task/blob/master/readme%20img/hsv.JPG)
![RGB](https://github.com/timofey-s/Int20H_2020_test_task/blob/master/readme%20img/rgb.JPG)
  
Например, на изображение, отвечающее за оттенок (самое верхнее), можно сразу понять какого цвета платье, и какой на нём узор (в данном случае оно однотонно). А уже по насыщенности и яркости можно понять форму объекта из-за его освещения. Та и данные при таком анализе становятся более нормализированными. В дополнение, данный подход смог бы легче продемонстрировать, какие узоры и оттенки одежды предпочитают больше всего. Подход, конечно, классный, но к сожалению у нас не хватило времени и навыков на его реализацию. Но мы продолжим, работать над реализацией денного подхода, чтобы посмотреть улучшит ли он результат. 
Также мы хотели использовать [mmdetection]( https://github.com/open-mmlab/mmdetection). это дало бы отличный результат из-за своей возможности не просто сообщать что на этой картинке, а ещё и указывать пиксели, на которых оно находится. К сожалению, у нас были проблемы с установкой, поэтому мы не успели использовать данный подход.

## Основные трудности
Были проблемы со знаниями в этой области. Никто из нас не использовал ранее машинное обучение для изображений, так что мы только ближе к дедлайну начали в этом ориентироваться. Ещё сложностью стало то, что у нас не было достаточно мощного компа та ещё и на Linux. 

## Выводы
Чтобы распознать изображение, нужно думать, как изображение. Сперва может показаться что ты только красно-зелено-синий пиксель, ну если присмотреться, то видно, что у тебя есть оттенок, насыщенность и яркость (можно использовать не только RGB). А ещё у тебя есть другие друзья пиксели, которые тебя окружают не только справа и слева, а ещё и снизу и сверху, та ещё и по диагонали есть несколько знакомых (Нужно использовать свёрточную нейронную сеть). Также понятно, что лучше несколько категорий в руках, чем все в небе (не нужно гнаться за всеми категориями, так как это может уменьшить точность).

