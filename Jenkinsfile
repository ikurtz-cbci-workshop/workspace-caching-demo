pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.9.4-eclipse-temurin-11
    command:
    - sleep
    args:
    - infinity
'''
            defaultContainer 'maven'
        }
    }
    stages {
        stage('Build') {
            steps {
                    checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/jenkinsci/matrix-auth-plugin.git']])
                    readCache 'mvn-deps'
                    sh """
                    mvn -Dmaven.repo.local=.m2 -DskipTests=true package
                    """
                    writeCache includes: '.m2/**', name: 'mvn-deps'
            }
        }
    }
}
