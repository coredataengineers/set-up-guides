# Apache Spark Local Development Environment
### Installation & Setup Guide — Docker Compose · PySpark · Jupyter Lab

---

## 1. Overview

This guide walks you through setting up a fully containerised Apache Spark cluster on your local machine using Docker Compose. The stack includes a Spark Master node, a Spark Worker node, and a Jupyter Lab notebook server with PySpark pre-installed — giving you a realistic distributed-computing environment without any cloud costs.

### Services

| Container | Role |
|---|---|
| `spark-master` | Cluster coordinator — accepts job submissions and manages workers |
| `spark-worker` | Executor node — processes data partitions (2 cores, 2 GB RAM) |
| `jupyter-node` | Interactive notebook server — PySpark pre-installed and pre-configured |

### Architecture at a Glance

**Jupyter Lab** acts as the driver. It submits Spark jobs to `spark-master` on port `7077`, which distributes tasks to `spark-worker`. All three containers share the same Docker bridge network created automatically by Compose, so they resolve each other by hostname.

---

## 2. Prerequisites

Ensure the following are installed and running before proceeding.

| Requirement | Notes |
|---|---|
| Docker Desktop | v4.x or later. Ensure the Docker engine is running before any step. to install docker, click on this link to see guide:  |
| Docker Compose | Bundled with Docker Desktop. Verify with: `docker compose version` |
| Git | Needed for cloning the repository. |
| 4 GB free RAM | The worker is allocated 2 GB; allow headroom for master and Jupyter. |
| ~3 GB disk space | For the `apache/spark` and `jupyter/pyspark-notebook` images. |

---

## 3. Project Structure

Your project directory should match the following layout. The `data/` folder is mounted into all containers so they can all read and write the same files.

```
spark-dev-environment/
├── data/
│   └── bucketing/
│       ├── orders.csv
│       └── products.csv
├── notebook-work/
│   └── demo.ipynb
├── .gitignore
├── docker-compose.yaml
└── README.md
```

`notebook-work/` is mounted to `/home/jovyan/notebook-work` inside Jupyter, so any notebook you save there persists on your host disk between container restarts.

---

## 4. Installation

### Step 1 — Create the course directory in your local device

```bash
mkdir apache_spark-course
cd apache_spark-course
```
#### optional run the code . to open in VS code 

### Step 2 — Clone the repository 
##### open your vs code terminal and inside the spark-course directory, run command below

```bash
git clone https://github.com/coredataengineers/local-spark-cluster.git

```
### Step 3 — Confirm the result of the clone 
You can use the ls command in terminal or look at the explorer tab on the left tab of your Vs code IDE (check images folder of this guide to see sample results)


### Step 4 — Start the stack
To ensure uniformity, the docker-compose.yaml file has already been created with the necesary services configured; 
cd into the spark-dev-environment directory and run the below commands
N.B ensure that your docker desktop application is running 

```bash
cd spark-dev-environment
docker compose up -d
```

The `-d` flag runs all containers in the background (detached mode). Omit it if you want to watch live logs in your terminal.

> **Expected download time: 2633.2s `apache/spark` is roughly 500 MB; `jupyter/pyspark-notebook` is around 2.5 GB. 
### Step 5 — Verify all containers are running

```bash
docker compose ps
```

All three containers should show status `Up`. If any show `Exit`, check the [Troubleshooting](#7-troubleshooting) section.

---

## 5. Port Reference

Once the stack is running, the following URLs are available on your host machine. you can click on the links to view them.

| Port | Service / Purpose |
|---|---|
| `9090` | Spark Master Web UI — monitor cluster health, workers, and running apps |
| `7077` | Spark Master internal comms — workers register and receive job instructions |
| `8888` | Jupyter Lab interface — your primary development environment |
| `4040` | Spark Driver UI — appears only while a job is running |

> **Note on port 4040:** This port only becomes active while a Spark job is actively running. If you open it when no job is executing, the browser will show a connection error — this is expected behaviour.

---

## 6. Common Commands

| Action | Command |
|---|---|
| Start the stack (detached) | `docker compose up -d` |
| Stop all containers | `docker compose down` |
| Stop and delete volumes | `docker compose down -v` |
| View live logs (all services) | `docker compose logs -f` |
| View logs for one service | `docker compose logs -f spark-master` |
| Restart a single container | `docker compose restart jupyter` |
| Open a shell in Jupyter | `docker exec -it jupyter-node bash` |
| Check resource usage | `docker stats` |

---

## 7. Troubleshooting

### Worker does not appear in the Spark Master UI

- Wait 15–30 seconds after startup; the worker registers asynchronously.
- Run `docker compose logs spark-worker` and look for `"Successfully registered with master"`.
- Confirm both containers are on the same Docker network: `docker network ls`.

### Jupyter notebook cannot connect to Spark

- Ensure you are using `.master("spark://spark-master:7077")` and not `localhost`.
- Restart the stack: `docker compose down && docker compose up -d`.

### Port already in use error on startup

- Another process is bound to 9090, 7077, 8888, or 4040.
- Find the conflicting process: `lsof -i :<port>` (macOS/Linux) or `netstat -ano | findstr <port>` (Windows).
- Either stop the conflicting process or edit the host-side port in `docker-compose.yaml` (left side of the colon).

### Containers exit immediately after start

- Run `docker compose logs <service-name>` to view the exit reason.
- **Apple Silicon (M1/M2):** the `apache/spark` image may need `--platform linux/amd64`. Add `platform: linux/amd64` under the service definition in the compose file.

> **Tip:** `docker compose logs -f` gives you a combined, real-time stream from all three containers — the fastest way to diagnose any startup problem.

---

## 8. Stopping & Cleanup

### Stop without losing notebooks

```bash
docker compose down
```

Containers are stopped and removed, but your `notebook-work/` and `data/` folders on disk are untouched.

### Full reset (removes all container state)

```bash
docker compose down -v --rmi local
```

Removes containers, anonymous volumes, and locally built images. Downloaded images are kept in Docker's image cache.

### Remove cached images entirely

```bash
docker rmi apache/spark:latest
docker rmi jupyter/pyspark-notebook:latest
```

---

*Apache Spark Dev Environment — Local Setup Guide | Docker Compose Stack*
