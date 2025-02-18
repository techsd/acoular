# EXAMPLE.md

## Приклади використання

### Використання ключових компонентів

#### Приклад 1: Використання TimeSamples для завантаження даних

```python
import acoular

# Створення об'єкта TimeSamples для завантаження даних з файлу
ts = acoular.TimeSamples(file='three_sources.h5')

# Перевірка частоти дискретизації
print(ts.sample_freq)
```

#### Приклад 2: Використання PowerSpectra для обчислення спектрів потужності

```python
import acoular

# Створення об'єкта TimeSamples для завантаження даних з файлу
ts = acoular.TimeSamples(file='three_sources.h5')

# Створення об'єкта PowerSpectra для обчислення спектрів потужності
ps = acoular.PowerSpectra(source=ts, block_size=128, window='Hanning')

# Перевірка обчислених спектрів потужності
print(ps.csm)
```

### Сценарії завдань

#### Приклад 1: Отримання даних з API

```python
import requests

# Отримання даних з API
response = requests.get('https://api.example.com/data')
data = response.json()

# Виведення отриманих даних
print(data)
```

#### Приклад 2: Керування станом за допомогою React хуків

```jsx
import React, { useState, useEffect } from 'react';

const ExampleComponent = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    // Отримання даних з API
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []);

  return (
    <div>
      {data ? (
        <pre>{JSON.stringify(data, null, 2)}</pre>
      ) : (
        <p>Завантаження даних...</p>
      )}
    </div>
  );
};

export default ExampleComponent;
```

#### Приклад 3: Взаємодія з серверною частиною

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Маршрут для отримання даних
@app.route('/data', methods=['GET'])
def get_data():
    data = {'message': 'Hello, world!'}
    return jsonify(data)

# Запуск серверу
if __name__ == '__main__':
    app.run(debug=True)
```

### Налаштування середовищ

#### Локальна розробка

1. Клонування репозиторію:

```bash
git clone https://github.com/your-repo/project.git
cd project
```

2. Встановлення залежностей:

```bash
pip install -r requirements.txt
```

3. Запуск проекту:

```bash
python app.py
```

#### Staging

1. Налаштування середовища staging:

```bash
export FLASK_ENV=staging
```

2. Запуск проекту:

```bash
python app.py
```

#### Production

1. Налаштування середовища production:

```bash
export FLASK_ENV=production
```

2. Запуск проекту:

```bash
gunicorn -w 4 app:app
```
