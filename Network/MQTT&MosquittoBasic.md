#Simple JS demo  

```
// Create a client instance
client = new Paho.MQTT.Client(location.hostname, Number(location.port), "clientId");

// set callback handlers
client.onConnectionLost = onConnectionLost;
client.onMessageArrived = onMessageArrived;

// connect the client
client.connect({onSuccess:onConnect});


// called when the client connects
function onConnect() {
  // Once a connection has been made, make a subscription and send a message.
  console.log("onConnect");
  client.subscribe("/World");
  message = new Paho.MQTT.Message("Hello");
  message.destinationName = "/World";
  client.send(message);
}

// called when the client loses its connection
function onConnectionLost(responseObject) {
  if (responseObject.errorCode !== 0) {
    console.log("onConnectionLost:"+responseObject.errorMessage);
  }
}

// called when a message arrives
function onMessageArrived(message) {
  console.log("onMessageArrived:"+message.payloadString);
}
```
(https://eclipse.org/paho/clients/js/)  

Include mqttws31.js library in your project:  
https://raw.githubusercontent.com/eclipse/paho.mqtt.javascript/master/src/mqttws31.js  

##Installing Mosquitto on a PC  
1. Download mosquitto setup file from   http://www.eclipse.org/downloads/download.php?file=/mosquitto/binary/win32/mosquitto-1.4.10-install-win32.exe  
2. Run the setup file and follow the steps to complete the installation.  
3. Download and Install Win32 OpenSSL v1.0.2h from here http://slproweb.com/products/Win32OpenSSL.html and copy libeay32.dll and ssleay32.dll to the mosquitto root.  
4. Download pthreadVC2.dll from ftp://sources.redhat.com/pub/pthreads-win32/dll-latest/dll/x86/ and copy it to mosquitto root.  
5. Testing: Open command prompt and run the following commands under mosquitto root:  
```
mosquitto.exe -v -c mosquitto.conf
```

# Use remote Mosquitto server  
```
client = new Paho.MQTT.Client("broker.hivemq.com", 8000, "uniqueId");
```
