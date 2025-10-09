pipeline {
    agent any
    stages {
        stage('Install Dependencies') { steps { sh 'pip install -r requirements.txt' } }
        stage('Run Tests') { steps { sh 'pytest test_app.py' } }
        stage('Deploy') { when { branch 'main' }; steps { echo "Deploying main" } }
    }
    post {
        success { script { httpRequest(url: 'https://discord.com/api/webhooks/1425701485144182894/TfPGpcYk2tusUaZ989_jy4CQMCErmsPoJMjY3h35EaAd1zK2C6fz2mNE52SJPcG8fZUb', requestBody: '{"content": "✅ Build SUCCESS on `${env.BRANCH_NAME}`"}') } }
        failure { script { httpRequest(url: 'PASTE_WEBHOOK_URL_DI_SINI', requestBody: '{"content": "❌ Build FAILED on `${env.BRANCH_NAME}`"}') } }
    }
}
