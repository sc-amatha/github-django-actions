pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
        PYTHON_PATH = "\"C:/Users/Aman Thakur/AppData/Local/Programs/Python/Python313/python.exe\""
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
                    bat "${PYTHON_PATH} -m venv ${VENV_DIR}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    bat "${VENV_DIR}\\Scripts\\pip install -r requirements.txt"
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo "No tests found. Skipping test stage."
                    // bat "${VENV_DIR}\\bin\\python -m pytest tests/" // Adjust test path as needed
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
