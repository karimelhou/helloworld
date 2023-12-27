pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Build your Spring Boot application
                script {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Docker Build') {
            steps {
                // Build Docker image
                script {
                    docker.build("helloworld-app:${env.BUILD_ID}")
                }
            }
        }
        stage('Docker Run') {
            steps {
                // Run the Docker container
                script {
                    sh 'docker run -d -p 8081:8081 helloworld-app:${env.BUILD_ID}'
                }
            }
        }
    }
}
