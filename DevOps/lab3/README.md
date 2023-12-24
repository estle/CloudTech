# Описание лабораторной работы

### Лабораторная 3

Сделать, чтобы после пуша в ваш репозиторий автоматически собирался докер образ и результат его сборки сохранялся куда-нибудь. (например, если результат - текстовый файлик, он должен автоматически сохраниться на локальную машину, в ваш репозиторий или на ваш сервер).

# Выполнение лабораторной работы

Для загрузки в репозиторий взят проект на Flask из предыдущей лабораторной работы. Сборка будет выполняться в репозитории [estle/dockerlab3](https://github.com/estle/dockerlab3). Результат сборки будет выгружаться на [hub.docker.com](https://hub.docker.com/).

Прежде всего изучим документацию по CI/CD платформе GitHub Actions [docs.github.com](https://docs.github.com/ru/actions/learn-github-actions/understanding-github-actions)

---------

### publish.yml

Для начала создадим в репозитории проекта CloudTech/DevOps/lab3 файл .github/workflows/publish.yml

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

### Repository Secrets

Добавляем в настройках репозитория секретные ключи доступа к DockerHub.

<img width="796" alt="Снимок экрана 2023-12-24 в 12 43 41" src="https://github.com/estle/CloudTech/assets/52665965/50edfcf8-7d94-4ddc-9166-6ea964311bee">
<img width="783" alt="Снимок экрана 2023-12-24 в 12 44 09" src="https://github.com/estle/CloudTech/assets/52665965/29a6e1b9-2fa3-4694-8f44-5bab65eca471">

### Push to repo, DockerHub Image

Пушим файлы в репозиторий и проверяем наличие сборки образа и выгрузки на DockerHub.

<img width="789" alt="Снимок экрана 2023-12-24 в 13 09 31" src="https://github.com/estle/CloudTech/assets/52665965/faa7a4de-f465-4195-9367-a79702c2814d">

<img width="998" alt="Снимок экрана 2023-12-24 в 13 10 32" src="https://github.com/estle/CloudTech/assets/52665965/a7e2d756-673a-4b5b-bb31-0f5b8f4c9ac6">
<img width="1015" alt="Снимок экрана 2023-12-24 в 13 11 22" src="https://github.com/estle/CloudTech/assets/52665965/40d34a91-0c55-447c-a93e-7c6c47f25296">

### Итог

DockerFile в репозитории успешно собрался, как мы видим из вкладки Actions/workflows. На платформе DockerHub появился dockerlab3:latest, значит докер образ успешно выгрузился.
