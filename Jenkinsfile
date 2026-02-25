pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/TON_USERNAME/landing-page-example'
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Build en cours..."'
                sh 'ls -la'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Tests en cours..."'
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'alwaysdata-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh """
                        sshpass -p "\$PASS" ssh -o StrictHostKeyChecking=no \
                        \$USER@ssh-\$USER.alwaysdata.net "mkdir -p /home/\$USER/www/landing-page-example"
                        
                        sshpass -p "\$PASS" scp -o StrictHostKeyChecking=no -r ./* \
                        \$USER@ssh-\$USER.alwaysdata.net:/home/\$USER/www/landing-page-example/
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Déploiement réussi sur Alwaysdata !'
        }
        failure {
            echo 'Échec du déploiement !'
        }
    }
}
