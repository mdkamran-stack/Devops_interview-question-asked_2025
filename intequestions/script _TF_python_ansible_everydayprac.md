## Write a python file to backup all files older than 30 days from a directory.

```python
import os
import shutil
import time

# Configuration
SOURCE_DIR = "/path/to/source"      # Directory to check
BACKUP_DIR = "/path/to/backup"      # Directory to store backups
DAYS_OLD = 30                        # Files older than 30 days

# Convert days to seconds
time_threshold = time.time() - (DAYS_OLD * 24 * 60 * 60)

# Ensure backup directory exists
os.makedirs(BACKUP_DIR, exist_ok=True)

# Iterate through files in the source directory
for filename in os.listdir(SOURCE_DIR):
    file_path = os.path.join(SOURCE_DIR, filename)
    
    # Skip directories, only process files
    if os.path.isfile(file_path):
        file_mtime = os.path.getmtime(file_path)
        
        # Check if file is older than threshold
        if file_mtime < time_threshold:
            shutil.move(file_path, os.path.join(BACKUP_DIR, filename))
            print(f"Backed up: {filename}")

print("Backup completed!")
```


## Build a Java/Maven application Package it into a Docker image Push image to DockerHub Deploy to Kubernetes

pipeline {
    agent any

    environment {
        APP_NAME   = "myapp"
        APP_VERSION = "1.0.${BUILD_NUMBER}"
        DOCKER_REGISTRY = "docker.io/your-dockerhub-user"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/org/repo.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    docker build -t $DOCKER_REGISTRY/$APP_NAME:$APP_VERSION .
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $DOCKER_REGISTRY/$APP_NAME:$APP_VERSION
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig(credentialsId: 'k8s-config') {
                    sh """
                    kubectl set image deployment/$APP_NAME $APP_NAME=$DOCKER_REGISTRY/$APP_NAME:$APP_VERSION -n dev
                    kubectl rollout status deployment/$APP_NAME -n dev
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build & Deployment Successful: $APP_NAME:$APP_VERSION"
        }
        failure {
            echo "❌ Pipeline Failed. Please check logs."
        }
    }
}
