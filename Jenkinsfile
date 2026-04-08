pipeline {
    agent any

    environment {
        strDockerImage = "${Today}_${BUILD_ID}"
        strDockerImage = "ysh810/cicd-test:${strDockerTag}"
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
                     sh "ssh -o StrictHostKeyChecking=no ubuntu@13.209.75.128 docker container rm -f sampleweb "
                     sh "ssh -o StrictHostKeyChecking=no ubuntu@13.209.75.128 docker run -d -p 80:80 --name sampleweb ${strDockerImage}"
                }
            }
        }
    }
}