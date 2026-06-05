# DreamJobs AI Service Layer

## Description
This project is a standalone AI service layer built for the DreamJobs platform. It is designed to automate candidate matching, assist in creating job advertisements, and manage data-cleaning pipelines. 

## Features
*   **AI Recommender System:** An agentic workflow that automatically searches and ranks candidates when a new job is posted. It evaluates candidates using semantic similarity and keyword matching, sends personalized emails via AWS SES, and learns from user engagement.
*   **Job Ad Assistant:** An interactive, chat-based AI agent that helps users create job postings. It asks relevant questions and generates a structured JSON output for the main application.
*   **Automated CV Pipeline:** A webhook-triggered system that processes newly uploaded CVs. It uses AI to structure the data and generates vector embeddings before indexing them into the database.

## Global Tech Stack
*   **API Gateway:** FastAPI (Python 3.12).
*   **AI / LLMs:** AWS Bedrock (Claude Sonnet 4, Claude Haiku 4.5) and Cohere Embed Multilingual v3.
*   **Search Engine:** Elasticsearch 8.14+ (Hybrid KNN + BM25 + RRF fusion).
*   **Databases:** PostgreSQL 18 (Scoring DB) and Valkey 7.2 (Cache/Queue).
*   **Infrastructure:** Docker Compose on AWS EC2, configured with an Nginx reverse proxy.

## Architecture Notes
*   **Nginx auth_request:** The architecture separates responsibilities to prevent blocking the main Laravel application. Nginx validates authentication via Laravel, while the streaming logic runs entirely on the FastAPI side.
*   **Event Streaming:** The Job Ad Assistant utilizes the Server-Sent Events (SSE) protocol to stream AI responses in real-time.
