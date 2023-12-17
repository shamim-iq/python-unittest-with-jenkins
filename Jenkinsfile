pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your Git repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/shamim-iq/python-unittest-with-jenkins.git']])
            }
        }

        stage('Install Dependencies') {
            steps {
                
                script {
                    // Install python3-venv package
                    sh 'apt update'
                    sh 'apt install -y python3-venv'

                    // Install Python and any dependencies
                    sh 'python3 -m venv venv'
                    sh 'source venv/bin/activate && pip install -r requirements.txt'
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Run your Python unit tests
                script {
                    sh 'source venv/bin/activate && python3 -m unittest test_website_loader.py'
                }
            }
        }
    }

    post {
        failure {
            // If the tests fail, print a message and set the build as unstable
            script {
                echo 'Website loading test failed.'
                currentBuild.result = 'UNSTABLE'
            }
        }
    }
}
