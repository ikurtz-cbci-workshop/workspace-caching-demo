pipeline {
    agent none
    options {
        timeout(time: 10, unit: 'MINUTES') 
    }
    stages {
        stage('Checkout') {
            agent { label 'maven' }
            steps {
                container ('maven') {
                    checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/jenkinsci/kubernetes-plugin.git']])
                }
            }
        }
        
        stage('Read Cache') {
            steps {
                container ('maven') {
                    readCache 'mvn-cache'
                }
            }
        }

        stage('Build') {
            agent { label 'maven' }
            steps {
                container ('maven') {
                    sh """
                    mvn clean verify -Dmaven.repo.local=.m2 -DskipTests=true package
                    """
                }
            }
            post {
                success {
                    writeCache includes: '.m2/**', name: 'mvn-cache'
                }
            }
        }
    }
}
