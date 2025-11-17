pipeline {
    agent any
    
    stages {
        stage('Pull Code') {
            steps {
                git 'https://github.com/user/projet.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
