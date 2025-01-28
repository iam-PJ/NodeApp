pipeline {
    agent any
    
    tools {
        nodejs 'MyNode'
    }

    stages {
        stage('Cloning Code') {
            steps {
                echo 'This Step is for Cloning the code'
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/iam-PJ/NodeApp.git']])
                echo 'Cloning Sucessful'
            }
        }
        stage('Build') {
            steps {
                echo 'This Step is for Building the code'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'This step is for Testing the code'
                sh './node_modules/mocha/bin/_mocha --exit ./test/test.js'
            }
        }
        stage('Deploy') {
            steps {
                echo 'This Step is for Deploying the code'
                script {
                    sshagent(['Ubuntukey']) {
                        sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@3.144.92.126 << EOF
                            cd /home/ubuntu/NodeApp/
                            git pull https://github.com/iam-PJ/NodeApp.git
                            npm install
                            sudo npm install -g pm2
                            pm2 restart index.js || pm2 start index.js
                            exit
                        EOF
                        '''
                    }
                }
               }
            }
        }
    }
