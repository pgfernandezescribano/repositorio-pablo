pipeline {
    agent any
    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/pgfernandezescribano/repositorio-pablo.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh './gradlew sonarqube'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 5, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}
