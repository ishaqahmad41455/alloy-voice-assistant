pipeline {
    agent any

    triggers {
        githubPush()
    }
    environment {
        // Define your SSH credentials ID in Jenkins
        SSH_CREDENTIALS = 'deploy-key-new'
        // Define your Docker Compose file path on the target server
        DOCKER_COMPOSE_FILE = '/home/ubuntu/stable-admin-backend-dev/docker-compose.yaml'
    }


    stages {
        stage('Deploy to Server') {
            steps {
                // Deploy your application to the server
                script {
                    // SSH into the server and execute commands
                    sshagent(credentials: ['deploy-key-new'], ignoreMissing: true) {
                        // Remove all files in the target directory
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@54.88.67.90 'rm -rf /home/ubuntu/stable-admin-backend-dev/*'"

                        // Change permissions on the destination directory
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@3.81.225.103 'sudo chown -R ubuntu:ubuntu /home/ubuntu/stable-admin-backend-dev'"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@3.81.225.103 'sudo chmod -R 755 /home/ubuntu/stable-admin-backend-dev'"

                        // Upload the files to the server
                        sh "scp -o StrictHostKeyChecking=no -r ${env.WORKSPACE}/* ubuntu@54.88.67.90:/home/ubuntu/stable-admin-backend-dev/"

                        // SSH into the server and deploy using Docker Compose
                        // sh "ssh -o StrictHostKeyChecking=no ubuntu@3.81.225.103 'cd /home/ubuntu/stable-admin-backend-dev && docker compose -f ${DOCKER_COMPOSE_FILE} up -d'"
                    }
                }
            }
        }
    }
}
