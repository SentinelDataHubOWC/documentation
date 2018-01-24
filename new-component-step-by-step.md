Build your first OWC web-component
=========

# Overview
This short tutorial shows how to build components for OWC platform. You will create a simple OWC element integrating it in OWC web application.

# Get set up
Before to create and develop a new component, we will setup the development environment. This guide is for linux users, future cross-OS enviroments will be provided using docker. OWC is a web project based on javascript tools. npm is a task manager for nodejs modules and bower is the dependency manager for web projects.
As first step we will install **nodejs** in our local machine.

Linux binaries are available from the official nodejs portal
[https://nodejs.org/en/download/](https://nodejs.org/en/download/).
```
wget https://nodejs.org/dist/v8.9.4/node-v8.9.4-linux-x64.tar.xz
```
Unzipping the *tar.xz* package
```
tar -xf node-vx.x.x-linux-x64.tar.xz
```
we extract the nodejs contents. We have to create a new folder in ```/usr/lib``` to host the node binaries,
```
sudo mkdir /usr/lib/nodejs
```

and we move the executables in the new ```/usr/lib/``` sub folder.
```
sudo mv /usr/lib/nodejs/node-vx.x.x-linux-x64 /usr/lib/nodejs/node-vx.x.x
```

Editing the ```~/.profile``` file we add the following lines
```
export NODEJS_HOME=/usr/lib/nodejs/node-vx.x.x
export PATH=$NODEJS_HOME/bin:$PATH
```
to set the environment variable.
Then we refresh the profile
```
. ~/.profile
```

Node is available to run at this point.

```
node -v
```

Node installation includes also *npm* setup, that we use to download and run modules of nodejs for *OWC* compilation phase.

We can start working on *OWC* now that we have installed *nodejs* in our machine.

The next step is to get OWC code. OWC is an opensource project, the code is hosted on *Github*.
With *wget* we will download the code and with *unzip* we will open the package:
```
wget https://github.com/SentinelDataHub/dhus-owc/archive/master.zip
unzip master.zip
```
The folder ```dhus-owc-master``` contains the whole OWC project, including the *java* part to integrate it as module of DHuS. The goal of this paper is to develop a new owc element, so we will focus only on the JS/HTML/CSS part. To further details about the integration of OWC with DHuS we advice to refer to the *DataHubSystem* official documentation.
Entering in the folder ```frontend``` of the module,
```
cd src/main/frontend/
```
we will find the OWC source code.

Now we have to download the *nodejs* modules to manage the *OWC* code, with the command:
```
npm install
```
After the downlading and installation phase of *npm*,  we have to download the web dependencies via *bower*, launching the command
```
bower install
```

*OWC* is a front-end client of *DHuS*. In development environment here exposed, we have to connect the *OWC* client with a running DHuS instance.

How to launch a *DHuS*  application please refer to the online guide (https://github.com/SentinelDataHub/DataHubSystem/wiki/Smart-Installation)[https://github.com/SentinelDataHub/DataHubSystem/wiki/Smart-Installation].
Assuming that the *DHuS* address is ```http://localhost:8081```, we will setup OWC instance to point the *DHUS* server, with this simple steps:

Open the file ```owc-app.html```:
```
vim app/elements/owc-app/owc-app.html
```
Replace the snippet:
```
var config = {

            baseUrl: "../",
            clientUrl: "./"
        };
```
with the snippet:
```
var config = {

            baseUrl: "http://localhost:8081/",
            clientUrl: "./"
        };
```

Where *http://localhost:8081* is the *DHuS* instance address.

In our scenario the *OWC* instance will be launched via [*gulp.js*](https://gulpjs.com/) local server, so the client is published in a different domain, resulting the [Cross-Domain issue](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) due for security reasons is not to have an ajax request in a web page to another server domain, unless set server side. To bypass the cross domain issue, we use a specific addon of chrome ( to use only while OWC programming) or we launch the browser disabling the web security. This procedure is to consider only for Development context, does not disable web security during normal web browsing.
To run  Chrome  disabling the *web security* launch the following command, closing all instances of Chrome previously:
```
google-chrome --disable-web-security --user-data-dir
```

Otherwise the name of Chrome addon to disable the cross-domain check is [Allow-Control-Allow-Origin: *](https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi).

The development environment is ready to use at this point.


# OWC component anatomy
OWC components are Polymer elements, it follows the structure proposed by *Polymer Authors*.
OWC component implements the [*Model View Controller*](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) (MVC) design pattern.  The view consists of HTML markup where is possible to bind the value of UI elements with application variable. The Controller is written in JS, it contains the business logic of the component using [EcmaScript6](https://github.com/lukehoban/es6features).
The model of component is contained in the property section of component.

**VIEW**

OWC component creator as front-end developer defines  input and outputs of GUI component, these input/output are defined in the view  with the markup language (HTML/CSS). The input and output variables will be bound with *Controller* variables, following the [two-way-binding](https://www.polymer-project.org/1.0/docs/devguide/data-binding) pattern.

**MODEL**

The bound data represents the component *Model*.

**CONTROLLER**  

The controller contains the business logic of component. The main goal of the *Controller* is to create the binding of the *Model* with the *View*, to provide the model from the *datasources* and to manage the behaviour of model. It represents the logic-layer between the user and the datasources.








# Create a new component

# Integrate in OWC

# Launch application

# Enjoy coding

# Summary
