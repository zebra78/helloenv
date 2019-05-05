pipeline {
 
    agent any
 
    options {
        buildDiscarder(logRotator(numToKeepStr: '3', daysToKeepStr: '3'))
    }
 
    parameters { 
      string(name: 'version', defaultValue: 'r19.5.1', description: 'release version')
      string(name: 'env', defaultValue: 'tst', description: 'deploy env')
    }
 
    stages {
        stage('Clone support repos') {
            steps {
                dir('${version}') {
                    git branch: '${version}', credentialsId: 'jenkins_gitbucket', url: 'ssh://git@192.168.100.100:29418/hello/helloapp.git'
                }
                dir('cfm') {
                    git branch: 'master', credentialsId: 'jenkins_gitbucket', url: 'ssh://git@192.168.100.100:29418/hello/hellocfm.git'
                }
            }
        }
 
        stage('Run ansible playbook') {
            steps {
              ansiblePlaybook disableHostKeyChecking: true, extras: '-e "version=${version}"', inventory: 'cfm/hosts', playbook: 'cfm/webdeployer.yml'
            }
        }
    }
 
}
