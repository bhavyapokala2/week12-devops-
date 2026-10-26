pipeline {
    agent any

    stages {
        stage('Run Selenium Tests with pytest') {
    steps {
        echo "Running Selenium Tests using pytest"

        bat '''
            set PYTHONHOME=
            set PYTHONPATH=
            python -m pip install --upgrade pip
            python -m pip install -r requirements.txt
        '''

        bat 'start /B python app.py'

        // Wait a few seconds for the server to start
        bat 'ping 127.0.0.1 -n 5 > nul'

  
        bat 'python -m pytest -v'
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
                bat 'docker login -u bhavyasrip -p <your_dockerhub_password>'
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
