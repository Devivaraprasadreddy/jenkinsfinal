pipeline {
    agent any

    environment {
        APP_DIR = "portfolio-app"
    }

    triggers {
        githubPush() // This will trigger build on push (if webhook is configured properly)
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Repository using GitSCM') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Devivaraprasadreddy/portfolioreactwithjenkins.git',
                        credentialsId: 'gitkey' // ✅ Replace this with your real Jenkins credential ID
                    ]]
                ])
            }
        }

        stage('Install Node.js and npm') {
            steps {
                sh '''
                curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
                sudo apt-get install -y nodejs
                node -v
                npm -v
                '''
            }
        }

        stage('Install Dependencies and Build React App') {
            steps {
                dir(APP_DIR) {
                    sh '''
                    npm install
                    npm run build
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Something went wrong. Check build logs."
        }
    }
}
