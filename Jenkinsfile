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
                    sh 'mvn clean verify sonar:sonar -Dsonar.host.url=http://sonarqube:9000'
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
