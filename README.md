# Entresoft Website

##Mezzanine
Install [Mezzanine](http://mezzanine.jupo.org/), a Django based CMS.
####Install and Edit Theme
因為每次Deploy會重新collectstatic，直接改static裡面的檔案會被覆蓋
所以正確安裝Theme的方法是把它下載下來放到根目錄裡，在settings.py中修改
(以[moderna](http://thecodinghouse.in/demo/moderna/)為例)
```
TEMPLATE_DIRS = (
  os.path.join(PROJECT_ROOT, "moderna/templates"),
  os.path.join(PROJECT_ROOT, "templates"),
)
```
以及
```
INSTALLED_APPS = (
  "moderna",
  ...
)
```
之後直接修改moderna資料夾裡面的template就可以編輯它了
[Instruction](http://stackoverflow.com/questions/28024595/how-to-deploy-installing-mezzanine-theme)


##Heroku

###requirements.txt
`$ git push heroku`的時候自動安裝的套件
```
dj-database-url==0.3.0
django-postgrespool==0.3.0
```
###Procfile
執行緒型態:安裝完後自動做的事情
```
web: python manage.py syncdb; python manage.py collectstatic --noinput; python manage.py runserver "0.0.0.0:$PORT"
```
[Instructions](https://devcenter.heroku.com/articles/procfile)
###settings.py
####設定Django secret key
```
SECRET_KEY = ""
NEVERCACHE_KEY = ""
```
####連線至Heroku的PostgreSQL
```
import dj_database_url
DATABASES = {}
DATABASES['default'] =  dj_database_url.config()
DATABASES['default']['ENGINE'] = 'django_postgrespool'
```
[Instructions](https://devcenter.heroku.com/articles/django-app-configuration#migrating-an-existing-django-project)
###Migrate Database
####土法煉鋼
```
$ python manage.py dumpdata > temp.json
$ git add -A
$ git commit -m "xD"
$ git push
$ git push heroku
$ heroku run python manage.py loaddata temp.json
```
