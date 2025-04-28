# Iris Flower Classification MLOps Project

This project demonstrates a simple Machine Learning Operations (MLOps) workflow for predicting Iris flower classes using a Random Forest model. The model is served via a Flask web application, containerized with Docker, and integrated with a Jenkins CI/CD pipeline for automated building, testing, and deployment.

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Repository Structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [Usage](#usage)
- [CI/CD Pipeline](#cicd-pipeline)
- [Testing](#testing)
- [Potential Improvements](#potential-improvements)
- [License](#license)

## Project Overview
The project trains a Random Forest classifier on the Iris dataset to predict the class of an Iris flower (Setosa, Versicolor, or Virginica) based on four features: Sepal Length, Sepal Width, Petal Length, and Petal Width. The trained model is deployed as a Flask web application, allowing users to input features and receive predictions. The project incorporates MLOps best practices, including automated testing, linting, containerization, and security scanning.

## Features
- **Machine Learning Model**: A Random Forest classifier trained on the Iris dataset.
- **Web Interface**: A Flask-based web application with a form for users to input flower measurements and view predictions.
- **Containerization**: Dockerized application for portability and reproducibility.
- **CI/CD Pipeline**: Jenkins pipeline for automated code linting, model training, testing, security scanning, and Docker image deployment to DockerHub.
- **Testing**: Unit tests for the model and Flask application using `pytest`.
- **Code Quality**: Linting and formatting with `pylint`, `flake8`, and `black`.
- **Security**: Vulnerability scanning with Trivy for both filesystem and Docker images.

## Repository Structure
```
├── app.py                  # Flask application for serving the model
├── Dockerfile              # Docker image configuration
├── Jenkinsfile             # Jenkins CI/CD pipeline configuration
├── model/                  # Directory for the trained model
│   └── iris_model.pkl      # Trained Random Forest model (generated)
├── requirements.txt        # Python dependencies
├── templates/              # Flask templates
│   └── index.html          # HTML form for user input
├── tests/                  # Unit tests
│   └── test_model.py       # Tests for model and Flask app
└── train.py                # Script to train and save the model
```

## Prerequisites
- Python 3.8+
- Docker
- Jenkins (for CI/CD)
- Git
- DockerHub account (for pushing images)
- Optional: AWS ECS (for deployment, currently commented out)

## Setup Instructions
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/adrenalin2517/MLOps_simple_project.git
   cd MLOps_simple_project
   ```

2. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Train the Model**:
   ```bash
   python train.py
   ```
   This generates `model/iris_model.pkl`.

4. **Run the Flask Application Locally**:
   ```bash
   python app.py
   ```
   Access the app at `http://localhost:5000`.

5. **Build and Run with Docker**:
   ```bash
   docker build -t iris-prediction:latest .
   docker run -p 5000:5000 iris-prediction:latest
   ```

## Usage
1. Open the web application at `http://localhost:5000`.
2. Enter the Sepal Length, Sepal Width, Petal Length, and Petal Width of an Iris flower.
3. Click "Predict" to see the predicted Iris class (0: Setosa, 1: Versicolor, 2: Virginica).

## CI/CD Pipeline
The `Jenkinsfile` defines an automated pipeline with the following stages:
- **Clone Repository**: Clones the GitHub repository.
- **Lint Code**: Runs `pylint`, `flake8`, and `black` for code quality.
- **Build Model**: Trains the model using `train.py`.
- **Test Code**: Executes unit tests with `pytest`.
- **Trivy FS Scan**: Scans the filesystem for vulnerabilities.
- **Build Docker Image**: Builds a Docker image tagged as `olehsafronov/mlops-project-01:latest`.
- **Trivy Docker Image Scan**: Scans the Docker image for vulnerabilities.
- **Push Docker Image**: Pushes the image to DockerHub.
- **Deploy (Commented)**: Placeholder for AWS ECS deployment.

To use the pipeline, configure Jenkins with the necessary credentials (`mlops-git-token`, `mlops-jenkins-dockerhub-token`) and run the pipeline.

## Testing
Run unit tests with:
```bash
pytest tests/
```
The tests in `test_model.py` validate:
- Model predictions with sample input.
- Flask application responses for the `/predict` endpoint.

## Potential Improvements
- Add input validation in `app.py` for non-numeric or invalid form inputs.
- Enable the AWS ECS deployment stage for production.
- Expand test cases to cover edge cases and invalid inputs.
- Include a `.gitignore` file to exclude generated files (e.g., `pylint-report.txt`, `model/iris_model.pkl`).
- Add monitoring and logging for production deployments.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.