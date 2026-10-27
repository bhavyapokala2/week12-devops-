pipeline {
    agent any

    stages {
        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                // Install Python dependencies
                bat 'pip install -r requirements.txt'

                // ✅ Start Flask app in background
                bat 'start /B python app.py'

              sleep time: 5, unit: 'SECONDS'
                // ✅ Run tests using pytest
                //bat 'pytest tests\\test_registrationapp.py --maxfail=1 --disable-warnings --tb=short'
                bat 'pytest -v'
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
                bat 'docker login -u bhavyasrip -p Bhavya2612@@'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"
                bat "docker tag seleniumdemoapp:v1 bhavyasrip/w12-registrationapp:v1"
                bat "docker push bhavyasrip/w12-registrationapp:v1"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // apply deployment & service
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
