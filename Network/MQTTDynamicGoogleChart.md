# MQTT Subscriber - .js file
```
// Create a client instance
client = new Paho.MQTT.Client("broker.hivemq.com", 8000, "jiabeipsubscriber");

// set callback handlers
client.onConnectionLost = onConnectionLost;
client.onMessageArrived = onMessageArrived;

// connect the client
client.connect({onSuccess:onConnect});

// called when the client connects
function onConnect() {
  // Make the connection but subscribe no channels
  console.log("Connected to MQTT broker");
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
  // Append the received message to the html element "demo"
  elements = message.payloadString;
  elements = elements.split(':');
  document.getElementById("demo").innerHTML = document.getElementById("demo").innerHTML + "<br>" + "Temperature: " + elements[0] + " F, Timestamp: " + elements[1];
  // Append the received message to contents, which used to draw the chart
  if (timestamp == 0) {
    content[count] = [count + '', timestamp, parseInt(elements[0]), parseInt(elements[0])]
    timestamp = parseInt(elements[1])
  }
  else {
    content[count] = [count + '', (parseInt(elements[1]) - timestamp)/1000, parseInt(elements[0]), parseInt(elements[0])]
  }
  drawSeriesChart();
  count++;
}
```
# MQTT Subscriber - .html file
```
<!DOCTYPE html>
<html>
    <head>
        <!-- Import .js libraries -->
        <script type="text/javascript" src="mqttws31.js"></script>
        <script type="text/javascript" language="javascript" src="client.js"></script>
        <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
        <script type="text/javascript">
            // Initialzie Google charts
            google.charts.load('current', {'packages': ['corechart']});
            var count = 1         // Used to trace the number of tuples received
            var timestamp = 0     // Used to record time
            var content = [];     // Contents based on which the chart would be drawn 
            // Initialize the columns of the chart. The second temperature is used to activate different colors
            content[0] = ['ID', 'Timestamp', 'Temperature','Temperature']; 
            
            // A function used to draw the char
            function drawSeriesChart() {
                // Load data with temperature contents
                var data = google.visualization.arrayToDataTable(
                    content
                );
                // Configurations of the chart
                var options = {
                    title: 'Temperature Change within the Given Scope',
                    hAxis: {title: 'Timestamp'},
                    vAxis: {title: 'Temperature'},
                    bubble: {textStyle: {fontSize: 8}},
                    colorAxis: {colors: ['lightblue', 'red']},
                    sizeAxis:{maxSize:10,minSize:5}
                };
                // Draw the chart and display it in 'series_chart_div'
                chart = new google.visualization.BubbleChart(document.getElementById('series_chart_div'));
                chart.draw(data, options);
            }
        </script>
        <script>
            // Subscribe all three temperature channels
            function subscribe_all() {
                client.subscribe("/pittsburgh/coldTemps");
                client.subscribe("/pittsburgh/niceTemps");
                client.subscribe("/pittsburgh/hotTemps");
                document.getElementById("hint").innerHTML = "Start to receive all temperatures:" // Change hint
            }

            // Subscribe only the cold temperature channel
            function subscribe_cold() {
                client.subscribe("/pittsburgh/coldTemps");
                document.getElementById("hint").innerHTML = "Start to receive cold temperatures equal to or lower than 45 F:" // Change hint
            }

            // Subscribe only the hot temperature channel
            function subscribe_hot() {
                client.subscribe("/pittsburgh/hotTemps");
                document.getElementById("hint").innerHTML = "Start to receive hot temperatures equal to or higher than 80 F:" // Change hint
            }

            // Subscribe only the nice temperature channel
            function subscribe_nice() {
                client.subscribe("/pittsburgh/niceTemps");
                document.getElementById("hint").innerHTML = "Start to receive nice temperatures between 45 F and 80 F:" // Change hint
            }
        </script>
        <!-- Styles to make the page more readable -->
        <style>
            h1, h2, h3, p {
                font-family: "Times New Roman", Times, serif
            }
            h1, h3 {
                padding: 10px;
                background-color: lightblue;
            }
            button {
                width: 300px;
                text-align: center;
                margin: 5px;
                cursor: pointer;
            }
        </style>
    </head>
    <body>
        <!-- Hint for users to pick a topic -->
        <h1>Choose one topic to subscribe:</h1>

        <!-- Four buttons corresponding to four types of subscriptions -->
        <button onclick="subscribe_all()">Subscribe All Temperature</button><br />
        <button onclick="subscribe_cold()">Subscribe Cold Temperature</button><br />
        <button onclick="subscribe_hot()">Subscribe Hot Temperature</button><br />
        <button onclick="subscribe_nice()">Subscribe Nice Temperature</button><br />

        <!-- Display channel subscription condition -->
        <h2 id="hint"></h2>
        
        <!-- Display and explain the Google chart -->
        <h3>Google Chart:</h3>
        <div id="series_chart_div" style="width: 900px; height: 500px;"></div>
        <p>The X axis of the chart is time elapsed (timestamp) and the Y axis is the temperature.</p>
        <p>Since the time is already increasing, the new dot will always appear on the right side.</p>
        <p>The changing color makes it easier for the viewer to read the temperatures :) </p>
        
        <!-- Display text content -->
        <h3>Data Received:</h3>
        <p id="demo"></p>

    </body>
</html>
```
# MQTT Publisher - .java file
```
import java.util.Random;
import org.eclipse.paho.client.mqttv3.MqttClient;
import org.eclipse.paho.client.mqttv3.MqttConnectOptions;
import org.eclipse.paho.client.mqttv3.MqttException;
import org.eclipse.paho.client.mqttv3.MqttMessage;
import org.eclipse.paho.client.mqttv3.persist.MemoryPersistence;
/**
 * A publisher that publishes a random temperature between 0 and 100 every five seconds.
 * @author Phoenix
 */
public class TemperatureSensor {

    /**
     * Main method of the class.
     * @param args 
     */
    public static void main(String[] args) {
        // Initialize parameters for connections
        int qos = 2;
        String host = "tcp://broker.hivemq.com:1883";
        String clientId = "jiabeippublish";
        MemoryPersistence persistence = new MemoryPersistence();

        try {
            // Establish connection to the remote Mosquitto broker using unique id
            MqttClient client = new MqttClient(host, clientId, persistence);
            MqttConnectOptions connOpts = new MqttConnectOptions();
            connOpts.setCleanSession(true);
            client.connect(connOpts);
            System.out.println("Connected to " + host);
            
            // Initialize topics
            String topicCold = "/pittsburgh/coldTemps";
            String topicNice = "/pittsburgh/niceTemps";
            String topicHot = "/pittsburgh/hotTemps";

            while (true) {
                // Generate a random number betwen 0 - 100
                Random random = new Random();
                int temperature = random.nextInt(101);
                long timestamp = System.currentTimeMillis();
                
                // Compose the number into a message
                String content = temperature + ":" + timestamp;
                MqttMessage message = new MqttMessage(content.getBytes());
                message.setQos(qos);
                
                // Publish temperatures to different topics 
                if (temperature <= 45) {
                    client.publish(topicCold, message);
                } else if (temperature >= 80) {
                    client.publish(topicHot, message);
                } else {
                    client.publish(topicNice, message);
                }
                System.out.println("Message published");
                
                // Wait for 5 seconds before publishing next message
                try {   
                Thread.sleep(5000);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        } catch (MqttException me) {
            // Give out hints if exception raises
            System.out.println("reason " + me.getReasonCode());
            System.out.println("msg " + me.getMessage());
            System.out.println("loc " + me.getLocalizedMessage());
            System.out.println("cause " + me.getCause());
            System.out.println("excep " + me);
            me.printStackTrace();
        }
    }
}
```
