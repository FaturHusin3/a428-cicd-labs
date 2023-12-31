    pipeline {
        agent {
            docker {
                image 'node:16-buster-slim'
                args '-p 3000:3000'
            }
        }
        stages {
            stage('Build') {
                steps {
                    sh 'npm install'
                }
            }
            stage('Test') {
                steps {
                    sh './jenkins/scripts/test.sh'
                }
            }
            stage('Manual Approval') {
                steps {
                    script {
                        def deploymentDelay = input id: 'Deploy', message: 'Lanjutkan ke tahap Deploy?', submitter: 'admin', parameters: [choice(choices: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24'], description: 'Hours to delay deployment?', name: 'deploymentDelay')]
                        sleep time: deploymentDelay.toInteger(), unit: 'HOURS'
                    }
                }
            } 

            stage('Deploy') { 
                steps {
                    sh './jenkins/scripts/deliver.sh' 
                    script {sleep time: '1', unit: 'MINUTES'} 
                    sh './jenkins/scripts/kill.sh' 
                }
            }
        }
    }