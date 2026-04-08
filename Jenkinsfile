pipeline {
    agent any

    environment {
        strDockerImage = "ysh810/cicd-test:0.1"
    }

    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/ysh810/cicd-test.git'
            }
        }
        stage('Docker Image Build') {
             steps {
                script {
                    oDockImage = docker.build(strDockerImage, "-f Dockerfile .")
                }
            }
        }
        stage('Docker Image Push') {
            steps {
                script {
                    docker.withRegistry('','docker-auth') {
                        oDockImage.push()
                    }
                }
            }
        }
        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-PrivateKey']) {
                     sh "scp -o StrictHostKeyChecking=no index.html ubuntu@13.209.75.128:/home/ubuntu/"
                     sh "ssh -o StrictHostKeyChecking=no ubuntu@13.209.75.128 sudo cp /home/ubuntu/index.html /var/www/html/"
                }
            }
        }
    }
}