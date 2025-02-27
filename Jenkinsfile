pipeline {
    agent any
    environment {
        SONARQUBE = 'SonarQube' // Define tu servidor de SonarQube
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
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
