
pipeline {
    agent any
    stages {
        stage('Build app') {
            steps {
                sh "mvn clean install -Dmaven.test.skip=true"
            }
        }
        stage('Sonarqube scanner') {
            steps {
             withSonarQubeEnv("SonarQube") {
                script {
                    sonarLinksScm= 'https://github.com/pgfernandezescribano/repositorio-pablo.git'
                }
                sh "sonar-scanner -Dsonar.projectVersion=${pomVersion} -Dsonar.links.scm=${sonarLinksScm} -Dsonar.branch.name=${env.BRANCH_NAME}"
                }
            }
        }
        stage('Archive') {
            steps {
                dir ('target') {
                    archiveArtifacts artifacts: '*.jar', fingerprint: true
                }
            }           
        }
    }
    post {
        failure {
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "pgfernandezescribano@indra.es",
                sendToIndividuals: true])       
        }
    }
}
