@Library('shared-library') _

pipeline {
    agent any
    environment {
        VENV_DIR = 'venv'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Atul-Torane/Jenkins.git'
            }
        }
        stage('Set Up Virtual Environment') {
            steps {
                script {
                    deployFlaskApp(pipelineParams: [
                        VENV_DIR: "${VENV_DIR}"
                    ])
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh '''
                    source ${VENV_DIR}/bin/activate
                    pip3 install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh '''
                    source ${VENV_DIR}/bin/activate
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                sh '''
                    source ${VENV_DIR}/bin/activate
                    nohup gunicorn --bind 0.0.0.0:5000 wsgi:app &
                    sleep 60
                '''
            }
        }
        
    }
   post {
        always {
            echo 'Cleaning up...'
            sh '''
                . ${VENV_DIR}/bin/activate
                pkill -f gunicorn || true
            '''
        }
    }
}
