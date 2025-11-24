pipeline {
    agent any

    environment {
        IMAGE = "marambeji/student-management"
        TAG = "1.0.0"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/marambeji/maram1_beji_4twin-8.git',
                    credentialsId: 'github-token'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE}:${TAG} ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-maram', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh "docker push ${IMAGE}:${TAG}"
            }
        }
    }

    post {
        success {
            echo '🎉 SUCCESS : Docker image built & pushed successfully on DockerHub (marambeji/student-management:1.0.0) !'
        }
        failure {
            echo '❌ FAILURE : Something went wrong.'
        }
    }
}
