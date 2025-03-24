pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
        PYTHON = "python"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Set Up Python') {
            steps {
                script {
                    bat "${PYTHON} -m venv ${VENV_DIR}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    bat "./${VENV_DIR}/bin/pip install -r requirements.txt"
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    bat "./${VENV_DIR}/bin/python -m pytest tests/" // Adjust test path as needed
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
        }
        success {
            echo 'Build and tests successful!'
        }
        failure {
            echo 'Tests failed! Check logs for details.'
        }
    }
}
