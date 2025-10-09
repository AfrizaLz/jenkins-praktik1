pipeline {
    agent { docker { image 'python:3.10' } }
    tools {
        // INI BARIS YANG DIPERBAIKI
        dockerTool 'docker' 
    }
    environment {
        VENV = "venv"
    }
    stages {
        stage('Setup & Install') {
            steps {
                script {
                    sh "python -m venv ${VENV}"
                    sh ". ${VENV}/bin/activate && pip install -r requirements.txt"
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    sh ". ${VENV}/bin/activate && pytest test_app.py"
                }
            }
        }
        stage('Deploy') {
            when { branch 'main' }
            steps { echo "Deploying main branch" }
        }
    }
    post {
        success {
            script {
                httpRequest(
                    url: 'https://discord.com/api/webhooks/1425727349357281345/7rBi27X5bkbWZwCOICOBeF0bWXUPhXaXSKu4FWStOuTvXQbB3WAddfKADQPJZgf980H9',
                    contentType: 'APPLICATION_JSON',
                    requestBody: '{"content": "✅ Build SUCCESS on `${env.BRANCH_NAME}`. Cek: ${env.BUILD_URL}"}'
                )
            }
        }
        failure {
            script {
                httpRequest(
                    url: 'https://discord.com/api/webhooks/1425727349357281345/7rBi27X5bkbWZwCOICOBeF0bWXUPhXaXSKu4FWStOuTvXQbB3WAddfKADQPJZgf980H9',
                    contentType: 'APPLICATION_JSON',
                    requestBody: '{"content": "❌ Build FAILED on `${env.BRANCH_NAME}`. Cek: ${env.BUILD_URL}"}'
                )
            }
        }
    }
}
