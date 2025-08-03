pipeline {
    agent any
    triggers{
        githubPush()
    }
    parameters {
        choice(name: 'BRANCH_NAME', choices: ['main'], description: 'Choose the branch to deploy')
    }

    environment {
        APP_DIR = "portfolio-app"
        GIT_CREDENTIALS_ID = 'jenkinsfinal'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout') {
            steps {
                script {
                    // Checkout the specified branch using Git credentials
                    checkout([$class: 'GitSCM', branches: [[name: "${params.BRANCH_NAME}"]],
                              userRemoteConfigs: [[url: 'https://github.com/Devivaraprasadreddy/portfolioreactwithjenkins.git', credentialsId: "${env.GIT_CREDENTIALS_ID}"]]])
                }
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
        always {
            echo 'Deployment completed.'
        }
    }
}
