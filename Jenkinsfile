pipeline {
    agent { label 'jenkins-laravel-agent-I4A' }
    environment {
        BOT_TOKEN = credentials('telegram_bot_token')
        CHAT_ID = credentials('telegram_chat_id')
        ANSIBLE_PASS = credentials('ansible_ssh_pass')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Ty-168/laravel-jenkins.git'
            }
        }
        
        stage('Deploy') {
            steps {
                // Running the Ansible playbook
                withCredentials([string(credentialsId: 'ansible_ssh_pass', variable: 'ANSIBLE_PASS')]) {
                    sh 'ansible-playbook -i inventory.ini deploy.yml -e ansible_ssh_pass=$ANSIBLE_PASS'
                }
            }
        }
    }

    post {
        failure {
            // Telegram Notification Example
            sh "curl -s -X POST https://api.telegram.org/bot${BOT_TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d text='❌ Build Failed for Laravel Project!'"
        }
        success {
            echo 'Deployment successful!'
        }
    }
}