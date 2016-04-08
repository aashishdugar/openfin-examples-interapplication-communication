# Openfin Examples: InterApplicationBus
The InterApplicationBus allows for publish/subscribe interactivity between applications.

There are two types of OpenFin windows - applications and child windows. 

##Windows
Child windows 'belong' to the parent application which spawned them and will be closed when the parent application closes. Providing they share the same domain, parent application may access the child window's DOM elements via getNativeWindow(). For example - to access a DOM Element with the id of 'Content' in a child window you may do the following:

```
var win = new fin.desktop.Window([config object],[success callback],[fail callback]);
var content = win.getNativeWindow().document.querySelector('#Content');
content.innerHTML = "<h1>Changed without the InterApplicationBus!</h1>"
```
This does not, however, preclude you from using the InterApplicationBus, it just may not be necessary.

##Applications
An application window, even when spawned by another application, is independent of all other applications. Messaging between OpenFin's applications is handled by a Publish/Subscribe (pub-sub) system where Events are dispatched with a 'topic', a string identifying the type of event, and a 'message', which may be a JavaScript primitive, Object or Array.

##Publish options
There are a number of ways to communicate between windows:

####Publishing 
```
publish(topic, message, callbackopt, errorCallbackopt)
```

This publishes to ALL applications running on the OpenFin runtime who have subscribed to the topic.

####Sending
```
send(destinationUuid, topic, message, callbackopt, errorCallbackopt)
```

This sends only to an application with a specific UUID. The receiving application must also subscribe to the topic to receive results.

####Sending with an optional window name
```
send(destinationUuid, name, topic, message, callbackopt, errorCallbackopt)
```

This sends to a specific named child window of the destination application. The receiving child window must also subscribe to the topic to receive results.

##Subscribe options

####Subscribing to a topic
```
subscribe(senderUuid, topic, listener, callbackopt, errorCallbackopt) 
```

An Application, or child window of an Application, may subscribe to a specific topic. The subscription may be a 'wildcard' subscription, by specifying '*' as the senderUuid, meaning it will receive all bus messages sent on the specific topic, regardless of the publisher's UUID. They will not receive messages form senders who target a UUID which is not their own via the 'send' method.

####Subscribing to a topic as a named window
```
subscribe(senderUuid, name, topic, listener, callbackopt, errorCallbackopt) 
```
A child window may subscribe to a topic which has been specifically targeted at it via the send method's optional 'name' property being set to the child windows UUID. To do this it must specify the sender's UUID as the  'senderUuid' AND 'name' properties when calling the 'subscribe' method. For example - if a child window wishes to receive notifications on a topic 'i-called-your-name' from an application with the UUID of 'App123' it would specify:

```
subscribe(App123, App123, 'i-called-your-name', listener, callbackopt, errorCallbackopt) 
```

The sender with the UUID of 'App123' would then send adding the optional name of the child window to be targeted. 

The subscriber may also set the UUID to a wildcard by specifying '*' as the UUID allowing it to subscribe to sends from any app.

```
subscribe("*", "*", 'i-called-your-name', listener, callbackopt, errorCallbackopt) 
```

##Running the example
To run this example, in a command line terminal, navigate to the root directory (Interapp-bus-windows-and-apps) as shown below:


```
$ npm install
```
NB: on a Mac you may need to type 'sudo npm install'

Navigate to the root folder where 'server.js' resides with your command line tool and run:

```
$ node server
```

This should start a simple Node server at [http://localhost:9098](http://localhost:9098), then, click the link below to install as an openFin app.

If you wish to change to localhost port you will need to change the references in "server.js", "app.json" and in the installer link below.

[installer](https://dl.openfin.co/services/download?fileName=inter-app-api&config=http://localhost:9098/app.json)





