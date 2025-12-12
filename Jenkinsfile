pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                  branches: [[name: '*/main']],   // change to master if needed
                  userRemoteConfigs: [[
                    url: 'https://github.com/nishal3098-eng/mynewwebsitehtml.git',
                    credentialsId: 'github-pat2'
                  ]]
                ])
            }
        }

        stage('Deploy to Nginx') {
            steps {
                sh '''
                sudo rm -f /var/www/html/index.nginx-debian.html || true
                sudo rm -rf /var/www/html/*
                sudo cp -r * /var/www/html/
                sudo systemctl restart nginx
                '''
            }
        }
    }
}
