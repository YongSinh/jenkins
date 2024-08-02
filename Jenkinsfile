pipeline {
    agent any
    environment { 
        git_repo = 'https://github.com/SalaCyber/employee-api.git'
        registry_url="registry.digitalocean.com/jenkins-server"
        docker_hub_registry= "yongsinh59312/employee-api"
        digitalocean_token = credentials('digitalocean_token')
        docker_registry = credentials('docker_registry_pwd')
        PASSWORD = credentials('docker_registry_pwd')
        TOKEN = credentials('telegram-credentials')
        CHAT_ID = credentials('Telegram_ChatID')
    }
    parameters {
        text(name: 'ReleaseNote', defaultValue: 'Hello', description: 'Enter some information about the release')
        choice(name: 'APP_ENV', choices: ['unt', 'preparerod', 'prod'], description: 'Pick the environment')
    }
    stages {
        stage('config') {
            steps {
                sh '''
                    rm -rf employee-api password.txt
                    touch password.txt
                    echo ${PASSWORD} > password.txt
                    cat password.txt
                    git clone ${git_repo}
                    pwd
                '''
            }
        }
        stage('Build') {
            steps {
                 sh '''                   
                    cat password.txt | docker login --username yongsinh59312 --password-stdin
                    cd employee-api
                    docker build . -t ${docker_hub_registry}:${APP_ENV}${BUILD_NUMBER}
                    docker push ${docker_hub_registry}:${APP_ENV}${BUILD_NUMBER}
                '''
            }
        }
        stage('Test') {
            steps {
               sh '''                   
                    echo ${digitalocean_token} | docker login -u ${digitalocean_token} --password-stdin ${registry_url}
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    post {
        success {
            script {
                sh """
                curl -s -X POST https://api.telegram.org/bot${TOKEN}/sendMessage \
                -d chat_id=${CHAT_ID} \
                -d parse_mode="HTML" \
                -d text="<b>Stage</b>: Deploy employee on service employee-api<br>\
                <b>Status</b>: SUCCESS<br>\
                <b>Version</b>: uat-23<br>\
                <b>Environment</b>: uat<br>\
                <b>Application URL</b>: https://app.sothy.cloud<br>\
                <b>User Build</b>: Salacyber DevOps<br>\
                <b>Release Note</b>: Some Release Noted"
                """
            }
        }
        failure {
            script {
                sh """
                curl -s -X POST https://api.telegram.org/bot${TOKEN}/sendMessage \
                -d chat_id=${CHAT_ID} \
                -d parse_mode="HTML" \
                -d text="<b>Stage</b>: Deploy employee on service employee-api<br>\
                <b>Status</b>: FAILED<br>\
                <b>Version</b>: uat-23<br>\
                <b>Environment</b>: uat<br>\
                <b>Application URL</b>: https://app.sothy.cloud<br>\
                <b>User Build</b>: Salacyber DevOps<br>\
                <b>Release Note</b>: Some Release Noted"
                """
            }
        }
    }
}
