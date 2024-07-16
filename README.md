Digital Brain Technologies ML Engineer Task

TASK 1

Step 1: Install Docker and Docker Compose

Step 2: Create a Docker Compose File
Create a file named docker-compose.yml. This Docker Compose file starts Zookeeper and Kafka. Zookeeper opens to the outside world on port 2181, while Kafka opens to the outside world on port 9092.

Step 3: Starting Kafka and Zookeeper
Start Kafka and Zookeeper by running your Docker Compose file in its directory in the terminal or command client.

-> docker-compose up -d

Step 4: Checking if Kafka is Working
After Kafka and Zookeeper are started, check if Kafka is running properly.

-> docker-compose ps

Step 5: Connecting to Kafka Container
First, you need to connect to the Kafka container from terminal or command client. In this step, connect to the Kafka service you started with Docker Compose.

-> docker-compose exec kafka bash
This command connects you to the Kafka container via the bash shell.

Step 6: Creating a Topic Using Kafka CLI
Create a new topic using Kafka CLI. For example, you can use the following command to create a topic named my-topic.

-> kafka-topics.sh --create --topic my-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

create: The parameter used to create a new topic

topic my-topic: The name of the topic to be created (you can use any name instead of my-topic).

bootstrap-server localhost:9092: The address and port needed to connect to the Kafka broker. 
When started with Docker Compose, it is usually localhost:9092.

partitions 1: The number of partitions for the topic.

replication-factor 1: The replication factor for the topic (in this example, only one replica is used).

Step 7: Send Message to Kafka Topic Using Kafka CLI Command
To consume messages from the my-topic topic, run the following command:

-> kafka-console-consumer.sh --topic my-topic --from-beginning --bootstrap-server localhost:9092
This command will read all messages from the beginning of the my-topic topic.

Step 8: Viewing Messages
After running this command, you should be able to see the messages you previously sent displayed in the terminal.

>Hello, Kafka!

>This is a test message.


Step 9: Exiting the Kafka Container
By following these steps, you can verify and consume the messages you sent to the Kafka topic.

-> exit

Step 10: Listening to Messages Produced on a Topic Using Kafka CLI Command
First, you need to connect to the Kafka container from your terminal or command prompt:

-> docker-compose exec kafka bash

Step 11: Consume Messages Using the Kafka Consumer CLI Command
To consume and listen to messages from the my-topic topic, run the following command:

-> kafka-console-consumer.sh --topic my-topic --bootstrap-server localhost:9092 --from-beginning

This command will read all messages from the beginning of the my-topic topic. If you only want to listen to new incoming messages, you can remove the --from-beginning parameter.

-> kafka-console-consumer.sh --topic my-topic --bootstrap-server localhost:9092

Step 12: Listening to Messages
When you run this command, you will see all messages coming into the my-topic topic displayed in the terminal. As long as this command is running, new messages will be displayed in real-time.

Step 13: Stop Sending Messages
When you are done sending messages, press Ctrl+C. This will terminate the Kafka producer session and return you to the Kafka container's bash shell.

Step 14: Exit the Container
If you want to completely exit the Kafka container, use the exit command:

-> exit

TASK 2

1) Importing Libraries
   
These libraries are imported to support various functionalities in the script:
requests: Used to make HTTP requests to fetch web pages.

BeautifulSoup (from bs4): Enables parsing of HTML fetched from web pages.

KafkaProducer (from kafka): Facilitates sending messages (product data) to a Kafka topic.

json: Handles encoding and decoding of JSON data.

time: Provides functionality to introduce delays in code execution.

matplotlib.pyplot: Allows for data visualization, specifically used here to plot product prices.

2) Scraping Data from a Website
This part of the script fetches HTML content from "https://scrapeme.live/shop/" and parses it using BeautifulSoup. This allows extraction of product names and prices from the webpage's HTML structure.

