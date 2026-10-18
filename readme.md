# Application Activity Events using Apache Kafka

## 1. Project Objective

The objective of this project is to understand how Apache Kafka is used to send and receive application activity events.

In this project, a website generates events when users log in, search, or make a purchase. The producer sends these events to a Kafka topic, and the consumer reads and processes them.

The consumer converts the event type and device values to uppercase, counts the different event types, handles incomplete messages, and displays the Kafka offset for each consumed message.

## 2. Technologies Used

- Python
- Apache Kafka
- Docker Desktop and Docker Compose
- Jupyter Notebook
- `kafka-python` library
- JSON

## 3. Kafka Topic

**Topic name:** `app-events-piyush`

The producer sends application activity messages to this topic, and the consumer reads messages from it.

## 4. Project Files

### `docker-compose.yml`
This file contains the Kafka configuration. I use Docker Compose to start the Kafka broker locally.

### `environment_check.ipynb`
This notebook checks whether Python, the `kafka-python` library, and Docker are available in my environment.

### `producer.ipynb`
This notebook creates a Kafka producer and sends 10 valid application activity messages to the `app-events-piyush` topic. It also sends one incomplete message without an `event_id` to test invalid-message handling.

### `consumer.ipynb`
This notebook creates a Kafka consumer and processes the messages received from the topic. It parses JSON, checks required fields, converts `event_type` and `device` to uppercase, counts LOGIN, SEARCH, and PURCHASE events, and displays the partition and offset of each consumed message.

## 5. Input Message Format

The messages are written in JSON format. A sample message is:

```json
{
  "event_id": "E101",
  "user_id": "U101",
  "event_type": "login",
  "device": "Mobile"
}
```

The project uses three valid event types:

- LOGIN
- SEARCH
- PURCHASE

The producer sends the event type in lowercase so that the consumer can standardize it to uppercase.

## 6. How the Project Works

1. I start the Kafka environment using Docker Compose.
2. I check that the required Kafka topic is available.
3. I start the consumer notebook so it can wait for new messages.
4. I run the producer notebook to publish the sample messages.
5. The consumer reads and parses the messages.
6. The consumer validates required fields and converts the event type and device to uppercase.
7. Valid events are counted by type, while incomplete or invalid messages are reported.
8. The consumer displays the partition and offset of each consumed message and prints the final summary.

## 7. Expected Output

After processing 10 valid messages and one incomplete message, the expected summary is:

```text
--- FINAL SUMMARY ---
Messages processed: 11
Invalid messages: 1
Event counts: {'LOGIN': 4, 'SEARCH': 3, 'PURCHASE': 3}
```

The event counts are based on the 10 valid sample messages. The incomplete message is counted as invalid and is not included in the event-type counts.

The actual partition and offset values may differ between runs.

## 8. Kafka Offset

The consumer displays the partition and offset for each consumed message.

A partition is a section of a Kafka topic where records are stored. An offset identifies a record's position within that partition. It helps identify and track the records consumed by the application.

## 9. Setup and Run Instructions

### Prerequisites

- Python installed
- Docker Desktop installed and running
- Jupyter Notebook support in VS Code
- `kafka-python` installed in the Python environment

### Step 1: Open the project folder

Open the project folder in VS Code and select the Python environment in which the required library is installed.

### Step 2: Check the environment

Open `environment_check.ipynb` and run all cells to check Python, `kafka-python`, and Docker.

### Step 3: Start Kafka

Open the VS Code terminal in the project folder and run:

```powershell
docker compose up -d
```

Check the running container:

```powershell
docker compose ps
```

### Step 4: Check the Kafka topic

Run this command:

```powershell
docker compose exec kafka /opt/kafka/bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

Confirm that `app-events-piyush` appears in the topic list.

The Kafka configuration automatically creates the topic when the producer first sends a message.

### Step 5: Start the consumer

Open `consumer.ipynb` in VS Code.

Run the import cell and the consumer setup cell. Wait until the notebook displays:

```text
Consumer is ready. Run the producer now.
```

Keep the consumer cell running while publishing messages.

### Step 6: Run the producer

Open `producer.ipynb` in VS Code.

Run the cells in order:

1. Import the required libraries.
2. Create the Kafka producer.
3. Prepare the sample events, including one incomplete message.
4. Send the messages to the Kafka topic.

The output should show that 11 messages were sent, including the incomplete test message.

### Step 7: Check the consumer output

Return to the running consumer notebook. It should display the consumed messages, their partition and offset, normalized event details, and the invalid-message report.

After processing the messages, check the final summary for the number of messages processed, invalid messages, and counts for LOGIN, SEARCH, and PURCHASE.

### Step 8: Save evidence

Capture screenshots showing:

- Kafka running and the topic available
- Producer publishing the messages
- Consumer displaying normalized events
- Invalid-message handling
- Partition and offset information
- Final event counts

## 10. Conclusion

Through this project, I learned how to use Apache Kafka to publish and consume application activity events. I also practiced JSON processing, event validation, uppercase standardization, event counting, and reading Kafka offsets.

The project demonstrates the basic producer-consumer workflow using a local Kafka environment.