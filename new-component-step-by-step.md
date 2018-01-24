Build your first OWC web-component
=========

# Overview
This short tutorial shows how to build components for OWC platform. You will create a simple OWC element integrating it in OWC web application.
The technology involved are multiple.

**Polymer**

Polymer is an open-source library for creating web applications using web components, following Material Design principles. Polymer library is designed to make it easier and faster for developers to create great, reusable components for the modern web. Polymer is built on top of the web components standards and it helps developers in building their own custom elements.

Polymer library provides a declarative syntax that makes it simpler to define custom elements. It adds features like templating, two-way data binding and property observation to help building powerful, reusable elements with less code.

More information about Polymer library can be found at: https://www.polymer-project.org/1.0/docs/start/what-is-polymer.html

**Bower**

bower is package manager of web (html/css/js) dependencies for client side (browser). It simplifies developers' job downloading and managing all external libraries used by a web application. It's enough to include in your application a file, called bower.json, with the list of dependencies and the version to use.
Node.js

Node.js is is an open-source, cross-platform JavaScript runtime environment for developing tools and application. Node.js javascript interpreter is Google's V8. In the context of OWC, Node.js is required as Bower dependency.
**Npm (Node Package Manager)**

npm is the Node.js package manager, automatically included when Node.js is installed. npm manages installation of packages that are local dependencies of a particular project, resolving also, in one command, all the dependencies of a project.
Gulp



**Gulp**
helps automation of time-consuming tasks in development workflow. It is a Node.js task manager used mainly for: - Minification and concatenation of Javascript and CSS files - CSS and Javascript preprocessing and validation - Image optimization - Unit testing

The philosophy of gulp is code over configurations

**OData**

OData (Open Data Protocol) is a standard that defines a set of best practices for building and consuming RESTful APIs. OData RESTful APIs are easy to consume. The OData metadata, a machine-readable description of the data model of the APIs, enables the creation of powerful generic client proxies and tools.

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

In our component the *View*  code is present in the ```<template>``` node.

**MODEL**

The bound data represents the component *Model*.

In our component the *Model* is represented by the

**CONTROLLER**  

The controller contains the business logic of component. The main goal of the *Controller* is to create the binding of the *Model* with the *View*, to provide the model from the *datasources* and to manage the behaviour of model. It represents the logic-layer between the user and the datasources.





# Create a new component
The OWC provides a python script to create a new component from scratch with the structure of the OWC component:

during command execution, please insert the requested information:
```
Repository path (empty to load the path from configuration file):

Insert the path of the owc source folder (e.g. /client/owc-client/src/main/frontend/) and then press enter.

Repository url (empty to load the path from configuration file):

Insert the git repository url for this component (e.g. https: //github.com//DataHubSystem.git), or leave field empty if there isn't a repository, then press enter.

New element name:

Insert the name of the new component. It must be composed with two words separated by '-' character (e.g. new-component), then press enter.

New element class:

Insert the name of the ES6 class that will contain the code of the new Polymer component (e.g. NewComponent), then press enter.

New element description:

Insert a description of the new component, then press enter.

script output will be similar to snippet below:

Template path: /data/owc-project/app/elements/_template-element, new component path: /data/owc-project/app/elements/new-component
setting demo...
setting test...
setting README...
setting bower.json...
setting wct files...
setting element file
[DONE]
```
The source code of the new component is created in the folder <clone_path>/client/owc-client/app/elements/new-component/, which contains all basic files of a standard Polymer component. This grants the possibility to: - write and run easily component unit tests - run component demo - generate automatically component documentation


# Integrate in OWC

After creating a new component, it is necessary to integrate it into OWC to make it visible to all the other components of the application. To do this, it is enough to insert the reference link to the component in elements.html file located in the folder /client/owc-client/src/main/app/elements.

Some examples of components import are reported below:
```
<link rel="import" href="navigation-manager/navigation-manager.html">
<link rel="import" href="http-manager/http-manager.html">
<link rel="import" href="authentication-manager/authentication-manager.html">
```
# Write component documentation
Tools to auto-generate documentation from code comments are integrated on OWC.

Comment written to generate component generic description is:
```
<!--
The `iron-ajax` element exposes network request functionality.
    <iron-ajax
        auto
        url="https://www.googleapis.com/youtube/v3/search"
        params='{"part":"snippet", "q":"polymer", "key": "YOUTUBE_API_KEY", "type": "video"}'
        handle-as="json"
        on-response="handleResponse"
        debounce-duration="300"></iron-ajax>
With `auto` set to `true`, the element performs a request whenever
its `url`, `params` or `body` properties are changed. Automatically generated
requests will be debounced in the case that multiple attributes are changed
sequentially.
Note: The `params` attribute must be double quoted JSON.
You can trigger a request explicitly by calling `generateRequest` on the
element.
@demo demo/index.html
@hero hero.svg
-->

Comment written to generate documentation about activeRequests property is:

/**
 * An Array of all in-flight requests originating from this iron-ajax
 * element.
 */
activeRequests: {
  type: Array,
  notify: true,
  readOnly: true,
  value: function() {
    return [];
  }
},
```
iron-ajax code and documentation is available in the official google portal and repository on github (code, documentation)

The documentation page is generated by the iron-component-page. The environment to exploit iron-component-page is set using the component generation tool described in paragraph How to create a new web component for OWC (generation tool).

To deploy the documentation is enough to serve the element folder via http server. If you want to publish new-component documentation in the port 8081, you need to perform these commands:
```
cd <clone_path>/client/owc-client/src/main/frontend/app/
python -m SimpleHTTPServer 8081
```
Then you can take a look at your component documentation opening a browser at this url:
```
http://localhost:8081/elements/new-component/
```
# Launch application

Gulp tasks to compile and launch the web application are provided in the package.
Run this command in the ```frontend``` folder:

```
gulp serve:dist
```

The local server will be run and the default browser will be opened loading the client page.



# Enjoy coding

The last step is to write down the implementation code of your new component. Many open source examples are present on the web, for the sake of completeness we advice to read some code from official Polymer community compoments [https://github.com/PolymerElements](https://github.com/PolymerElements).


# Summary
OWC is based on the most modern web technologies. The developer interested to create a new component has to setup the development environment, to run the creator script and to develop the code following the javascript best practices.
