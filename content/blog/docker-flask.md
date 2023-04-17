---
title: "Getting Started with Docker & Flask"
description: "Data compression using Principal Component Analysis (PCA)"
dateString: Sep 2021
draft: false
tags: ["Docker", "Flask", "Python", "PCA", "Data Compression"]
weight: 107
cover:
    image: "/blog/pca-visualized/cover.jpg"
---
# Introduction

This article offers a look at how to get started with docker and flask through a practical and hands-on (approach) python application. The goal is to provide a first look and understanding of some of the most quintessential tools for developers in the contemporary world.

# Docker

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

## Docker Container

Essentially, a container refers to a unit that houses all the code and dependencies of an application in a standard format that allows it to runs quickly and reliably across computing environments. Therefore, a docker container lightweight, standalone, executable container that comprises all components necessary to run an application such as libraries,code, and runtime.

## Docker Installation

The recommended method of installation is through setting up Docker's repositories and installing from them; it's not only easy to install but also enables quick upgradability. Therefore, we shall install using the repository approach in Ubuntu 20.04 LTS.

## Set up the repository

- First, update apt and install necessary packages to us a repository over **HTTPS**:

```terminal
$ sudo apt-get update

$ sudo apt-get install \
> apt-transport-https \
> ca-certificates \
> curl \
> gnupg \
> lsb-release
```

- Add Docker’s official GPG key:

```terminal
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

- Set up a stable repository and add the nightly or test (or both) repository:


```terminal
$ echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```

## Install Docker Engine

- Run an update apt and install the latest version of Docker Engine and containerd:

```
$ sudo apt-get update

$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

- Confirm whether the Docker Engine is correctly installed using hello-world:

```
$ sudo docker run hello-world
```

You can visit the official [Docker Docs](https://docs.docker.com/engine/install/) page to view the installation procedures for other operating systems.

# Flask

This is a simple and lightweight Python web framework that offers useful tools for developing Python web applications.
Flask is super useful for building microservices, you can utilize any number of its built-in extensions to design and deploy microservices at high velocity.

## Setting Up Our Environment

This project will require:
- Python 3.6+ and PIP3
- Virtual environment - Venv
- Create project directory
- Create venv
- Install required packages using PIP e.g. Flask
- Create a directory for the app and navigate into the directory:

```terminal
$ mkdir demoapp

$ cd demoapp/
```

- Create a virtual environment using `venv`:

```terminal
$ python3 -m venv venv

$ ls
venv
```

- Activate the virtual environment:
```terminal
$ source venv/bin/activate
```
The prefix (`venv`) is an indication that the virtual environment is active.

- Install Flask using pip3:
```terminal
$ pip3 install flask
```

# Demo Flask Application
## Build the python application:

This is a simple python application that utilizes the **GET** and **POST** requests. It creates a form where a user (say a student), is prompted to enter their name and the field of interest. The user inputs are the formatted and displayed upon clicking the submit button.

- Create `app.py` and code using an IDE of your choice. In this case, we use VS Code:

```terminal
$ touch app.py
```

- Here is the code for the application:

```python
# import Flask and request
from flask import Flask, app, request

# develop the app
app = Flask(__name__)

# both GET and POST requests
@app.route("/form-example", methods=['GET', 'POST'])
def form_example():
    # the POST request
    if request.method == 'POST':
        username = request.form.get('username')
        interest = request.form.get('interest')
        return '''
            <h1>The student's name is: {}</h1>
            <h1>The field of interest is: {}</h1>'''.format(username, interest)

    # the GET request
    return '''
              <form method="POST">
                  <div><label>Username: <input type="text" name="username"></label></div>
                  <div><label>Interest: <input type="text" name="interest"></label></div>
                  <input type="submit" value="Submit">
              </form>'''

if __name__ == '__main__':
    # we'll run the app in debug mode on port 3000
    app.run(debug=True, port=3000, host="0.0.0.0")
```

- Generate a `requirements.txt` file using freeze command.

```terminal
$ pip freeze > requirements.txt
```

## Build the App's container image:

In order to build the application, you need to use a Dockerfile. A Dockerfile is simply a text-based script of instructions that is used to create a container image.

- Create a file named Dockerfile in the same directory as `app.py`:

```terminal
$ touch Dockerfile
```

- Contents of the Dockerfile:

```
FROM python:3.6

COPY . /src

COPY ./requirements.txt /src/requirements.txt

WORKDIR src

EXPOSE 3000:3000

RUN pip3 install -r requirements.txt

CMD [ "python", "app.py" ]
```

- Build the container image using docker build command:

```terminal
$ sudo docker build -t demoapp .
```

## Running the application:

- Start the container using `docker run` command

```terminal
$ sudo docker run -dp 3000:3000 demoapp
```

- Use your browser to access http://172.17.0.2:3000/form-example. You should see the app!

# Results

The first page is a form with Username and Interest field entries and a **Submit** button:

![](/blog/docker-flask/img1.png#center)

Upon entering the name, field of interest press **Submit**.

![](/blog/docker-flask/img2.png#center)

And Voila!

![](/blog/docker-flask/img3.png#center)

