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
                    checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/jenkinsci/github-branch-source-plugin.git']])
                }
            }
        }
        
        stage('Read Cache') {
            agent { label 'maven' }
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
                    mvn -Dmaven.repo.local=maven-repo -DskipTests=true package
                    """
                }
            }
            post {
                success {
                    writeCache includes: 'maven-repo/**', name: 'mvn-cache'
                }
            }
        }
    }
}
