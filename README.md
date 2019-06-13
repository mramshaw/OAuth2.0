# OAuth 2.0

[![Known Vulnerabilities](https://snyk.io/test/github/mramshaw/OAuth2.0/badge.svg?style=plastic&targetFile=requirements.txt)](https://snyk.io/test/github/mramshaw/OAuth2.0?style-plastic&targetFile=requirements.txt)

# Udacity ud330 - Authentication & Authorization

Another great course from [Udacity](http://www.udacity.com/) which explains the use of [OAuth 2.0](http://oauth.net/2/) (updated here to __v2__).

![OAuth logo](images/oauth-logo.png)

## Contents

The contents are as follows:

* [OAuth](#oauth)
* [Identity Providers](#identity-providers)
* [Scope](#scope)
* [Frameworks Used](#frameworks-used)
* [Updates (as of July 2017)](#updates-as-of-july-2017)
    * [apt-get](#apt-get)
    * [pip](#pip)
    * [Flask-login](#flask-login)
    * [Current Versions](#current-versions)
    * [jquery](#jquery)
    * [Google OAuth 2.9](#google-oauth-29)
* [Get the Source Code](#get-the-source-code)
    * [git](#git)
* [Running the Restaurant Menu App](#running-the-restaurant-menu-app)
    * [Initialization](#Initialization)
    * [Running](#running)
    * [Testing](#testing)
* [Security scanning with Bandit](#security-scanning-with-bandit)
* [To Do](#to-do)
* [Credits](#credits)

## OAuth

__OAuth__ is an open standard for authorization which allows for "delegated access" via a third-party authentication service
(referred to as an _"__identity provider__"_). This avoids having to log into the service in question. It has also been known
as __SSO__ (single sign-on) or ___federated identity___. There can be multiple OAuth paths; the one used here does not involve
any __refresh tokens__ (which are optional).

## Identity Providers

Most of the social networks (FaceBook, Google, HipChat, Instagram, Slack, Spotify, Twitch) can be used as Identity Providers.

Of course, using these services as Identity Providers also facilitates their tracking of individual users and user preferences,
so it is generally a service that they are happy to provide.

[Here we will be using ___Google___.]

In addition, a lot of coder networks (GitHub, GitLab, BitBucket) can also be used as Identity Providers.

![GitHub](images/GitHub.png) ![GitLab](images/logo_wordmark.svg) ![BitBucket](images/BitBucket.png)

[Identity Providers are sometimes referred to as ___Authorization Servers___ in the literature.]

## Scope

The use of OAuth grants the application a specified sphere of influence (often simply an email address,
but it can also be a range of things). The user will be informed of the exact permissions that the app
is requesting and can either grant or deny the access. Of course, if the user denies access the app
will generally have to terminate.

## Frameworks Used

The server-side code is __python__ (using the __Flask__ framework with __sqlalchemy__) while the client-side code is __javascript__ with __jquery__ and __ajax__.

This version of things assumes the current __Ubuntu LTS__ (16.04). It does not use __Vagrant__, which simplifies things quite a bit.

The key functions implemented were the __login__ (with Google) and __logout__ features.

## Updates (as of July 2017)

The version supplied uses __sqlite3__ so neither __postgres__ nor __python-psycopg2__ is needed.

All of the various components were updated to their current __Ubuntu LTS__ (16.04) equivalents.

#### apt-get

The following packages are needed:

	$ sudo apt-get install python-pip python-sqlalchemy

[This will probably require a host of dependencies to be installed.]

#### pip

The various Python components may be installed/upgraded as follows:

	$ pip install --user --upgrade pip
	$ pip install --user Werkzeug
	$ pip install --user Flask
	$ pip install --user oauth2client
	$ pip install --user requests
	$ pip install --user httplib2

[It may be necessary to install additional dependencies as well.]

Or simply use the `requirements.txt` file as follows:

    $ pip install --user -r requirements.txt

[Note that I do not recommend Global component installation.]

#### Flask-Login

Installing this into a single-user system was problematic, the following worked:

	$ pip install --user Flask-Login

[It may be necessary to install additional dependencies as well.]

#### Current Versions

These are as follows:

* Python 2.7.12
* sqlite3 3.11.0

The Python components are as follows:

Component | Version
--------- | -------
Flask        | 0.12.2
Flask-Login  | 0.4.0
httplib2     | 0.10.3
pip          | 9.0.1
oauth2client | 4.1.2
requests     | 2.18.1
Werkzeug     | 0.12.2

#### jquery

Updated from __1.8.2__ to __3.2.1__.

#### Google OAuth 2.9

Updated from __v1__ to __v2__.

## Get the Source Code

Download __OAuth2.0-master.zip__ and unzip it into a directory of your choosing.

Or from a terminal, run:

    git clone http://github.com/mramshaw/OAuth2.0.git

#### git

To install __git__: download from [git-scm.com](http://git-scm.com/downloads) and install the version for your operating system.

## Running the Restaurant Menu App

Open a terminal. Type the following:

	$ ls -al

Verify that you are inside the directory that contains two directories named `templates` and `static` as well as:

* database_setup.py
* lotsofmenus.py
* project.py

#### Initialization

Initialize the database:

	$ python database_setup.py

[Optional] Populate the database with restaurants and menu items:

	$ python lotsofmenus.py

#### Running

Run the Flask web server [Ctrl-C to terminate]:

	$ python project.py

Open the following link in a web browser to view the restaurant application:

	http://127.0.0.1:5000

The web browser of choice for testing this application is probably __chrome__ (or __chromium__ on linux).

#### Testing

You should be able to view restaurants and menu items.

You should be able to log in (with Google) and log out.

Once logged in, you should be able to:
* Create restaurants
* Edit or Delete restaurants you have created
* Create, Edit, or Delete menu items for restaurants you have created

## Security scanning with Bandit

We will use [bandit](http://github.com/PyCQA/bandit) to scan our code for
any insecure coding practices.

Bandit describes itself as follows:

> Bandit is a tool designed to find common security issues in Python code.

Run `bandit` as follows:

```bash
$ bandit -r .
[main]	INFO	profile include tests: None
[main]	INFO	profile exclude tests: None
[main]	INFO	cli include tests: None
[main]	INFO	cli exclude tests: None
[main]	INFO	running on Python 2.7.12
Run started:2018-12-16 00:48:09.951919

Test results:
>> Issue: [B311:blacklist] Standard pseudo-random generators are not suitable for security/cryptographic purposes.
   Severity: Low   Confidence: High
   Location: ./project.py:33
   More Info: https://bandit.readthedocs.io/en/latest/blacklists/blacklist_calls.html#b311-random
32	    # Create a random 32 character string with a mix of uppercase letters and digits
33	    state = ''.join(random.choice(string.ascii_uppercase + string.digits) for x in xrange(32)) 
34	    login_session['state'] = state

--------------------------------------------------
>> Issue: [B104:hardcoded_bind_all_interfaces] Possible binding to all interfaces.
   Severity: Medium   Confidence: Medium
   Location: ./project.py:344
   More Info: https://bandit.readthedocs.io/en/latest/plugins/b104_hardcoded_bind_all_interfaces.html
343	   app.debug = True
344	   app.run(host = '0.0.0.0', port = 5000)

--------------------------------------------------

Code scanned:
	Total lines of code: 462
	Total lines skipped (#nosec): 0

Run metrics:
	Total issues (by severity):
		Undefined: 0
		Low: 1
		Medium: 1
		High: 0
	Total issues (by confidence):
		Undefined: 0
		Low: 0
		Medium: 1
		High: 1
Files skipped (0):
$
```

The low priority warning is easily fixed with a code annotation (we are not using `random`
for cryptography purposes so a pseudo-random function will suffice for our purposes). The
other warning is legitimate but as this project is for ___testing___ purposes we will leave
the insecure code as is (we are running our web server in __promiscuous mode__, which is
definitely insecure).

UPDATE: Snyk.io scanning flagged `bandit` as insecure due to a `pyyaml` dependency. There
is a pyyaml dependency in `flask-ask` for an incompatible version of pyyaml; the easy fix
is simply to remove `bandit` as a project dependency. How ironic that a security linter
should itself use insecure code.

Having fixed the low-priority warning we can create a bandit __baseline__ file as follows:

    $ bandit -r . -f json -o bandit_baseline

Then to re-parse our code - ignoring the baseline - we run `bandit` as follows:

    $ bandit -r . -b bandit_baseline

This should look as follows:

```bash
$ bandit -r . -b bandit_baseline
[main]	INFO	profile include tests: None
[main]	INFO	profile exclude tests: None
[main]	INFO	cli include tests: None
[main]	INFO	cli exclude tests: None
[main]	INFO	running on Python 2.7.12
Run started:2018-12-16 01:45:00.468466

Test results:
	No issues identified.

Code scanned:
	Total lines of code: 462
	Total lines skipped (#nosec): 1

Run metrics:
	Total issues (by severity):
		Undefined: 0
		Low: 0
		Medium: 1
		High: 0
	Total issues (by confidence):
		Undefined: 0
		Low: 0
		Medium: 1
		High: 0
Files skipped (0):
$
```

Note that our annotated warning is listed, as well as our insecure code issue. All that
has been suppressed is the details of our insecure code.

[Of course it is still possible to produce a normal run via <kbd>bandit -r .</kbd>.]

## To Do

- [x] Add Table of Contents
- [x] Add notes on Identity Providers
- [ ] Implement GitHub as an identity provider
- [ ] Implement BitBucket as an identity provider
- [x] Add `bandit` checks for insecure coding practices
- [x] Revert `bandit` as a project dependency as it is itself insecure
- [ ] Refactor code to more easily accomodate different identity providers
- [x] Refactor dependencies into a `requirements.txt` file
- [ ] Dockerize everything to avoid local dependencies
- [ ] Verify code with Python 3 and `pip3`
- [ ] Verify code with latest components
- [ ] Clean up code to conform to `pylint`, `pycodestyle` and `pydocstyle`

## Credits

Based upon:

	http://www.udacity.com/course/authentication-authorization-oauth--ud330

The course materials are available here:

	http://github.com/udacity/OAuth2.0

Of course, to really learn OAuth it is probably best to follow the course!
