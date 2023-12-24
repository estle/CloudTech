# Описание лабораторной работы

### Лабораторная 3

Сделать, чтобы после пуша в ваш репозиторий автоматически собирался докер образ и результат его сборки сохранялся куда-нибудь. (например, если результат - текстовый файлик, он должен автоматически сохраниться на локальную машину, в ваш репозиторий или на ваш сервер).

# Выполнение лабораторной работы

Для загрузки в репозиторий взят проект на Flask из предыдущей лабораторной работы. Сборка будет выполняться в репозитории [estle/dockerlab3](https://github.com/estle/dockerlab3). Результат сборки будет выгружаться на [hub.docker.com](https://hub.docker.com/).

Прежде всего изучим документацию по CI/CD платформе GitHub Actions [docs.github.com](https://docs.github.com/ru/actions/learn-github-actions/understanding-github-actions)  

Создадим в репозитории проекта CloudTech/DevOps/lab3 файл .github/workflows/publish.yml

Добавим в файл publish.yml код автосборки докер образа и сохранения результата сборки на DockerHub.

```
name: Docker Image CI

on: 
  push:
    branches:
      - 'main'

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: checkout repository
        uses: actions/checkout@v4
      
      - name: log in to docker hub 
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.LOGIN_DOCKER }}
          password: ${{ secrets.PASSWORD_DOCKER }}

      - name: build image and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: estle/dockerlab3:latest
```

Добавляем в настройках репозитория секретные ключи доступа к DockerHub.



Пушим файлы в репозиторий и проверяем наличие сборки образа и выгрузки на DockerHub.


На платформе DockerHub появился наш докер образ, значит цель лабораторной работы достигнута.
