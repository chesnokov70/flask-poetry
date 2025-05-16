# Используем официальный Python образ
FROM python

# Устанавливаем Poetry
ENV POETRY_VERSION=2.1.3

RUN apt-get update \
  && apt-get install -y curl build-essential \
  && curl -sSL https://install.python-poetry.org | python3 - \
  && ln -s /root/.local/bin/poetry /usr/local/bin/poetry \
  && apt-get purge -y curl \
  && apt-get clean

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем только файлы зависимостей
COPY pyproject.toml poetry.lock* /app/

# Устанавливаем зависимости (в отдельный слой, кэшируется)
RUN poetry config virtualenvs.create false \
 && poetry install --no-root


# Копируем исходный код
COPY . /app

# Открываем порт
EXPOSE 5000

# Запускаем приложение
CMD ["python", "server.py"]


