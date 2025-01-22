pipeline {
    agent {
        label 'ssl08' // Etiqueta del nodo esclavo
    }
    stages {
        stage('Build app') {
            steps {
                sh "mvn clean install -Dmaven.test.skip=true"
            }
        }
        stage('Sonarqube scanner') {
            steps {
                script {
                    pom = readMavenPom file: 'pom.xml'
                    pomVersion = pom.version
                    sonarLinksScm= 'https://github.com/pgfernandezescribano/repositorio-pablo.git'
                }
                sh "sonar-scanner -Dsonar.host.url=https://sonarqube.indra.es -Dsonar.login=squ_8ec74bb3ad43a1c3a2a0aed73a16ff2195b30df8 -Dsonar.projectVersion=${pomVersion} -Dsonar.branch.name=${env.BRANCH_NAME} -Dsonar.links.scm=${sonarLinksScm}"
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
