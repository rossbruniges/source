source
======

Source is a website [dedicated to][sinker-explain] "advocating for, shining a spotlight on, and helping to generate community around the code that's being written in journalism." It's built with [Django][django], using Mozilla's [Playdoh web app template][gh-playdoh].

[sinker-explain]: http://sinker.tumblr.com/post/12203160394/journalism-in-the-open-hard-coding-community
[django]: http://www.djangoproject.com/
[gh-playdoh]: https://github.com/mozilla/playdoh


Installation
------------

### Requirements

You need Python 2.6 or 2.7, MySQL, git, virtualenv (remeber to install as sudo so it has the correct permissions), and a Unix-like OS including a compiler (something like [xcode][xcode] is perfect)

[xcode]: https://developer.apple.com/xcode/

### Setup

1. Fork [the core source repository][source-repo]
2. Clone your new repo:
    `git clone --recursive git@github:{you}/source.git`
3. Jump inside of the new app you've just cloaned
    `cd source`
4. Set up a new [virtual environment][venv] (no, you really should):
    1. If you haven't already, install virtualenv[^1]:
        `pip install virtualenv`
    2. Make sure you're in the repo folder:
        `cd source`
    3. Create a virtual environment in the `venv` subfolder:
        `virtualenv venv`
    4. And activate it:
        `source venv/bin/activate`
5. And make sure you have all the development and compiled requirements
    `pip install -r requirements/dev.txt`

### Configuration

The app has a base settings file that can be found at source/settings/base.py, you can override any of the values there inside a local.py file. 

`cp source/settings/local.py-dist source/settings/local.py`

Please ensure that you create your own SECRET_KEY ([use this to create a secret key][secretkey-gen]) and HMAC_KEY (use todays date and any word you like)

You can point your database config to sqlite for quick testing, or if you'd rather use MySQL, you'll need to create a new database. Adjust the DATABASES dict in source/settings/local.py accordingly, and then

`python manage.py syncdb`

The primary content apps are managed by django-south, so next run

`python manage.py migrate articles`
`python manage.py migrate code`
`python manage.py migrate people`

This repository includes a few fixtures with test articles, people, organizations and code records for you to play with. If you'd like to add them, next run

`python manage.py loaddata test_data`
`python manage.py loaddata taggit_test_data`

And then it's time to fire it up!

`python manage.py runserver`

Now you should be able view your dev server at [http://localhost:8000/][localhost]

[localhost]: http://localhost:8000/
[source-repo]: https://github.com/mozilla/source
[venv]: http://pypi.python.org/pypi/virtualenv
[secretkey-gen]: http://www.miniwebtool.com/django-secret-key-generator/

### Troubleshooting

#### Installation 

If you receive error messages complaining about files inside of the vendor directory
you've probably forgotten the --recursive flag when doing your initial clone.

The vendor directory gets created from a large number of git submodules containing
a number of useful Mozilla products that we like to bundle with all our apps.

From inside of the root source directory run:
`git submodule update --init --recursive`

and the vendor directory should be created this time around.

#### Running the app 

If when trying to runserver you get a number of 'app missing or out of date errors'
then you probably need to update or install some sub-modules.

From inside of the root source directory run:
`git submodule update --init --recursive`
