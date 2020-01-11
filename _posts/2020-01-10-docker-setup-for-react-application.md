---
title: A simple docker setup for a React app
author: Shema
categories: [docker, React]
tags: [docker, react, node, development]
---
Containerized application development has become an industry standard over the past couple of years. One of the many motivations for adopting a container based workflow is that container tools such as docker let you encapsulate the application execution environment which is guaranteed to be the same for both development and production. 

In this post I want to present a simple docker setup for a React application. I will be building the application from the [official Node.js docker image](https://hub.docker.com/_/node "reference"). This is not a post about docker, so I will not expand on what it is or how to use it. For this purpose, there are many excellent resources, including the [official Docker Guides](https://docs.docker.com/get-started/ "reference").

Many tutorials on how to containerize a React application assume Node.js being installed on the system, which is perfectly fine. In most cases this approach makes the most sense. However, I think it is important to point out that docker being a container solution requires no such prerequisite to deliver the functionality you want. Also, there may be situations where, for one reason or another you are unable to set up a Node.js development environment on the system you are working on, but have access to docker. Whatever the case may be, suffice it to say that it is possible to encapsulate everything you need to have your React app up and running with docker all by itself.

### The <code>Dockerfile</code>
The Dockerfile itself is simple. All it does is to pull in the Node.js image, sets up the working directory before finally launching the app.
{% gist d8525b4275745568b6251bb60c5a835e Dockerfile %}

This container will run as an executable, so we need an entrypoint:
{% gist d8525b4275745568b6251bb60c5a835e docker-entrypoint.sh %}

The script first checks to see if our app has been initialized, builds it and then runs `npm start` to execute it.

### Docker compose
I always find it convenient to control my containers using docker compose. The `docker-compose.yml` for this setup is dead simple. It creates a mount point for our app so that we can edit files outside the container as we continue to add to our application. Then we specify which port to listen to and map it to a port on the host so we may access it from the outside.
{% gist d8525b4275745568b6251bb60c5a835e docker-compose.yml %}

### Running the application
With our setup in place, we can now build and run the application. Since we are not relying on Node.js being installed on the system, we will use the container to create the React app. First,  add a folder to your project and name it something like "myapp", then run this command:
{% gist d8525b4275745568b6251bb60c5a835e create-react-app %}

Then fire up the container:
{% gist d8525b4275745568b6251bb60c5a835e launch %}

Now if you open up the browser and go to localhost:3000 you will see you app up and running!

### Final thoughts
I haven't been using docker for long but I have quickly grown to love it. It has now become a staple in my development workflow. The method described here works with a caveat. If you don't know what you application's dependencies will be as it grows, you most certainly are better off setting up your environment on your machine, building the app when it's done and then copying the built artifact into the container. 
Happy hacking!