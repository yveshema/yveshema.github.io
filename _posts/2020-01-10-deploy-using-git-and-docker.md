---
title: How to deploy applications using version control
author: Shema
---
I have [previously](https://yveshema.github.io/2020/01/10/docker-setup-for-react-application/ "reference") spoken about container based application development with docker, alluding to a workflow utilized by most developers today. Today I want to switch gears and talk about deployment. Although I will be describing deployment of a containerized application, there is no correlation between the method used and docker itself.

The app itself is a simple node application consisting of a web page which  in turn renders a React contact form component. The React library itself is loaded from a CDN directly onto the page. The React component is first compiled using Babel and the resulting code served with the page.

The HTML looks something like this:
{% gist 2d494d87fcfdd4970c3d7dd56f2028f2 index.html %}

The server itself is built with Express and all it does respond to requests and process email messages from the contact form using **[nodemailer](https://nodemailer.com/about/ "reference")**.
{% gist 2d494d87fcfdd4970c3d7dd56f2028f2 server.js %}

On the front-end, the app consists of a contact form to allow visitors to send feedback via email. It relies on axios to make post requests.
 {% gist 2d494d87fcfdd4970c3d7dd56f2028f2 contact-form.js %}

The app is containerized using docker compose. Two configuration files are used, one for development and one in production.
{% gist 2d494d87fcfdd4970c3d7dd56f2028f2 docker-compose.yml %}

One thing to note in the configuration is the fact that it uses an external docker network, meaning that the app is accessed through a proxy. I will not go into the details of how this works, but you can find an excellent explanation in [here](https://github.com/jwilder/nginx-proxy "reference").

Now that the application is built and ready for deployment, we can deploy it with **git**. Although this process is described here, ideally this should be the first thing you do at the start of your project cycle. This ensures that you have the entire revision history of your project. So, the following steps assume that the project is already under version control using git. 

Create a bare repository on production server. This assumes write access to the server and git installed.

```
mkdir -p node-project && cd node-project
git init --bare
```

Add the repository just created as a remote from the local machine.

```
git remote add prod ssh://remote_server/path_to_remote_repo
```

At this point you can push your project to the remote repository.

Create a directory from which the app will be served on the remote server and use it to checkout files from the remote repo using the post-receive git hook.

```
mkdir -p my_app
```

and change the contents of the post-receive script to the following:

```
#!/bin/sh
GIT_WORK_TREE=/path/to/my_app git checkout -f
```

A bare git repository stores only the project tree, meaning that if you look inside you won't see any of our app files. Therefore, we need a way to access these files on the remote server, which is what this script essentially does. It checks out the project's files and copies them inside my_app folder upon every push. So updating our app is as simple as running `git push` everytime we make a change.

Now the app is ready for deployment. But first we have to push:

```
git push prod master
```

The app is deployed. From inside my_app directory, run the following to deploy:

```
docker-compose up -d
```

### Final thoughts
Version control is an important part of any software development workflow. Being able to leverage this toolchain to also deploy my applications is not only amazing, it also provides me with an excellent motivation to always keep my projects under version control. Combined with the benefits of containerized application development, this deployment method has become one of my favorites. 