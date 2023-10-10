pipeline {
    agent none
    options {
        timeout(time: 10, unit: 'MINUTES') 
    }
    stages {
        stage('cleanWS') {
            steps {
                cleanWs()
            }
        }
        stage('Build') {
            agent { label 'maven' }
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/jenkinsci/matrix-auth-plugin.git']])
                readCache name: 'mvn-cache'
                sh """
                mvn -Dmaven.repo.local=.m2 -DskipTests=true package
                """
                writeCache name: 'mvn-cache', includes: '.m2/**'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
