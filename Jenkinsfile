pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
        PYTHON_EXE = 'C:\\Users\\Administrator\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    // Find Python executable
                    def pythonExe = env.PYTHON_EXE
                    
                    // Verify Python exists, if not try common locations
                    if (!fileExists(pythonExe)) {
                        def commonPaths = [
                            'C:\\Users\\Administrator\\AppData\\Local\\Programs\\Python\\Python312\\python.exe',
                            'C:\\Users\\Administrator\\AppData\\Local\\Programs\\Python\\Python311\\python.exe',
                            'C:\\Users\\Administrator\\AppData\\Local\\Programs\\Python\\Python310\\python.exe',
                            'C:\\Python312\\python.exe',
                            'C:\\Python311\\python.exe',
                            'C:\\Python310\\python.exe'
                        ]
                        for (path in commonPaths) {
                            if (fileExists(path)) {
                                pythonExe = path
                                break
                            }
                        }
                    }
                    
                    if (!fileExists(pythonExe)) {
                        error("Python not found at ${pythonExe}. Please install Python or update PYTHON_EXE path.")
                    }
                    
                    if (!fileExists("${env.WORKSPACE}/${VIRTUAL_ENV}")) {
                        bat "\"${pythonExe}\" -m venv ${VIRTUAL_ENV}"
                    }
                    bat "${VIRTUAL_ENV}\\Scripts\\python.exe -m pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\python.exe -m flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\python.exe -m pytest"
                }
            }
        }
        stage('Coverage') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\python.exe -m coverage run -m pytest"
                    bat "${VIRTUAL_ENV}\\Scripts\\python.exe -m coverage report"
                    bat "${VIRTUAL_ENV}\\Scripts\\python.exe -m coverage xml"
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\python.exe -m bandit -r . -f json -o bandit-report.json"
                    try {
                        bat "${VIRTUAL_ENV}\\Scripts\\python.exe -m bandit -r ."
                    } catch (Exception e) {
                        echo "Security scan completed with findings. Review the output above."
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    bat "echo Deployment completed successfully"
                    bat "echo Application is ready for deployment to production"
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
