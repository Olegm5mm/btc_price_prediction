Прогнозирование цены Bitcoin с использованием машинного обучения
📌 Описание проекта
Этот проект представляет собой систему прогнозирования цены Bitcoin (BTC-USD) с использованием алгоритма Random Forest Regressor. Он анализирует исторические данные, включая цену открытия, закрытия, максимальную и минимальную стоимость за определенные временные интервалы, а также временные признаки (час, минута, день недели) и лаговые переменные.

Программа загружает почасовые данные за последние 7 дней с Yahoo Finance, обучает модель и прогнозирует цену BTC на ближайшее время (например, через 10 минут).

⚙️ Функциональность
✔ Автоматическая загрузка данных о цене Bitcoin с Yahoo Finance
✔ Очистка и предобработка данных (удаление пропусков, добавление временных признаков)
✔ Обучение модели Random Forest Regressor на исторических данных
✔ Прогнозирование цены BTC через 10 минут
✔ Визуализация предсказаний на графике

🛠 Используемые технологии
🔹 Python 3.x
🔹 Библиотеки:

pandas – для работы с табличными данными
numpy – для математических операций
matplotlib и seaborn – для построения графиков
yfinance – для загрузки котировок Bitcoin
scikit-learn – для машинного обучения
📂 Структура проекта
bash
Копировать
Редактировать
/btc_price_prediction
│── data/                 # Каталог для сохранения загруженных данных (опционально)
│── btc_predict.py        # Основной код проекта
│── requirements.txt      # Список зависимостей
│── README.md             # Описание проекта (этот файл)
🚀 Установка и запуск
🔹 1. Клонирование репозитория
bash
Копировать
Редактировать
git clone https://github.com/your_username/btc_price_prediction.git
cd btc_price_prediction
🔹 2. Установка зависимостей
Рекомендуется использовать виртуальное окружение:

bash
Копировать
Редактировать
python -m venv venv
source venv/bin/activate  # Для Linux/Mac
venv\Scripts\activate     # Для Windows
pip install -r requirements.txt
🔹 3. Запуск кода
bash
Копировать
Редактировать
python btc_predict.py
📝 Пример кода
Загрузка данных
python
Копировать
Редактировать
import yfinance as yf
import pandas as pd

def get_btc_data():
    btc = yf.Ticker("BTC-USD")
    df = btc.history(period="7d", interval="1m")
    df.reset_index(inplace=True)
    return df.dropna()
Предобработка данных
python
Копировать
Редактировать
df["hour"] = df["Datetime"].dt.hour
df["minute"] = df["Datetime"].dt.minute
df["dayofweek"] = df["Datetime"].dt.dayofweek
df["Close_lag1"] = df["Close"].shift(1)
df["Close_lag5"] = df["Close"].shift(5)
df["Close_lag10"] = df["Close"].shift(10)
df = df.dropna()
Обучение модели
python
Копировать
Редактировать
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(df[features], df["Close"], test_size=0.2, random_state=42)
model = RandomForestRegressor(n_estimators=200, random_state=42)
model.fit(X_train, y_train)
Прогнозирование
python
Копировать
Редактировать
future_time = df.index[-1] + pd.Timedelta(minutes=10)
last_row["hour"] = future_time.hour
last_row["minute"] = future_time.minute
last_row["dayofweek"] = future_time.dayofweek
predicted_price = model.predict([last_row])[0]
Визуализация результатов
python
Копировать
Редактировать
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
plt.plot(df.index[-500:], df["Close"].tail(500), label="Фактическая цена", color="blue")
plt.scatter([future_time], [predicted_price], color="red", label="Прогноз", marker="x", s=100)
plt.legend()
plt.grid(True)
plt.show()
🎯 Результаты
Модель обучена на исторических данных за последние 7 дней.
Прогнозирование выполняется с использованием лаговых переменных и временных признаков.
График показывает хорошую корреляцию между реальной и предсказанной ценой.
📌 Что можно улучшить?
🔹 Добавить новые признаки, такие как объем торгов, индикаторы MACD, RSI.
🔹 Использовать нейросети (например, LSTM) для более точного прогнозирования.
🔹 Прогнозировать на более долгий срок, а не только на 10 минут.
🔹 Добавить автоматическое обновление данных без перезапуска кода.
