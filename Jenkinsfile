pipeline {
    agent any
    stages {
        stage('Build app') {
            steps {
                // Construye la aplicación, omitiendo las pruebas
                sh "mvn clean install -Dmaven.test.skip=true"
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Ejecuta el análisis SonarQube
                withSonarQubeEnv("SonarQube") {
                    sh "mvn sonar:sonar -Dsonar.branch.name=${env.BRANCH_NAME}"
                }
            }
        }
    }
    post {
        failure {
            // Notificación por email si el pipeline falla
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "pgfernandezescribano@indra.es",
                sendToIndividuals: true])
        }
    }
}

