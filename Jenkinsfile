pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "nikolai7"  // replace for your username at docker hub
        SERVICE_NAME = "java-spring-template" // change for your service name
        SERVICE_VERSION = "1.1"
        IMAGE_NAME = "${DOCKER_REGISTRY}/${SERVICE_NAME}:${SERVICE_VERSION}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    sh './gradlew build test jacocoTestReport'
                }
            }
        }

        stage('Code Coverage Report') {
            steps {
                script {
                    archiveArtifacts 'build/reports/jacoco/tests'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }

//         stage('Deploy') {
//             steps {
//                 script {
//                     sh "kubectl apply -f kubernetes/deployment.yaml"
//                 }
//             }
//         }
    }

//     post {
//         success {
//             echo 'Deployment successful!'
//         }
//         failure {
//             echo 'Deployment failed!'
//         }
//     }
}