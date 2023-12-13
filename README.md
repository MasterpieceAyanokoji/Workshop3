# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Тарасова Дениса Дмитриевича
- ФО-220007
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Предложите вариант изменения найденных переменных для 10 уровней в игре. Визуализируйте изменение уровня сложности в таблице.
- Задание 2.
- Создайте 10 сцен на Unity с изменяющимся уровнем сложности.
- Задание 3.
-  Решение в 80+ баллов должно заполнять google-таблицу данными из Python. В Python данные также должны быть визуализированы.
.
- Выводы.
- ✨Magic ✨

## Цель работы
Научиться разрабатывать оптимальный баланс для игры на примере игры Dragon Picker.

## Задание 1
### Предложите вариант изменения найденных переменных для 10 уровней в игре. Визуализируйте изменение уровня сложности в таблице.
Ход работы:
Зайдя в Dragon picker я обнаружил 4 переменные, влияющие на сложность игры, это: скорость дракона, время между сбросом яиц, ширина рамок поля и шанс изменения направления дракона.
Были определены оптимальные начальные значения, которые используются в первом уровне, после я решил увеличивать сложность, постепенно меняя переменные, а именно, увеличивать скорость дракона, увеличивать рамку движения дракона, увеличить шанс изменения направления дракона и уменьшить время между сбросом яиц.
изуализация коэффициента сложности, а также данные для всех 10 уровней представлены в следующей таблице:
https://docs.google.com/spreadsheets/d/1uNk4U4e5vfBHSzskI_gGvvB61gUxn5gyo8as8rahlT4/edit?usp=sharing

## Задание 2
### Создайте 10 сцен на Unity с изменяющимся уровнем сложности.

Ссылка на проект: https://drive.google.com/drive/folders/1OSZzk67wcbVosxDtS8POyvn-2IPvVHGn
## Задание 3
### Решение в 80+ баллов должно заполнять google-таблицу данными из Python. В Python данные также должны быть визуализированы.

```py
import enum
import time

import gspread
import matplotlib.pyplot as plt
import numpy as np


class DifficultIndex(enum.Enum):
    speed = 0
    egg_drop_time = 1
    right_left_distance = 2
    change_direction_chance = 3


class DifficultData:
    values = [2, 2, 2, 0.01]
    increase_coef = [0.5, -0.2, 1, 0.005]
    column_names = ("B", "C", "D", "E")
    fields_count = 4
    difficulty_coef = []

    def __init__(self):
        self.append_difficult_coef()

    def append_difficult_coef(self):
        self.difficulty_coef.append(
            self.values[DifficultIndex.speed.value] * self.values[DifficultIndex.right_left_distance.value] /
            self.values[
                DifficultIndex.egg_drop_time.value] * self.values[DifficultIndex.change_direction_chance.value])

    def icrease_difficulty(self):
        for i in range(self.fields_count):
            self.values[i] += self.increase_coef[i]
        if self.values[DifficultIndex.right_left_distance.value] > 10:
            self.values[DifficultIndex.right_left_distance.value] = 10
        self.append_difficult_coef()


gc = gspread.service_account(filename='difficultysettings-ec2fd7a63b18.json')
sh = gc.open("Ya Ayanokoji")

sh.sheet1.update(('A1'), "Level number")
sh.sheet1.update(('B1'), "Dragon speed")
sh.sheet1.update(('C1'), "Egg drop time")
sh.sheet1.update(('D1'), "Right-left distance")
sh.sheet1.update(('E1'), "change_direction_chance")

difficulty_data = DifficultData()


def update_sheet(row):
    for i in range(difficulty_data.fields_count):
        sh.sheet1.update(("A" + str(row)), row - 1)
        sh.sheet1.update((difficulty_data.column_names[i] + str(row)), difficulty_data.values[i])


for i in range(1, 11):
    time.sleep(5)
    if i != 1:
        difficulty_data.icrease_difficulty()
    update_sheet(i + 1)
fig, ax = plt.subplots()
x = np.arange(1, 11)
y = np.array(difficulty_data.difficulty_coef)
ax.plot(x, y)
ax.grid()
plt.xticks(range(1, 11, 1), range(1, 11, 1))
plt.yticks(range(1, 50, 2), range(1, 50, 2))
plt.xlabel("Level number")
plt.ylabel("Difficulty")
plt.show()

```

## Выводы

Абзац умных слов о том, что было сделано и что было узнано. В данной лабораторной работе я более подробно ознакомился с тем, как следует балансировать игру, на примере лучшей игры Dragon Picker, в которую играл с детства, была проведена визуализация динамики изменения данных средствами Python и Google Sheets.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
