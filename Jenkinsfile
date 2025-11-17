pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    stages {

        stage('GIT') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/marambeji/maram1_beji_4twin-8.git'
            }
        }

        stage('Compile Stage') {
            steps {
                sh 'mvn clean compile'
            }
        }
    }
}
