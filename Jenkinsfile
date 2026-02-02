pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = "3980332b-a0ea-4374-abc3-23b668b42349"
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

           steps {
            sh '''
                test -f build/index.html
                npm test
            '''
           }
        }

       

        stage('Deploy') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify --status
                '''
            }
        }
    }

     post {
            always {
                junit 'test-results/junit.xml'
            }
        }
}
