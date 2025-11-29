pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'M2_HOME'
    }

    triggers {
        githubPush()
    }

    environment {
        IMAGE = "marambeji/student-management"
        TAG = "1.0.0"
        SONAR_HOST = "http://192.168.10.132:9000"
        SONAR_TOKEN = "squ_c35356c3e293949b2f425b7228a89063d6dba189"
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

        stage('SonarQube Analysis') {
            steps {
                sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey=student-management \
                        -Dsonar.host.url=${SONAR_HOST} \
                        -Dsonar.token=${SONAR_TOKEN}
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    echo "⚙️ Building Docker image..."
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
            echo '🎉 SUCCESS : Docker image built & pushed successfully on DockerHub + SonarQube OK !'
        }
        failure {
            echo '❌ FAILURE : Something went wrong.'
        }
    }
}
