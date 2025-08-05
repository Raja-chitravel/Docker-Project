pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Running Maven install"
                sh 'mvn clean install'
            }
        }

        stage('Build the Docker Image') {
            steps {
                echo "Building Docker image"
                sh '''
                    docker build -t raja/springpetclinic-123923498:v1.2 .
                    docker images
                '''
            }
        }

        stage('Trivy Scan') {
            steps {
                echo "Scanning the image with Trivy"
                sh '''
                    trivy image --no-progress --severity HIGH,CRITICAL --exit-code 1 --format table raja/springpetclinic-123923498:v1.2
                '''
            }
        }

        stage('Push the Docker Image') {
            steps {
                echo "Pushing the Docker image to registry"
                sh '''
                    docker push raja/springpetclinic-123923498:v1.2
                    docker rmi raja/springpetclinic-123923498:v1.2
                    docker images
                '''
            }
        }
    }
}
