pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}/${VIRTUAL_ENV}")) {
                        bat "py -m venv ${VIRTUAL_ENV}"
                    }
                    bat "call ${VIRTUAL_ENV}\\Scripts\\activate.bat && python -m pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat "call ${VIRTUAL_ENV}\\Scripts\\activate.bat && python -m flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat "call ${VIRTUAL_ENV}\\Scripts\\activate.bat && python -m pytest"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Deployment logic, e.g., pushing to a remote server
                    echo "Deploying application..."
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
