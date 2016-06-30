# Azure IoT Hub ping #
Simple "ping" solution to help validate a device connectivity to Azure IoT Hub.
The solution consists in an Azure IoT Hub and an Azure Function deployed at once. The Azure Function is triggered when a new message is received on the IoT Hub and will send the same message content back to the device that sent it.
The project contains all the code and deployment configuration needed for the solution.
It also contains a simple device sample written in JavaScript for Node.js.

## Deploy the ping solution ##

Deployment of the solution simply consists in clicking on the below button.

<a href="https://azuredeploy.net/" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
 
Once you have clicked on the button, you will be asked to login with your Azure subscription's credentials.
When you are logged in, you will be prompted for entering parameters for the solution:
  - Directory: Your Azure acount is bound to an Azure Active Directory. Usually the default one is the only one, but in some cases, you might be using several, in which case you can select the one you want your solution to be secured with.
  - Subscription: this is the subscription you want the services to be deployed to if you have several Azure subscriptions.
  - Resource Group/Resource Group Name: The services will be deployed and grouped under the specified resource group allowing a simpler management in the Azure portal. You can choose to create a new one, or to use  an existing resource group.
  - Region: pick the region you want the services to be deployed to.
  - Choose a solution name: **Important**, the solution name should be all lower case, only ascii caracters and no longer that 16 characters.
  - IoT Hub Sku: select the Sku you want for the IoT Hub : F1 (Free), S1 or S2. Note that you can only have 1 instance of F1 per Azure subscription.
  - Funciton Sku: select the Sku you wnat for the Azure Function.


![][1]

Hit **Next**
The wizard checks the selected parameters and lists the services it will deploy:
  - Microsoft.Storage is the an Azure storage service required by both IoT Hub and Function
  - Microsoft.Devices is the Azure IoT Hub service
  - Website is the Azure Function service.  

![][3]

Hit **Deploy** in the wizard and wait a couple minutes for the services to be deployed and configured.

![][2]

To visualize the newly deployed services in the Azure portal, you can simply click on the "Manage your resources" link displayed in the final screen of the deployment wizard.

Now that the services are deployed, you can look at how you can connect devices to Azure IoT Hub following [instructions from the Azure IoT SDKs repository][manageazureiothub].


## Message format ##

In order for a device to send a ping and receive the response from the solution, it needs to send a JSON message using the following format:

  ```
  {
      "deviceId":"<deviceId>",
      "message":"Hello IoT World"
  }
  ```
Where 'deviceId' is the device's ID as created in the Azure IoT Hub device registry. 
The message the device will receive back will contain the "message" itself.

## Send a ping from the Node device sample ##

Now that you have your IoT Hub ping solution deployed, you can test any of your devices that you are trying to connect to Azure IoT Hub using the [Azure IoT Hub SDKs][azureiotsdks].

The [Azure IoT Hub SDKs][azureiotsdks] are portable and can run on most types of devices out there, but if you want to get started on a device that has been certified for Azure IoT Hub, look into the list of [devices certified for Azure IoT][azureiotcertified].

And if you are looking for some cool cheap device to get started fast, ckeck out the [Azure IoT Starter Kits][azureiotstarterkits].

... Or you can test and play around with the Node.js sample provided in this repository. In order to use this one, follow the below steps:
The prerequisite to run this sample is to have [node.js](http://nodejs.org) installed on the machine you are using and that you have cloned or downloaded the current repository locally.

1. Create a new device Id in the IoT Hub deployed previously and copy its connection string: you will find instructions on how to do this [here][manageazureiothub].
1. Open the file devicesample/simple_sample_device.js
1. Find the below line of code and replace 'connectionstring' with the device's connection string you just copied

  ```
  var connectionString = '<connectionstring>';
  ```

1. Open a command prompt, navigate to the devicesample folder and type the following commands

  ```
  npm install
  node .
  ```

At this point the node sample device will establish a secure connection with Azure IoT Hub, send messages every other second and should receive the message back from IoT Hub.
If you want to get started with Azure IoT Hub, visit [Azure.com/iotdev](http://azure.com/iotdev).

[1]:media/azuredeploy1.png
[2]:media/azuredeploy2.png
[3]:media/azuredeploy3.png
[manageazureiothub]:https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[azureiotsdks]:https://github.com/Azure/azure-iot-sdks
[azureiotstarterkits]:https://azure.microsoft.com/develop/iot/starter-kits/
[azureiotcertified]:https://azure.microsoft.com/en-us/marketplace/certified-iot-partners/