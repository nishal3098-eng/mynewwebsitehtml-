pipeline {
    agent any

    environment {
        REPO_URL   = 'https://github.com/nishal3098-eng/mynewwebsitehtml.git'
        BRANCH     = 'main'          // change to 'master' if your repo uses master
        WEB_ROOT   = '/var/www/html'
        CRED_ID    = 'github-test'   // change if your credential ID is different
    }

    stages {

        stage('Checkout') {
            steps {
                echo "✅ Checking out ${REPO_URL} (branch: ${BRANCH})"
                checkout([$class: 'GitSCM',
                    branches: [[name: "*/${BRANCH}"]],
                    userRemoteConfigs: [[
                        url: "${REPO_URL}",
                        credentialsId: "${CRED_ID}"
                    ]]
                ])
            }
        }

        stage('Show Files') {
            steps {
                sh '''
                echo "✅ Workspace path: $WORKSPACE"
                echo "✅ Files in workspace:"
                ls -la
                '''
            }
        }

        stage('Deploy to Nginx') {
            steps {
                sh """
                set -e
                echo "✅ Deploying to Nginx Web Root: ${WEB_ROOT}"

                # remove default nginx page if exists
                sudo rm -f ${WEB_ROOT}/index.nginx-debian.html || true

                # clean web root
                sudo rm -rf ${WEB_ROOT}/*

                # copy website files
                sudo cp -r ${WORKSPACE}/* ${WEB_ROOT}/

                # permissions
                sudo chown -R www-data:www-data ${WEB_ROOT}
                sudo chmod -R 755 ${WEB_ROOT}

                # restart nginx
                sudo systemctl restart nginx

                echo "🎉 Deployment Completed!"
                """
            }
        }
    }

    post {
        success {
            echo "✅ PIPELINE SUCCESS: Website deployed successfully!"
        }
        failure {
            echo "❌ PIPELINE FAILED: Check console output for error."
        }
    }
}
