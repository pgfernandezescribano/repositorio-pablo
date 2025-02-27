pipeline {
    agent any
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
        stage('Run Tests & Generate Coverage') {
            steps {
                sh './gradlew clean test jacocoTestReport'
            }
        }
        stage('Publish JaCoCo Report') {
            steps {
                jacoco execPattern: '**/build/jacoco/*.exec'
            }
        }

    }
}
