# sales manager

sales manager

[![Built with Cookiecutter Django](https://img.shields.io/badge/built%20with-Cookiecutter%20Django-ff69b4.svg?logo=cookiecutter)](https://github.com/cookiecutter/cookiecutter-django/)
[![Black code style](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/ambv/black)

# Settings
This project relies extensively on environment settings which **will not work with Apache/mod_wsgi setups**. It has been deployed successfully with both Gunicorn/Nginx and even uWSGI/Nginx.

For configuration purposes, the following table maps environment variables to their Django setting and project settings:

| Environment Variable | Django Setting | Development Default | Production Default |
| ------------ | ------------ | ------------ | ------------ |
| DJANGO_READ_DOT_ENV_FILE | READ_DOT_ENV_FILE | False | False |

| Environment Variable | Django Setting | Development Default | Production Default |
| ------------ | ------------ | ------------ | ------------ |
| DATABASE_URL | DATABASES | auto w/ Docker; postgres://project_slug w/o | raises error |
| DJANGO_ADMIN_URL | n/a | ‘admin/’ | raises error |
| DJANGO_DEBUG | DEBUG | True | False |
| DJANGO_SECRET_KEY | SECRET_KEY | auto-generated| raises error |
| DJANGO_SECURE_BROWSER_XSS_FILTER | SECURE_BROWSER_XSS_FILTER | n/a | True |
| DJANGO_SECURE_SSL_REDIRECT | SECURE_SSL_REDIRECT | n/a | True |
| DJANGO_SECURE_CONTENT_TYPE_NOSNIFF | SECURE_CONTENT_TYPE_NOSNIFF | n/a | True |
| DJANGO_SECURE_FRAME_DENY | SECURE_FRAME_DENY | n/a | True |
| DJANGO_SECURE_HSTS_INCLUDE_SUBDOMAINS | HSTS_INCLUDE_SUBDOMAINS | n/a | True |
| DJANGO_SESSION_COOKIE_HTTPONLY | SESSION_COOKIE_HTTPONLY | n/a | True |
| DJANGO_SESSION_COOKIE_SECURE | SESSION_COOKIE_SECURE | n/a | False |
| DJANGO_DEFAULT_FROM_EMAIL | DEFAULT_FROM_EMAIL | n/a | “your_project_name <noreply@your_domain_name>” |
| DJANGO_SERVER_EMAIL | SERVER_EMAIL | n/a | “your_project_name <noreply@your_domain_name>” |
| DJANGO_EMAIL_SUBJECT_PREFIX | EMAIL_SUBJECT_PREFIX | n/a | “[your_project_name] “ |
| DJANGO_ALLOWED_HOSTS | ALLOWED_HOSTS | [‘*’] | [‘your_domain_name’ ] |

The following table lists settings and their defaults for third-party applications, which may or may not be part of your project:

| Environment Variable | Django Setting | Development Default | Production Default |
| ------------ | ------------ | ------------ | ------------ |
| CELERY_BROKER_URL | CELERY_BROKER_URL | auto w/ Docker; raises error w/o | raises error |
| DJANGO_AWS_ACCESS_KEY_ID | AWS_ACCESS_KEY_ID | n/a | raises error |
| DJANGO_AWS_SECRET_ACCESS_KEY | AWS_SECRET_ACCESS_KEY | n/a | raises error |
| DJANGO_AWS_STORAGE_BUCKET_NAME | AWS_STORAGE_BUCKET_NAME | n/a | raises error |
| DJANGO_AWS_S3_REGION_NAME | AWS_S3_REGION_NAME | n/a | None |
| DJANGO_AWS_S3_CUSTOM_DOMAIN | AWS_S3_CUSTOM_DOMAIN | n/a | None |
| DJANGO_AWS_S3_MAX_MEMORY_SIZE | AWS_S3_MAX_MEMORY_SIZE | n/a | 100_000_000 |
| DJANGO_GCP_STORAGE_BUCKET_NAME | GS_BUCKET_NAME | n/a | raises error |
| GOOGLE_APPLICATION_CREDENTIALS | n/a | n/a | raises error |
| SENTRY_DSN | SENTRY_DSN | n/a | raises error |
| SENTRY_ENVIRONMENT | n/a | n/a | production |
| SENTRY_TRACES_SAMPLE_RATE | n/a | n/a | 0.0 |
| DJANGO_SENTRY_LOG_LEVEL | SENTRY_LOG_LEVEL | n/a | logging.INFO |
| MAILGUN_API_KEY | MAILGUN_API_KEY | n/a | raises error |
| MAILGUN_DOMAIN | MAILGUN_SENDER_DOMAIN | n/a | raises error |
| MAILGUN_API_URL | n/a | n/a | “https://api.mailgun.net/v3” |
| MAILJET_API_KEY | MAILJET_API_KEY | n/a | raises error |
| MAILJET_SECRET_KEY | MAILJET_SECRET_KEY | n/a | raises error |
| MAILJET_API_URL | n/a | n/a | “https://api.mailjet.com/v3” |
| MANDRILL_API_KEY | MANDRILL_API_KEY | n/a | raises error |
| MANDRILL_API_URL | n/a | n/a | “https://mandrillapp.com/api/1.0” |
| POSTMARK_SERVER_TOKEN | POSTMARK_SERVER_TOKEN | n/a | raises error |
| POSTMARK_API_URL | n/a | n/a | “https://api.postmarkapp.com/” |
| SENDGRID_API_KEY | SENDGRID_API_KEY | n/a | raises error |
| SENDGRID_GENERATE_MESSAGE_ID | True | n/a | raises error |
| SENDGRID_MERGE_FIELD_FORMAT | None | n/a | raises error |
| SENDGRID_API_URL | n/a | n/a | “https://api.sendgrid.com/v3/” |
| SENDINBLUE_API_KEY | SENDINBLUE_API_KEY | n/a | raises error |
| SENDINBLUE_API_URL | n/a | n/a | “https://api.sendinblue.com/v3/” |
| SPARKPOST_API_KEY | SPARKPOST_API_KEY | n/a | raises error |
| SPARKPOST_API_URL | n/a | n/a | “https://api.sparkpost.com/api/v1” |

## Other Environment Settings
**DJANGO_ACCOUNT_ALLOW_REGISTRATION (=True)**
Allow enable or disable user registration through *django-allauth* without disabling other characteristics like authentication and account management. (Django Setting: ACCOUNT_ALLOW_REGISTRATION)

# Basic Commands

### Setting Up Your Users

-   To create a **normal user account**, just go to Sign Up and fill out the form. Once you submit it, you'll see a "Verify Your E-mail Address" page. Go to your console to see a simulated email verification message. Copy the link into your browser. Now the user's email should be verified and ready to go.

-   To create a **superuser account**, use this command:

        $ python manage.py createsuperuser

For convenience, you can keep your normal user logged in on Chrome and your superuser logged in on Firefox (or similar), so that you can see how the site behaves for both kinds of users.

### Type checks

Running type checks with mypy:

    $ mypy server_sales_manager

### Test coverage

To run the tests, check your test coverage, and generate an HTML coverage report:

    $ coverage run -m pytest
    $ coverage html
    $ open htmlcov/index.html

#### Running tests with pytest

    $ pytest

### Celery

This app comes with Celery.

To run a celery worker:

``` bash
cd server_sales_manager
celery -A config.celery_app worker -l info
```

Please note: For Celery's import magic to work, it is important *where* the celery commands are run. If you are in the same folder with *manage.py*, you should be right.

### Email Server

In development, it is often nice to be able to see emails that are being sent from your application. For that reason local SMTP server [MailHog](https://github.com/mailhog/MailHog) with a web interface is available as docker container.

Container mailhog will start automatically when you will run all docker containers.
Please check [cookiecutter-django Docker documentation](http://cookiecutter-django.readthedocs.io/en/2022.03.27/deployment-with-docker.html) for more details how to start all containers.

With MailHog running, to view messages that are sent by your application, open your browser and go to `http://127.0.0.1:8025`

### Sentry

[Sentry](https://sentry.io) is an error logging aggregator service. The system is set up with reasonable defaults, including 404 logging and integration with the WSGI application.

You must set the DSN url in production.

## Deployment

The following details how to deploy this application.

### Heroku

See detailed [cookiecutter-django Heroku documentation](http://cookiecutter-django.readthedocs.io/en/2022.03.27/deployment-on-heroku.html).

### Docker

See detailed [cookiecutter-django Docker documentation](http://cookiecutter-django.readthedocs.io/en/2022.03.27/deployment-with-docker.html).
