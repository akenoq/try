# try TravisCI + Heroku

1. Устанавливаем консоли Travis и Heroku
2. Заходим на сайт Travis под своим акаунтом на Github (подробнее https://docs.travis-ci.com/user/getting-started/)
3. Создаем `.travis.yml` файл подобный данному
(подробнее https://docs.travis-ci.com/user/deployment/heroku/)
Обратите внимание, в инструкции рассматривается несколько вариантов деплоя, например, при `git push` на любую ветку или только на одну. Также возможно деплоить с разных веток (например, `master` и `dev`) в приложения двух различных heroku-аккаунтов - главное верно указать шифрованный ключ для доступа к Travis к нужному Heroku

4. Выполняем
```
travis login --org
travis setup heroku --org
heroku login
```
5. После того, как залогинились и там, и там
```
travis encrypt $(heroku auth:token) --add deploy.api_key
```
В файле `.travis.yml` появился нужный ключ
```
deploy:
  provider: heroku
  app:
    master: MASTER_APP_NAME
    dev: DEV_APP_NAME
  api_key:
    master:
      secure: ...MASTER-OWNER ENCRYPTED KEY...
```
6. Осталось указать репозиторий, с которым должен работать Travis.

Travis начинает сборку как только происходит `git push` на отслеживаемой ветке в отслеживаемом репозитории.

Для того, чтобы включить репозиторий в отслеживаемые, необходимо выбрать его из списка своих репозиториев https://travis-ci.org/profile/YOUR_TRAVIS_NAME и поставить переключатель в положение "Activate"

7. Теперь все успешные сборки Travis'ом приложения должны автоматически деплоиться на Heroku.
