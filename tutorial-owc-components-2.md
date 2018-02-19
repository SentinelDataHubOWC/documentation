# OWC Tutorial 2
&nbsp;
# Building and Playing with two Open Web Components

This Tutorial provides a sample of two Open Web Components interacting each other.

&nbsp;
## What you’ll learn
You'll learn how to create a basic Open Web Component, called "exaple-data-visualizer" with a string content and three buttons, modifiable inside the same component.
You'll also learn how to create another Open Web Component, called "example-data-source" which can create different components and modify the content of all the created components at once.

You'll use some existing Open Web Components:
 - **navigation-manager:** basic Open Web Component to handle the positioning of other components.
 - **message-broker:** useful implementation of Observer Pattern, with a component "publishing" and another one "subscribing" to some specific event. A Message is also passed through this event Handler
 - **toast-manager:** An utility Open Web Component to display a bar containing a log about something happening in the application



&nbsp;

## Specifications 
Create an Open Web Component called "example-data-visualizer". It must contain a two-ways data binding string with default value (eg "default"), and three buttons. These three buttons must triggers these simple funtions to change the string content:
 - Modify the default content with another different string
 - Change CSS style of the string (for example setting the color style to "red")
 - Reset string value to default value and CSS value to default one (for example setting color back to black). Also notify the function call to the user, using toast-manager component

Create another OWC Component called "example-data-source". It must contain two buttons, with the following labels, triggering these two functions associated on-click():
 - "Create Data Visualizer": triggers pushComponent() to create a new "example-data-visualizer" component
 - "Message Broker Example": calls the function to publish() which belongs to "message-broker" component, to change content on each example-data-visualizer component, which must contain a subscribe() function, using message-broker component. It must also trigger a notification to the user, using toast-manager component.

## What you’ll need
About 15-20 minutes

All prerequistes from tutorial 1

A text editor or IDE (for Example Atom)

&nbsp;
## How to complete this tutorial
In order to complete this tutorial, you need to complete OWC Tutorial 1

[OWC Tutorial 1 link](https://github.com/SentinelDataHubOWC/documentation/blob/master/new-component-step-by-step.md)

&nbsp;
## Let's Roll!
We start creating an Open Web Component. If you need more information please visit [OWC Tutorial 1 link](https://github.com/SentinelDataHubOWC/documentation/blob/master/new-component-step-by-step.md)

### Create example-data-visualizer Component
Create an empty new component using the python script.

name: **example-data-visualizer**

class: **ExampleDataVisualizer**

&nbsp;
### Modify example-data-visualizer.html
**Purpose:** Modify the template html file to create some buttons and a basic dynamic content.

**Modify** *example-data-visualizer.html* with a custom html template, containing a dynamic string and 3 buttons

```
<template>
    <br>
    <p id="cont">{{content}}</p>
    <br>
    <button on-click="OnModifyContent">MODIFY</button>
    <br>
    <button on-click="OnReset">RESET</button>
    <br>
    <button on-click="OnRedify">RED</button>
    <br>
</template>
```
The paragraph {{content}} is a string, modifiable inside the same component by clicking one of the three buttons.

&nbsp;
Now we add a model to beforeRegister() function. In this case the model is represented by a simple string

We'll also create a reference to the owc.app component

**Modify** BeforeRegister() function
```
beforeRegister() {
	this.is = 'example-data-visualizer';
    this.owcApp = document.querySelector('#owc-app');

    // model
    this.properties = {
    /**
     * UI variable: If it is true, the list is hidden. If it is false, the list is shown.
     * This property is mainly used to hide the empty list in case of 0 results
     *
     * @type {Boolean}
    */
		content: {
    	type: String,
    	value: "default",
    	notify: true
        }
    }
}
```
&nbsp;
**Modify** ExampleDataVisualizer Class adding functions called 'on-click' for each button

The example-data-visualizer will show a message to user, exploiting the toast-manager component, when the "RESET" button is clicked

```
class ExampleDataVisualizer {

...
OnModifyContent(){
 this.content ="modified message";
}

OnReset(){
 this.content = "default"
 this.$$('#cont').style.color = "black";
 this.owcApp.toastManager.warn(this.i18n("Data Reset On Data Visualizer"));
}

OnRedify(){
 this.$$('#cont').style.color = "red";
}
```

### Test Open Web Components
To test the Open web components, execute:
```
user@example:~/owc/dhus-owc-master/src/main/frontend$ gulp serve:dist
```
---
## Part2

### Create and handle an existing Open Web Component from another one
In this part we'll create another component, called "example.data.source". The purpose of this part is to create (push) the existing "example-data-visualizer" in the navigation system.

Then, using the message-broker component, we will "publish" a method inside the example-data-source which modifies a string, and then we'll "subscribe" to the same method on example-data-visualizer. This will modify all {{content}} fields on each example-data-visualizer component in the navigation system.

The example-data-source will show a message to user, exploiting the toast-manager component

### Delete example-data-visualizer pushComponent
In order to proceed, first **delete** these 2 lines in owc.app:

~~var exampleDataVisualizer = document.createElement('example-data-visualizer');
self.navigationManager.pushComponent(exampleDataVisualizer, "300px", "Source", true, true, 5)~~

The component "example-data-visualizer" will be pushed by the new component.

&nbsp;
### Create example-data-source Component
Create an empty new component using the python script.

name: **example-data-source**

class: **ExampleDataSource**

&nbsp;
### Modify example-data-source.html
**Modify** example-data-source.html **adding** a new template
```
<template>
  <br>
  <button on-click="OnCreateVisualizer">Create Data Visualizer</button>
  <br>
  <button on-click="OnMsgBroker">Message Broker Example</button>
</template>
```
The first button will call a function, as the one in owc.app, which pushes a new component on the UI
The second one will call a function to _modify_ all the content on all specific instances of the "example-data-visualizer" component

&nbsp;
**Modify** example-data-source.html **adding** these functions in ExampleDataSource Class
```
class ExampleDataSource {
...
        beforeRegister() {
        this.is = 'example-data-source';
        this.owcApp = document.querySelector('#owc-app');
      }

      OnCreateVisualizer(){
        var newComponent = document.createElement('example-data-visualizer') ;
        this.owcApp.navigationManager.pushComponent(newComponent, "400px", "Example Data 				Visualizer Component", true, true, 5);
       }

       OnMsgBroker(){
        this.owcApp.messageBroker.publish('hello from Data Source Component', 'newString');
        this.owcApp.toastManager.info(this.i18n("Data Changed via Data Source Component"));
       }
...
}
```
Note that you can remove last created Component pressing "Escape" button.
&nbsp;
**Modify** ExampleDataVisualizer Class adding **attached()** function
```
attached(){
	var self = this
    this.owcApp.messageBroker.subscribe('hello from Data Source Component',function(cMessage){
      self.content = cMessage;
    });
}
```
The "example-data-visualizer" component is using the messagge-broker component to subscribe to a specific event
&nbsp;
### Test Open Web Components
You can now finally test the Open web components, execute:
```
user@example:~/owc/dhus-owc-master/src/main/frontend$ gulp serve:dist
```

&nbsp;
## Summary
Congratulations! You built two OWC Components. 
The first one can modify its own content locally. 
The second one, using **navigation-manager** component, can push the first component in the navigation system.

Finally, the second component can modify all first components' content with a single command, using **message-broker** component.

All Notification in the Bottom bar are done using **toast-manager** component.
