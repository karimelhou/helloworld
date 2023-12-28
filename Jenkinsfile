pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'maven_3_2_1', type: 'maven'
        PATH = "${env.MAVEN_HOME}/bin:${env.PATH}"
        DOCKERHUB_CREDENTIALS = credentials('karimelhou-dockerhub') 
        DOCKER_IMAGE_NAME = "karimelhou/mydocker" 
        DOCKER_IMAGE_TAG = "${BUILD_NUMBER}" // Use BUILD_NUMBER directly
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

        stage('Login') {
            steps {
                script {
                    sh "echo \${DOCKERHUB_CREDENTIALS_PSW} | docker login -u \${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    sh "docker push ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }

    stage('Docker Run') {
        steps {
            script {
                echo "BUILD_ID: ${BUILD_NUMBER}" // Use BUILD_NUMBER directly
                sh "docker run -d -p 8081:8081 ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
            }
        }
    }

}
