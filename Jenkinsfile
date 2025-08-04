pipeline {
    agent any

    environment {
        REMOTE_USER = "ec2-user"
        REMOTE_HOST = "54.252.171.14"
        SSH_KEY_ID = "ec2-key"
    }

    triggers {
        githubPush()  // Automatically trigger on GitHub push
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Optional: Checkout code on Jenkins node (if needed)
                git branch: 'master', url: 'https://github.com/your-org/your-repo.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: [env.SSH_KEY_ID]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST '
                        sudo su - root -c "
                            cd /root/node_deeployment &&
                            git pull origin master &&
                            npm install &&
                            pm2 restart all || pm2 start app.js
                        "
                    '
                    '''
                }
            }
        }
    }
}
