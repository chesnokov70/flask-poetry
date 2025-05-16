# flask-poetry

#Установить Poetry
#✅ Способ 1: Установка через официальный install-скрипт
curl -sSL https://install.python-poetry.org | python3 -
#После установки добавьте Poetry в PATH. Обычно это можно сделать так (для zsh):
export PATH="$HOME/.local/bin:$PATH"
#Добавьте эту строку в файл ~/.zshrc, чтобы настройка сохранялась после перезапуска терминала:
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
#✅ Проверка:
poetry --version
#Kак использовать poetry install внутри виртуального окружения?
#📦 Полный процесс установки зависимостей с Poetry:
#✅ 1. Убедись, что Poetry установлен:
poetry --version
#✅ 2. Перейди в директорию проекта (где находится файл pyproject.toml):
cd flask-poetry
#✅ 3. Установи зависимости (включая создание виртуального окружения):
poetry install # poetry install --no-root
#Poetry автоматически создаст виртуальное окружение и установит зависимости, указанные в pyproject.toml.
#💻 4. Войти в виртуальное окружение:
poetry shell # poetry run python app.py
#Ты увидишь, что терминал сменит приглашение, например:
(.venv) ➜ flask-poetry
#Теперь ты работаешь в окружении проекта.
#🚀 5. Запустить приложение (если у тебя Flask):
flask run

#========================================================

#Установи Python 3.11 (если ещё не установлен):
sudo add-apt-repository ppa:deadsnakes/ppa -y
sudo apt update
sudo apt install python3.11 python3.11-venv python3.11-dev
#Укажи Poetry использовать Python 3.11:
poetry env use python3.11

#========================================================
#📌 Как проверить доступные интерпретаторы:
pyenv versions   # если используешь pyenv
whereis python   # для общего поиска

#### To run use
```
cd flask-poetry
poetry install --no-root
poetry run python app.py
```
