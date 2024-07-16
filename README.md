Digital Brain Technologies Tasks

TASK 1

Step 1: Install Docker and Docker Compose

Step 2: Create a docker-compose.yml file to run Kafka and Zookeeper.
This Docker Compose file starts Zookeeper and Kafka. Zookeeper opens to the outside world on port 2181, while Kafka opens to the outside world on port 9092.

Step 3: Starting Kafka and Zookeeper: Start Kafka and Zookeeper by running your Docker Compose file in its directory in the terminal or command client.
-> docker-compose up -d

Step 4: Checking if Kafka is Working: After Kafka and Zookeeper are started, check if Kafka is running properly.
-> docker-compose ps

Step 5: Connecting to Kafka Container:
First, you need to connect to the Kafka container from terminal or command client. In this step, connect to the Kafka service you started with Docker Compose.
-> docker-compose exec kafka bash
This command connects you to the Kafka container via the bash shell.

Step 6: Creating a Topic Using Kafka CLI.
Create a new topic using Kafka CLI. For example, you can use the following command to create a topic named my-topic.
-> kafka-topics.sh --create --topic my-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

-create: The parameter used to create a new topic
-topic my-topic: The name of the topic to be created (you can use any name instead of my-topic).
-bootstrap-server localhost:9092: The address and port needed to connect to the Kafka broker. When started with Docker Compose, it is usually localhost:9092.
-partitions 1: The number of partitions for the topic.
-replication-factor 1: The replication factor for the topic (in this example, only one replica is used).

Step 7: Send message to kafka topic using kafka cli command.
To consume messages from the my-topic topic, run the following command:
-> kafka-console-consumer.sh --topic my-topic --from-beginning --bootstrap-server localhost:9092
This command will read all messages from the beginning of the my-topic topic.

Step 8: Viewing Messages
After running this command, you should be able to see the messages you previously sent displayed in the terminal.
>Hello, Kafka!
>This is a test message.

Step 9: Exiting the Kafka Container
By following these steps, you can verify and consume the messages you sent to the Kafka topic.
->exit

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













