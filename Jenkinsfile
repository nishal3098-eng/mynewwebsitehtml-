pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/nishal3098-eng/mynewwebsitehtml.git',
                    credentialsId: 'github-pat'
            }
        }

        stage('Deploy to Nginx') {
            steps {
                sh '''
                sudo rm -f /var/www/html/index.nginx-debian.html || true
                sudo rm -rf /var/www/html/*
                sudo cp -r * /var/www/html/
                sudo chown -R www-data:www-data /var/www/html
                sudo chmod -R 755 /var/www/html
                sudo systemctl restart nginx
                '''
            }
        }
    }
}
