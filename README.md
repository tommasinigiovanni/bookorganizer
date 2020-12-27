# bookorganizer
A web app to manage a book library with python + Flask + PostgreSQL and deploy on Heroku.  
We use pipenv to work in a Pipenv-based Virtual Environment and to create all the needed files to run the project Heroku.  

## OS REQUIREMENTS
```bash
pip install pipenv
```

## Creation of Pipenv-based venv and installation of needed packages
```bash
# create a virtual env
#   https://pipenv-fork.readthedocs.io/en/latest/basics.html
pipenv shell

# check which packages are installed in you venv 
pip freeze

# install the needed packages 
pipenv install flask
pipenv install flask_sqlalchemy
pipenv install flask_script
pipenv install flask_migrate
pipenv install psycopg2-binary
pipenv install python-dotenv
pipenv install gunicorn
```

# DEV
## create a DEV postgres DB  
- We use 2 docker containers, organized in docker-compose to run a postgres dev instance
```bash
cd dev-db
docker-compose up -d
```
- create a `.env` file with DEV env
```bash
export APP_SETTINGS="config.DevelopmentConfig"
export DATABASE_URL="postgresql://postgres:your_password@localhost/bookorg-app"
```
- create the need table with db migrations
```bash
python manage.py db migrate
python manage.py db upgrade
```

## Run the app on you PC to dev porpuse
```bash

# activate the vevn
pipenv shell

# run the server
flask run
```

# PROD 
## Deploy in PROD env: HEROKU
- create some important files (in project already created)
```bash
pip freeze > requirements.txt
touch Procfile
touch runtime.txt
```

- create the app in HEROKU
```bash
heroku login
heroku create --region eu bookorganizer
```

- create the DB in HEROKU
```bash
heroku addons:create heroku-postgresql:hobby-dev --app bookorganizer
```
- set var in HEROKU to say you are in PROD
```bash
heroku config:set APP_SETTINGS=config.ProductionConfig --app bookorganizer
```

- check to have 2 var in HEROKU
```bash
heroku config --app bookorganizer
```

- Now connect you github repository with HEROKU app  
    1. Enter in HEROKU daskboard
    2. click on you app
    3. go in `Deploy` tab
    4. and in `Deployment method` section
    here select GitHub and connect your github repo.

- Deploy th app in HEROKU
    1. Enter in HEROKU daskboard
    2. click on you app
    3. go in `Deploy` tab
    4. click on `Deploy Branch`

- run the migration for the creation of DB in HEROKU
```bash
heroku run python manage.py db upgrade --app bookorganizer
```

Your app is now ready in Heroku, mine is **here**: [https://bookoranizer.herokuapp.com/](https://bookoranizer.herokuapp.com/)


# LINKOGRAPHY
[https://medium.com/@dushan14/create-a-web-application-with-python-flask-postgresql-and-deploy-on-heroku-243d548335cc](https://medium.com/@dushan14/create-a-web-application-with-python-flask-postgresql-and-deploy-on-heroku-243d548335cc)