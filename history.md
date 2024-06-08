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
(venv) ~/code-abbey/CodeAbbey $ python manage.py startapp polls
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
