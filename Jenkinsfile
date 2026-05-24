pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Cloning GitHub repository...'
                git branch: 'main',
                url: 'https://github.com/saurabh633848-max/Declarative-pipeline.git'
            }
        }

        stage('Deploy Website to Nginx EC2') {
            steps {

                sshagent(credentials: ['nginx-server']) {

                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@10.0.1.20 "

                    sudo rm -rf /var/www/html/*

                    sudo mkdir -p /var/www/html

                    "
                    '''

                    sh '''
                    scp -o StrictHostKeyChecking=no -r * ubuntu@10.0.1.20:/tmp/
                    '''

                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@10.0.1.20 "

                    sudo cp -r /tmp/* /var/www/html/

                    sudo systemctl restart nginx

                    "
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Website deployed successfully to Nginx EC2'
        }

        failure {
            echo '❌ Deployment failed'
        }
    }
}
