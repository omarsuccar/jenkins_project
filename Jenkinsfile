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
                        sh "python -m venv ${VIRTUAL_ENV}"
                    }
                    sh "source ${VIRTUAL_ENV}/bin/activate && pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    sh "source ${VIRTUAL_ENV}/bin/activate && flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh "source ${VIRTUAL_ENV}/bin/activate && pytest"
                }
            }
        }
        stage('Coverage') {
            steps {
                script {
                    sh """
                    source ${VIRTUAL_ENV}/bin/activate && \
                    coverage run -m pytest && \
                    coverage report -m && \
                    coverage html
                    """
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    sh "source ${VIRTUAL_ENV}/bin/activate && bandit -r ."
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    // Deployment logic goes here
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