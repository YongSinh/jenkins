pipeline {
    agent any
    environment { 
        git_repo = 'https://github.com/SalaCyber/employee-api.git'
        digital_ocean ="dop_v1_aa34ee4697ad3c9f5e09ca06a5c97322ae3f2449bfd799bb534cbaf1579eb8b1"
    }
    parameters {

        text(name: 'Release note', defaultValue: 'Hello', description: 'Enter some information about the person')

        choice(name: 'APP_ENV', choices: ['unt', 'preparerod', 'prod'], description: 'Pick something')
    }
    stages {
        stage('config') {
            steps {
                sh'''
                    rm -rf employee-api
                    git clone ${git_repo}
                    pwd
                '''
            }
        }
        stage('Build') {
            steps {
                 sh'''
                    cd employee-api
                    docker build . -t registry.digitalocean.com/jenkins-server:${APP_ENV}${BUILD_NUMBER}
                    docker login registry.digitalocean.com -u dop_v1_011eee63881aa66be55ca252ad73ab907a4a78ca1183113d301cbf33ffe159f1 -p dop_v1_011eee63881aa66be55ca252ad73ab907a4a78ca1183113d301cbf33ffe159f1
                    docker push  registry.digitalocean.com/jenkins-server:${APP_ENV}-${BUILD_NUMBER}
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}