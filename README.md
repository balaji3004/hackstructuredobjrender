[![Build Status](https://travis-ci.org/symphonyoss/extension-api-examples.svg)](https://travis-ci.org/symphonyoss/extension-api-examples)
[![Dependencies](https://www.versioneye.com/user/projects/578010725bb1390040177cb0/badge.svg?style=flat-square)](https://www.versioneye.com/user/projects/578010725bb1390040177cb0#tab-dependencies)

# Structured Object Renderer Example


This repo contains an example of an Extension API app that works as an entity renderer for structured objects. This example is part of the implementation for a PWM demo, in which this application acts as the renderer for messages sent by [Trade Alert Bot](https://github.com/symphonysa/dropwizard-so-starter) . The renderer puts an iframe in the Symphony chat in which the message is received. The data that is received as structured object payload is passed to the iframe as url parameters. This code is a modified version of the [Hello World sample app](https://github.com/symphonyoss/extension-api-examples) with the addition of an iframe as the renderer and the web views that would be rendered by the iframe.


## Prerequisites


* Have `node` and `npm` installed.
* Have port 4000 available as we'll run the demo application on this port.
* If you are planning to use Chrome to run the Hello World app, you will need to first enable the #allow-insecure-localhost flag in Chrome. To do this, go to chrome://flags and enable the setting called "Allow invalid certificates for resources loaded from localhost". 

## Run the example project


* Clone this repo and `cd` into the directory
* Run `npm i webpack -g`
* Run `npm i webpack-dev-server -g`
	* If you have trouble running the webpack-dev-server over HTTPS, check out the --https option described here: https://webpack.github.io/docs/webpack-dev-server.html
* Run `npm i`
* Run `npm run watch`
* Ensure the sample app is running by visiting https://localhost:4000/app.html
* Append "bundle=https://localhost:4000/bundle.json" as a URL param to your pods Symphony address
    * For example https://corporate.symphony.com/client/index.html?bundle=https://localhost:4000/bundle.json
* Accept the "Warning: Unauthorized App(s)" dialog
* Click on the "Applications > App Store" entry in your left nav to install the Hello World app


## Testing

If coupled with the Trade Alert Bot, the bot running should send the structured object messages for this example app to render. But if the intent is just to experiment with entity renderers, follow the following instructions from the Hello World App modified to work with this renderer:

This example app subscribes to the ‘entity’ Service of the Extension API, which allows the application to render Structured Objects. To use the service to render your own Structured Objects, you’ll need to send either a JCurl or Postman POST messagev4 to a room in the pod where you’ll be running the application. Here is an example of a REST payload that can be used to send a custom entity that will be rendered by the app:

```
{
     message: "<messageML><div class='entity' data-entity-id='summary'><b><i>Please install the PWM Demo application to render this entity.</i></b></div></messageML>"
     data: "{"summary": { "type": "com.symphony.fa", "version\":  "1.0", "userEmail":  "manuela.caicedo@symphony.com" }}"
}
```

When the message is sent, the default rendering will be “Please install the PWM Demo application to render this entity.” However, the following function tells the message controller render method to look for messages containing a Structured Object of the type ‘com.symphony.fa’:

`entityService.registerRenderer( "com.symphony.fa",{}, "message:controller");`

When the app is running, that default rendering will be superseded by the custom template in controller.js. For more information about the message format for Structured Objects, see https://rest-api.symphony.com/docs/objects#sending-structured-objects-in-messages.


