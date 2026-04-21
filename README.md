# DevOps Q3 & Q4 – Monolithic App + CI Pipeline

## Objective

To develop a monolithic Flask application and implement a CI pipeline using GitHub Actions to build and push a Docker image.

---

## Q3 – Monolithic Flask Application

### Application Code (app.py)

```python
from flask import Flask, request

app = Flask(__name__)
tasks = []

@app.route('/')
def home():
    return "<h3>To-Do</h3>" + str(tasks) + \
           "<form action='/add' method='post'>" \
           "<input name='task'><button>Add</button></form>"

@app.route('/add', methods=['POST'])
def add():
    tasks.append(request.form['task'])
    return "Added <a href='/'>Back</a>"

if __name__ == '__main__':
    app.run(debug=True)
```

---

### Commands to Run (Q3)

pip install flask
python app.py

👉 Open browser: http://localhost:5000

---

## Q4 – CI Pipeline using GitHub Actions

### requirements.txt

flask

---

### Dockerfile

FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]

---

### GitHub Actions Workflow (.github/workflows/docker.yml)

name: Build and Push Docker Image

on:
push:
branches: [ main ]

jobs:
build-push:
runs-on: ubuntu-latest

```
steps:  
  - uses: actions/checkout@v3  

  - uses: docker/login-action@v3  
    with:  
      username: ${{ secrets.DOCKER_USERNAME }}  
      password: ${{ secrets.DOCKER_PASSWORD }}  

  - run: docker build -t priyadharsh26/app:latest .  
  - run: docker push priyadharsh26/app:latest  
```

---

## Git Commands Used

git init
git add .
git commit -m "Q3 + Q4 setup"
git branch -M main
git remote add origin https://github.com/Priyadh7/DevopsQ3Q4.git
git push -u origin main

---

## GitHub Secrets

DOCKER_USERNAME = priyadharsh26
DOCKER_PASSWORD = Docker access token

---

## Output

* Flask app runs successfully
* Docker image is built automatically
* Image is pushed to Docker Hub using GitHub Actions

---

## Result

Successfully implemented a monolithic Flask application and automated CI pipeline using GitHub Actions.

for Q7 change 
priyadharsh26/app:latest
priyadharsh26/app:${{ github.sha }}
