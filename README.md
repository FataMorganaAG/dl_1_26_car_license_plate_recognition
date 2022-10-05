# Automatic plates recognition
Система для автоматического распознавания российских номерных знаков

## Getting started

````
$ pip install ...
````

## Датасет
Для работы был выбран открытый набор данных с kaggle VKCV_2022_Contest_02: Carplates Recognition, содержащий 
автомобильные номера (государственные регистрационные знаки). На одном изображении может быть 1 или более номерных знаков. Файл с описанием ГРЗ для каждого изображения обучающей выборки – train.json, имеет вид:

````
{"file": rel_path", "nums": [[[x1, y1], [x2, y2], [x3, y3],[ x4, y4]], [...]}
````
В обучающей выборке 26806 картинок (jpg: 17929, bmp: 7704), в тестовой – 3157 (jpg: 2225, bmp: 833, jpeg: 51, png: 41, 
JPG: 6, webp: 1).

Данные можно загрузить по [ссылке](https://disk.yandex.ru/d/NANSgQklgRElog).

## Модели
Для детекции номерных знаков использовались модели yolo v4 и yolo v5, которые дообучались на наших данных.
Для обучения использовался фреймворк darknet.

Результаты обучеения представлены в таблице.

| Модель  | Число эпох обучения | Время обучения |  avg_loss  | Гиперпараметры | Точность |
|:-------:|:-------------------:|:--------------:|:----------:|:--------------:|:--------:|
| yolo v4 |        9000         |      22 ч      |   0.1718   |                |          | 
| yolo v5 |        9000         |      00 ч      |     d      |                |          |

## OCR 
Для распознавания символов на изображении использовался Tesseract OCR, для оценки качества использовались метрики 
Character error rate (CER) и Word Error Rate (WER). В таблице представлены результаты для трёх инференсных изображений.

| Метрика |  1.jpg  | 2.jpg |  3.jpg  | 
|:-------:|:-------:|:-----:|:-------:|
|   CER   |  10.0   | 10.0  |  10.0   |
|   WER   |  10.0   | 10.0  |  10.0   |          

Для улучшения качества распознавания была предпринята попытка дообучения Tesseract OCR на собственных данных. 

Для этого были собраны и размечены (посимвольно) изображения номерных знаков. Для разметки использовался jTessBoxEditor.
Из полученного .tif файла с помощью Serak Trainer For Tesseract был сгенерирован файл ````rus.traineddata````, который 
нужно поместить в каталог к языковым моделям ````/usr/local/share/tessdata/````. 

В таблице представлены результаты для трёх инференсных изображений.

| Метрика |  1.jpg  | 2.jpg |  3.jpg  | 
|:-------:|:-------:|:-----:|:-------:|
|   CER   |  10.0   | 10.0  |  10.0   |
|   WER   |  10.0   | 10.0  |  10.0   | 
