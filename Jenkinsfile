pipeline {
    agent any

    environment {
        // Point Jenkins directly to your Python site-packages
        PYTHONPATH = 'C:\\Users\\BHAVYA\\AppData\\Roaming\\Python\\Python312\\site-packages'
    }

    stages {
        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                // Start Flask app in background
                bat 'start /B "FlaskServer" "C:\\Program Files\\Python312\\python.exe" app.py'

                // Wait 5 seconds for the server to start
                bat 'C:\\Windows\\System32\\ping.exe 127.0.0.1 -n 5 > nul'

                // Run pytest using the global Python executable
                bat '"C:\\Program Files\\Python312\\python.exe" -m pytest -v'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t seleniumdemoapp:v1 ."
            }
        }
        stage('Docker Login') {
            steps {
                // Ensure your real Docker Hub password or personal access token is here
                bat 'docker login -u bhavyasrip -p Bhavya2612@@'
            }
        }
        stage('push Docker Image to Docker Hub') {
            steps {
                echo "push Docker Image to Docker Hub"
                bat "docker tag seleniumdemoapp:v1 bhavyasrip/w12-registrationapp:v1"
                bat "docker push bhavyasrip/w12-registrationapp:v1"
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                // Apply deployment and service
                bat 'kubectl apply -f deployment.yaml --validate=false'
                bat 'kubectl apply -f service.yaml'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
