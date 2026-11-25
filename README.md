# TailorAI

Here is the architecture diagram for your custom AI model platform, drawn to bottom in six layers.

1. Client: the customer's web dashboard, the customer's own apps (which call the deployed model over REST), and your admin view.

2. API layer: FastAPI handles login, keeps each company's data separate, and limits request rates.

3. Orchestration: a job queue (Redis with Celery, or Prefect) runs each project as a pipeline of ingest, build, evaluate and deploy.

4. Pipeline workers: four workers do the real work.
   - Data ingestion
   - RAG builder
   - Fine-tune trainer
   - Evaluator
5. Storage: MinIO for files, Qdrant for vectors, MLflow as the model registry, and PostgreSQL for users, projects and jobs.

6. Serving and monitoring: vLLM or Ollama serves one endpoint per company, and Grafana and Langfuse watch quality. Feedback from users turns into new data for retraining.