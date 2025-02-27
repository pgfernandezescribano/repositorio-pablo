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
            agent {
                docker { image 'maven:3.8.6' }
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn clean verify sonar:sonar'
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
