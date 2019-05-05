pipeline {
 
    agent any
 
    options {
        skipStagesAfterUnstable()
        buildDiscarder(logRotator(numToKeepStr: '3', daysToKeepStr: '3'))
    }
 
    parameters { 
        string defaultValue: 'r19.5.1', description: 'r19.5.1', name: 'version', trim: false
    }
 
    stages {
        stage('Clone support repos') {
            steps {
                dir("${version}") {
                    git branch: '${version}', url: 'https://github.com/zebra78/helloapp.git'
                }
                dir('cfm') {
                    git branch: 'master', url: 'https://github.com/zebra78/hellocfm.git'
                }
            }
        }
 
        stage('Run ansible playbook') {
            steps {
              ansiblePlaybook disableHostKeyChecking: true, extras: '-e "version=${version}}"', inventory: 'cfm/hosts', playbook: 'cfm/webdeployer.yml'
            }
        }
    }
 
}
