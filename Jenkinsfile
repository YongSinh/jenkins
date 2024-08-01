pipeline {
    agent any
    environment { 
        git_repo = 'https://github.com/SalaCyber/employee-api.git'
        digital_ocean ='dop_v1_135a8b85a6acbe7226a0f5b51a7f04f7638a7b77771860f1620a18bb14f31c42'
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
                    docker login registry.digitalocean.com -u ${digital_ocean} -p ${digital_ocean} 
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