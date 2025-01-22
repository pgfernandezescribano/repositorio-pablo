
pipeline {
    agent {
        label 'linux'
    }
    stages {
        stage('Build app') {
            steps {
                sh "mvn clean install -Dmaven.test.skip=true"
            }
        }
        stage('Sonarqube scanner') {
            steps {
             withSonarQubeEnv("Produccion Sonar 7 CE") {
                script {
                    pom = readMavenPom file: 'pom.xml'
                    pomVersion = pom.version
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
