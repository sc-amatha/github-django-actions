pipeline {
    agent any

    environment {
        DATABASE_URL = "postgres://${{ secrets.POSTGRES_USER }}:${{ secrets.POSTGRES_PASSWORD }}@localhost:5432/${{ secrets.POSTGRES_DB }}"
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_REGION = credentials('AWS_REGION')
        SSH_PRIVATE_KEY = credentials('SSH_PRIVATE_KEY')
        EC2_HOST = credentials('EC2_HOST')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/sc-amatha/github-django-actions.git'
            }
        }

        stage('Set up Python') {
            steps {
                sh 'python3 -m venv venv'
                sh 'source venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Run Migrations') {
            steps {
                sh 'source venv/bin/activate && python manage.py migrate'
            }
        }

        stage('Collect Static Files') {
            steps {
                sh 'source venv/bin/activate && python manage.py collectstatic --noinput'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'source venv/bin/activate && python manage.py test'
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                sshagent(['SSH_PRIVATE_KEY']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@$EC2_HOST << 'EOF'
                    cd /home/ubuntu/github-django-actions
                    git pull origin master
                    source venv/bin/activate
                    pip install -r requirements.txt
                    python manage.py migrate
                    python manage.py collectstatic --noinput
                    sudo systemctl restart gunicorn
                    sudo systemctl restart nginx
                    EOF
                    '''
                }
            }
        }
    }
}
