pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }
    stages {
        stage('Check Python') {
            steps {
                script {
                    if (isUnix()) {
                        sh "python --version || echo 'Python not found'"
                    } else {
                        bat "python --version || echo 'Python not found'"
                    }
                }
            }
        }
        stage('Setup') {
            steps {
                script {
                    if (isUnix()) {
                        sh "python -m venv ${VIRTUAL_ENV}"
                        sh "source ${VIRTUAL_ENV}/bin/activate && pip install -r requirements.txt"
                    } else {
                        bat "python -m venv ${VIRTUAL_ENV}"
                        bat "${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt"
                    }
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    if (isUnix()) {
                        sh "source ${VIRTUAL_ENV}/bin/activate && flake8 app.py"
                    } else {
                        bat "${VIRTUAL_ENV}\\Scripts\\activate && flake8 app.py"
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Set PYTHONPATH and run pytest
                    if (isUnix()) {
                        sh "export PYTHONPATH=${env.WORKSPACE} && source ${VIRTUAL_ENV}/bin/activate && pytest"
                    } else {
                        bat "set PYTHONPATH=${env.WORKSPACE} && ${VIRTUAL_ENV}\\Scripts\\activate && pytest"
                    }
                }
            }
        }
        stage('Coverage') {
            steps {
                script {
                    // Run code coverage analysis
                    if (isUnix()) {
                        sh """
                        source ${VIRTUAL_ENV}/bin/activate && pip install pytest coverage
                        source ${VIRTUAL_ENV}/bin/activate && coverage run -m pytest
                        source ${VIRTUAL_ENV}/bin/activate && coverage report
                        source ${VIRTUAL_ENV}/bin/activate && coverage html
                        """
                    } else {
                        bat """
                        ${VIRTUAL_ENV}\\Scripts\\activate && pip install pytest coverage
                        ${VIRTUAL_ENV}\\Scripts\\activate && coverage run -m pytest
                        ${VIRTUAL_ENV}\\Scripts\\activate && coverage report
                        ${VIRTUAL_ENV}\\Scripts\\activate && coverage html
                        """
                    }
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    if (isUnix()) {
                        sh "source ${VIRTUAL_ENV}/bin/activate && bandit -r app/"
                    } else {
                        bat "${VIRTUAL_ENV}\\Scripts\\activate && bandit -r app/"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    // Add deployment logic here
                    if (isUnix()) {
                        sh """
                        echo "Deploying on Unix system"
                        # Example deployment command
                        """
                    } else {
                        bat """
                        echo Deploying on Windows system
                        # Example deployment command
                        """
                    }
                }
            }
        }
    }
    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
    }
}
