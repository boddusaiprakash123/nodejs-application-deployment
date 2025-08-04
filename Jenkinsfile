pipeline {
    agent any

    environment {
        REMOTE_USER = "ec2-user"
        REMOTE_HOST = "13.211.168.117 "
        SSH_KEY_ID = "ec2-key"
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/boddusaiprakash123/nodejs-application-deployment.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: [env.SSH_KEY_ID]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${env.REMOTE_USER}@${env.REMOTE_HOST} \\
                        "sudo su - root -c 'cd /root/node_deeployment && git pull origin master && npm install && pm2 restart all || pm2 start app.js'"
                    """
                }
            }
        }
    }
}
