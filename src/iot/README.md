# IoT on Android

Running `node-red` on Android as a test IoT device.

## Setting up development environment

Follow the instructions [here](https://nodered.org/docs/platforms/android) to set up the Termux app and to set up `node-red` on the Android device. Connect to the `node-red` server via wi-fi at `<yourandroidipaddress>:1880`.

Install the `termux:api` app and package as well in order to access the device sensors and capabilities. The API information is described [here](https://termux.com/add-on-api.html).

## Getting IoT Packages

Install the `node-red-contrib-azureiothubnode` package from the `Manage Palettte` option to connect to the Azure IoT Hub.

# Using Device Twins

* Set up npm packages for twin. From `termux` prompt
    * `cd .node-red`
    * `npm install -g azure-iot-device`
    * `npm install -g azure-iot-device-mqtt`
* Add packages to global settings
    * `nano settings.js`
    * Find the `functionGlobalContext` section (`vol-down W` to search for the text)
    * Add in two lines after the function call to this:
``` 
functionGlobalContext: {
    azureiotdevice:require('azure-iot-device'),
    azuremqtt:require('azure-iot-device-mqtt')
}   
```
* Now the code from the [Microsoft examples](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-node-node-twin-getstarted) can be used with the following modifications:

    * The `require(azure-iot-device)` code is replaced with `global.get('azureiotdevice')` and the same for the other require.
    
    * Note that the `process.exit()` line should be cut from the device sample.

    * Change all of the `console` references to `node`.

## Monitoring Device Twins

The `Device Explorer` has a device twin monitor that makes it much easier to adjust and monitor device twin behavior.

## Monitoring Device Twins from Web App

The next step is generating a web app that runs and monitors the device twins and gathers the predicitions from the ML endpoint.

### Setting up the Node-red web app

I used a github repository to set up the web app:
> [https://github.com/jmservera/node-red-azure-webapp](https://github.com/jmservera/node-red-azure-webapp)

I deployed the web app as a free `F1` instance. In order to update the security on the web app administrator, I used the `Kudu` developer tools (under the `Development Tools/Advanced Tools` blade). Following the instructions from [here](http://nodered.org/docs/security.html), first create the password hash in the console. Then edit the `settings.js` file (in the `home\site\wwwroot\` directory), adding the admin username and password hash.

Install the npm module for Azure Iot Hub: `npm install azure-iothub --save` from the same directory.

Update the `settings.js` file to allow for global access to the `azure-iothub` module.

## Accessing ML Prediction Endpoint

In order to access the Azure ML prediction endpoint, use the HTTP node, following the setup instructions [here](https://blogs.msdn.microsoft.com/bigdatasupport/2016/02/18/how-to-call-a-azure-machine-learning-web-service-from-nodejs/).


