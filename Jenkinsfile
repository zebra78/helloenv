pipeline {
 
    agent any
 
    options {
        buildDiscarder(logRotator(numToKeepStr: '3', daysToKeepStr: '3'))
    }
 
    parameters { 
        string defaultValue: 'r19.5.1', description: 'r19.5.1', name: 'version', trim: false
        string defaultValue: 'tst', description: 'env', name: 'env', trim: false
    }
 
    stages {
        stage('Clone support repos') {
            steps {
                dir("${version}") {
                    git branch: '${version}', credentialsId: 'jenkins_gitbucket', url: 'ssh://git@192.168.100.100:29418/hello/helloapp.git'
                }
                dir('cfm') {
                    git branch: 'master', credentialsId: 'jenkins_gitbucket', url: 'ssh://git@192.168.100.100:29418/hello/hellocfm.git'
                }
            }
        }
 
        stage('Run ansible playbook') {
            steps {
              ansiblePlaybook disableHostKeyChecking: true, extras: '-e "version=${version} env=${env}"', inventory: 'cfm/hosts', playbook: 'cfm/webdeployer.yml' -vvvv
            }
        }
    }
 
}
