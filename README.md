# 🦁 Scalable Zoo Guide Agent with Gemma 3

A production-ready AI Chatbot acting as a virtual "Zoo Guide." Built for the **Build and Blog Marathon 2025**, this application uses Google's **Gemma 3** model to answer user questions about animals, habitats, and conservation in real-time.

## 🚀 Live Demo
[**Click here to Chat with Gem, the Zoo Guide**](https://production-adk-agent-30183910092.europe-west1.run.app)

## 🏗️ Architecture
This project utilizes a **Microservices Architecture** deployed on **Google Cloud Run** to ensure scalability and cost-efficiency:

1.  **Frontend Agent (CPU):** A lightweight Python service using the **Google Agent Development Kit (ADK)**. It handles user sessions, routing, and the chat interface.
2.  **AI Backend (GPU):** A dedicated service running **Ollama** with the **Gemma 3 (270m)** model on an **NVIDIA L4 GPU**.

### The Flow:
`User` -> `Cloud Run Frontend (FastAPI/ADK)` -> `Cloud Run Backend (Ollama/Gemma)`

## 🛠️ Tech Stack
- **Cloud Platform:** Google Cloud Run (Serverless)
- **AI Model:** Gemma 3 (270m parameters)
- **Inference Engine:** Ollama
- **Framework:** Google Agent Development Kit (ADK), FastAPI
- **Language:** Python 3.13
- **Hardware:** NVIDIA L4 GPU (for inference)

## ✨ Features
- **Real-time AI Responses:** Instant answers to educational questions about wildlife.
- **Persona-Based:** The agent "Gem" is designed to be friendly, enthusiastic, and educational.
- **Scalable:** The frontend scales to zero to save costs, while the backend scales independently based on inference load.
- **GPU Accelerated:** Optimized for low-latency responses using Cloud Run GPU support.

## 📦 Deployment

### Prerequisites
- Google Cloud Project with Billing Enabled
- `gcloud` CLI installed

### Steps to Reproduce
1.  **Deploy the Backend (GPU):**
    ```bash
    gcloud run deploy ollama-gemma3-270m-gpu --source ./ollama-backend --gpu 1 --gpu-type nvidia-l4
    ```

2.  **Deploy the Agent (CPU):**
    ```bash
    gcloud run deploy production-adk-agent --source ./adk-agent --set-env-vars OLLAMA_API_BASE=[YOUR_BACKEND_URL]
    ```

## 📄 License
MIT License. Built for the Google Cloud Build & Blog Marathon 2025.
