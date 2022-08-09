pipeline {
  agent {label 'docker'}
    stages {
        stage("Initial config") {
            steps {
                script {
                    properties([pipelineTriggers([pollSCM('* * * * *')])])
                }
           }
        }
                stage('Checkout') {
            steps{
                git branch: 'master',
                    url: 'https://github.com/pivanaivy/Project.git'        
                }
        }
        stage("start docker-compose") {
            steps {
                sh '''
                echo $USER
                pwd
                ls -la
                cd classsed-docker-tutorial
                docker-compose up --build -d
                '''
                }
            }
        }
        post { 
            always {
                withCredentials([string(credentialsId: 'tel_secret', variable: 'TOKEN'),string(credentialsId: 'tel_chat_id', variable: 'CHAT_ID')]) {
                   sh """
                   curl -s -X POST https://api.telegram.org/bot${TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d parse_mode=markdown -d\
                   text='Отчёт:\r
                   *Project name :* ${currentBuild.projectName}\r
                   *Version :* ${currentBuild.displayName}\r
                   *Status :* ${currentBuild.result}'
                   """
            }
            cleanWs()
        }
    }
    }
