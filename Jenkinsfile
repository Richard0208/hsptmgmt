pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Check out your source code from GitHub
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/Richard0208/hsptmgmt.git', credentialsId: 'github-credentials']]])
            }
        }

        stage('Build Docker Images') {
            steps {
                catchError(buildResult: 'FAILURE') {
                    // Build Docker images using docker-compose build
                    sh 'docker-compose build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests (if applicable)
                // Adjust the command based on your specific project
                // For example:
                // bat 'docker-compose run your_service_name npm test'
                echo "Running tests..."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Log in to Docker Hub with credentials from Jenkins
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                script {
                    // Use sh to pass the password securely to docker login
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                }
            
            // Push the Docker images to Docker Hub
            sh 'docker-compose push'
                }
            }
        }
    }
}
