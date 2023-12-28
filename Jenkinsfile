pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'maven_3_2_1', type: 'maven'
        PATH = "${env.MAVEN_HOME}/bin:${env.PATH}"
        DOCKER_HUB_CREDENTIALS = credentials('karimelhou-dockerhub') 
        DOCKER_IMAGE_NAME = "karimelhou/mydocker" 
        DOCKER_IMAGE_TAG = "${env.BUILD_ID}"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Docker Run') {
            steps {
                script {
                    echo "BUILD_ID: ${env.BUILD_ID}"
                    sh "docker run -d -p 8081:8081 ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                }
            }
        }
    }
}
