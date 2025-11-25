# 🦁 Scalable Zoo Guide Agent with Gemma 3

> **🏆 Project Submission for the Google Cloud Build and Blog Marathon 2025**

## 📖 Introduction
This project was designed and built during the **Google Cloud Build and Blog Marathon 2025**, a developer event focused on leveraging Google Cloud's serverless infrastructure to deploy next-generation AI applications.

The goal of this project was to demonstrate how to build a **cost-effective, production-grade AI Agent** using open models. We utilized **Google's Gemma 3** model hosted on **Cloud Run** to create an interactive "Zoo Guide" that answers visitor questions in real-time.

## 🚀 Live Demo
[**Click here to Chat with Gem, the Zoo Guide**](https://production-adk-agent-30183910092.europe-west1.run.app)

## 🏗️ Technical Architecture
To optimize for both **performance** and **cost**, this application implements a **Split Microservices Architecture**:

1.  **Frontend Service (CPU-Optimized):**
    -   Built with **Python 3.13**, **FastAPI**, and the **Google Agent Development Kit (ADK)**.
    -   Handles user sessions, input validation, and message routing.
    -   Scales to zero when not in use to minimize costs.
    -   Deployed on Google Cloud Run (CPU only).

2.  **Inference Service (GPU-Accelerated):**
    -   Runs **Ollama** inside a Docker container.
    -   Hosts the **Gemma 3 (270m)** open model.
    -   Utilizes **NVIDIA L4 GPUs** via Cloud Run to deliver low-latency inference.
    -   Operates as a private backend API, accessible only by the Frontend Agent.

### System Flow
`User` -> `[HTTPS]` -> `Frontend Agent (Cloud Run)` -> `[Internal API]` -> `Inference Backend (Cloud Run + GPU)`

## 🛠️ Tech Stack
-   **Cloud Provider:** Google Cloud Platform (GCP)
-   **Compute:** Cloud Run (Serverless Container Streaming)
-   **AI Model:** Gemma 3 (270m parameter variant)
-   **Model Serving:** Ollama (Containerized)
-   **Agent Framework:** Google Agent Development Kit (ADK)
-   **Backend API:** FastAPI (Python)
-   **Hardware Acceleration:** NVIDIA L4 Tensor Core GPU
-   **Containerization:** Docker

## ✨ Key Features
-   **Microservices Pattern:** Decouples the lightweight web server from the heavy inference engine.
-   **Serverless Autoscaling:** Automatically scales instances based on traffic demand.
-   **GPU Acceleration:** Drastically reduces token generation time for a smooth chat experience.
-   **Contextual Persona:** The agent is prompted with a specific system instruction to act as "Gem," a friendly and educational guide.

## 📦 Deployment Instructions

### Prerequisites
-   Google Cloud Project with Billing Enabled.
-   `gcloud` CLI installed and authenticated.
-   `uv` package manager for Python.

### 1. Deploy the AI Backend (GPU)
We deploy Ollama with the Gemma model to a Cloud Run instance attached to a GPU.

```
gcloud run deploy ollama-gemma3-270m-gpu \
  --source ./ollama-backend \
  --region europe-west1 \
  --gpu 1 \
  --gpu-type nvidia-l4 \
  --memory 16Gi \
  --no-cpu-throttling
```


### 2. Deploy the Agent Frontend (CPU)
We deploy the ADK agent and connect it to the backend using environment variables.

```
gcloud run deploy production-adk-agent \
  --source ./adk-agent \
  --region europe-west1 \
  --set-env-vars OLLAMA_API_BASE=$OLLAMA_URL \
  --allow-unauthenticated
```

### 📄 License
This project is open-source and available under the MIT License. Created for the Google Cloud Build & Blog Marathon 2025.
