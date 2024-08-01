pipeline {
    agent any
    environment { 
        git_repo = 'https://github.com/SalaCyber/employee-api.git',
        digital_ocean ='dop_v1_d72094a51a2d3c8f7161b90ca3b539f62857b685446e32b581742ec537d259eb'
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
                    docker build . -t jenkins-server/employee-api:${APP_ENV}${BUILD_NUMBER}
                    docker login docker login -u ${digital_ocean} -p  ${digital_ocean} registry.digitalocean.com
                    sudo snap install doctl
                    docker push  jenkins-server/employee-api:${APP_ENV}-${BUILD_NUMBER}
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