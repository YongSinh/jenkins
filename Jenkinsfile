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
        text(name: 'ReleaseNote', defaultValue: 'Hello', description: 'Enter some information about the person')
        choice(name: 'APP_ENV', choices: ['unt', 'preparerod', 'prod'], description: 'Pick something')
    }
    stages {
        stage('config') {
            steps {
                sh'''
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
                 sh'''                   
                    docker login --username yongsinh59312 --password-stdin < password.txt
                    cd employee-api
                    docker build . -t ${docker_hub_registry}:${APP_ENV}${BUILD_NUMBER}
                    docker push  ${docker_hub_registry}:${APP_ENV}${BUILD_NUMBER}
                '''
            }
        }
        stage('Test') {
            steps {
               sh'''                   
                    docker login -u ${digitalocean_token} -p ${digitalocean_token} registry.digitalocean.com
                '''
            }
        }
        post {
            always {
                echo 'I will always say Hello again!'
            }
        }
    }
}
