# flask-poetry
#### To run use
```
cd flask-poetry
poetry install --no-root
poetry run python app.py
```
docker build -t flask-poetry-app .
docker run -p 5000:5000 flask-poetry-app
docker login
docker tag flask-poetry-app chesnokov70/flask-poetry-app:1.0.0
docker push chesnokov70/flask-poetry-app:1.0.0
