pipeline {
    agent any
    environment { 
        git_repo = 'https://github.com/SalaCyber/employee-api.git'
        digital_ocean ='dop_v1_be94922de18cf588efb3b32d217ab39edf8260b91a13ea0d1c8d5fe875df55ed'
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
                    docker login -u ${digital_ocean} -p  ${digital_ocean} registry.digitalocean.com
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