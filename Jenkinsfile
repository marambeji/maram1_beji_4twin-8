pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        IMAGE = "marambeji/student-management"
        TAG = "1.0.0"
    }

  tools {
    jdk 'jdk17'
    maven 'M2_HOME'
}

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/marambeji/maram1_beji_4twin-8.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                sh 'mvn sonar:sonar \
                    -Dsonar.projectKey=mon-projet \
                    -Dsonar.host.url=http://192.168.10.132:9000 \
                    -Dsonar.login=admin \
                    -Dsonar.password=sonar'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    echo "⚙️ Building Docker image WITHOUT BuildKit..."
                    export DOCKER_BUILDKIT=0
                    docker build -t ${IMAGE}:${TAG} .
                """
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-maram',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]) {
                    sh """
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                    """
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
            echo '🎉 SUCCESS : Docker image built & pushed successfully on DockerHub (marambeji) !'
        }
        failure {
            echo '❌ FAILURE : Something went wrong.'
        }
    }
}
