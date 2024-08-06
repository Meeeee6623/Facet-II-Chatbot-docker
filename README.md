# SLAC-Elog-Chatbot-docker

This repository contains the Docker configuration for a chatbot designed for the SLAC National Lab. The chatbot utilizes open-webui/pipelines, ollama, langfuse, and ragflow to quickly answer questions about the E-log and other technical details of the lab.

## Getting Started

### Prerequisites

Ensure you have the following installed on your machine:

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

### Cloning the Repository

To clone this repository along with its submodules, first clone this repository, then run the following command to clone and initialize the submodules:

```sh
cd SLAC-Chatbot-docker
git submodule update --init
```

### Running the Project

In order for Ragflow's elastic search to work properly, you might need to increase the vm.max_map_count on your machine. 
You can check the current value with the following command:
```sh
sysctl vm.max_map_count
```
If the value is less than 262144, you should increase it with the following command:
```sh
sudo sysctl -w vm.max_map_count=262144
```
However, it will reset every time you restart your machine. To make the change permanent, you can add the following line to /etc/sysctl.conf:
```sh
vm.max_map_count=262144
```
Now, you can run the project:

1. **Build and start the Docker containers:**

   ```sh
   docker-compose up
   ```

   To run the containers in the background (detached mode), use:

   ```sh
   docker-compose up -d
   ```

2. **Access the Dashboards:**

Upon accessing the dashboards, you will be asked to log in with an email/password pair. Right now, emails are not validated, so feel free to sign up with any combination and just remember that - everything's stored locally. 

   - **Open WebUI Dashboard (Chats):** [http://localhost:3001](http://localhost:3001)
   - **Ragflow Dashboard (Vector Search):** [http://localhost:8001](http://localhost:8001)
   - **Langfuse Dashboard (Logging):** [http://localhost:3000](http://localhost:3000)

API services (should not need to directly access these):
   - **Pipelines API:** Accessible at [http://localhost:9099](http://localhost:9099)
   - **Ollama Service:** Accessible at [http://localhost:11434](http://localhost:11434)

## Configuration

To configure the project, you need to set up ragflow and open-webui.

To set up open-webui, follow this pipelines tutorial to get the pipelines set up. 
To set up ragflow, navigate to the dashboard and create a knowledge base and upload the documents you need. Then, you can use the graph tab to test the knowledge base and see how it responds to queries.

For logging, you need to create a new project in Langfuse and get the API keys. Then, you can enter them in the pipelines settings of the open-webui dashboard. 

## Stopping the Project

To stop the running containers, use:

```sh
docker-compose down
```

This will stop and remove the containers but will keep the volumes for persistent data storage.

## Additional Information

- The `open-webui` service provides the web interface.
- The `ragflow` service is used for vector search and stores the knowledge base.
- The `pipelines` service connects open-webui with ragflow and langfuse.
- The `postgres` service is used to store data for Langfuse.
- The `langfuse` service provides logging and monitoring for chats and connects to the PostgreSQL database.
- The `ollama` service hosts chat models on GPU resources for enhanced processing capabilities.

