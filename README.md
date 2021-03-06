# Timestrap

[![Travis](https://img.shields.io/travis/overshard/timestrap.svg?style=flat-square)](https://travis-ci.org/overshard/timestrap) [![Coveralls](https://img.shields.io/coveralls/overshard/timestrap.svg?style=flat-square)](https://coveralls.io/github/overshard/timestrap) [![license](https://img.shields.io/github/license/overshard/timestrap.svg?style=flat-square)](https://github.com/overshard/timestrap/blob/master/LICENSE.md)

Time tracking and invoicing you can host anywhere. Full export support in
multiple formats and easily extensible.


### Warning

This app is currently very unstable as I have just started coding it.
Everything may, and probably will, change.


## Quickstart

Want to get up and running quickly?

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/overshard/timestrap)

We create a default username and password with superuser access to get you
started, please change it via the admin panel:

- Username: `admin`
- Password: `changeme123`

If you are manually deploying to Heroku without using the deploy button make
sure you create two settings before pushing using `heroku config:set`:

    heroku config:set DJANGO_SETTINGS_MODULE=timestrap.settings.heroku
    heroku config:set SECRET_KEY=ChangeMeToSomethingRandom

You will also need to create a superuser after your push has been successful in
order to login:

    heroku run python manage.py createsuperuser

## Demo Website

I've setup an [instance on Heroku](https://timestrap.herokuapp.com/) of
Timestrap that resets every 10 minutes if you want to play with it. The
username and password are set to the same as the Quickstart ones. If someone
messes up the fun for everyone wait till the next reset or start your own
Heroku instance.


## Installation

For all systems you are going to need:

- Python 2.7, 3.4, 3.5, or 3.6
- Python virtualenv and pip packages
- Ability to compile python native extensions

Once you have all of that you can run the following and move onto Testing
and/or Running Timestrap:

    virtualenv .venv
    source .venv/bin/activate
    pip install -r requirements/development.txt

If you'd prefer to have a minimal installation of Timestrap you can use our
base requirements instead of the development or Heroku requirements by running:

    pip install -r requirements/base.txt

If you plan on helping with front-end development you will also need Node.js.
You can then install our Node.js dependencies by running:

    npm install

### Ubuntu

You can install everything you need from apt.

    sudo apt install build-essential python-dev python-virtualenv python-pip

If you are doing front-end development you also need NPM and Node.js:

    sudo apt install npm nodejs

If you want to run tests you will need to install some additional packages,
these are not required though and if you are working on small changes or
documentation then you can rely on Travis CI to run tests for you.

    sudo apt install firefox xvfb
    wget https://github.com/mozilla/geckodriver/releases/download/v0.16.1/geckodriver-v0.16.1-linux64.tar.gz
    tar zxf geckodriver-v0.16.1-linux64.tar.gz
    sudo mv geckodriver /usr/local/bin/

If you run into issues with the version of geckodriver above, you'll want to
make sure you have the latest or get one for your specific processor if you
aren't running a linux64 environment go to the
[geckodriver releases GitHub page](https://github.com/mozilla/geckodriver/releases).


## Testing

We use selenium with the geckodriver for testing. If you wish to run tests you
will need to make sure you have Firefox installed and on a headless Linux
machine you'll need xvfb. For installation instructions on those see the above
documentation

I'm trying to push for 100% code coverage on this project! If you want to add
or change something and test that everything still works you can do so easily
with:

    python manage.py test

If you push code to our primary repository we test for style adherence and code
coverage. If you get a failed build to either of these we won't accept your
code till it's fixed.


## Running Timestrap

Always make sure you are in the virtual environment before running additional
commands by first running `source .venv/bin/activate`. If you have already done
this from the previous step and have not left the environment continue on!

If you have not yet migrated your database do so by running:

    python manage.py migrate

You'll need to create your first user too:

    python manage.py createsuperuser

After this you can run Timestrap and access it from your browser at
`localhost:8000`.

    python manage.py runserver

### Running Timestrap With Gulp

Similar to above you will still need to be in the virtual environment by running
`source .venv/bin/acivate`. Also make sure you've migrated, created a superuser
and the other tasks. Once that is done you can then run Timestrap with Gulp:

    node_modules/.bin/gulp

This provides you with live updated static files for working on the files in
`static_src`. Once you've completed your changes you can stop Gulp, run 
`node_modules/.bin/gulp build` and commit the changes. We do this to reduce the
number of dependencies required to install Timestrap for people who don't want
to update static files source code or dependencies.


## Generate Fake Data

Want to see how Timestrap would look after being used a while? Run `fake` to
generate some data. Don't run this on a production database or you'll have to
do a lot of clean up.

    python manage.py fake


## Password Resets and Email

To support email for things like password resetting you need to update
Timestrap's settings. I will not presume your email situation and allow you to
do this yourself by reading [Django's documentation](https://docs.djangoproject.com/en/1.11/ref/settings/#email-backend).

If you are using Heroku you can add `sendgrid` to your apps addons on the 
Heroku admin panel or by running:

    heroku addons:create sendgrid

You then need to add these settings to `timestrap/settings/heroku.py`:

    EMAIL_HOST = 'smtp.sendgrid.net'
    EMAIL_HOST_USER = os.environ.get('SENDGRID_USERNAME')
    EMAIL_HOST_PASSWORD = os.environ.get('SENDGRID_PASSWORD')
    EMAIL_PORT = 587
    EMAIL_USE_TLS = True


## Time and Date Localization

If you wish to change things like the date strings you'll need to play around
with [Django's format localization settings](https://docs.djangoproject.com/en/1.11/topics/i18n/formatting/#controlling-localization-in-templates)
in `timestrap/settings/base.py`. We don't do anything to try and localize by
default but we are trying to avoid lock-in to a specific format. If you enable
localization and run into any bugs let us know!
