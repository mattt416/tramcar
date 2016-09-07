# Welcome!

Tramcar is a _multi-site_, _self-hosted_ __job board__ built using
[Django](https://www.djangoproject.com/).  This project is still in its infancy,
but we welcome your involvement to help us get Tramcar ready for production
installs.

## Development Installation

First, clone and install dependencies.  This requires python 2.7, pip, and
virtualenv to already be installed.

```
$ git clone https://github.com/wfhio/tramcar
$ cd tramcar
$ virtualenv .venv
$ source .venv/bin/activate
(.venv) $ pip install -r requirements.txt
```

Now, apply database migrations, create an admin user, and start the
development server:

```
(.venv) $ python manage.py migrate
(.venv) $ python manage.py createsuperuser
Username (leave blank to use 'root'): admin
Email address: admin@tramcar.org
Password:
Password (again):
Superuser created successfully.
(.venv) $ python manage.py runserver
```

The default site has a domain of example.com, this will need to be changed to
localhost for testing.  From the shell, issue the following command:

```
(.venv) $ sqlite3 db.sqlite3 "UPDATE django_site SET domain='localhost' WHERE name='example.com';"
```

## Fixtures

We have a fixtures file in `job_board/fixtures/countries.json`, which you can
load into your database by running the following:

```
(.venv) $ python manage.py loaddata countries.json
```

This will save you having to import your own list of countries.  However, be
aware that any changes made to the `job_board_country` table will be lost if
you re-run the above.

## Final Steps

At this point, Tramcar should be up and running and ready to be used.  Before
you can create a company and job, log into <http://localhost:8000/admin> using
the username and password defined above.  Once in, click Categories under
JOB_BOARD and add an appropriate category for the localhost site.

That's it!  With those steps completed, you can now browse
<http://localhost:8000> to create a new company, and then post a job with that
newly created company.

## Job Expiration

Jobs can be expired manually by logging in as an admin user and then clicking
the `Expire` button under `Job Admin` when viewing a given job.  A simpler
solution is to run this instead:

```
(.venv) $ python manage.py expire
```

The above will scan through all jobs across all sites and expire out any jobs
older than the site's `expire_after` value.  Ideally, the above should be
scheduled with cron so that jobs are expired in a consistent manner.
