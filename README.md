
# NYC Taxi Cab Data Processing Pipeline

This project demonstrates a scalable data processing pipeline for analyzing NYC Taxi Cab data using graph-based algorithms and real-time streaming. The pipeline integrates **Neo4j**, **Kafka**, and **Kubernetes**, providing insights through graph modeling and analysis using algorithms like **PageRank** and **Breadth-First Search (BFS)**.

___


## Project Phases

### Phase 1: Graph-Based Data Analysis with Neo4j
 - **Goal**: Build a graph database to model NYC Taxi data and implement graph algorithms for analysis.
 - **Steps**:
   - Set up **Neo4j** in a Docker container.
   - Load NYC Taxi data into Neo4j as a graph:
     - **Nodes**: Pickup/Drop-off locations (`Location`).
     - **Relationships**: Trips between locations with properties (distance, fare, timestamps).
   - Implement and execute graph algorithms:
     - **PageRank**: Identify important locations based on trip data.
     - **BFS**: Traverse from one location to another, exploring increasing distances.
 - **Outcome**:
   - Explored graph database concepts and graph algorithms.
   - Gained hands-on experience with data modeling and querying in Neo4j.

---

### **Phase 2: Scalable Real-Time Data Processing**
 - **Goal**: Build a real-time, scalable pipeline for streaming and analyzing NYC Taxi data.
 - **Steps**:
   - Deploy services in **Kubernetes**:
     - **Zookeeper**: Manage Kafka.
     - **Kafka**: Stream data from producers to consumers.
     - **Neo4j**: Store and analyze data as a graph database.
   - Real-time streaming:
     - Use **Kafka Producer** to ingest NYC Taxi data from a Parquet file into Kafka topics.
     - Stream data to **Neo4j** using Kafka-Connect.
   - Implement graph algorithms (PageRank and BFS) in Neo4j for real-time insights.
 - **Outcome**:
   - Built a fault-tolerant pipeline using Kubernetes for scalability.
   - Enabled real-time data ingestion, transformation, and analysis.

---
## Features

- **Graph Modeling**:
  - Represent locations and trips as nodes and relationships.
  - Use graph properties to enhance data analysis.
- **Real-Time Data Streaming**:
  - Ingest NYC Taxi data using Kafka.
  - Process and load data into Neo4j in near real-time.
- **Graph Algorithms**:
  - **PageRank**: Rank important locations based on trips.
  - **BFS**: Efficient traversal between locations.
- **Scalability**:
  - Deploy and manage the pipeline in Kubernetes.
  - Ensure fault tolerance and distributed processing.

## Tech Stack

- **Programming Languages**: Python, YAML
- **Graph Database**: Neo4j (with GDS plugin for advanced graph analysis)
- **Streaming Platform**: Apache Kafka
- **Containerization**: Docker
- **Orchestration**: Kubernetes (Minikube)
- **Libraries**: pandas, PyArrow, confluent_kafka, Neo4j Python Driver

___


## Setup and Installation

### Phase 1: Neo4j in Docker
1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo/project-nyc-taxi.git
   cd project-nyc-taxi
    ```
2. Build the Docker image:
   ```bash
    docker build -t neo4j-graph-analysis .
    ```
3. Run the Docker container
   ```bash
   docker run -d -p 7474:7474 -p 7687:7687 --name neo4j-container neo4j-graph-analysis
    ```
4. Load data into Neo4j using the data_loader.py script:
   ```bash
   python data_loader.py
   ```

### Phase 2: Kubernetes Deployment
1. Start Minikube
   ```bash
   minikube start --cpus=4 --memory=8192
   ```
2. Deploy Zookeeper
   ```bash
   kubectl apply -f zookeeper-setup.yaml
   ```
3. Deploy Kafka
   ```bash
   kubectl apply -f kafka-setup.yaml
   ```
4. Deploy Neo4j
   ```bash
   helm repo add neo4j https://helm.neo4j.com/helm
   helm install neo4j-release neo4j/neo4j -f neo4j-values.yaml
   kubectl apply -f neo4j-service.yaml
   ```
5. Stream data to Kafka
   ```bash
   python data_producer.py
   ```
6. Verify pipeline integration (Port forward services to your local machine):
   ```bash
   kubectl port-forward svc/neo4j-service 7474:7474 7687:7687
   kubectl port-forward svc/kafka-service 9092:9092
   ```
## Usage

- Data Loading:
   - Load NYC Taxi data into Neo4j as a graph structure.
- Real-Time Streaming:
   - Stream data into Kafka and process it in Neo4j.
- Graph Analysis - Use Neo4j to run PageRank and BFS algorithms:
     - PageRank
     ```cypher
     CALL gds.pageRank.stream('graph_name') 
     YIELD nodeId, score
     RETURN gds.util.asNode(nodeId).name AS location, score
     ORDER BY score DESC
     ```
     - Breadth First Search (BFS)
     ```cypher
     CALL gds.bfs.stream('graph_name', {
     startNode: 'start_location',
     endNode: 'end_location'
     })
     YIELD nodeId, path
     RETURN gds.util.asNode(nodeId).name AS location, path
     ```




## Results
- Phase 1: Offline graph analysis of NYC Taxi data using PageRank and BFS.
- Phase 2: Real-time streaming and graph-based analysis with Kafka and Neo4j.
## File Structure

- *data_producer.py*: Streams data from Parquet to Kafka.
- *zookeeper-setup.yaml*: Kubernetes configuration for Zookeeper.
- *kafka-setup.yaml*: Kubernetes configuration for Kafka.
- *neo4j-values.yaml*: Helm configuration for Neo4j deployment.
- *neo4j-service.yaml*: Kubernetes service configuration for Neo4j.