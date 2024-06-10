This document describes the steps taken in development of CodeAbbey web site.

# Friday, June 7th, 2024

End of Qwerty's working week. He is working five days each week, from Monday to Friday, inclusive.

# Saturday, June 8th, 2024
## Registration on GitHub

Qwerty decided to register a GitHub account for CodeAbbey development.

1. He went to <https://github.com/>
2. Entered his email in textbox with a placeholder "Email address".
3. Pressed "Sign up for GitHub" button.
4. Confirmed choice of email.
5. Entered a password, simple but strong by GitHub standards.
6. Entered a username.
7. Solved the captcha.

Then the following actions were taken in order to set up the account:

1. Logged in to GitHub.
2. Entered GitHub launch code.
3. Dismissed all suggestions on extra tools and paid plans.

## Deciding what license to use

Qwerty read the article describing [what one need to know to choose an open source license](https://gist.github.com/nicolasdao/a7adda51f2f185e8d2700e1573d8a633).
He decided that MIT License is the most appropriate.

## Setting up a repository for CodeAbbey web site

The **Create repository** button redirected Qwerty to <https://github.com/new> page where one can set several creation options for the new repository.
Here he made the following actions:

1. Entered the repository name: `code-abbey`.
2. Entered the description for repository.
3. Set the type of repository to `Public`.
4. Ticked the checkbox `Add a README file`.
5. Chose `Python` from .gitignore templates.
6. Chose to use a `MIT License`.
7. Finally, pressed the **Create repository** button again.

## Setting up a repository for CodeAbbey documentation

The steps are almost the same as for CodeAbbey web site repository, with the following exceptions:

1. The repository name is `code-abbey-docs`.
2. The description is different.
3. No .gitignore file.

## Setting up Git

GitHub is great, but it is not possible to test source code on GitHub. Therefore, Qwerty wanted to create source code not directly on GitHub, but on his local machine, where he could set up and run a local server for testing.

Here is his machine configuration:

- Type: Lenovo Tab 11 Tablet PC
- Operating system: Android 11
- Firmware version: TB-J606L_USR_S320233_2210131628_Q00070

Though Android is based on Linux, it does not have a mechanism for working with native packages, such as git. To deal with that, let us install Termux: <https://play.google.com/store/apps/details?id=com.termux&hl=en>

After installation, Qwerty found that the default font size in Termux is very small. He disliked that, because years of sitting at PC greatly decreased the sharpness of his eyesight. Fortunately, he found out that one can use `Ctrl Alt +` keyboard combination to increase font size.

After adjusting font size to proper value, he proceeded with installing Git:

```
$ pkg install git
```

Set up configuration settings:

```
$ git config --global user.name Qwerty
$ git config --global user.email qwerty@uo.com
```

Qwerty did not want to start experimenting with Git on `code-abbey` or `code-abbey-docs` repositories yet, because chances were that something will go wrong and data in these repositories will be corrupted.
Therefore, he created one more repository called `termux-git-test` within GitHub web interface and cloned this repository to local machine:

```
$ git clone https://github.com/Qwertyu89o/termux-git-test.git
```

Then he created a new file in local repository and edited it:

```
$ cd termux-git-test
$ touch hello-world.md
$ nano hello-world.md
```

In the `hello-world.md` file, he wrote the following text:

```
This file was added in Termux and edited by nano editor.
```

And added file to the repository:

```
$ git add hello-world.md
$ git commit --message="Added a test file"
```

When pushing the changes to remote repository, he at first tried the following command:

```
$ git push
```

The command requested his GitHub username and password, and then failed with the following message:

```
remote: Support for password authentication was removed on August 13, 2021.
```

Great! Why request username and password if password authentication is not supported anymore?

Anyway, Qwerty needed to work around this problem. He read about personal access tokens on the pages <https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github> and <https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic>

Following the instructions, he clicked on profile photo, scrolled menu down and chose **Settings**. And in **Settings**, he again scrolled the page down and chose **Developer settings**.

In **Developer settings**, Qwerty chose **Personal access tokens** -> **Tokens (classic)**.
Then clicked the link **Generate personal access token** and entered GitHub password.

Then the following actions were made:

1. In the **Note** section, wrote: "Git on Termux".
2. Expiration - left the default value (30 days).
3. Clicked all permissions to be sure (because of unfamiliarity with GitHub and its system of permissions).
4. Clicked **Generate token** button and saved the generated token in a place Qwerty can easily remember.

Then Qwerty started to think what should he do with that token. Surprisingly, GitHub documentation says nothing on that!

After some googling, he found the following discussion on Stack Overflow:

<https://stackoverflow.com/questions/18935539/authenticate-with-github-using-a-token>

The accepted answer looks promising, but does Qwerty really have to type a very long access token by hand?

He checked `termux-clipboard-set` and `termux-clipboard-get` commands, but they did not work. Thet just hang indefinitely.

He also tried to give Termux access to local storage with `termux-setup-storage`. However, this command just crashes the Termux application, without any messages.

Okay, no luck with these workarounds, so he had to type a very long access token by hand.
Nevertheless, Qwerty hopes that it will be better for you and you could get token from clipboard or local storage.

So that is the command he typed (actual token replaced with \<MYTOKEN\> for security):

```
$ git push https://Qwertyu89o:<MYTOKEN>@github.com/Qwertyu89o/termux-git-test.git
```

And it worked! Not on first attempt though, because Qwerty was typing token by hand and misunderstood 'I' as 'l'. So please, before start to typing, copy token in such a editor where the difference between these and possibly other confusing characters is visually clear.

Before moving on, let us clean the test repository and revert it to the initial commit, because maybe this repository will be used later for another tests:

```
$ git reset HEAD^ --hard
$ git push https://Qwertyu89o:<MYTOKEN>@github.com/Qwertyu89o/termux-git-test.git --force
```

## Setting up Django

For creating CodeAbbey web site, the Django framework was chosen, because Python was one of Qwerty's preferred languages and Django is one of Python's web frameworks.

The following commands were executed in Termux in order to install Django:

1. Installing Python:
   ```
   $ pkg install python
   ```
2. Verifying Python:
   ```
   $ python
   Python 3.11.9 (main, May 30 2024) .. on linux
   >>> exit()
   ```
3. Setting up repository in which the source code of web site will reside:
   ```
   $ git clone https://github.com/Qwertyu89o/code-abbey.git
   $ cd code-abbey
   ```
4. Setting up a virtual environment:
   ```
   $ python -m venv venv
   ```
5. Activating virtual environment:
   ```
   $ source venv/bin/activate
   ```
6. Installing Django:
   ```
   (venv) ~/code-abbey $ pip install django
   ```
7. Verifying Django:
   ```
   (venv) ~/code-abbey $ python -m django --version
   5.0.6
   ```

## Creating basic Django application

Qwerty is experienced with Python, but never developed a web site outside of his company (as a side note, his company is specialized on PHP and never created a single Django application). So he decided to start with official [tutorial][django-tutorial].

Tutorial suggests to use `django-admin` command to do some intial setup. Let us try it:

```
(venv) ~/code-abbey $ django-admin startproject CodeAbbey
(venv) ~/code-abbey $ cd CodeAbbey
(venv) ~/code-abbey/CodeAbbey $ ls
CodeAbbey manage.py
(venv) ~/code-abbey/CodeAbbey $ cd CodeAbbey
(venv) ~/code-abbey/CodeAbbey/CodeAbbey $ ls
__init__.py  asgi.py  settings.py  urls.py   wsgi.py
```

Let us try to run a simple local server provided by framework:

```
(venv) ~/code-abbey/CodeAbbey/CodeAbbey $ cd ..
(venv) ~/code-abbey/CodeAbbey $ python manage.py runserver
Performing system checks...
...

...
Starting development server at http://127.0.0.1:8000/
```

After switching from Termux to his favorite web browser and typing the address http://127.0.0.1:8000/, Qwerty saw the rocket taking off. Amazing, it works even on tablet PC!

Now let us create a basic web page for our site:

```
(venv) ~/code-abbey/CodeAbbey $ python manage.py startapp app
(venv) ~/code-abbey/CodeAbbey $ cd app
(venv) ~/code-abbey/CodeAbbey/app $ nano views.py
```

Qwerty modified app/views.py as follows:

```
# from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def index(request):
    return HttpResponse('Welcome to CodeAbbey!')
```

And app/urls.py as follows:

```
(venv) ~/code-abbey/CodeAbbey/app $ nano urls.py
```

```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

And CodeAbbey/urls.py as follows:

```
(venv) ~/code-abbey/CodeAbbey/app $ cd ../CodeAbbey
(venv) ~/code-abbey/CodeAbbey/CodeAbbey $ nano urls.py
```

```
from django.urls import include, path

urlpatterns = [
    path('', include('app.urls'))
]
```

After all these changes, Qwerty run the server again and the site is now greets the good people of CodeAbbey.

```
(venv) ~/code-abbey/CodeAbbey/CodeAbbey $ cd ..
(venv) ~/code-abbey/CodeAbbey $ python manage.py runserver
```

It is a good time to submit current work to the repository:

```
(venv) ~/code-abbey/CodeAbbey $ cd ..
(venv) ~/code-abbey $ deactivate
~/code-abbey $ git add CodeAbbey
~/code-abbey $ git commit --message="Created basic Django application"
~/code-abbey $ git push https://Qwertyu89o:<MYTOKEN>@github.com/Qwertyu89o/code-abbey.git
```

And here the end of June, 8th comes.

# Sunday, June 9th, 2024
## Publishing the web site to the Web

Yesterday, Qwerty managed to create a basic web application using Django. Now he wants to make our web site available over internet, so we can be proud of our work.

What he tried after some googling:

1. Heroku: <https://www.heroku.com/> - got a strange message saying that "Cookies are required to sign up for and use the Heroku platform. Please enable cookies and try again.". Since Qwerty did not made anything to disable cookies, he feels very puzzled.
2. Railway: <https://railway.app/> - after registration site requests to enter credit card number.
3. Fly.io: <https://fly.io> - the same story, site requests to enter credit card number.
4. Vercel: <https://vercel.com/> - there was no request for credit card number, but after deployment site responds with 404 error code and the docs did not explained why. Qwerty wished he could change something, but there was no deployment options on this site, or he failed to find these.
5. Render: <https://render.com/> - here was more deployment options than on Vercel. Qwerty chose "Web service" option of platform, chose "Public Github repository" as source and filled in fields as follows:

- Source Code: Qwertyu89o / code-abbey
- Web service name: code-abbey
- Region: Oregon (US West)
- Branch: main
- Root Directory: CodeAbbey
- Runtime: Python 3
- Build Command: pip install -r requirements.txt
- Start Command: gunicorn CodeAbbey/wsgi.py
- Instance Type: Free (The site requires users to wait for 1-2 minutes while it loads).

Before that, he created **requirements.txt** at code-abbey repository, because there was no such file before (it was not needed to run Django on local machine yesterday).
In that file, he added the following line:

```
Django==5.0.6
```

He liked all these live logs about Django install.
However, the deployment process hang for a while and then failed with following lines:

```
==> Running 'gunicorn CodeAbbey/wsgi.py'
bash: gunicorn: command not found
```

So Qwerty added one more line to requirements.txt:

```
gunicorn==22.0.0
```

Again a failure:

```
ModuleNotFoundError: No module named 'CodeAbbey/wsgi'
ImportError: Failed to find application, did you mean 'CodeAbbey/wsgi:application'?
```

Qwerty changed start command to:

```
Start Command: gunicorn CodeAbbey/wsgi:application
```

Unfortunately, the `ModuleNotFoundError` persists, and Qwerty had no clue what happening, so he tried googling and found this Stack Overflow topic: <https://stackoverflow.com/questions/48121048/gunicorn-no-module-named-wsgi>

In this topic, Slipstream suggests the following:

> To change to the project folder you can use the command --chdir.

So Qwerty changed the command accordingly:

```
Start Command: gunicorn --chdir CodeAbbey wsgi:application
```

It worked! But... there is one more problem according to logs. Django disallows running the site on `code-abbey.onrender.com`:

```
django.core.exceptions.DisallowedHost: Invalid HTTP_HOST header: 'code-abbey.onrender.com'. You may need to add 'code-abbey.onrender.com' to ALLOWED_HOSTS.
```

Qwerty again went to `code-abbey` repository and made required changes in CodeAbbey/CodeAbbey/settings.py:

```
- ALLOWED_HOSTS = []
+ ALLOWED_HOSTS = ['code-abbey.onrender.com']
```

And the website is live now!

## Improvements for web site

Yesterday, we ended up with a very simple site which just prints "Welcome to CodeAbbey!".

Now, when the tricky part of deploying the web side is done, Qwerty feels that this is a time to make some improvements to our web site.

First, Qwerty went to the version of site which is more famillar to us ([link to official site][codeabbey-official]) and started studying that version.

What he noted at first glance:

- The official site have a nice favicon, the current work does not.
- There is a menu on top of page, the current work does not.
- Below menu to the left there is a beautiful castle which absent at current work.
- Below menu to the right there is "Top Brethren and Sistren of the Week" ranking, and below that ranking table there are links to forum posts. Again, this is not implemented in current work.

Qwerty decided to work on these issues, as well as any other issues that may be discovered during a process.

### Adding a favicon to the web site

It is not hard to obtain favicon location by studying web page source code. Being at [codeabbey official][codeabbey-official], we can press `Ctrl U` and we see the following code:

```
<html lang="en">
<head>
    <title>CodeAbbey - programming problems to practice and learn for beginners</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="icon" type="image/png" href="/img/icon.png"/>
```

The line we are interested in is:

```
<link rel="icon" type="image/png" href="/img/icon.png"/>
```

So, Qwerty downloaded favicon from "/img/icon.png" and started to think how he would serve this static file.

According to [django documentation][django-static-files], to serve static files during development, one needs to define STATIC_URL in its settings file:

```
STATIC_URL = "static/"
```

As well as modifying template file accordingly, for example:

```
{% load static %}
<img src="{% static 'my_app/example.jpg' %}" alt="My image">
```

As it turned out, STATIC_URL is already defined in settings file, so Qwerty only created and modified the template file `code-abbey/CodeAbbey/templates/app/index.html`:

```
<!DOCTYPE html>
{% load static %}
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CodeAbbey</title>
    <meta name="author" content="Rodion Gorkovenko">
    <link rel="icon" type="image/png" href="{% static 'images/favicon.png' %}">
    {% endblock %}
</head>
<body>
    <div>Welcome to CodeAbbey!</div>
</body>
</html>
```

And the `index` view in `code-abbey/CodeAbbey/app/views.py` was changed to use a template instead:

```
from django.shortcuts import render


def index(request):
    return render(request, 'app/index.html')
```

And the favicon itself was downloaded into `code-abbey/CodeAbbey/app/static/images/favicon.png`.

And because he is working on static files anyway, Qwerty decided to add some nice styling as well to `code-abbey/CodeAbbey/app/static/styles/main.css`:

```
body {
    font-family: Arial, serif;
    font-size: 32px;
}
```

It is time now to checkout repository to local machine and check if favicon works:

```
$ cd code-abbey
~/code-abbey $ git pull https://Qwertyu89o:<MYTOKEN>@github.com/Qwertyu89o/code-abbey.git
~/code-abbey $ source venv/bin/activate
(venv) ~/code-abbey $ cd CodeAbbey
(venv) ~/code-abbey/CodeAbbey $ python manage.py runserver
```

It did not work because of following reasons:

Qwerty added an entry to ALLOWED_HOSTS and suddenly the address 127.0.0.1 stopped to work. It worked with empty ALLOWED_HOSTS list, but now this list is not empty, and the address 127.0.0.1 is not an implicitly allowed host anymore...
Qwerty had to add it to ALLOWED_HOSTS explicitly:

```
ALLOWED_HOSTS = ['127.0.0.1'] if DEBUG else ['code-abbey.onrender.com']
```

Django turned out to be not too smart to find a newly created folder for templates. Qwerty helped Django to find that folder with the following entry in settings.py:

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
```

Now static files are working properly when testing on local machine. Unfortunately, this is not the case for production site, so Qwerty needed to learn more on serving static files on production server.

After reading the docs at <https://docs.render.com/deploy-django>, he decided to do following:

Add the following dependencies to `requirements.txt`:

```
asgiref==3.8.1
Brotli==1.1.0
click==8.1.7
colorama==0.4.6
dj-database-url==2.2.0
Django==5.0.6
gunicorn==22.0.0
h11==0.14.0
packaging==24.0
psycopg2-binary==2.9.9
sqlparse==0.5.0
typing_extensions==4.12.2
tzdata==2024.1
uvicorn==0.30.1
whitenoise==6.6.0
```

Update `settings.py` with PostgreSQL database settings.

```
if DEBUG:
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': BASE_DIR / 'db.sqlite3',
        }
    }
else:
    DATABASES = {
        'default': dj_database_url.config(
            default='postgresql://postgres:postgres@localhost:5432/mysite',
            conn_max_age=600
        )
    }
```

Add WhiteNoise to serve static assets:

```
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    ...
]
```

Create a `build.sh` file to prepare site for production:

```
#!/usr/bin/env bash
# Exit on error
set -o errexit

pip install -r requirements.txt

# Convert static asset files
python manage.py collectstatic --no-input

# Apply any outstanding database migrations
python manage.py migrate
```

Qwerty also paid attention to the security notices in documentation and set up a secret key and a conditional to switch off debug mode:

```
# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = os.getenv('SECRET_KEY')
```

```
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = os.getenv('ABBEY_PRODUCTION_SERVER') is None
```

After these actions, he tested the new configuration on local machine and was satisfied with that the site still runs on local machine.

Then Qwerty set up database which is mentioned in docs. He went to <https://dashboard.render.com/new/database>, and filled in fields:

- Name: codeabbey_postgresql
- Database: codeabbey_db
- User: codeabbey_admin
- Region: Oregon (US West)
- PostgreSQL Version: 16
- Instance Type: Free (The database does not allow backups and self-destroys after 30 days, effectively requiring a creation of new one from scratch. It expires on July 9, 2024).

And he also set up environment variables on <https://dashboard.render.com/web/{some_token}/env>: SECRET_KEY, DATABASE_URL, WEB_CONCURRENCY and ABBEY_PRODUCTION_SERVER (instead of ABBEY_PRODUCTION_SERVER he could use environment variable RENDER, which is provided by render.com automatically, but this means less portability of code between different hosting providers).

Finally, Qwerty pulled the code to local machine, changed build.sh file permissions as follows:

```
chmod a+x build.sh
```

And pushed the file back to repository. He then deployed the repository to production server and now favicon is showing and styles are applied. This was great!

### Adding a beautiful castle

Unlike adding favicon, it wasn't necessary to view source code in order to obtain resource. One can just right-click on image and choose "Save image as...". The problem is how to position image to the left of ranking table, but Qwerty decided to postpone this problem later. For now, he just added image to web page instead of "Welcome to Codeabbey":

```
{% load static %}
...
{% block body %}
	<img src="{% static 'images/facade.gif' %}" alt="Abbey of coding problems">
{% endblock %}
```

### Adding ranking of top brethren and sistren of the week

Qwerty feels that this task is a complex one...

First, opensource version of site does not have any real users. How can we work around this?
Possibly by adding some fake users...

Second, the ranking table is not just a table, it uses some complex CSS styling.
Qwerty had to study styles carefully to be close to original.

Okay, so Qwerty opened the [official][codeabbey-official] source code and it looked like this:

```
    <div class="strong">Top Brethren and Sistren of the Week</div>
    <a href="/index/user_ranking">(see best of all times)</a><br/>
    <table class="table table-striped table-condensed table-bordered full-width">
        <thead>
            <tr class="centered">
                <th>User</th>
                <th title="Level">Rank</th>
                <th title="Solved during last 7 days">Solved</th>
            </tr>
        </thead>
        <tbody id="table-week-top">
        </tbody>
    </table>
```

No embedded styles, but a lot of classes, and they have interesting styles. For example, let us check how table stripes were implemented in `bs-cm.css`:

```
.table-striped>tbody>tr:nth-child(odd)>td, .table-striped>tbody>tr:nth-child(odd)>th {
    background-color: #e4eeff;
}
```

`table-condensed` stands for:

```
.table-condensed>thead>tr>th,.table-condensed>tbody>tr>th,.table-condensed>tfoot>tr>th,.table-condensed>thead>tr>td,.table-condensed>tbody>tr>td,.table-condensed>tfoot>tr>td{
    padding:5px
}
```

`table-bordered` means:

```
.table-bordered th,.table-bordered td{
    border:1px solid #ddd!important
}
.table-bordered{
    border:1px solid #ddd
}
.table-bordered>thead>tr>th,.table-bordered>thead>tr>td{
    border-bottom-width:2px
}
```

Since we are not sure yet, if we need the whole Bootstrap on opensource version, Qwerty decided to create a separate `table.css` file with above rules and place a link to stylesheet.

Another tricky thing were these coloured rankings. After some investigation, Qwerty figured out that classes responsible for ranking colors are defined in document sent by `index/api_weekbest` API endpoint. They look like `rank rank1`, `rank rank2`, ..., `rank rankN` and defined in the same `bs-cm.css` file like:

```
.rank {
    font-weight: bold;
    background-repeat: no-repeat;
    background-size: 16px;
}
.rank0 {
    color: #888;
}
.rank1 {
    color: #4f4;
}
...
```

Qwerty quickly took these styles and wrote then to `ranks.css` file. And he thinks that colors in these styles are chosen really well, except for `rank1`, for which he changed color from #4f4 to #4d4.

### Adding a menu on top of page

After several hours of googling, Qwerty finally figured out how to compose the menu: <https://www.w3schools.com/css/tryit.asp?filename=trycss_inline-block_nav>

He created a new CSS file named `layout.css` and placed styles from W3Schools example to that file.

### Composing image and table

This was suprisingly hard, and Qwerty probably would never make a dream possible, but [Flex containers][flex-containers] saved the day for him.

## Summary

So, the simple sketch of CodeAbbey main page is done. This does not sound like much, but Qwerty hopes that this is just a beginning.



[codeabbey-official]: <https://www.codeabbey.com/> "CodeAbbey official site"

[django-tutorial]: <https://docs.djangoproject.com/en/stable/intro/tutorial01/> "Django official tutorial"

[django-static-files]: <https://docs.djangoproject.com/en/5.0/howto/static-files/> "Management of static files in Django"

[flex-containers]: <https://www.w3.org/TR/css-flexbox-1/#flex-containers> "Flex containers"
