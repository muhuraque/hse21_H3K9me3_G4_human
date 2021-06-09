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

