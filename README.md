Проект по биоинформатике
========================
#### В репозитории создаем папку data и скачиваем туда 2 .bed файла ChIP-seq экспериментов из ENCODE (пики гистоновой метки).

Я сделал это несколькими командами:
* git clone https://github.com/muhuraque/hse21_H3K9me3_G4_human
* wget https://www.encodeproject.org/files/ENCFF723YDW/@@download/ENCFF723YDW.bed.gz
* wget https://www.encodeproject.org/files/ENCFF070HBN/@@download/ENCFF070HBN.bed.gz
* zcat ENCFF723YDW.bed.gz  |  cut -f1-5 > H3K9me3_HCT116.ENCFF723YDW.hg19.bed
* zcat ENCFF070HBN.bed.gz  |  cut -f1-5 > H3K9me3_HCT116.ENCFF070HBN.hg19.bed
* mkdir data
* mkdir images
* mkdir src

А потом я скопировал все нужные файлы в нужную папку, а ненужные удалил.

#### Среди ChIP-seq пиков для нужной версии генома (hg19 для человека и mm10 для мыши) выкидываем слишком длинные пики (outliers).

1. Создаем файл filter_peaks.R
2. В нем пишем вывод сначала гистограммы до филтрации, а потом после.
3. По первой гистограмме видим, что 5000 подходит идеально и мы теряем всего 12 пиков. 
4. Потом также проделываем для второго файла. (Для него получается отсечка по 2500 и потеряны всего 11 пиков) 

Получаем такие гистограммы: 

![init1](./images/photo_2021-06-09_21-56-46.jpg)
![filt1](./images/photo_2021-06-09_21-56-49.jpg)
![init2](./images/photo_2021-06-09_21-56-51.jpg)
![filt2](./images/photo_2021-06-09_21-56-54.jpg)

Также я зашел на сервер через команду ssh и потом туда скопировал репозиторий с помощью git clone. 

#### Смотрим, где располагаются пики гистоновой метки относительно аннотированных генов. 

Я сделал файл chip_seeker.R и запустил его. Получились такие пай-чарт графики:

![1](./images/chip_seeker.H3K9me3_HCT116.ENCFF723YDW.hg19.filtered.plotAnnoPie.png)

![2](./images/chip_seeker.H3K9me3_HCT116.ENCFF070HBN.hg19.filtered.plotAnnoPie.png)


#### Объединяем два набора отфильтрованных ChIP-seq пиков с помощью утилиты bedtools merge.

Я снова зашел на сервер. Сделал команду cat  *.filtered.bed  |   sort -k1,1 -k2,2n   |   bedtools merge   >  H3K9me3_HCT116.merge.hg19.bed

Потом получившийся файл с помощью git add; git commit; git push залил к себе в репозиторий. 

#### Визуализируем исходные два набора ChIP-seq пиков, а также их объединение в геномном браузере, и проверяем корректность работы bedtools merge.
Я загрузил три файла (два фильтрованных и один мердженный этих двух) в геномный браузер. Я получил такую картину: 
![Screenshot from 2021-06-10 06-40-34](https://user-images.githubusercontent.com/26713337/121461219-172dae80-c9b7-11eb-86c8-6d92cd92399d.png)
Как видно один из файлов сильно больше другого и поэтому второй немного пропадает на его фоне. Но видно, что мердженный файл такой же большой, примерно над первым, а значит выглядит все правильно. 

### Анализ участков вторичной стр-ры ДНК
#### Скачиваем файл со вторичной стр-рой ДНК и добавляем его на github в папку data
