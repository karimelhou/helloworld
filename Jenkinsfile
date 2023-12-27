pipeline {
    agent any

    environment {
            // Define the path to the Maven executable
            MAVEN_HOME = tool name: 'maven_3_2_1', type: 'maven'
            PATH = "${env.MAVEN_HOME}/bin:${env.PATH}"
        }

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
                script {
                    echo "BUILD_ID: ${env.BUILD_ID}"
                    sh 'docker run -d -p 8081:8081 helloworld-app:${env.BUILD_ID}'
                }
            }
        }
    }
}
