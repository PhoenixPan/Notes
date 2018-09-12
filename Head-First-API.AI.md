# Head-First-API.AI

## Introduction
We will build a simple project that gives user information upon requests. 

We won't cover the very basic processes, as there are super good tutorial on the official website. However, I do like to elaborate some key concepts here.

### Entity
Entity is a collection of things(usually nouns) belongs to a category. For example:  
_@pizza-top: pepperoni, bacon, ground beef, mushroom_  
_@hotel: Marriott, Hilton, IHG, Hyatt, Four Seasons_  
Entity is used to extract key values from natural language. Rather than creating your own entities, there are system entities ready to use, such as city and date. Check the full list [here](https://docs.api.ai/docs/concept-entities#entity-types)  

### Intents
Use Example is better than template.  
#### Follow-up intents
Simple follow-up intents can be replaced with multiple REQUIRED actions.

### Text Response
use $parameter_name to refer the keyword mentioned by the user.  

### Fulfillment - Webhook
Must use HTTPS.  

## Demo Server
In this project, I use Undertow, a light framework for web server. I used Eclipse IDE, with Maven plug-in installed. 

1. To build the demo project, we firstly create a Maven project with default settings. Then we put this test code in App.java of the project:
    ```
    package accenture.accenture;

    import io.undertow.Undertow;
    import io.undertow.server.*;
    import io.undertow.util.Headers;

    public class App {
        public static void main(final String[] args) {
        Undertow server = Undertow.builder()
              .addHttpListener(80,"0.0.0.0")
          .setHandler(new HttpHandler() {

              public void handleRequest(final HttpServerExchange exchange)
                throws Exception {
            exchange.getResponseHeaders().put(Headers.CONTENT_TYPE,
              "text/plain");
            exchange.getResponseSender().send("Hello World");
              }
          }).build();
        server.start();

        }
    }
    ```

2. We also need to put Undertow dependencies into the pom.xml of the project. You can find the latest verion [here](http://undertow.io/downloads.html)
    ```
    <dependency>
        <groupId>io.undertow</groupId>
        <artifactId>undertow-core</artifactId>
        <version>1.4.12.Final</version>
    </dependency>

    <dependency>
        <groupId>io.undertow</groupId>
        <artifactId>undertow-servlet</artifactId>
        <version>1.4.12.Final</version>
    </dependency>

    <dependency>
        <groupId>io.undertow</groupId>
        <artifactId>undertow-websockets-jsr</artifactId>
        <version>1.4.12.Final</version>
    </dependency>
    ```

3. After we done with this, enter the file location of the project using command line. Then we need to install all dependencies we need and clean the project by entering:
    ```
    mvn install
    mvn clean
    ```

4. Restart Eclipse, and now we should be able to run the project and see "Hello world" in "localhost:80"
5. Export the maven project as zip file
6. Create an instance of Ubuntu 16.04 on EC2 and move the zip file to the instance
7. Install necessary tools. I can't believe the Ubuntu instance has no unzip and jdk! 
    ```
    sudo apt-get install unzip
    sudo apt-get install maven
    sudo apt-get install openjdk-8-jdk
    ```
8. After the installation, run these two commands to launch the server
    ```
    mvn compile
    sudo mvn -e exec:java -Dexec.mainClass="accenture.accenture.App" 
    ```
9. We can now see "hello world" on "instance-public-dns:80"

## Get the real deal
