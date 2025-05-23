# Техническое задание Маркетинг
Интернет-магазин собирает историю покупателей, проводит рассылки предложений и планирует будущие продажи. Для оптимизации процессов надо выделить пользователей, которые готовы совершить покупку в ближайшее время.

## Цель
Предсказать вероятность покупки в течение 90 дней

## Задачи
- Изучить данные
- Разработать полезные признаки
- Создать модель для классификации пользователей
- Улучшить модель и максимизировать метрику roc_auc
- Выполнить тестирование

## План проведения исследования:

1. Загрузить библиотеки
2. Загрузить файлы.
3. Сделать предобработку.
4. Сделать исследовательский анализ
5. Составить из файлов таблицу с признаками.
6. Провести исследовательский анализ и предобработку.
7. Проверить признаки на мультиколлинеарность.
8. Подготовить данные для паплайна. Закодировать.
9. Паплайн с выбором модели и гиперпараметров.
10. Выбор самых важных признаков. Обучение модели на новом наборе признаков.
11. Oversampling. Обучение модели на сэмплированных данных.
12. Выбор лучшей модели. Тестирование.

## Исходные данные
Данные о покупках клиентов по дням и по товарам. В каждой записи покупка определенного товара, его цена, количество штук. В таблице есть списки идентификаторов, к каким категориям относится товар. Часто это вложенные категории (например автотовары-аксессуары-освежители), но также может включать в начале списка маркер распродажи или маркер женщинам/мужчинам. Нумерация категорий сквозная для всех уровней, то есть 44 на второй позиции списка или на третьей – это одна и та же категория. Иногда дерево категорий обновляется, поэтому могут меняться вложенности, например ['4', '28', '44', '1594'] или ['4', '44', '1594'].

Датасэт apparel-purchases история покупок:

- client_id идентификатор пользователя
- quantity количество товаров в заказе
- price цена товара
- category_ids вложенные категории, к которым отнсится товар
- date дата покупки
- message_id идентификатор сообщения из рассылки

Датасэт apparel-messages история рекламных рассылок

- bulk_campaign_id идентификатор рекламной кампании
- client_id идентификатор пользователя
- message_id идентификатор сообщений
- event тип действия
- channel канал рассылки
- date дата рассылки
- created_at точное время создания сообщения

apparel-target_binary совершит ли клиент покупку в течение следующих 90 дней

- client_id идентификатор пользователя
target целевой признак

При построении модели использовались следующие библиотеки:
1. import pandas as pd
2. import ast
3. from ast import literal_eval
4. import matplotlib.pyplot as plt
5. from sklearn.metrics import confusion_matrix
6. import phik
7. import seaborn as sns
8. from sklearn.model_selection import train_test_split
9. import numpy as np
10. from sklearn.pipeline import Pipeline
11. from sklearn.compose import ColumnTransformer
12. from sklearn.preprocessing import StandardScaler, MinMaxScaler
13. from sklearn.impute import SimpleImputer
14. from sklearn.preprocessing import StandardScaler
15. from sklearn.model_selection import GridSearchCV, RandomizedSearchCV
16. import time
17. from sklearn.metrics import roc_auc_score, classification_report
18. from sklearn.datasets import make_classification
19. from datetime import datetime, timedelta
20. from sklearn.linear_model import LogisticRegression
21. from sklearn.neighbors import KNeighborsClassifier
22. from sklearn.tree import DecisionTreeClassifier
23. from sklearn.inspection import permutation_importance
24. from sklearn.feature_selection import SelectFromModel
from imblearn.over_sampling import SMOTE

**ВЫВОД:** Найдена модель для предсказания интернет-магазином совершения покупки клиентом, который уже покупал, в течение 90 дней. Эта модель LogisticRegression(C=1, class_weight='balanced', max_iter=1500, random_state=42, solver='saga'). Качество предсказания этой модели ROC-AUC 0,67.
