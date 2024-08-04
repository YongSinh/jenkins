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
        stage('Deploy UAT') {
            steps {
                script {
                    sh '''
                       ssh root@146.190.82.217 'cd srv;\
                                                cat password.txt | docker login --username yongsinh59312 --password-stdin
                                                docker compose up -d'
                    '''
                }
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
                    -d text="<b>Stage</b>: Deploy employee on service employee-api \
                    %0A<b>Status</b>: ${BUILD_USER} \
                    %0A<b>Status</b>: SUCCESS \
                    %0A<b>Version</b>: ${APP_ENV}-${BUILD_NUMBER} \
                    %0A<b>Environment</b>: ${APP_ENV}\
                    %0A<b>Application URL</b>: ${docker_hub_registry}\
                    %0A<b>User Build</b>: Salacyber DevOps \
                    %0A<b>Release Note</b>: Some Release Noted"
                """
            }
        }
        failure {
            script {
                sh """
                     curl -s -X POST https://api.telegram.org/bot${TOKEN}/sendMessage \
                    -d chat_id=${CHAT_ID} \
                    -d parse_mode="HTML" \
                    -d text="<b>Stage</b>: Deploy employee on service employee-api \
                    %0A<b>Status</b>: ${BUILD_USER} \
                    %0A<b>Status</b>: FAILED \
                    %0A<b>Version</b>: ${APP_ENV}-${BUILD_NUMBER} \
                    %0A<b>Environment</b>: ${APP_ENV}\
                    %0A<b>Application URL</b>: ${docker_hub_registry}\
                    %0A<b>User Build</b>: Salacyber DevOps \
                    %0A<b>Release Note</b>: Some Release Noted"
                """
            }
        }
    }
}
