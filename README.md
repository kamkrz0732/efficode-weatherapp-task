# Weatherapp

In order to better move through various elements of the application and its features, below structure was created:

* locally

/weatherapp
├── ansible
│   ├── ansible.cfg
│   ├── hosts.txt
│   ├── playbooks
│   │   ├── playbook-docker.yml
│   │   └── playbook-docker-compose.yml
├── backend
├── docker-compose.yml
├── frontend
├── .git
├── .gitignore
├── package-lock.json
├── README.md

While backend communicates with OpenWeather map portal through API calls, frontend serves the data in form of images to indicate current weather in specified (in backend) location.

## Prerequisites and setup 

* In order to obtain API key an account was needed within [openweathermap](http://openweathermap.org/) service.

* On local machine (in my case Virtual Machine) couple of additional software needed to be installed, including:

**docker**
**ansible**


## Returning your solution

### Via github

* Make a copy of this repository in your own github account (do not fork unless you really want to be public).
* Create a personal repository in github.
* Make changes, commit them, and push them in your own repository.
* Send us the url where to find the code.

### Via tar-package

* Clone this repository.
* Make changes and **commit them**.
* Create a **.tgz** -package including the **.git**-directory, but excluding the **node_modules**-directories.
* Send us the archive.

## Exercises

Here are some things in different categories that you can do to make the app better. Before starting you need to get yourself an API key to make queries in the [openweathermap](http://openweathermap.org/). You can run the app locally using `npm i && npm start`.

### Docker

*Docker containers are central to any modern development initiative. By knowing how to set up your application into containers and make them interact with each other, you have learned a highly useful skill.*

* Add **Dockerfile**'s in the *frontend* and the *backend* directories to run them virtually on any environment having [docker](https://www.docker.com/) installed. It should work by saying e.g. `docker build -t weatherapp_backend . && docker run --rm -i -p 9000:9000 --name weatherapp_backend -t weatherapp_backend`. If it doesn't, remember to check your api key first.

* Add a **docker-compose.yml** -file connecting the frontend and the backend, enabling running the app in a connected set of containers.

* The developers are still keen to run the app and its pipeline on their own computers. Share the development files for the container by using volumes, and make sure the containers are started with a command enabling hot reload.

### Node and React development

*Node and React applications are highly popular technologies. Understanding them will give you an advantage in front- and back-end development projects.*

* The application now only reports the current weather. It should probably report the forecast e.g. a few hours from now. (tip: [openweathermap api](https://openweathermap.org/forecast5))

* There are [eslint](http://eslint.org/) errors. Sloppy coding it seems. Please help.

* The app currently reports the weather only for location defined in the *backend*. Shouldn't it check the browser location and use that as the reference for making a forecast? (tip: [geolocation](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/Using_geolocation))

### Testing

*Test automation is key in developing good quality applications. Finding bugs in early stages of development is valuable in any software development project. With Robot Framework you can create integration tests that also serve as feature descriptions, making them exceptionally useful.*

* Create automated tests for the application. (tip: [mocha](https://mochajs.org/))

* Create [Robot Framework](http://robotframework.org/) integration tests. Hint: Start by creating a third container that gives expected weather data and direct the backend queries there by redefining the **MAP_ENDPOINT**.

### Cloud

*The biggest trend of recent times is developing, deploying and hosting your applications in cloud. Knowing cloud -related technologies is essential for modern IT specialists.*

* Service was deployed within Azure VM - while being IaaS, this deployment is most suitable for "lift and shift" scenarios. With free subscription trial offer and multiple size to choose from, the VM itself can be suited to every needs (development, testing or production environments)

Resource Group
Name: weatherapp-resourcegroup

Azure VM
name: weatherapp-appvm
OS:Linux (redhat 8.2)
Location:North Europe
Size: Standard B1s (1 vcpu, 1 GiB memory)
Tags
	type : task-efficode
	
	
The most important part from the Azure VM was its IP address, needed for Ansible inventory configuration	

* However during deploying the app using Ansible playbook there were issues regarding using docker-compose module for Ansible. Due to insuficient time and technical skills (at the moment), a different approach was made: to deploy weatherapp via Azure WebApp service 

!IMPORTANT NOTE
Azure webapp currently has multi-container deployment (via docker compose) in Preview mode. That means that while the service is available for use, there might be some issues still to work, regarding its functionality. Moreover Microsoft does not quarantee any SLA for services within Preview mode.


*Container registries
Login server/name: weatherappcontainers.azurecr.io
Location:North Europe
SKU: Basic
Tags: 
	type: efficode-task

*WebApp
Name: weatherapp-appservice
Publish :Docker Container
Image:Tag Docker Compose
Tags: 
	type: efficode-task
	
App Service Plan (New)
Operating System: Linux
Region: North Europe
SKU: Free	

**Docker-compose file for Azure WebApp**
# Docker Compose version 3
version: "3"

services:
  backend:
    image: weatherappcontainers.azurecr.io/weatherapp-backend-1.0
    ports:
       - "9000:9000"
    container_name: app-backend
    restart: always
  
  web:
    image: weatherappcontainers.azurecr.io/weatherapp-frontend-1.0
    ports:
      - "80:8000"
    container_name: app-frontend
    restart: always
    depends_on: backend


### Ansible

*Automating deployment processes saves a lot of valuable time and reduces chances of costly errors. Infrastructure as Code removes manual steps and allows people to concentrate on core activities.*

* Ansible playbook steps are as follows (general overview):
. Install and start docker (its repository and Python module)
. Retrieve files from GitHub repository



### Documentation

*Good documentation benefits everyone.*

* Remember to update the README

* Use descriptive names and add comments in the code when necessary

### ProTips

* When you are coding the application imagine that you are a freelancer developer developing an application for an important customer.

* The app must be ready to deploy and work flawlessly.

* The app must be easy to deploy to your local machine with and without Docker. 

* Detailed instructions to run the app should be included in your forked version because a customer would expect detailed instructions also.

* Structure the code and project folder structure in a modular and logical fashion for extra points.

* Try to avoid any bugs or weirdness in the operating logic.

