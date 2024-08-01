pipeline {
    agent any
    environment { 
        git_repo = 'https://github.com/SalaCyber/employee-api.git'
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
                '''
            }
        }
        stage('Build') {
            steps {
                 sh'''
                    cd employee-api
                    docker build . -t jenkins-server/employee-api:${APP_ENV}-${BUILD_NUMBER}
                    doctl registry login
                    docker tag jenkins-server/employee-api:${APP_ENV}-${BUILD_NUMBER} registry.digitalocean.com/jenkins-server/jenkins-server/employee-api:${APP_ENV}-${BUILD_NUMBER}
                    docker push  registry.digitalocean.com/jenkins-server/jenkins-server/employee-api:${APP_ENV}-${BUILD_NUMBER}
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