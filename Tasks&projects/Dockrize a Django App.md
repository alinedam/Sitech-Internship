# **Step 1 - Set Up Your Development Environment**

```bash
$ mkdir tutotial
$ cd tutorial
```

# create a virtual environment to manage dependencies

```bash
$ python3 -m venv alisystemenv
```

# activate the virtual environment by running the following command:

```bash
$ source alisystemenv/bin/activate
```

# Step 2 - Create a Django Project

you need to add the standard Python project dependencies file which
  is usually named `requirements.txt`,

```bash
$ cat requirements.txt
Django==2.2.5
```

you can then install Django using the following command:

```bash
$ django-admin startproject mysite
```

![tree](https://github.com/alinedam/Sitech-Internship/assets/108859223/d10bf066-6c93-4d72-b66e-c3d1a4b597cd)


You can control the application for development purposes using the `manage.py` file,

```bash
$ cd mysite
$ python manage.py runserver
```

![runserver](https://github.com/alinedam/Sitech-Internship/assets/108859223/183daefb-2778-4005-a18b-a6ccf898545a)


# **Step 3 - Create a Django Application**

o create the app, run the following command:

```bash
$ python manage.py startapp hello_world
```

Once you’ve created the app, you need to install it in your project. In `mysite/settings.py`, add the following line of code under `INSTALLED_APPS`:

```bash
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'hello_world',  # <-- ADD THIS LINE
]
```

![Installedapps](https://github.com/alinedam/Sitech-Internship/assets/108859223/794df6af-7ed1-45dd-ba71-194ef356ebaa)


That line of code means that your project now knows that the app you just created exists. The next step is to create a view so that you can display something to a user.

# **Create a View**

Navigate to the `views.py` file in the `hello_world` directory. There’s already a line of code in there that imports render(). Add the following code:

```bash
from django.shortcuts import render

def hello_world(request):
    return render(request, 'hello_world.html', {})
```

![Screenshot from 2023-08-22 20-33-26](https://github.com/alinedam/Sitech-Internship/assets/108859223/2da9651d-a35b-4362-90ad-703f9dc9aad6)

*Create that directory and subsequently a file named hello*world.html inside it:

```bash
$ mkdir hello_world/templates/
$ touch hello_world/templates/hello_world.html
```

Add the following lines of HTML to your file:

```bash
# hello_world.html
<h1>Hello, World!</h1>
```

![Screenshot from 2023-08-22 20-37-04](https://github.com/alinedam/Sitech-Internship/assets/108859223/56a7f87a-e3ac-4616-a84b-1710c959992d)

Your project has a module called `urls.py` in which you  need to include a URL configuration for the hello_world app. Inside `mysite/urls.py`, add the following:

```bash
from django.contrib import admin
from django.urls import path, include     # <--  ADD THIS LINE

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('hello_world.urls')),   # <--  ADD THIS LINE
]
```

The `hello_world.urls` module doesn’t exist yet, so you’ll need to create it:

```bash
$ touch hello_world/urls.py
```

```bash
from django.urls import path
from hello_world import views

urlpatterns = [
    path('', views.hello_world, name='hello_world'),
]
```

Now, when you restart the server and visit localhost:8000, you should be able to see the HTML template you created:
![serveralisystem](https://github.com/alinedam/Sitech-Internship/assets/108859223/2558a87f-817c-4035-998c-c8fdd5faae1c)

# **Make the docker App image**

```bash
# Dockerfile

# The first instruction is what image we want to base our container on
# We Use an official Python runtime as a parent image
FROM python:3.7

# Allows docker to cache installed dependencies between builds
COPY requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Mounts the application code to the image
COPY . code
WORKDIR /code

EXPOSE 8000

# runs the production server
ENTRYPOINT ["python", "mysite/manage.py"]
CMD ["runserver", "0.0.0.0:8000"]
```

# **Building and Running the Container**

```bash
$ cd
$ docker build -t python-django-app .
```

The final step is to run the container you have just built using Docker:
```bash
# Allows docker to cache installed dependencies between builds
COPY ./ ./ 
RUN pip install --no-cache-dir -r requirements.txt

# Mounts the application code to the image
COPY . code
WORKDIR /code

EXPOSE 8000

# runs the production server
ENTRYPOINT ["python", "mysite/manage.py"]
CMD ["runserver", "0.0.0.0:8000"]
```

```bash
$ docker run -it -p 8000:8000 python-django-app
```
# **Create the Docker network that will host everything**
## creating the network 
```bash
alisystem@ali:/home/ali/djangotask#$ docker network create --subnet=10.10.10.0/24 backend
4ccaac8c0373c86c6912647e85604daddb86821781056665d7f33ccb03fa7e14
```
## Ruining the Django Image using the Network we just created

```bash
alisystem@ali:/home/ali/djangotask $ docker run --network backend --ip=10.10.10.6 -h django-app -d django-app
4ccaac8c0373c86c6912647e85604daddb86821781056665d7f33ccb03fa7e14
```
![Screenshot from 2023-09-17 08-38-13](https://github.com/alinedam/Sitech-Internship/assets/108859223/c769d769-5b77-4ba5-a3d9-fada8cb7a785)
# **Step 3 : host Nginx container acting as a reverse proxy for the Django app**

## First you have to Create a directory named nginx with two files dockerfile and default.conf in it 
```bash

alisystem@ali:`home/nginx$ ls
dokerfile   default.conf
```
## Create default nginx config file in it and add proxy pass inside the config file (exactly inside the server block then inside the location block )

```bash
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://10.10.10.6:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
} 
```
## Creating an Nginx Dockerfile

```bash
FROM nginx:stable@sha256:4a1d2e00b08fce95e140e272d9a0223d2d059142ca783bf43cf121d7c11c7df8
FROM nginx:stable
COPY default.conf /etc/nginx/conf.d/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
## Now Building the Nginx Docker Image
```bash
alisystem@ali:/home/ali/nginx#:$ docker build -t nginx-django-app .
```
## running the docker image 
```bash
alisystem@ali:/home/ali/nginx# docker container run -itd --network=backend --ip=10.10.10.8 -h nginx-django-app nginx-reverse-proxy 
4ccaac8c0373c86c6912647e85604daddb86821781056665d7f33ccb03fa7e14
```
# **Now we have a running Dockraized Nginx Reverse proxy that redirects the traffic to the django application** 
![image](https://github.com/alinedam/Sitech-Internship/assets/108859223/e86971bd-64d3-40dd-bfba-f9ddf2a10e75)
