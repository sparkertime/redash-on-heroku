# Redash on Heroku

Dockerfiles for hosting redash on heroku

## How to create

```sh
git clone git@github.com:sparkertime/redash-on-heroku.git
cd redash-on-heroku
heroku create --stack=container your_app_name
```

## How to setup

### Add Addons

Add following addons on heroku dashboard.

- Redis Cloud

### Add environment variables

Add environment variables like following.

```sh
heroku config:set PYTHONUNBUFFERED=0
heroku config:set QUEUES=periodic emails default scheduled_queries queries schemas
heroku config:set REDASH_COOKIE_SECRET=YOUR_SECRET_TOKEN
heroku config:set REDASH_SECRET_KEY=YOUR_SECRET_KEY
heroku config:set REDASH_DATABASE_URL=YOUR_POSTGRES_URL
heroku config:set REDASH_LOG_LEVEL=INFO
heroku config:set REDASH_REDIS_URL=YOUR_REDIS_URL
heroku config:set REDASH_HOST=YOUR_DOMAIN_URL
heroku config:set REDASH_MAIL_PASSWORD=YOUR_ADDON_PASSWORD
heroku config:set REDASH_MAIL_PORT=587
heroku config:set REDASH_MAIL_SERVER=YOUR_ADDON_DOMAIN
heroku config:set REDASH_MAIL_USERNAME=YOUR_ADDON_USERNAME
heroku config:set REDASH_MAIL_USE_TLS=true
heroku config:set REDASH_MAIL_DEFAULT_SENDER=YOUR_MAIL_ADDRESS
```

See also https://redash.io/help/open-source/setup#-setup

### Release container

```sh
git push heroku master
```

### Create database

After deploy and add postgres addon, create database like following.

```sh
heroku run /app/manage.py database create_tables
```

### Enable worker dyno

```sh
heroku ps:scale worker=1
heroku ps:scale scheduler=1
```

## How to upgrade

```sh
heroku ps:scale web=0 worker=0 scheduler=0
git push heroku master
heroku run /app/manage.py db upgrade
heroku ps:scale web=1 worker=1 scheduler=1
```

See also https://redash.io/help/open-source/admin-guide/how-to-upgrade