3) Setting Up Kafka Producer
The Kafka producer is initialized to connect to a Kafka broker running locally on localhost:9092. It's configured to serialize Python dictionaries into JSON format (value_serializer=lambda v: json.dumps(v).encode('utf-8')) before sending them to the Kafka topic named product_data.

4) Fetching Product Data
Product names and prices are extracted from the parsed HTML. Up to 100 products are fetched, with prices converted to float after removing currency symbols and commas.

5) Sending Data to Kafka
Each product's data (name and price) is sent to the Kafka topic product_data. A 1-second delay (time.sleep(1)) is added between each message to regulate the rate of data transmission. producer.flush() ensures all pending messages are sent to Kafka before proceeding.

6) Consuming Data from Kafka
A Kafka consumer is configured to read messages from the product_data topic. It retrieves up to 100 messages, ensuring correct decoding of JSON data (value_deserializer=lambda x: json.loads(x.decode('utf-8'))), and writes these messages to a local file named products.json. This process handles potential encoding issues with prices.

7) Plotting the Data
Using Matplotlib, a horizontal bar chart is generated to visualize product prices fetched earlier. Product names are plotted on the y-axis and corresponding prices on the x-axis. This chart provides a quick visual overview of product pricing data.

8) Checking File Size
The script checks the size of the products.json file and prints its size in bytes (file_size). This step helps verify the amount of data written to the file.

9) Setting Up a Flask API
The Flask application sets up a simple REST API with two endpoints:
   Home endpoint (/): Displays a welcome message directing users to the /products endpoint for 
   accessing the product list.

   Products endpoint (/products): Retrieves product data from products.json and returns it as 
   JSON. If the file doesn't exist, it returns a JSON error message with a 404 status code.

SUMMARY

Scrapes product data from a website.

Sends the data to a Kafka topic.

Consumes data from the Kafka topic and writes it to a JSON file.

Plots the product prices.

Checks the JSON file size.

Serves the product data through a Flask API.


TASK 3

Step 1: Update Docker Compose File
Open your docker-compose.yml file and add the jupyter service.
    
    jupyter:
    image: python:3.9-slim
    ports:
      - "8888:8888"
    volumes:
      - ./notebooks:/notebooks  # Mount host ./notebooks directory to /notebooks in container
    command: >
      bash -c "pip install jupyter && jupyter notebook --ip='0.0.0.0' --port=8888 --no-browser --allow-root"
    depends_on:
      - kafka      

Step 2: Explanation of Docker Compose Sections

image: Specifies the base Docker image for the Jupyter service. Here, it uses python:3.9-slim.

ports: Maps port 8888 on the host to port 8888 inside the container, allowing access to the Jupyter Notebook web interface.

volumes: Mounts the ./notebooks directory from the host machine to /notebooks inside the container. This allows the Jupyter server to access notebooks stored on your host machine and persist changes.

command: Defines the command to run when the container starts. It installs Jupyter (pip install jupyter) and then starts the Jupyter Notebook server (jupyter notebook --ip='0.0.0.0' --port=8888 --no-browser --allow-root). This command ensures that Jupyter is installed and configured properly within the container.

depends_on: Specifies that the jupyter service depends on the kafka service. Docker Compose will start the kafka service first before starting jupyter, ensuring that kafka is ready for interaction when jupyter starts.


Step 3: Start Docker Containers
Save your docker-compose.yml file after making changes. Open a terminal or command prompt and navigate to the directory where your docker-compose.yml file is located.

Run the following command to start Docker Compose and all defined services:

-> docker-compose up

Step 4: Access Jupyter Notebook
Once Docker Compose has started all services, open your web browser. Navigate to http://localhost:8888. You should see the Jupyter Notebook interface.

If you have notebooks in the ./notebooks directory on your host machine, they will appear in the Jupyter interface under /notebooks.

Step 5: Finish and Cleanup
When you're done working with Jupyter Notebook, stop Docker Compose. In the terminal where Docker Compose is running, press Ctrl + C.

Docker Compose will stop all running containers while keeping your data and configurations intact (./notebooks directory remains unchanged).


